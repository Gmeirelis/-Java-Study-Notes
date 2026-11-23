# Inversão de Controle (IoC) e Injeção de Dependência (DI)

## 1. O problema: acoplamento forte

Quando uma classe cria suas próprias dependências com `new`, ela fica presa a uma implementação concreta.

```java
public class PedidoService {

    // A classe decide QUAL implementação usar e COMO criá-la
    private PedidoRepository repository = new PedidoRepositoryMySQL();

    public void salvar(Pedido pedido) {
        repository.salvar(pedido);
    }
}
```

**Problemas:**

- Difícil trocar o MySQL por outro banco (ou por um mock nos testes).
- `PedidoService` precisa conhecer detalhes de construção de `PedidoRepositoryMySQL`.
- Testes unitários ficam praticamente impossíveis sem acessar o banco real.

---

## 2. Inversão de Controle (IoC)

**IoC é um princípio de design**: em vez de a sua classe controlar a criação e o ciclo de vida das suas dependências, esse controle é **transferido para outra entidade** (um framework, um container ou o código que monta a aplicação).

> "Não nos chame, nós chamamos você." (Princípio de Hollywood)

Exemplos de IoC (nem todos são DI):

| Forma | Como funciona |
|-------|---------------|
| Injeção de dependência | Alguém de fora entrega a dependência pronta |
| Eventos / callbacks | O framework chama o seu código quando algo acontece |
| Template Method | A classe base controla o fluxo, a subclasse preenche os passos |
| Service Locator | A classe pede a dependência a um registro central |

**Resumo:** IoC é o **princípio** (o "o quê"). DI é uma **técnica** para aplicá-lo (o "como").

---

## 3. Injeção de Dependência (DI)

**DI é o padrão em que uma classe recebe suas dependências de fora**, em vez de criá-las.

### 3.1 Passo 1: depender de abstrações (interface)

```java
public interface PedidoRepository {
    void salvar(Pedido pedido);
}

public class PedidoRepositoryMySQL implements PedidoRepository {
    @Override
    public void salvar(Pedido pedido) {
        // grava no MySQL
    }
}

public class PedidoRepositoryEmMemoria implements PedidoRepository {
    private final List<Pedido> pedidos = new ArrayList<>();

    @Override
    public void salvar(Pedido pedido) {
        pedidos.add(pedido);
    }
}
```

### 3.2 Passo 2: receber a dependência de fora

```java
public class PedidoService {

    private final PedidoRepository repository;

    // A dependência é INJETADA pelo construtor
    public PedidoService(PedidoRepository repository) {
        this.repository = repository;
    }

    public void salvar(Pedido pedido) {
        repository.salvar(pedido);
    }
}
```

### 3.3 Passo 3: quem monta tudo (composition root)

```java
public class Main {
    public static void main(String[] args) {
        PedidoRepository repo = new PedidoRepositoryMySQL();
        PedidoService service = new PedidoService(repo);

        service.salvar(new Pedido());
    }
}
```

Agora `PedidoService` não sabe qual banco está sendo usado. Quem decide é o `Main`. **O controle foi invertido.**

---

## 4. Tipos de injeção

### 4.1 Por construtor (recomendada)

```java
public PedidoService(PedidoRepository repository) {
    this.repository = repository;
}
```

- Dependências obrigatórias e explícitas.
- Permite campos `final` (imutabilidade).
- Fácil de testar.

### 4.2 Por setter

```java
public void setRepository(PedidoRepository repository) {
    this.repository = repository;
}
```

- Útil para dependências **opcionais** ou que podem mudar.
- Objeto pode ficar em estado incompleto se esquecerem de chamar o setter.

### 4.3 Por campo (evitar)

```java
@Autowired
private PedidoRepository repository;
```

- Código curto, mas esconde dependências.
- Dificulta testes (precisa de reflexão ou do framework).
- Não permite `final`.

| Tipo | Obrigatória? | Testável? | Recomendação |
|------|:---:|:---:|:---:|
| Construtor | Sim | Ótimo | ✅ Preferir |
| Setter | Não | Bom | ⚠️ Casos opcionais |
| Campo | Sim | Ruim | ❌ Evitar |

---

## 5. Benefícios

- **Baixo acoplamento:** classes dependem de interfaces, não de implementações.
- **Testabilidade:** basta injetar um mock ou uma implementação falsa.
- **Flexibilidade:** trocar implementações sem alterar a classe consumidora.
- **Reuso e manutenção:** responsabilidades mais claras.
- **Princípio da Inversão de Dependência (o "D" do SOLID):** módulos de alto nível não dependem dos de baixo nível; ambos dependem de abstrações.

### Exemplo de teste com DI

```java
@Test
void deveSalvarPedido() {
    PedidoRepositoryEmMemoria repo = new PedidoRepositoryEmMemoria();
    PedidoService service = new PedidoService(repo);

    service.salvar(new Pedido());

    // verifica sem precisar de banco de dados
}
```

