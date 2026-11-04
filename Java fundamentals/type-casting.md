# Type Casting in Java

Type casting é o processo de conversão de um tipo de dado para outro.

Em Java, essa conversão pode acontecer automaticamente ou exigir uma
conversão explícita, dependendo dos tipos envolvidos e da possibilidade
de perda de informação.

Existem duas categorias principais de conversão entre tipos primitivos:

-   **Widening Casting:** conversão de um tipo menor para um tipo maior,
    geralmente realizada automaticamente.
-   **Narrowing Casting:** conversão de um tipo maior para um tipo
    menor, que exige uma conversão explícita.

## 1. Widening Casting (Conversão Implícita)

Acontece quando um valor é convertido de um tipo primitivo com menor
capacidade de representação para outro com maior capacidade.

Nesse caso, o Java realiza a conversão automaticamente, sem necessidade
de informar o tipo de destino.

### Ordem comum de conversão numérica

``` text
byte → short → int → long → float → double
          char → int
```

Essa sequência representa as conversões numéricas de ampliação
permitidas. `char` também pode ser convertido para `int`, pois
representa um valor inteiro sem sinal de 16 bits.

### Example

``` java
int number = 100;

double result = number;

System.out.println(result); // 100.0
```

Nesse exemplo, um valor `int` é atribuído a uma variável `double`.

A conversão acontece automaticamente porque `double` consegue
representar uma faixa de valores maior que `int`.

Outro exemplo:

``` java
byte value = 10;

int result = value;

System.out.println(result); // 10
```

**Importante:** widening não significa que toda conversão preserva
exatamente a precisão. Por exemplo, um `long` pode ser convertido para
`float`, mas valores muito grandes podem perder precisão.

## 2. Narrowing Casting (Conversão Explícita)

Acontece quando um valor é convertido de um tipo com maior capacidade de
representação para outro com menor capacidade.

Como pode ocorrer perda de informação, o Java exige que o programador
informe explicitamente o tipo de destino.

### Syntax

``` java
targetType variable = (targetType) value;
```

### Example

``` java
double price = 19.99;

int result = (int) price;

System.out.println(result); // 19
```

Nesse exemplo, o valor decimal `19.99` é convertido para `int`.

A parte decimal é descartada. Isso não é arredondamento: a conversão de
um valor decimal para um tipo inteiro descarta a parte fracionária em
direção a zero.

Outro exemplo:

``` java
int number = 130;

byte result = (byte) number;

System.out.println(result); // -126
```

Nesse caso, o valor `130` está fora da faixa permitida por `byte`
(`-128` até `127`). A conversão provoca perda de informação e o
resultado sofre o efeito do estreitamento dos bits.

## 3. Widening vs Narrowing

  -----------------------------------------------------------------------
  Característica          Widening                Narrowing
  ----------------------- ----------------------- -----------------------
  Direção                 Tipo menor para maior   Tipo maior para menor

  Conversão               Implícita               Explícita

  Cast obrigatório        Não                     Sim

  Risco de perda          Pode haver perda de     Pode haver perda de
                          precisão                informação
  -----------------------------------------------------------------------

### Example

``` java
// Widening
int number = 50;
double decimal = number;

// Narrowing
double price = 50.75;
int integerPrice = (int) price;
```

## 4. Numeric Promotion (Promoção Numérica)

Em expressões aritméticas, o Java pode promover automaticamente tipos
menores para tipos numéricos maiores antes de realizar a operação.

Por exemplo, operações envolvendo `byte`, `short` e `char` normalmente
são realizadas como `int`.

### Example

``` java
byte numberOne = 10;
byte numberTwo = 20;

int result = numberOne + numberTwo;

System.out.println(result); // 30
```

Mesmo que as duas variáveis sejam `byte`, o resultado da soma é `int`.

Por isso, o código abaixo não compila sem uma conversão explícita:

``` java
byte result = (byte) (numberOne + numberTwo);
```

Atenção: essa conversão pode perder informação caso o resultado esteja
fora da faixa de `byte`.

## 5. Casting with char

O tipo `char` representa um caractere UTF-16 de 16 bits e também possui
um valor numérico correspondente.

Por isso, é possível converter um `char` para `int` e um valor numérico
compatível para `char`.

### Example

``` java
char letter = 'A';

int code = letter;

System.out.println(code); // 65
```

Também é possível realizar a conversão inversa:

``` java
int code = 66;

char letter = (char) code;

System.out.println(letter); // B
```

## 6. Casting vs String Conversion

É importante diferenciar type casting de conversão entre `String` e
tipos numéricos.

Type casting é utilizado principalmente nas conversões entre tipos
primitivos compatíveis.

Para converter uma `String` em um tipo numérico, utilizamos métodos de
conversão das classes wrapper, como `Integer` e `Double`.

### String to int

``` java
String value = "100";

int number = Integer.parseInt(value);

System.out.println(number); // 100
```

### String to double

``` java
String value = "19.99";

double price = Double.parseDouble(value);

System.out.println(price); // 19.99
```

### Number to String

``` java
int number = 100;

String value = String.valueOf(number);

System.out.println(value); // 100
```

**Importante:** se a `String` não representar um número válido, métodos
como `Integer.parseInt()` podem lançar `NumberFormatException`.

## 7. Important Rules

-   Widening casting normalmente acontece automaticamente.
-   Narrowing casting exige informar explicitamente o tipo de destino.
-   Converter `double` para `int` descarta a parte decimal; não
    arredonda.
-   Conversões narrowing podem causar perda de precisão ou informação.
-   Em operações aritméticas, `byte`, `short` e `char` são normalmente
    promovidos para `int`.
-   O tipo `boolean` não pode ser convertido para tipos numéricos, nem o
    contrário.
-   Converter `String` para número não é type casting: utilize métodos
    como `Integer.parseInt()` e `Double.parseDouble()`.

## Summary

Type casting permite converter valores entre tipos de dados compatíveis
em Java.

Os conceitos principais são:

-   **Widening Casting:** conversão automática para um tipo numérico
    mais amplo.
-   **Narrowing Casting:** conversão explícita que pode perder
    informação.
-   **Numeric Promotion:** promoção automática de tipos em expressões
    numéricas.
-   **String Conversion:** conversão entre textos e valores numéricos
    por meio de métodos específicos.
