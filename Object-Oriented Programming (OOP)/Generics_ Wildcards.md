# Generics e Wildcards em Java

## 1. O que são Generics?

Generics permitem criar classes, interfaces e métodos que trabalham com diferentes tipos de dados, mantendo a segurança de tipos (*Type Safety*).

Seu principal objetivo é permitir a reutilização de código sem precisar criar implementações específicas para cada tipo.

### Exemplo sem Generics

```java
public class CalculateService {

    public static Product max(List<Product> list) {
        // implementação
    }
}
```

Nesse exemplo, o método está limitado à classe `Product`.

Se quisermos trabalhar com outra classe, precisaremos modificar ou criar outro método.

### Exemplo com Generics

```java
public class CalculateService {

    public static <T> T max(List<T> list) {
        // implementação
    }
}
```

Agora o método pode trabalhar com diferentes tipos.

- `<T>` declara um tipo genérico.
- `List<T>` recebe uma lista desse tipo.
- `T` como retorno devolve o próprio tipo recebido.

**Importante:** `T` é uma convenção. Poderíamos utilizar outros identificadores, como `E`, `K`, `V`.

---

## 2. Bounded Types (Tipos Delimitados)

Podemos restringir quais tipos serão aceitos por um parâmetro genérico utilizando `extends`.

### Exemplo

Interface:

```java
public interface Price {
    double getPrice();
}
```

Classe Product:

```java
public class Product implements Price {

    private String name;
    private double price;

    @Override
    public double getPrice() {
        return price;
    }
}
```

CalculateService:

```java
public class CalculateService {

    public static <T extends Price> T max(List<T> list) {

        if (list.isEmpty()) {
            throw new IllegalStateException("List is empty");
        }

        T max = list.get(0);

        for (T item : list) {
            if (item.getPrice() > max.getPrice()) {
                max = item;
            }
        }

        return max;
    }
}
```

### Como funciona?

```java
<T extends Price>
```

Significa que `T` pode representar diferentes classes, desde que implementem a interface `Price` ou sejam compatíveis com o limite estabelecido.

Exemplos:

- `Product implements Price` → permitido.
- `Service implements Price` → permitido.
- `Employee` sem implementar `Price` → não permitido.

### Por que utilizar?

Porque o compilador garante que os objetos recebidos possuem o método `getPrice()`.

Assim, podemos acessar esse método sem precisar conhecer a classe concreta.

**Observação:** em parâmetros genéricos, `extends` também pode estabelecer limites por interfaces, não apenas por classes.

---

# 3. Wildcards (Curingas)

Wildcards são representados pelo caractere `?`.

Eles permitem trabalhar com tipos desconhecidos, especialmente em parâmetros que recebem estruturas genéricas, como `List`.

## 3.1 Wildcard sem limite: `<?>`

Representa uma lista de tipo desconhecido.

```java
public static void showList(List<?> list) {

    for (Object item : list) {
        System.out.println(item);
    }
}
```

Podemos receber:

```java
List<String>
List<Integer>
List<Product>
```

Todas são aceitas.

### Limitação

Não podemos adicionar elementos arbitrários:

```java
list.add("Java"); // Erro de compilação
```

Isso acontece porque não sabemos qual é o tipo específico da lista.

Podemos ler os elementos como `Object`.

---

## 3.2 Wildcard com limite superior: `<? extends T>`

Permite receber listas cujo tipo seja `T` ou algum subtipo de `T`.

Exemplo:

```java
public static double totalArea(List<? extends Shape> list) {

    double sum = 0.0;

    for (Shape item : list) {
        sum += item.getArea();
    }

    return sum;
}
```

Se `Circle` e `Rectangle` implementam `Shape`, podemos receber:

```java
List<Circle>
List<Rectangle>
```

### Características

- Permite ler os elementos como `Shape`.
- Permite acessar os métodos definidos em `Shape`.
- Não permite adicionar livremente objetos à lista.

É utilizado principalmente quando queremos consumir ou consultar elementos.

