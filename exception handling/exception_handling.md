# Tratamento de Exceções em Java

Este material apresenta os fundamentos de tratamento de exceções em
Java: `try`, `catch`, `throw new` e criação de exceções personalizadas.
Os exemplos usam um sistema bancário simples para conectar os conceitos
a regras de negócio.

## 1. O que é uma exceção?

Uma exceção é um evento que interrompe o fluxo normal de execução de um
programa. Ela pode ocorrer, por exemplo, quando:

-   o programa tenta acessar uma posição inexistente de um array;
-   uma entrada não pode ser convertida para o tipo esperado;
-   ocorre um problema ao ler um arquivo;
-   uma operação viola uma regra de negócio, como tentar sacar um valor
    maior que o saldo.

Em Java, exceções são representadas por objetos. Quando uma exceção é
lançada, o fluxo normal é interrompido e o Java procura um bloco capaz
de tratá-la.

## 2. `try` e `catch`

O bloco `try` envolve o código que pode lançar uma exceção. O bloco
`catch` define o que fazer caso uma exceção de determinado tipo
aconteça.

### Exemplo

``` java
import java.util.InputMismatchException;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        try {
            System.out.print("Digite sua idade: ");
            int age = sc.nextInt();

            System.out.println("Idade: " + age);
        } catch (InputMismatchException e) {
            System.out.println("Entrada inválida. Digite um número inteiro.");
        }

        sc.close();
    }
}
```

Se a pessoa digitar um texto em vez de um número inteiro, `nextInt()`
poderá lançar `InputMismatchException`. O `catch` captura essa exceção e
executa o tratamento definido.

### O objeto da exceção (`e`)

No trecho:

``` java
catch (InputMismatchException e) {
    System.out.println(e.getMessage());
}
```

-   `InputMismatchException` é o tipo de exceção que será capturado.
-   `e` é uma variável que referencia o objeto da exceção.
-   `e.getMessage()` recupera a mensagem associada à exceção, quando
    houver.

O nome `e` é uma convenção; poderia ser `exception`, por exemplo.

### Vários blocos `catch`

É possível tratar tipos diferentes de exceção separadamente:

``` java
try {
    int[] numbers = {10, 20, 30};
    System.out.println(numbers[5]);
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Posição inexistente no array.");
} catch (InputMismatchException e) {
    System.out.println("Entrada inválida.");
}
```

A ordem importa: quando há exceções relacionadas por herança, os tipos
mais específicos devem vir antes dos mais genéricos. Um
`catch (Exception e)` colocado antes de um `catch` mais específico pode
impedir que o segundo seja alcançado.

### `finally`

O bloco `finally` é opcional e normalmente é usado para executar uma
ação de finalização, independentemente de uma exceção ter ocorrido ou
não.

``` java
try {
    System.out.println("Executando operação...");
} catch (RuntimeException e) {
    System.out.println("Ocorreu um erro.");
} finally {
    System.out.println("Finalizando operação.");
}
```

Para fechar recursos como arquivos e conexões, prefira, quando
aplicável, o `try-with-resources`, que fecha automaticamente recursos
que implementam `AutoCloseable`.

## 3. `throw new`: lançando uma exceção

A palavra-chave `throw` é usada para lançar uma exceção explicitamente.
`new` cria o objeto da exceção.

``` java
if (amount <= 0.0) {
    throw new IllegalArgumentException("O valor deve ser maior que zero.");
}
```

Leia assim:

1.  Verifique se `amount` é menor ou igual a zero.
2.  Se a condição for verdadeira, crie uma `IllegalArgumentException`.
3.  Lance essa exceção com `throw`.
4.  A execução normal do método é interrompida naquele ponto, e o Java
    procura um tratamento adequado.

**Atenção à diferença entre `throw` e `throws`:**

-   `throw`: lança uma exceção naquele ponto do código.
-   `throws`: aparece na assinatura de um método e declara que ele pode
    propagar uma ou mais exceções verificadas.

Exemplo:

``` java
public void withdraw(double amount) throws DomainException {
    if (amount <= 0.0) {
        throw new DomainException("O valor deve ser maior que zero.");
    }
}
```

Nesse exemplo, `throws DomainException` declara a possibilidade de
propagação; `throw new DomainException(...)` efetivamente lança a
exceção.