---

## 6. Container de IoC

Montar tudo manualmente (como no `Main`) funciona, mas em aplicações grandes vira trabalho demais. Um **container de IoC** automatiza isso:

1. Descobre quais classes são gerenciadas ("beans").
2. Identifica as dependências de cada uma.
3. Cria os objetos na ordem certa.
4. Injeta as dependências.
5. Gerencia o ciclo de vida (singleton, escopo por requisição etc.).

Exemplos: **Spring** (Java), Guice, Dagger, CDI (Jakarta EE).

---

## 7. Como o Spring faz (prévia para Spring Boot)

```java
@Repository
public class PedidoRepositoryMySQL implements PedidoRepository {
    @Override
    public void salvar(Pedido pedido) { /* ... */ }
}

@Service
public class PedidoService {

    private final PedidoRepository repository;

    // Spring injeta automaticamente (com um único construtor, @Autowired é opcional)
    public PedidoService(PedidoRepository repository) {
        this.repository = repository;
    }
}

@RestController
@RequestMapping("/pedidos")
public class PedidoController {

    private final PedidoService service;

    public PedidoController(PedidoService service) {
        this.service = service;
    }
}
```

### Principais anotações

| Anotação | Função |
|----------|--------|
| `@Component` | Marca uma classe genérica como bean |
| `@Service` | Bean da camada de regras de negócio |
| `@Repository` | Bean da camada de acesso a dados |
| `@Controller` / `@RestController` | Bean da camada web |
| `@Configuration` + `@Bean` | Define beans manualmente em código |
| `@Autowired` | Pede injeção (opcional em construtor único) |
| `@Qualifier("nome")` | Escolhe um bean específico quando há várias implementações |
| `@Primary` | Define a implementação padrão |

### Quando existem duas implementações

```java
@Service
@Primary
public class PedidoRepositoryMySQL implements PedidoRepository { }

@Service("emMemoria")
public class PedidoRepositoryEmMemoria implements PedidoRepository { }

// Escolhendo explicitamente:
public PedidoService(@Qualifier("emMemoria") PedidoRepository repository) {
    this.repository = repository;
}
```

### Definindo bean manualmente

```java
@Configuration
public class AppConfig {

    @Bean
    public PedidoRepository pedidoRepository() {
        return new PedidoRepositoryMySQL();
    }
}
```

---

## 8. Escopos de beans no Spring

| Escopo | Comportamento |
|--------|---------------|
| `singleton` (padrão) | Uma única instância para toda a aplicação |
| `prototype` | Nova instância a cada injeção |
| `request` | Uma instância por requisição HTTP |
| `session` | Uma instância por sessão HTTP |

---

## 9. Analogia no React (você já usa DI sem perceber)

Passar dependências por **props** ou **Context** é a mesma ideia:

```jsx
// Acoplado: o componente cria/importa a implementação diretamente
function Lista() {
  const dados = apiMySQL.buscar();
  // ...
}

// Com injeção: o componente recebe a dependência de fora
function Lista({ servico }) {
  const dados = servico.buscar();
  // ...
}

<Lista servico={servicoReal} />
<Lista servico={servicoFake} /> // em testes
```

Com `Context API`, o `Provider` age como o "container" que injeta o serviço nos componentes filhos.

---

## 10. Erros comuns

- Injetar a **implementação concreta** em vez da interface.
- Usar `new` dentro de classes de negócio para criar serviços.
- Abusar de injeção por campo.
- **Dependência circular** (A precisa de B, que precisa de A): indica falha de design.
- Classes com construtores com muitos parâmetros: sinal de que a classe tem responsabilidades demais (violação do SRP).
- Confundir **IoC** (princípio) com **DI** (técnica) ou com **Spring** (ferramenta).

---

## 11. Resumo rápido

| Conceito | O que é |
|----------|---------|
| **IoC** | Princípio: o controle de criar/gerenciar dependências sai da classe |
| **DI** | Técnica: dependências são entregues de fora (construtor, setter, campo) |
| **Container IoC** | Ferramenta que automatiza a DI (ex.: Spring) |
| **Bean** | Objeto gerenciado pelo container |
| **DIP (SOLID)** | Depender de abstrações, não de implementações |

**Em uma frase:** a classe para de *buscar/criar* o que precisa e passa a *receber* o que precisa.

---

## 12. Checklist de estudo

- [ ] Entender o problema do acoplamento forte com `new`
- [ ] Diferenciar IoC de DI
- [ ] Criar interfaces e múltiplas implementações em Java puro
- [ ] Fazer injeção manual por construtor em um `Main`
- [ ] Escrever um teste usando uma implementação falsa
- [ ] Conhecer `@Component`, `@Service`, `@Repository`, `@Autowired`
- [ ] Entender `@Qualifier`, `@Primary` e `@Bean`
- [ ] Conhecer os escopos de bean
- [ ] Praticar em um projeto Spring Boot simples (controller → service → repository)
