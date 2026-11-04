# Java Operators

Os operadores em Java são símbolos utilizados para realizar operações sobre variáveis, valores e expressões.

Eles permitem executar cálculos matemáticos, comparar valores, combinar condições e atribuir valores às variáveis.

## 1. Arithmetic Operators (Operadores Aritméticos)

Utilizados para realizar operações matemáticas.

| Operador | Descrição | Exemplo |
|---|---|---|
| `+` | Adição | `10 + 5` |
| `-` | Subtração | `10 - 5` |
| `*` | Multiplicação | `10 * 5` |
| `/` | Divisão | `10 / 5` |
| `%` | Módulo (resto da divisão) | `10 % 3` |

### Example

```java
int numberOne = 10;
int numberTwo = 3;

System.out.println(numberOne + numberTwo); // 13
System.out.println(numberOne - numberTwo); // 7
System.out.println(numberOne * numberTwo); // 30
System.out.println(numberOne / numberTwo); // 3
System.out.println(numberOne % numberTwo); // 1
```

**Important:** Quando dois números inteiros são divididos, o resultado também será inteiro. A parte decimal é descartada.

Para obter um resultado decimal, utilize pelo menos um operando do tipo `double`.

```java
double result = 10.0 / 3;

System.out.println(result); // 3.3333333333333335
```

## 2. Assignment Operators (Operadores de Atribuição)

Utilizados para atribuir ou atualizar valores de variáveis.

| Operador | Equivalência | Exemplo |
|---|---|---|
| `=` | Atribuição | `x = 10` |
| `+=` | `x = x + 5` | `x += 5` |
| `-=` | `x = x - 5` | `x -= 5` |
| `*=` | `x = x * 5` | `x *= 5` |
| `/=` | `x = x / 5` | `x /= 5` |
| `%=` | `x = x % 5` | `x %= 5` |

### Example

```java
int number = 10;

number += 5; // 15
number -= 3; // 12
number *= 2; // 24
number /= 4; // 6
number %= 4; // 2

System.out.println(number);
```

## 3. Relational Operators (Operadores Relacionais)

Utilizados para comparar dois valores.

O resultado de uma comparação sempre será um valor booleano (`true` ou `false`).

| Operador | Descrição |
|---|---|
| `==` | Igual a |
| `!=` | Diferente de |
| `>` | Maior que |
| `<` | Menor que |
| `>=` | Maior ou igual a |
| `<=` | Menor ou igual a |

### Example

```java
int age = 20;

System.out.println(age == 20); // true
System.out.println(age != 20); // false
System.out.println(age > 18);  // true
System.out.println(age < 18);  // false
System.out.println(age >= 20); // true
System.out.println(age <= 18); // false
```

**Important:** Não confunda `=` com `==`.

- `=` atribui um valor a uma variável.
- `==` compara dois valores.

Para comparar o conteúdo de objetos `String`, utilize o método `.equals()` em vez de `==`.

```java
String nameOne = new String("Java");
String nameTwo = new String("Java");

System.out.println(nameOne == nameTwo);      // false
System.out.println(nameOne.equals(nameTwo)); // true
```

## 4. Logical Operators (Operadores Lógicos)

Utilizados para combinar ou inverter expressões booleanas.

| Operador | Descrição | Resultado |
|---|---|---|
| `&&` | AND (E) | true se ambas forem verdadeiras |
| `\|\|` | OR (OU) | true se pelo menos uma for verdadeira |
| `!` | NOT (NÃO) | Inverte o resultado booleano |

### AND (`&&`)

Todas as condições precisam ser verdadeiras.

```java
int age = 25;
boolean hasLicense = true;

if (age >= 18 && hasLicense) {
    System.out.println("Allowed to drive");
}
```

### OR (`||`)

Pelo menos uma condição precisa ser verdadeira.

```java
boolean isAdmin = false;
boolean isManager = true;

if (isAdmin || isManager) {
    System.out.println("Access granted");
}
```

### NOT (`!`)

Inverte o valor booleano.

```java
boolean isActive = true;

System.out.println(!isActive); // false
```

### Short-Circuit Evaluation