## 4. Exceção personalizada (Custom Exception)

Uma exceção personalizada permite representar uma situação específica do
domínio da aplicação. Em vez de usar uma exceção genérica para toda
regra de negócio, podemos criar um tipo com um nome que descreva o
problema.

Por exemplo, em um sistema bancário, podemos criar `DomainException`
para representar violações de regras da conta.

### Criando a classe

``` java
public class DomainException extends Exception {

    public DomainException(String message) {
        super(message);
    }
}
```

Entendendo cada parte:

-   `class DomainException`: declara uma classe chamada
    `DomainException`.
-   `extends Exception`: faz a classe herdar de `Exception`. Portanto,
    ela é uma **checked exception**.
-   `DomainException(String message)`: construtor que recebe uma
    mensagem.
-   `super(message)`: chama o construtor da classe pai (`Exception`) e
    armazena a mensagem.

A mensagem poderá ser recuperada depois com `getMessage()`.

### Criando uma exceção personalizada unchecked

Também é possível herdar de `RuntimeException`:

``` java
public class DomainException extends RuntimeException {

    public DomainException(String message) {
        super(message);
    }
}
```

Nesse caso, `DomainException` será uma **unchecked exception**. O
compilador não obriga quem chama o método a capturá-la com `try-catch`
nem a declará-la com `throws`.

## 5. Checked Exception vs. Unchecked Exception

A diferença principal é se o compilador exige que o código trate ou
declare a exceção.

### Checked Exception

Uma classe que herda diretamente de `Exception` (sem herdar de
`RuntimeException`) representa uma exceção verificada.

``` java
public class DomainException extends Exception {
    public DomainException(String message) {
        super(message);
    }
}
```

Ao chamar um método que pode lançar essa exceção, é necessário
capturá-la ou declarar sua propagação:

``` java
public void withdraw(double amount) throws DomainException {
    if (amount <= 0.0) {
        throw new DomainException("Valor inválido.");
    }
}
```

Quem chama `withdraw()` deverá tratar ou declarar `DomainException`.

### Unchecked Exception

Uma classe que herda de `RuntimeException` não exige tratamento
obrigatório pelo compilador.

``` java
public class DomainException extends RuntimeException {
    public DomainException(String message) {
        super(message);
    }
}
```

O método pode lançar a exceção sem declarar `throws`:

``` java
public void withdraw(double amount) {
    if (amount <= 0.0) {
        throw new DomainException("Valor inválido.");
    }
}
```

Ainda é possível capturá-la com `try-catch`; apenas não existe a
obrigação imposta pelo compilador.

### Comparação

  -----------------------------------------------------------------------------
  Característica          Checked                 Unchecked
  ----------------------- ----------------------- -----------------------------
  Herança típica          `Exception`             `RuntimeException`

  Tratamento obrigatório  Sim                     Não
  pelo compilador                                 

  Pode usar `try-catch`?  Sim                     Sim

  Pode declarar com       Sim                     Sim, embora não seja
  `throws`?                                       obrigatório

  Exemplos                `IOException`,          `IllegalArgumentException`,
                          `ParseException`        `NullPointerException`
  -----------------------------------------------------------------------------

A escolha entre checked e unchecked depende do contrato da operação e do
desenho da aplicação. Não é correto afirmar que toda regra de negócio
deve obrigatoriamente usar um dos dois tipos.

## 6. Exemplo completo: conta bancária

O exemplo a seguir reúne exceção personalizada, validações de negócio,
`throw`, `throws` e `try-catch`.

### `DomainException.java`

``` java
public class DomainException extends Exception {

    public DomainException(String message) {
        super(message);
    }
}
```

### `Account.java`

``` java
public class Account {

    private String holder;
    private double balance;
    private double withdrawLimit;

    public Account(String holder, double balance, double withdrawLimit) {
        this.holder = holder;
        this.balance = balance;
        this.withdrawLimit = withdrawLimit;
    }

    public void withdraw(double amount) throws DomainException {

        if (amount <= 0.0) {
            throw new DomainException("O valor deve ser maior que zero.");
        }

        if (amount > withdrawLimit) {
            throw new DomainException("O saque excede o limite da conta.");
        }

        if (amount > balance) {
            throw new DomainException("Saldo insuficiente.");
        }

        balance -= amount;
    }

    public double getBalance() {
        return balance;
    }

    @Override
    public String toString() {
        return "Titular: " + holder + " | Saldo: " + balance;
    }
}
```

