# Dependency Inversion Principle

## Conceito

aka: Princípio de Inversão de Dependência

O DIP define que as classes de alto nível (classes central que contém a lógica do sistema) não devem depender de classes de baixo nível. Ambas as classes devem depender de abstrações.

O DIP propõe duas regras principais:

- Módulos de alto nível não devem depender de módulos de baixo nível: ambos devem depender de abstrações.
- Abstrações não devem depender de detalhes: detalhes devem depender de abstrações.

## Benefícios do DIP

- **Flexibilidade e Facilidade de Mudança**: Como as classes de alto nível não dependem diretamente de implementações de baixo nível, você pode alterar essas implementações sem modificar o código que usa essas abstrações.

- **Redução de Acoplamento**: O DIP reduz o acoplamento entre módulos, permitindo que eles sejam mais independentes e mais facilmente reutilizáveis.

- **Facilidade de Testes**: Com o DIP, você pode substituir dependências concretas por mocks ou stubs, facilitando o teste isolado das classes de alto nível.

## Exemplo - Notifier Service
 
Em uma situação que viola o DIP, temos a implementação de um OrderService, classe responsável para confirmar compras e enviar notificação, e uma classe EmailService. Se a classe OrderService for dependente do EmailService, então em uma circustância que é necessário mudar o tipo de notificação, deverá ocorrer a implementação.

```typescript
class EmailService {
  sendEmail(message: string): void {
    console.log("Sending email:", message);
  }
}

class OrderService {
  private emailService: EmailService;

  constructor() {
    this.emailService = new EmailService();
  }

  confirmOrder(): void {
    // Lógica para confirmar o pedido
    this.emailService.sendEmail("Order confirmed!");
  }
}
```

Uma das soluções é tornar o OrderService dependendo de uma abstração Notifier, assim facilmente poderá ser substituído por outros módulos casso seja necessário.

```typescript
abstract class Notifier {
    abstract send(message: string): void;
}

class EmailService implements Notifier {
    send(message: string): void {
        console.log("Enviando email")
    }
}

class OrderService {
    constructor(
        private notifier: Notifier,
    ) {}

    confirmOrder() {
        this.notifier.send("Order confirmed!");
    }
}


```
