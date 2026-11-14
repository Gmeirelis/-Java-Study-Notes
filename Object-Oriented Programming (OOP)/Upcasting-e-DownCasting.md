# Upcasting, Downcasting e instanceof em Java

Esses três conceitos estão relacionados à **herança** e ao **polimorfismo** em Java. Eles permitem trabalhar com objetos de diferentes classes dentro de uma mesma hierarquia de herança.

---

## 1. Upcasting

Upcasting acontece quando utilizamos uma **referência de uma classe pai** para apontar para um **objeto de uma classe filha**.

Isso é possível porque a classe filha herda as características e os comportamentos da classe pai.

### Exemplo

Considerando que `BusinessAccount` herda de `Account`:

```java
Account account = new BusinessAccount(1001, "Alex", 0.0, 500.0);
```

Nesse exemplo:

- `Account`: tipo da variável de referência.
- `new BusinessAccount()`: tipo do objeto instanciado.
- `account`: referência que aponta para um objeto `BusinessAccount`.

O objeto continua sendo um `BusinessAccount`, mas a referência é do tipo `Account`.

### O que podemos acessar?

Através da referência `Account`, podemos acessar os métodos disponíveis na classe `Account`.

```java
account.deposit(100.0);
account.withdraw(50.0);
```

Entretanto, **não** podemos acessar diretamente métodos exclusivos de `BusinessAccount`.

```java
account.loan(200.0); // Erro de compilação
```

Isso acontece porque o compilador considera o **tipo da referência** para determinar quais métodos podem ser acessados.

> **Importante:** Upcasting é realizado **implicitamente** pelo Java, sem necessidade de conversão explícita.

---

## 2. Downcasting

Downcasting é o processo inverso do Upcasting. Acontece quando convertemos uma **referência de uma classe pai** para uma **referência de uma classe filha**.

Diferentemente do Upcasting, o Downcasting exige uma **conversão explícita**.

### Exemplo

```java
Account account = new BusinessAccount(1001, "Alex", 0.0, 500.0);

// Downcasting
BusinessAccount businessAccount = (BusinessAccount) account;
```

Nesse exemplo:

- Criamos um objeto `BusinessAccount`.
- Armazenamos sua referência em uma variável do tipo `Account`.
- Convertemos essa referência novamente para `BusinessAccount`.

Agora podemos acessar os métodos específicos da classe filha:

```java
businessAccount.deposit(100.0);
businessAccount.loan(200.0);
```

> **Importante:** Downcasting **não cria um novo objeto**. Apenas converte o tipo da referência utilizada para acessar o objeto existente.

### Cuidado com `ClassCastException`

O Downcasting pode provocar uma exceção em **tempo de execução** quando tentamos converter um objeto para um tipo incompatível.

Exemplo:

```java
Account account = new SavingsAccount(1002, "Maria", 0.0, 0.01);

// Conversão inválida
BusinessAccount businessAccount = (BusinessAccount) account;
```

Nesse caso, o Java lança uma `ClassCastException`, porque o objeto real é um `SavingsAccount`, e não um `BusinessAccount`.

---

## 3. instanceof

`instanceof` é um operador utilizado para verificar se um objeto é compatível com determinado tipo ou classe.

Ele retorna:

- `true`: quando o objeto é compatível com o tipo verificado.
- `false`: quando não é compatível.

É muito utilizado **antes de realizar um Downcasting**, evitando conversões inválidas.

### Exemplo

```java
Account account = new SavingsAccount(1002, "Maria", 0.0, 0.01);

if (account instanceof BusinessAccount) {
    BusinessAccount businessAccount = (BusinessAccount) account;

    businessAccount.loan(200.0);
}
```

Nesse exemplo, a condição retorna `false`, porque o objeto instanciado é `SavingsAccount`.

Consequentemente, o código dentro do `if` não será executado.

### Exemplo com resultado verdadeiro

```java
Account account = new BusinessAccount(1001, "Alex", 0.0, 500.0);

if (account instanceof BusinessAccount) {
    BusinessAccount businessAccount = (BusinessAccount) account;

    businessAccount.loan(200.0);
}
```

Agora a condição retorna `true`, pois o objeto real é compatível com `BusinessAccount`.

O Downcasting pode ser realizado com segurança.

---

## 4. Pattern Matching com instanceof (Java 16+)

A partir do **Java 16**, podemos utilizar o *Pattern Matching* com `instanceof`.

Essa funcionalidade simplifica o código, eliminando a necessidade de realizar o Downcasting manualmente.

### Exemplo tradicional

```java
if (account instanceof BusinessAccount) {
    BusinessAccount businessAccount = (BusinessAccount) account;

    businessAccount.loan(200.0);
}
```

### Exemplo com Pattern Matching

```java
if (account instanceof BusinessAccount businessAccount) {
    businessAccount.loan(200.0);
}
```

O Java verifica o tipo do objeto e, se a condição for verdadeira, disponibiliza a variável `businessAccount` já com o tipo específico.

---

## 5. Comparação dos três conceitos

| Conceito    | Descrição                                                                                     |
|-------------|-----------------------------------------------------------------------------------------------|
| Upcasting   | Referência da classe pai apontando para objeto da classe filha.                               |
| Downcasting | Conversão explícita de uma referência da classe pai para uma classe filha compatível.         |
| instanceof  | Verifica se um objeto é compatível com determinado tipo.                                      |

### Representação da herança

```text
              Account
                 |
        -------------------
        |                 |
 BusinessAccount    SavingsAccount
```

**Exemplo de Upcasting:**

```java
Account account = new BusinessAccount(...);
```

**Exemplo de Downcasting:**

```java
BusinessAccount businessAccount = (BusinessAccount) account;
```

**Exemplo de instanceof:**

```java
if (account instanceof BusinessAccount) {
    // Executa se o objeto for compatível.
}
```

---

## 6. Resumo para entrevistas técnicas

**Upcasting:**

> "É quando utilizamos uma referência de uma classe pai para apontar para um objeto de uma classe filha. Isso permite trabalhar com diferentes classes por meio de uma referência comum."

**Downcasting:**

> "É quando convertemos explicitamente uma referência de uma classe pai para uma classe filha, permitindo acessar os métodos específicos da classe filha. É necessário garantir que o objeto seja compatível com o tipo de destino."

**instanceof:**

> "É um operador que verifica se um objeto é compatível com determinado tipo. É bastante utilizado antes do Downcasting para evitar uma `ClassCastException`."

---

## Ideia principal para memorizar

- **Upcasting:** utilizo uma referência mais genérica.
- **Downcasting:** converto para uma referência mais específica.
- **instanceof:** verifico se o objeto é compatível com o tipo desejado.