Observe que as validações acontecem antes de alterar o saldo. Se alguma
condição for inválida, a exceção é lançada e a subtração não é
executada.

### `Main.java`

``` java
import java.util.InputMismatchException;
import java.util.Locale;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        Locale.setDefault(Locale.US);
        Scanner sc = new Scanner(System.in);

        Account account = new Account("Guilherme", 500.0, 200.0);

        try {
            System.out.print("Quanto deseja sacar? ");
            double amount = sc.nextDouble();

            account.withdraw(amount);

            System.out.println("Saque realizado com sucesso.");
        } catch (DomainException e) {
            System.out.println("Erro: " + e.getMessage());
        } catch (InputMismatchException e) {
            System.out.println("Entrada inválida. Digite um valor numérico.");
        }

        System.out.println(account);

        sc.close();
    }
}
```

### Fluxo da execução

1.  `Main` solicita o valor do saque.
2.  O método `withdraw()` recebe o valor.
3.  As regras de negócio são verificadas.
4.  Se uma regra for violada, `throw new DomainException(...)` lança a
    exceção.
5.  A execução retorna ao fluxo de tratamento e o
    `catch (DomainException e)` captura o erro.
6.  `e.getMessage()` exibe a mensagem.
7.  O programa continua após o bloco `try-catch` e imprime a conta.

Se o usuário digitar texto em vez de um número, o erro será capturado
por `InputMismatchException`, não por `DomainException`.

## 7. Boas práticas

-   **Use exceções para situações excepcionais:** não substitua retornos
    normais por exceções sem necessidade.
-   **Escreva mensagens claras:** informe o que aconteceu e, quando
    útil, como corrigir.
-   **Capture tipos específicos:** evite capturar `Exception`
    indiscriminadamente, pois isso pode esconder erros que deveriam ser
    tratados de outra forma.
-   **Não ignore exceções:** um `catch` vazio dificulta identificar
    problemas.
-   **Valide antes de alterar o estado:** no exemplo bancário, o saldo
    só é atualizado depois que todas as regras passam.
-   **Escolha conscientemente entre checked e unchecked:** considere se
    faz sentido exigir que o chamador trate ou declare aquele problema.
-   **Não confunda `throw` com `throws`:** um lança; o outro declara a
    possibilidade de propagação.

## 8. Resumo para revisão

  -----------------------------------------------------------------------
  Recurso                             Para que serve
  ----------------------------------- -----------------------------------
  `try`                               Delimita código que pode lançar
                                      exceções.

  `catch`                             Captura e trata uma exceção
                                      compatível.

  `finally`                           Executa código de finalização,
                                      normalmente independentemente do
                                      resultado.

  `throw`                             Lança uma exceção.

  `new`                               Cria uma instância, como um objeto
                                      de exceção.

  `throws`                            Declara exceções que um método pode
                                      propagar.

  `extends Exception`                 Cria uma exceção personalizada
                                      checked.

  `extends RuntimeException`          Cria uma exceção personalizada
                                      unchecked.

  `getMessage()`                      Recupera a mensagem da exceção.

  `super(message)`                    Encaminha a mensagem ao construtor
                                      da classe pai.
  -----------------------------------------------------------------------

## 9. Exercícios para praticar

1.  Crie uma classe `DomainException` que herde de `Exception` e receba
    uma mensagem no construtor.
2.  Crie uma classe `Account` com saldo e limite de saque.
3.  Implemente `withdraw(double amount)` e lance `DomainException`
    quando o valor for menor ou igual a zero.
4.  Adicione validações para limite de saque e saldo insuficiente.
5.  Trate `DomainException` e `InputMismatchException` separadamente na
    classe `Main`.
6.  Troque `extends Exception` por `extends RuntimeException`. Remova
    `throws DomainException` e teste a chamada sem `try-catch`.
7.  Explique com suas palavras por que a primeira versão exige
    tratamento ou declaração e a segunda não.

------------------------------------------------------------------------

**Conceito central:** o tratamento de exceções permite separar o fluxo
normal do programa do tratamento de situações inesperadas ou inválidas.
Exceções personalizadas tornam os erros do domínio mais expressivos e
ajudam a organizar as regras de negócio.