---

## 3.3 Wildcard com limite inferior: `<? super T>`

Permite receber listas cujo tipo seja `T` ou algum supertipo de `T`.

Exemplo:

```java
public static void addPayments(List<? super Payment> list) {

    list.add(new PixPayment(100.0));

    list.add(new CreditCardPayment(200.0));
}
```

Podemos receber:

```java
List<Payment>
List<Object>
```

Desde que `PixPayment` e `CreditCardPayment` implementem `Payment`.

### Características

- Permite adicionar objetos do tipo `Payment`.
- Permite adicionar objetos que implementam `Payment`.
- Ao ler elementos, o tipo garantido é `Object`.

É utilizado principalmente quando queremos adicionar elementos a uma estrutura genérica.

---

# 4. Diferença entre T e ?

Embora sejam relacionados, possuem finalidades diferentes.

### T — Tipo genérico

```java
public static <T extends Shape> T max(List<T> list)
```

Utilizamos `T` quando precisamos identificar e reutilizar o tipo genérico.

Nesse exemplo, se recebermos `List<Circle>`, o retorno será `Circle`.

### ? — Wildcard

```java
public static double totalArea(List<? extends Shape> list)
```

Utilizamos `?` quando não precisamos identificar o tipo específico.

Nesse exemplo, podemos receber `Circle` ou `Rectangle`, pois só precisamos acessar o contrato `Shape`.

---

# 5. Princípio PECS

PECS significa:

**Producer Extends, Consumer Super.**

É uma regra prática para escolher entre `extends` e `super`.

| Wildcard | Finalidade |
|---|---|
| `? extends T` | Consumir/ler elementos como T. |
| `? super T` | Adicionar elementos do tipo T. |

### Producer Extends

Quando uma estrutura fornece elementos para o nosso código, utilizamos `extends`.

```java
public static double calculate(List<? extends Payment> list) {

    double total = 0.0;

    for (Payment payment : list) {
        total += payment.getAmount();
    }

    return total;
}
```

### Consumer Super

Quando queremos inserir elementos em uma estrutura, utilizamos `super`.

```java
public static void addPayments(List<? super Payment> list) {

    list.add(new PixPayment(100.0));

    list.add(new CreditCardPayment(200.0));
}
```

---

# 6. Tabela de comparação

| Declaração | Significado |
|---|---|
| `<T>` | Declara um tipo genérico. |
| `<T extends Shape>` | Tipo genérico limitado a Shape. |
| `<?>` | Tipo desconhecido. |
| `<? extends Shape>` | Shape ou algum subtipo. |
| `<? super Shape>` | Shape ou algum supertipo. |

---

# 7. Quando utilizar cada um?

| Situação | Recurso |
|---|---|
| Criar um método reutilizável para diferentes tipos | `<T>` |
| Restringir os tipos aceitos | `<T extends TIPO>` |
| Receber listas de tipos desconhecidos | `<?>` |
| Consultar elementos de diferentes subclasses | `<? extends T>` |
| Adicionar elementos de determinado tipo | `<? super T>` |

---

# 8. Resumo para entrevistas

**O que são Generics?**

Generics permitem desenvolver classes, interfaces e métodos reutilizáveis, trabalhando com diferentes tipos de dados sem perder a segurança de tipos em tempo de compilação.

**O que são Wildcards?**

Wildcards são representados por `?` e permitem trabalhar com tipos desconhecidos em estruturas genéricas.

**Qual a diferença entre `T` e `?`?**

`T` representa um tipo genérico identificado e reutilizável dentro da declaração. Já `?` representa um tipo desconhecido.

**Qual a diferença entre `extends` e `super` nos Wildcards?**

`extends` permite receber estruturas de um tipo ou seus subtipos, sendo útil para leitura. `super` permite receber estruturas de um tipo ou seus supertipos, sendo útil para inserção.

**O que é PECS?**

É o princípio Producer Extends, Consumer Super, uma regra prática para escolher entre Wildcards com limite superior e inferior.
