# Java Fundamentals — Primitive Data Types

## 1. What Are Primitive Data Types?

Os tipos primitivos são tipos de dados básicos da linguagem Java. Eles permitem armazenar valores simples, como números inteiros, números decimais, caracteres e valores lógicos.

Java possui **8 tipos primitivos**:

| Type | Size | Description | Example |
|---|---:|---|---|
| `byte` | 8 bits | Inteiros pequenos | `byte age = 23;` |
| `short` | 16 bits | Inteiros curtos | `short year = 2026;` |
| `int` | 32 bits | Inteiros comuns | `int quantity = 100;` |
| `long` | 64 bits | Inteiros grandes | `long population = 8000000000L;` |
| `float` | 32 bits | Números de ponto flutuante | `float price = 19.99F;` |
| `double` | 64 bits | Números de ponto flutuante com maior precisão | `double salary = 2000.50;` |
| `char` | 16 bits | Um valor de caractere UTF-16 | `char grade = 'A';` |
| `boolean` | JVM-dependent | Valor lógico verdadeiro ou falso | `boolean isActive = true;` |

**Important:**
- `byte`, `short`, `int` e `long` são tipos inteiros com sinal.
- `float` e `double` representam números de ponto flutuante.
- `char` representa uma unidade de código UTF-16, não necessariamente um caractere Unicode completo.
- `boolean` aceita `true` ou `false`. A especificação da linguagem não define um tamanho de armazenamento fixo para esse tipo.

---

## 2. Integer Types

Os tipos inteiros armazenam números sem casas decimais.

### `byte`

O tipo `byte` ocupa 8 bits e possui valores entre -128 e 127.

```java
byte age = 23;
byte temperature = -10;
byte quantity = 100;
```

É útil quando precisamos representar valores inteiros pequenos e queremos utilizar um tipo com faixa limitada.

### `short`

O tipo `short` ocupa 16 bits e possui valores entre -32.768 e 32.767.

```java
short year = 2026;
short temperature = -150;
```

### `int`

O tipo `int` ocupa 32 bits e é a escolha padrão para números inteiros na maioria das aplicações.

Seu intervalo vai de -2.147.483.648 até 2.147.483.647.

```java
int age = 23;
int productQuantity = 1500;
int population = 1000000;
```

### `long`

O tipo `long` ocupa 64 bits e permite representar números inteiros muito grandes.

```java
long population = 8000000000L;
long transactionId = 9876543210L;
```

O sufixo `L` indica que o literal é do tipo `long`. É preferível utilizar `L` maiúsculo para evitar confusão com o número `1`.

### Integer Range

| Type | Minimum | Maximum |
|---|---:|---:|
| `byte` | -128 | 127 |
| `short` | -32.768 | 32.767 |
| `int` | -2.147.483.648 | 2.147.483.647 |
| `long` | -9.223.372.036.854.775.808 | 9.223.372.036.854.775.807 |

---

## 3. Floating-Point Types

Os tipos de ponto flutuante representam números com casas decimais.

### `float`

O tipo `float` utiliza 32 bits e oferece aproximadamente 6 a 7 dígitos decimais de precisão.

```java
float productPrice = 19.99F;
float discountRate = 0.15F;
```

O sufixo `F` indica que o literal é do tipo `float`.

### `double`

O tipo `double` utiliza 64 bits e oferece aproximadamente 15 a 16 dígitos decimais de precisão.

```java
double monthlySalary = 2000.50;
double productPrice = 149.99;
double average = 8.75;
```

O tipo `double` é o padrão para literais decimais em Java.

### `float` vs. `double`

| Feature | `float` | `double` |
|---|---|---|
| Size | 32 bits | 64 bits |
| Precision | Approximately 6–7 decimal digits | Approximately 15–16 decimal digits |
| Literal suffix | `F` or `f` | `D` or `d` (optional) |
| Default decimal literal | No | Yes |

**Important:** `float` e `double` utilizam representação binária de ponto flutuante. Por isso, alguns valores decimais não podem ser representados exatamente.

```java
double result = 0.1 + 0.2;

System.out.println(result);
```

O resultado pode ser exibido como:

```text
0.30000000000000004
```

Para cálculos financeiros que exigem precisão decimal, como valores monetários, prefira estudar a classe `BigDecimal` em vez de depender de `float` ou `double`.

---

## 4. Character Type — `char`

O tipo `char` ocupa 16 bits e representa uma unidade de código UTF-16.

Um literal `char` utiliza aspas simples.

```java
char grade = 'A';
char initial = 'G';
char symbol = '$';
```

Também podemos representar um caractere por meio de seu código Unicode.

```java
char letter = '\u0041';

System.out.println(letter);
```

Output:

```text
A
```

**Important