Os operadores `&&` e `||` utilizam avaliação de curto-circuito.

- `&&`: se a primeira condição for falsa, a segunda não será avaliada.
- `||`: se a primeira condição for verdadeira, a segunda não será avaliada.

```java
int number = 10;

if (number > 5 || number / 0 > 1) {
    System.out.println("Condition satisfied");
}
```

Nesse exemplo, a segunda expressão não é executada porque a primeira já é verdadeira.

## 5. Increment and Decrement Operators

Utilizados para aumentar ou diminuir o valor de uma variável em uma unidade.

| Operador | Descrição |
|---|---|
| `++` | Incrementa em 1 |
| `--` | Decrementa em 1 |

Existem duas formas de utilização:

**Prefix:** O valor é incrementado antes de ser utilizado na expressão.

```java
int number = 10;

System.out.println(++number); // 11
```

**Postfix:** O valor atual é utilizado primeiro e depois incrementado.

```java
int number = 10;

System.out.println(number++); // 10
System.out.println(number);   // 11
```

O mesmo comportamento se aplica ao operador de decremento (`--`).

## 6. Ternary Operator (Operador Ternário)

O operador ternário é uma forma simplificada de escrever uma expressão condicional.

Sua estrutura é:

```java
condition ? valueIfTrue : valueIfFalse;
```

### Example

```java
int age = 20;

String result = age >= 18 ? "Adult" : "Minor";

System.out.println(result);
```

Nesse exemplo:

- Se a condição for verdadeira, retorna `"Adult"`.
- Se a condição for falsa, retorna `"Minor"`.

É útil para condições simples. Para lógicas mais complexas, prefira utilizar `if` e `else`.

## 7. Bitwise Operators (Operadores Bit a Bit)

Realizam operações diretamente sobre os bits de valores inteiros.

| Operador | Descrição |
|---|---|
| `&` | AND bit a bit |
| `\|` | OR bit a bit |
| `^` | XOR bit a bit |
| `~` | NOT bit a bit |
| `<<` | Deslocamento à esquerda |
| `>>` | Deslocamento à direita com sinal |
| `>>>` | Deslocamento à direita sem sinal |

### Example

```java
int numberOne = 5; // 0101
int numberTwo = 3; // 0011

System.out.println(numberOne & numberTwo); // 1
System.out.println(numberOne | numberTwo); // 7
System.out.println(numberOne ^ numberTwo); // 6
```

Esses operadores são utilizados em situações específicas, como manipulação de flags, máscaras de bits e operações de baixo nível.

## 8. Operator Precedence (Precedência dos Operadores)

Quando uma expressão possui vários operadores, Java segue uma ordem de precedência para determinar qual operação será executada primeiro.

Ordem simplificada, da maior para a menor precedência:

1. Parênteses `()`
2. Incremento e decremento `++` `--`, operadores unários `!` `~`
3. Multiplicação, divisão e módulo `*` `/` `%`
4. Adição e subtração `+` `-`
5. Operadores de deslocamento `<<` `>>` `>>>`
6. Operadores relacionais `<` `>` `<=` `>=` `instanceof`
7. Igualdade `==` `!=`
8. AND bit a bit `&`
9. XOR bit a bit `^`
10. OR bit a bit `|`
11. AND lógico `&&`
12. OR lógico `||`
13. Operador ternário `?:`
14. Operadores de atribuição `=` `+=` `-=` `*=` `/=` etc.

### Example

```java
int result = 10 + 5 * 2;

System.out.println(result); // 20
```

A multiplicação acontece primeiro.

Para alterar a ordem de execução, utilize parênteses:

```java
int result = (10 + 5) * 2;

System.out.println(result); // 30
```

## Summary

Os operadores são fundamentais para construir expressões e implementar regras de negócio em Java.

Os principais grupos são:

- Arithmetic: operações matemáticas.
- Assignment: atribuição e atualização de valores.
- Relational: comparação de valores.
- Logical: combinação de condições booleanas.
- Increment and Decrement: alteração de valores em uma unidade.
- Ternary: expressões condicionais simplificadas.
- Bitwise: manipulação de bits.
- Precedence: ordem de execução dos operadores.
