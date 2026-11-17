# Classes e Métodos Abstratos em Java

## 1. O que é abstração?

Na orientação a objetos, **abstração** é o princípio de representar as características e os comportamentos essenciais de algo, sem precisar mostrar todos os detalhes de sua implementação.

Em Java, classes e métodos abstratos ajudam a definir uma estrutura comum para outras classes.

## 2. O que é uma classe abstrata?

Uma classe abstrata é uma classe declarada com a palavra-chave `abstract`.

Ela serve como uma classe-base para outras classes e **não pode ser instanciada diretamente**.

```java
public abstract class Animal {
    // Atributos e métodos
}
```

Isto não é permitido:

```java
Animal animal = new Animal(); // Erro de compilação
```

Uma classe abstrata pode conter:
- Atributos;
- Construtores;
- Métodos concretos (com implementação);
- Métodos abstratos (sem implementação).

### Exemplo: classe abstrata com método concreto

```java
public abstract class Animal {

    public void sleep() {
        System.out.println("Sleeping...");
    }
}
```

Mesmo sem possuir métodos abstratos, `Animal` pode ser abstrata. A palavra-chave `abstract` impede que ela seja instanciada diretamente.

## 3. O que é um método abstrato?

Um método abstrato é declarado com `abstract` e **não possui corpo**, ou seja, não contém o bloco `{ }` com as instruções que serão executadas.

```java
public abstract void makeSound();
```

Essa declaração informa que existe um comportamento chamado `makeSound`, mas não define como ele será executado.

A classe concreta que herdar esse método deverá fornecer sua implementação.

### Declaração x implementação

**Somente declaração (método abstrato):**

```java
public abstract void makeSound();
```

**Método com implementação:**

```java
public void makeSound() {
    System.out.println("Woof!");
}
```

- **Declaração:** define o nome do método, seus parâmetros e seu tipo de retorno.
- **Implementação:** contém o código que determina o que o método executa.

## 4. Exemplo completo: Animal, Dog e Cat

Imagine que diferentes animais possuem o comportamento de emitir um som, mas cada um emite um som diferente.

### Classe abstrata

```java
public abstract class Animal {

    public abstract void makeSound();

    public void sleep() {
        System.out.println("Sleeping...");
    }
}
```

### Classe Dog

```java
public class Dog extends Animal {

    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}
```

### Classe Cat

```java
public class Cat extends Animal {

    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}
```

As classes `Dog` e `Cat` herdam de `Animal` e implementam o método abstrato `makeSound()` de maneiras diferentes.

O método `sleep()` também é herdado, mas não precisa ser sobrescrito, pois já possui implementação na classe `Animal`.

## 5. Relação entre classe abstrata e método abstrato

Existe uma regra importante:

> Se uma classe possui pelo menos um método abstrato, ela obrigatoriamente precisa ser declarada como abstrata.

Exemplo válido:

```java
public abstract class Animal {
    public abstract void makeSound();
}
```

Exemplo inválido:

```java
public class Animal {
    public abstract void makeSound();
}
```

O segundo exemplo gera erro de compilação porque a classe não foi declarada como `abstract`.

### Uma classe abstrata precisa ter métodos abstratos?

Não. Uma classe pode ser abstrata mesmo que todos os seus métodos tenham implementação.

```java
public abstract class Vehicle {

    public void start() {
        System.out.println("Vehicle started.");
    }
}
```

Isso é válido. A classe continua não podendo ser instanciada diretamente.

## 6. E se a classe filha não implementar o método abstrato?

Se uma classe filha concreta não implementar todos os métodos abstratos herdados, o Java apresentará um erro de compilação.

Exemplo:

```java
public abstract class Animal {
    public abstract void makeSound();
}

public class Dog extends Animal {
    // Erro: não implementou makeSound()
}
```

Existe uma exceção: a classe filha também pode ser abstrata. Nesse caso, ela pode deixar a implementação para uma classe descendente.

```java
public abstract class Animal {
    public abstract void makeSound();
}

public abstract class Mammal extends Animal {
    // Pode deixar makeSound() sem implementação
}

public class Dog extends Mammal {

    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}
```

## 7. Classes abstratas e polimorfismo

O polimorfismo permite utilizar uma referência de um tipo mais geral para apontar para objetos de tipos mais específicos.

```java
public class Main {
    public static void main(String[] args) {

        Animal animal1 = new Dog();
        Animal animal2 = new Cat();

        animal1.makeSound();
        animal2.makeSound();
    }
}
```

Saída:

```text
Woof!
Meow!
```

