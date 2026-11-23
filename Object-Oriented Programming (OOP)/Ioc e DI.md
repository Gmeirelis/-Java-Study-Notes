
## 1. Inversão de Controle (IoC)

**Inversão de Controle (Inversion of Control — IoC)** é um princípio de desenvolvimento que consiste em retirar de uma classe a responsabilidade de instanciar suas próprias dependências.

Em vez de uma classe controlar a criação dos objetos de que precisa, essa responsabilidade é transferida para um componente externo.

### Exemplo sem IoC

Imagine um sistema de pedidos em que `OrderService` precisa de `PaymentService` para processar pagamentos.

```java
public class OrderService {

    private PaymentService paymentService = new PaymentService();

    public void processOrder() {
        paymentService.processPayment();
    }
}
```

Nesse exemplo, `OrderService` instancia diretamente `PaymentService` utilizando `new`.

**Problema:**
- Alto acoplamento entre as classes.
- Dificuldade para substituir implementações.
- Maior dificuldade para realizar testes.
- A classe controla a criação de suas dependências.

### Exemplo com IoC

Agora, vamos transferir a responsabilidade de criar a dependência para uma classe externa.

```java
public class OrderService {

    private PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    public void processOrder() {
        paymentService.processPayment();
    }
}
```

A classe `OrderService` não instancia mais `PaymentService`. Ela recebe o objeto através do construtor.

A criação fica sob responsabilidade de outro componente.

```java
public class Program {

    public static void main(String[] args) {

        PaymentService paymentService = new PaymentService();

        OrderService orderService =
                new OrderService(paymentService);

        orderService.processOrder();
    }
}
```

**Resultado:** a responsabilidade pela criação da dependência foi transferida para fora de `OrderService`.

---

## 2. Injeção de Dependência (DI)

**Injeção de Dependência (Dependency Injection — DI)** é uma técnica utilizada para implementar a Inversão de Controle.

Consiste em fornecer a uma classe os objetos de que ela precisa, em vez de permitir que ela mesma os instancie.

### Injeção via construtor

É uma das formas mais utilizadas de injeção de dependência.

```java
public class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Observe:

```java
private final PaymentService paymentService;
```

O atributo `final` garante que a referência seja inicializada uma única vez.

A dependência é recebida pelo construtor:

```java
public OrderService(PaymentService paymentService)
```

### Formas de Injeção de Dependência

Existem três formas comuns:

**1. Constructor Injection**

A dependência é fornecida pelo construtor.

**2. Setter Injection**

A dependência é fornecida por um método setter.

**3. Field Injection**

A dependência é injetada diretamente no atributo da classe.

No Spring Boot, recomenda-se utilizar Constructor Injection para dependências obrigatórias.

---

## 3. Diferença entre IoC e DI

| Conceito | Definição |
|---|---|
| IoC | Princípio de inverter o controle da criação e gerenciamento das dependências. |
| DI | Técnica utilizada para fornecer dependências a uma classe. |
| IoC Container | Mecanismo responsável por gerenciar os objetos e suas dependências. |

**Resumindo:**

- IoC é o princípio.
- DI é uma técnica para implementar esse princípio.
- IoC Container é o mecanismo que gerencia os objetos.

---

## 4. IoC no Spring Boot

No Spring Framework, o gerenciamento de dependências é realizado pelo **IoC Container**.

O container é responsável por:

- Instanciar objetos.
- Gerenciar os objetos da aplicação.
- Identificar dependências.
- Injetar dependências automaticamente.
- Gerenciar o ciclo de vida dos objetos.

Os objetos gerenciados pelo Spring são chamados de **Spring Beans**.

### Exemplo prático

Primeiro, criamos um serviço de pagamento:

```java
import org.springframework.stereotype.Service;

@Service
public class PaymentService {

    public void processPayment() {
        System.out.println("Pagamento processado!");
    }
}
```

A anotação `@Service` informa ao Spring que essa classe é um componente gerenciado.

Agora, criamos o serviço de pedidos:

```java
import org.springframework.stereotype.Service;

@Service
public class OrderService {

    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    public void processOrder() {
        paymentService.processPayment();
    }
}
```

O Spring identifica que `OrderService` precisa de `PaymentService`.

Como ambas as classes são componentes gerenciados pelo Spring, o container consegue fornecer a dependência automaticamente.

Não precisamos instanciar manualmente os objetos utilizando `new`.

---

## 5. Benefícios da Injeção de Dependência

### Baixo acoplamento
As classes deixam de controlar diretamente a criação de suas dependências.

### Facilidade de manutenção
É possível substituir implementações sem modificar toda a estrutura da aplicação.

### Testabilidade
Podemos fornecer dependências simuladas (mocks) durante os testes unitários.

### Flexibilidade
A aplicação fica mais preparada para evoluir e receber novas implementações.

---

## 6. Exemplo de aplicação com interfaces

Imagine que um sistema aceite diferentes formas de pagamento.

Criamos uma interface:

```java
public interface Payment {

    void processPayment();
}
```

Implementação para cartão:

```java
public class CreditCardPayment implements Payment {

    @Override
    public void processPayment() {
        System.out.println("Pagamento com cartão!");
    }
}
```

Implementação para PIX:

```java
public class PixPayment implements Payment {

    @Override
    public void processPayment() {
        System.out.println("Pagamento com PIX!");
    }
}
```

Nosso serviço:

```java
public class OrderService {

    private final Payment payment;

    public OrderService(Payment payment) {
        this.payment = payment;
    }

    public void processOrder() {
        payment.processPayment();
    }
}
```

Observe que `OrderService` depende da interface `Payment`, não de uma implementação específica.

Isso permite trabalhar com diferentes formas de pagamento sem alterar a lógica principal do serviço.

---

## 7. Perguntas de entrevista

### O que é Inversão de Controle?

É um princípio de desenvolvimento que transfere a responsabilidade de criar e gerenciar dependências de uma classe para um componente externo.

### O que é Injeção de Dependência?

É uma técnica que consiste em fornecer a uma classe os objetos de que ela precisa, em vez de permitir que ela mesma os instancie.

### Qual a diferença entre IoC e DI?

IoC é o princípio de inversão de controle, enquanto DI é uma técnica utilizada para implementar esse princípio.

### O que é IoC Container?

É o mecanismo responsável por instanciar, gerenciar e fornecer as dependências dos objetos da aplicação.

### O que são Spring Beans?

São objetos gerenciados pelo container do Spring.

### Qual tipo de injeção é recomendado no Spring?

Constructor Injection, pois permite declarar dependências obrigatórias, facilita testes e possibilita utilizar atributos `final`.

### Quais são os benefícios da injeção de dependência?

Redução de acoplamento, facilidade de manutenção, flexibilidade e facilidade para realizar testes.

---

## 8. Resumo para revisão

| Termo | Significado |
|---|---|
| IoC | Inversão de Controle |
| DI | Injeção de Dependência |
| IoC Container | Gerenciador de objetos e dependências |
| Spring Bean | Objeto gerenciado pelo Spring |
| Constructor Injection | Injeção pelo construtor |
| Setter Injection | Injeção por método setter |
| Field Injection | Injeção diretamente no atributo |

### Fluxo de funcionamento

```text
Spring Application
       |
       v
   IoC Container
       |
       v
 Instancia os Beans
       |
       v
 Identifica dependências
       |
       v
 Injeta as dependências
       |
       v
 Aplicação pronta para executar
```

**Conceito fundamental:** uma classe deve se preocupar com sua responsabilidade de negócio, e não necessariamente com a criação e o gerenciamento de todos os objetos dos quais depende.
