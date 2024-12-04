# Open Closed Principle (OCP)

## Conceito

aka: Princípio aberto e fechado.

A OCP tem o conceito de que o código deve ser construído de forma que possa ser aberto para adição de novas features e fechado para modificação de features que já estão em produção.

## Principais Benefícios

 - **Facilita a Manutenção**: Aumenta a estabilidade do sistema, pois evita que mudanças causem efeitos colaterais.
 
 - **Evita Regressões**: Diminuir a necessidade de modificar o código existente reduz o risco de introduzir bugs em funcionalidades já estáveis.
 
 - **Favorece a Extensibilidade**: Permite adicionar novas funcionalidades ao sistema, sem modificar o código que já foi testado e validado.
 
 - **Reduz o Acoplamento**: Seguir o OCP leva a um design onde o código é menos dependente de implementações específicas, permitindo mudanças com menos impacto.

 ## Exemplo - Cálculo de taxa de imposto sobre a Venda

 Digamos quem em um sistema há a classe Tax.ts que calcula os impostos que deve ser calculado sobre uma venda em 20% do valor:

```typescript
class Tax {
    taxValue: number = 0.2;

    static taxSell(sellValue: number) {
        return sellValue * taxValue; // cobra 20% do valor de venda
    }
}

class TaxService {
    constructor(private tax: Tax = new Tax()) {}

    taxSell(sell: number) {
        return this.tax.taxSell(sell);
    }
}
```

O problema acontece porque taxas tem uma alta recorrência de modificações de acordo com as regras monetárias, então é necessário sempre adicionar novas formas de calcular taxas. Digamos que nessa situação fosse nessário criar diversos services para cada categoria de venda.

```typescript
class Sell {
    category: 'A' | 'B';
    value: number;
}

class Tax {
    taxValueA: number = 0.2;
    taxValeuB: number = 0.3;

    static taxSellA(sellValue: number) {
        return sellValue * taxValueA;
    }

    static taxSellB(sellValue: number) {
        return  sellvalue * taxValueB;
    }
}

class TaxService {
    constructor(private tax: Tax = new Tax()) {}

    taxSell(sell: Sell) {
        if (sell.category == 'A')
            return this.tax.taxSellA(sell.value);
        return this.tax.taxSellB(sell.value);
    }
}
```

Essa implementação faz com que tenha a necessidade de modificar a classe Tax ou TaxService (condição dependendo do SRP), que pode gerar sideefecture por mudar bastante a estrutura do sistema. pAgora vamos para uma implementação com o OCP utilizando o princípio de injeção de dependência.

```typescript
export interface Tax {
    taxValue: number;
    taxSell(sell: number): number;
}

export class StandardTax implements Tax{
    taxValue = 0.2;
    taxSell(sell: number) {
        return sellValue * taxValue;
    }
}

export class TaxService {
    constructor(private tax: Tax)

    tax(sell) {
        return this.tax.taxSell(sell);
    }
}

export class TaxController {
    getSellTax() {
        const tax = new TaxService(new StandardTax());
    }
}
```

Agora adicionamos as categorias:

```typescript
class Sell {
    category: 'A' | 'B';
    value: number;
}

export class TaxB implements Tax{
    taxValue = 0.3;
    taxSell(sell: number) {
        return sellValue * taxValue;
    }
}

export class TaxController {
    private getTaxBySellCategory(category) {
        if (category === 'B')
            return new TaxB();
        return new StardardTax();
    }

    getSellTax(sell: Sell) {
        const taxer = new TaxService(this.sell.category);
        return taxer.tax(sell);
    }
}
```

Apesar de aumentar a complexidade, esse trecho de código fornecerá a possibilidade de implementar diversos tipos de taxas novas com mínimas modificações no Tax Controller e apenas adições de novas Taxas.
