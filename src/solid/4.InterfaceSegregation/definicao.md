# Interface Segregation Principle

## Conceito

aka: Princípio de segregação de interface

O ISP define que as interfaces não devem fazer com que as classes implemente métodos que elas não utilizem. É preferível ter várias interfaces que uma interface inchada.

## Princípios do ISP:

- **Interfaces específicas e focadas**: Cada interface deve ser pequena e fornecer um único conjunto de comportamentos coesos.

- **Evitar métodos irrelevantes**: Classes não devem ser obrigadas a implementar métodos que não fazem sentido para elas.

- **Segregação**: Dividir interfaces grandes em várias menores e específicas, cada uma atendendo um grupo específico de implementações.

## Exemplo - Worker

Descrevemos uma classe que viola os princípios de ISP:

```typescript
class HumanWorker implements Worker {
  work(): void {
    console.log("Working...");
  }
  eat(): void {
    console.log("Eating...");
  }
}

class Robot implements Worker {
  work(): void {
    console.log("Working...");
  }
  eat(): void {
    // Método não relevante para robôs, mas precisa ser implementado
  }
}
```

Para seguir o ISP, podemos dividir a interface Worker em interfaces menores e mais específicas, cada uma representando um aspecto particular do trabalho.

```typescript
interface Workable {
  work(): void;
}

interface Eatable {
  eat(): void;
}

class HumanWorker implements Workable, Eatable {
  work(): void {
    console.log("Working...");
  }
  eat(): void {
    console.log("Eating...");
  }
}

class Robot implements Workable {
  work(): void {
    console.log("Working...");
  }
}
```

Agora, HumanWorker pode implementar ambas as interfaces, enquanto Robot implementa apenas Workable, respeitando o ISP.