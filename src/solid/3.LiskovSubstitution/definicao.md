# Liskov Substitution Principle

aka: Princípio de Substituição de Liskov

O LSP sugere que uma classe derivada deve ser capaz de ser utilizada no lugar de sua classe base sem causar problemas de comportamento. Então significa que as subclasses devem ser desenvolvidas sem violar as expectativas da classe base.

## Principais Aspectos

- **Compatibilidade de Tipo**: As subclasses devem ser capazes de ser usadas de forma intercambiável com a superclasse, garantindo que o cliente da superclasse não precise se preocupar com a implementação da subclasse.

- **Preservação de Comportamento**: A subclasse deve manter a mesma funcionalidade e restrições que a superclasse. Não deve alterar o comportamento esperado das operações herdadas.

- **Ajuste de Pré-condições e Pós-condições**: As subclasses não devem restringir as condições necessárias (pré-condições) ou aumentar as condições de resultado (pós-condições) em relação à superclasse.

- **Exceções**: As exceções lançadas pela subclasse não devem ser mais restritivas do que aquelas da superclasse. Se a superclasse lança uma exceção, a subclasse deve estar em conformidade.

## Exemplo - Pássaros

Descrevemos uma classe que viola os princípios de Liskov:

```typescript

class Bird {
    fly(): void {
        console.log("flying.");
    }
}

class Sparrow extends Bird {
    fly(): void {
        console.log("flying.");
    }
}

class Ostrich extends Bird {
  fly(): void {
    throw new Error("Ostriches cannot fly.");
  }
}

```

Foi implementado uma classe aveztrus que por mais seja uma ave, ela não é capz de voar, violando o LSP.

```typescript

interface Bird {
  makeSound(): void;
}

interface FlyingBird extends Bird {
  fly(): void;
}

class Sparrow implements FlyingBird {
  makeSound(): void {
    console.log("Chirp");
  }

  fly(): void {
    console.log("Sparrow flying...");
  }
}

class Ostrich implements Bird {
  makeSound(): void {
    console.log("Boom");
  }
}

```

Nesse exemplo, Bird é uma interface que define o comportamento comum, enquanto FlyingBird é uma interface que estende Bird com a capacidade de voar. Assim, Sparrow implementa ambas, enquanto Ostrich implementa apenas Bird, respeitando o LSP. Isso permite que ambas as classes sejam usadas em diferentes contextos sem quebrar a lógica do programa.