As duas referências são do tipo `Animal`, mas cada objeto executa sua própria implementação de `makeSound()`.

Isso demonstra o **polimorfismo**: uma mesma chamada de método pode produzir comportamentos diferentes, conforme o tipo real do objeto.

## 8. Quando utilizar uma classe abstrata?

Utilize uma classe abstrata quando:

1. Existem características ou comportamentos comuns entre várias classes.
2. Você deseja compartilhar atributos, construtores e métodos implementados.
3. Algumas operações precisam obrigatoriamente ser implementadas de maneiras diferentes pelas classes filhas.
4. Não faz sentido permitir a criação direta de objetos da classe-base.

### Exemplo de sistema bancário

Imagine três tipos de conta:

- `Account`: classe-base;
- `SavingsAccount`: conta poupança;
- `BusinessAccount`: conta empresarial.

As contas compartilham informações como número, titular e saldo. Entretanto, podem ter regras diferentes para saque.

A classe `Account` pode ser abstrata, compartilhando os dados e declarando um método abstrato `withdraw()` para que cada tipo de conta implemente sua regra.

**Importante:** não transforme uma classe em abstrata apenas porque ela possui subclasses. Use abstração quando isso representar corretamente o modelo e as regras do sistema.

## 9. Classe comum, classe abstrata e interface

| Recurso | Finalidade |
|---|---|
| Classe comum | Permite criar objetos e pode compartilhar atributos e métodos. |
| Classe abstrata | Serve como base para subclasses, pode compartilhar implementação e declarar métodos abstratos. |
| Interface | Define um contrato que classes podem implementar, inclusive classes sem relação de herança entre si. |

Uma classe pode estender apenas uma classe (comum ou abstrata), mas pode implementar várias interfaces.

## 10. Resumo para revisão

- `abstract` na classe: impede a instanciação direta da classe.
- `abstract` no método: declara um método sem implementação.
- Uma classe com método abstrato precisa ser abstrata.
- Uma classe abstrata não precisa ter métodos abstratos.
- Uma classe abstrata pode ter atributos, construtores e métodos concretos.
- Uma classe concreta que herda métodos abstratos precisa implementá-los.
- Uma classe filha abstrata pode deixar a implementação para uma classe descendente.
- `@Override` indica que um método está sobrescrevendo um método herdado. É uma anotação recomendada, mas não é obrigatória para o compilador reconhecer a sobrescrita.

### Frase para memorizar

**A classe abstrata define uma estrutura comum; o método abstrato determina um comportamento que as classes concretas devem implementar.**

---

# Exercícios de fixação

Tente responder sem consultar o material.

### 1. Conceitos

1. O que é uma classe abstrata?
2. Uma classe abstrata pode ser instanciada com `new`?
3. O que significa dizer que um método não possui implementação?
4. Qual é a diferença entre declarar e implementar um método?
5. Toda classe abstrata precisa ter pelo menos um método abstrato?
6. Uma classe que possui um método abstrato pode ser uma classe comum?
7. Uma classe filha concreta pode deixar de implementar um método abstrato herdado?
8. Uma classe filha abstrata pode deixar a implementação para outra classe?

### 2. Análise de código

Considere:

```java
public abstract class Employee {

    protected String name;
    protected Double salary;

    public Employee(String name, Double salary) {
        this.name = name;
        this.salary = salary;
    }

    public abstract Double calculateBonus();

    public void displayData() {
        System.out.println(name + " - " + salary);
    }
}
```

Responda:

1. Por que `Employee` é abstrata?
2. Por que `calculateBonus()` não possui corpo?
3. O método `displayData()` é abstrato? Por quê?
4. É possível instanciar `Employee` diretamente?

### 3. Prática de programação

Implemente as seguintes classes:

**Classe abstrata `Employee`:**
- Atributo `name`;
- Atributo `salary`;
- Construtor para inicializar os atributos;
- Método abstrato `calculateBonus()`;
- Método concreto `displayData()`.

**Classe `Developer`:**
- Herda de `Employee`;
- Implementa `calculateBonus()` retornando 10% do salário.

**Classe `Manager`:**
- Herda de `Employee`;
- Implementa `calculateBonus()` retornando 20% do salário.

**Classe `Main`:**
- Crie um objeto `Developer`;
- Crie um objeto `Manager`;
- Utilize referências do tipo `Employee`;
- Exiba os dados e os bônus dos dois funcionários.

**Objetivo:** praticar classes abstratas, métodos abstratos, herança, sobrescrita e polimorfismo.
