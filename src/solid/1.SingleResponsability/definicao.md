# Single Responsibility Principle (SRP)

## Conceito

Aka: Princípio da Responsabilidade Única.

O Princípio da Responsabilidade Única é um dos cinco princípios SOLID da programação orientada a objetos. Ele estabelece que uma classe deve ter apenas uma razão para mudar, ou seja, deve ter apenas uma responsabilidade bem definida.

### Principais Aspectos

- **Foco**: Cada classe deve se concentrar em fazer apenas uma coisa bem feita.

- **Coesão**: Os métodos e propriedades da classe devem estar estreitamente relacionados à sua responsabilidade 
principal.

- **Manutenibilidade**: Facilita a manutenção do código, pois mudanças em uma responsabilidade não afetam outras 
partes do sistema.

- **Reusabilidade**: Classes com responsabilidades únicas são mais fáceis de reutilizar em diferentes contextos.

- **Testabilidade**: Simplifica os testes unitários, pois cada classe tem um conjunto bem definido de comportamentos 
para testar.

Quando uma classe possui múltiplas responsabilidades, cada uma dessas responsabilidades se torna uma razão potencial para modificar a classe. Isso aumenta o risco de que mudanças em uma responsabilidade impactem outras partes da classe, dificultando a manutenção e a previsibilidade do comportamento da aplicação.

Aplicar este princípio resulta em um código mais modular, flexível e fácil de entender, promovendo uma melhor organização e arquitetura do software.

---

## Exemplo - Invoice

Digamos que temos uma classe `Invoice` que serve como uma representação dos dados de uma nota fiscal. Ela possui a responsabilidade única de abstrair as informações presentes na nota fiscal.

Agora, imagine que é necessário adicionar uma funcionalidade para realizar um filtro em um conjunto de notas fiscais. Adicionar essa funcionalidade diretamente na classe `Invoice` significaria dar a ela uma nova responsabilidade, o que vai contra o SRP. Essa nova responsabilidade poderia afetar futuras manutenções e a reusabilidade da classe, adicionando complexidade desnecessária.

Em vez de modificar `Invoice` para lidar com o filtro, podemos aplicar o SRP criando uma nova classe `InvoiceFilter`, cuja responsabilidade é exclusivamente realizar operações de filtragem em notas fiscais.

Refatorando o Código com SRP: Veja o código completo no [Invoice.ts](./Invoice.ts).


```typescript

/**
 * Classe Invoice com as responsabilidades:
 *  - Ser uma abstração de Notas Fiscais
 *  - Filtrar as Notas Fiscais por CNPJ
 */
export class InvoiceDeprecated {
    cnpj: string;
    products: Object;

    constructor() {
        this.cnpj = '';
        this.products = {};
    }

    filterInvoicesByCnpj(invoices: InvoiceDeprecated[], cnpj: string) {
        return invoices.filter((invoice) => invoice.cnpj === cnpj);
    }
}


/**
 * Classe Invoice com a única responsabilidade de ser a abstração das notas fiscais.
 */
export class Invoice {
    cnpj: string;
    products: Object;

    constructor() {
        this.cnpj = '';
        this.products = {};
    }
}

/**
 * Classe InvoiceService com a única responsabilidade de gerir as notas fiscais.
 */
export class InvoiceService {
    filterInvoicesByCnpj(invoices: Invoice[], cnpj: string) {
        return invoices.filter((invoice) => invoice.cnpj === cnpj);
    }
}

```