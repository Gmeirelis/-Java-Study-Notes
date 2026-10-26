# Java Fundamentals — Variables and Constants

## 1. Variables

Uma variável é um espaço identificado na memória utilizado para armazenar um valor durante a execução de um programa.

Em Java, toda variável possui um tipo, um nome e um valor.

### Syntax

```java
type variableName = value;
```

- `type`: tipo de dado armazenado.
- `variableName`: nome da variável.
- `value`: valor atribuído à variável.

### Example

```java
public class Main {
    public static void main(String[] args) {
        int age = 23;
        String name = "Guilherme";
        double salary = 2000.00;
        boolean isEmployed = true;

        System.out.println(name);
        System.out.println(age);
        System.out.println(salary);
        System.out.println(isEmployed);
    }
}
```

### Variable declaration and initialization

Em Java, podemos declarar uma variável e atribuir um valor em momentos diferentes.

**Declaration:**

```java
int age;
```

Nesse momento, a variável foi declarada, mas ainda não recebeu um valor.

**Initialization:**

```java
age = 23;
```

Agora, a variável recebeu seu primeiro valor.

**Declaration and initialization:**

```java
int age = 23;
```

Essa forma realiza as duas operações na mesma instrução.

> **Important:** Variáveis locais precisam ser inicializadas antes de serem utilizadas. Variáveis de instância e de classe recebem valores padrão quando não são inicializadas explicitamente.

---

## 2. Reassigning Variables

Uma variável pode receber novos valores durante a execução do programa, desde que respeite seu tipo.

```java
public class Main {
    public static void main(String[] args) {
        int age = 23;

        System.out.println(age);

        age = 24;

        System.out.println(age);
    }
}
```

**Output:**

```text
23
24
```

O valor da variável `age` foi alterado de `23` para `24`.

### Type safety

Java é uma linguagem estaticamente tipada. Depois de declarada, uma variável possui um tipo definido.

```java
int age = 23;

age = 24;     // Valid
// age = "24"; // Invalid
```

O compilador impede a atribuição de uma `String` a uma variável do tipo `int`.

---

## 3. Constants with `final`

Em Java, a palavra-chave `final` impede que uma variável receba uma nova atribuição depois de inicializada.

Uma variável `final` precisa ser inicializada antes de ser utilizada e não pode receber outra atribuição após sua inicialização.

### Syntax

```java
final type CONSTANT_NAME = value;
```

### Example

```java
public class Main {
    public static void main(String[] args) {
        final double TAX_RATE = 0.10;

        System.out.println(TAX_RATE);

        // TAX_RATE = 0.15; // Compilation error
    }
}
```

O código não compila se a linha comentada for descomentada, pois `TAX_RATE` já recebeu um valor.

### Naming convention

Por convenção, constantes com valores fixos conhecidos são declaradas com nomes em letras maiúsculas, utilizando `_` para separar palavras.

```java
final double TAX_RATE = 0.10;
final int MAX_ATTEMPTS = 3;
final String APPLICATION_NAME = "StockManager";
```

Essa convenção facilita a identificação de constantes durante a leitura do código.

> **Important:** Letras maiúsculas são uma convenção de nomenclatura, não uma exigência da linguagem.

---

## 4. Variables vs. Constants

| Feature | Variable | Constant (`final`) |
|---|---|---|
| Keyword | No special keyword required | `final` |
| Can be reassigned? | Yes | No |
| Must be initialized at declaration? | Not always | Not always |
| Naming convention | `camelCase` | `UPPER_SNAKE_CASE` for constants |
| Example | `int age = 23;` | `final int MAX_USERS = 100;` |

Uma variável comum permite reatribuição. Uma variável `final` não permite que a referência ou o valor primitivo armazenado receba outra atribuição.

---

## 5. `final` with Objects

Quando `final` é utilizado com uma variável que referencia um objeto, ele impede a reatribuição da referência, mas não necessariamente a modificação do estado do objeto.

### Example

```java
public class Main {
    public static void main(String[] args) {
        final StringBuilder message = new StringBuilder("Hello");

        message.append(" World");

        System.out.println(message);

        // message = new StringBuilder("Java"); // Compilation error
    }
}
```

**Output:**

```text
Hello World
```

O método `append()` modifica o objeto existente. Isso é permitido porque a referência `message` continua apontando para o mesmo objeto.

Entretanto, atribuir um novo objeto à variável `message` não é permitido.

> **Important:** `final` não torna automaticamente um objeto imutável. Ele impede a reatribuição da variável, não todas as modificações no objeto referenciado.

---

## 6. Best Practices

### Use meaningful names

Prefira nomes que indiquem claramente o propósito da variável.

```java
// Bad
double x = 2000.00;

// Good
double monthlySalary = 2000.00;
```

### Use `final` when reassignment is unnecessary

Utilize `final` quando uma variável não precisar receber outra atribuição depois de inicializada.

```java
final double productPrice = 149.90;
final int MAX_LOGIN_ATTEMPTS = 5;
```

Isso ajuda a comunicar a intenção do código e evita reatribuições acidentais.

### Follow Java naming conventions

```java
String customerName = "John";       // Variable
final double SERVICE_FEE = 0.05;    // Constant
```

Variáveis e métodos normalmente utilizam `camelCase`. Constantes normalmente utilizam `UPPER_SNAKE_CASE`.

---

## 7. Exercises

### Exercise 1 — Variable declaration

Declare the following variables:

- An integer called `age`.
- A string called `customerName`.
- A double called `monthlySalary`.
- A boolean called `isActive`.

Initialize each variable with an appropriate value and print them.

### Exercise 2 — Reassigning variables

Create an integer variable called `score`, initialize it with `10`, and then change its value to `20`.

Print the value before and after reassignment.

### Exercise 3 — Constants

Create the following constants:

- `TAX_RATE` with the value `0.10`.
- `MAX_ATTEMPTS` with the value `3`.
- `APPLICATION_NAME` with the value `"StockManager"`.

Print their values.

### Exercise 4 — Understanding `final`

Analyze the following code:

```java
final int MAX_USERS = 100;

// MAX_USERS = 200;
```

Answer these questions:

1. Why can the variable not be reassigned?
2. What happens if the commented line is executed?
3. What is the purpose of the `final` keyword?

### Exercise 5 — Practical application

Create a program that stores a product's name, price, and quantity.

Requirements:

- Use appropriate data types.
- Declare a constant called `DISCOUNT_RATE`.
- Calculate the total price before the discount.
- Calculate the discount amount.
- Calculate the final price.
- Print all results.

---

## 8. Summary

- A **variable** stores a value that can be reassigned.
- A **constant** declared with `final` cannot be reassigned after initialization.
- Java is a statically typed language: variables have defined types.
- Local variables must be initialized before use.
- `camelCase` is the standard naming convention for variables.
- `UPPER_SNAKE_CASE` is the conventional naming style for constants.
- When applied to an object reference, `final` prevents reassignment of the reference but does not necessarily make the object immutable.

**Next topic:** Primitive Data Types in Java.
