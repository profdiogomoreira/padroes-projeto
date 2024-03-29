# Padrão Chain of Responsibility

O Padrão **Chain of Responsibility** é um padrão de projeto comportamental que permite que você repasse chamadas de métodos através de uma corrente de objetos que podem tratar essa chamada. Ao receber um pedido, cada objeto da cadeia deve decidir se processa o pedido ou se passa adiante para o próximo objeto na corrente.

### **Problema**

Imagine que você está construindo uma loja virtual pra vender **adesivos de notebook** e que, para atrair novos clientes, criou algumas regras para que descontos sejam aplicados. Entre essas regras estão:

* Pedidos com mais de 10 itens devem ter desconto de R$ 5,00
* Pedidos que contem um item do tipo **Especial** deve ter desconto de R$ 2,00
* Pedidos que totalizem mais de R$ 100,00 devem ter R$ 8,00 de desconto

Como iriamos criar uma implementação para aplicar esses descontos? Pensando em um exemplo inicial podemos ter o seguinte cenário.

```java
public class Desconto {

    public void implantarDesconto(Pedido pedido) {
        List<ItemPedido> itensPedido = pedido.getItensPedido();
        if (itensPedido.size() > 10) {
            pedido.addDesconto(5d);
        }
        boolean temItensEspeciais = itensPedido.stream()
            .anyMatch(x -> x.getTipo() == TipoItem.ESPECIAL);
        if (temItensEspeciais) {
            pedido.addDesconto(2d);
        }
        if (pedido.getTotal() > 100) {
            pedido.addDesconto(8d);
        }
    }

}
```

Para visualizar a implementação das demais classes, consulte o repositório, mas não é necessário por hora. O problema dessa abordagem é a **quantidade de condições** \(`ifs`\) concatenadas que podem ter nesse método de `implantarDesconto`, o que deixa o código **mais complexo para manutenção**.

### Solução

Transformaremos a classe Desconto em uma `interface`, evidenciando o método `implantarDesconto` e definindo um método `setProximo`, para que possamos definir os elementos que virão na sequência da cadeia.

```java
public interface Desconto {

    void implantarDesconto(Pedido pedido);

    void setProximo(Desconto desconto);

}
```

As implementações da interface `Desconto` se focam apenas nas regras de negócio que desejam processar, nesse caso as regras específicas de quando aplicar desconto ou não. Veja no exemplo abaixo a classe `DescontoQuantidadeItens`.

```java
public class DescontoQuantidadeItens implements Desconto {

    private Desconto proximo;

    public void implantarDesconto(Pedido pedido) {
        List<ItemPedido> itensPedido = pedido.getItensPedido();
        if (itensPedido.size() > 10) {
            pedido.addDesconto(5d);
        }
        if (proximo != null)
            proximo.implantarDesconto(pedido);
    }

    @Override
    public void setProximo(Desconto desconto) {
        this.proximo = desconto;
    }

}
```

 Poderiamos ter usado uma classe abstrata pra esse exemplo? Sim, mas ao utilizar interfaces, deixamos a possibilidade de usar herança pra um momento onde ela realmente é **inevitável**.

```java
public class DescontoTotalAcimaCem implements Desconto {

    private Desconto proximo;

    @Override
    public void implantarDesconto(Pedido pedido) {
        if (pedido.getTotal() > 100) {
            pedido.addDesconto(8d);
        }
        if (proximo != null)
            proximo.implantarDesconto(pedido);
    }

    @Override
    public void setProximo(Desconto desconto) {
        this.proximo = desconto;
    }
}
```

Cada uma das implementações deve verificar se existem objetos dentro da cadeia que ainda podem receber essa solicitação posteriormente, porém, não conhecem se aquela solicitação é a primeira da cadeia ou em que posição da cadeia ela está. Apenas trata a chamada de acordo com suas regras.

```java
public class DescontoItensEspeciais implements Desconto {

    private Desconto proximo;

    @Override
    public void implantarDesconto(Pedido pedido) {
        List<ItemPedido> itensPedido = pedido.getItensPedido();
        boolean temItensEspeciais = itensPedido.stream().anyMatch(x -> x.getTipo() == TipoItem.ESPECIAL);
        if (temItensEspeciais) {
            pedido.addDesconto(2d);
        }
        if (proximo != null)
            proximo.implantarDesconto(pedido);
    }

    @Override
    public void setProximo(Desconto desconto) {
        this.proximo = desconto;
    }
}
```

Dessa maneira, podemos ter variações de um mesmo algoritmo, que antes era **imutável** e agora é definido em tempo de execução. Veja o exemplo abaixo, onde definimos a sequência `DescontoItensEspeciais` &gt; `DescontoQuantidadeItens` &gt; `DescontoTotalAcimaCem`. A última parte do algoritmo é responsável por definir a sequência, e pode ser alterado como for necessário.

```java
public static void main(String[] args) {
    ItemPedido ip1 = new ItemPedido("Adesivo 1", TipoItem.NORMAL, 12d);
    ItemPedido ip2 = new ItemPedido("Adesivo 2", TipoItem.NORMAL, 10d);
    ItemPedido ip3 = new ItemPedido("Adesivo 3", TipoItem.ESPECIAL, 15d);
    
    // Criamos um pedido
    Pedido p = new Pedido();
    p.addItemPedido(ip1, ip2, ip3);
    
    // Criamos os objetos de desconto
    Desconto cadeiaDeDescontos = new DescontoItensEspeciais();
    Desconto descontoQtdeItens = new DescontoQuantidadeItens();
    Desconto valorAcimaDeCem = new DescontoTotalAcimaCem();
    
    // Definimos a cadeia
    descontoQtdeItens.setProximo(valorAcimaDeCem);
    cadeiaDeDescontos.setProximo(descontoQtdeItens);
}
```

### Exemplo de Chain of Responsibility nas APIs nativas do Java

**Filtros** utilizados na API de Servlets em Java são exemplos de um Chain of Responsibility. Ao implementarmos um filtro usando essa API, estamos fazendo com que uma cadeia de objetos trate uma requisição HTTP, por exemplo.

### Referências

* [https://emmanuelneri.com.br/2016/11/28/padrao-chain-of-responsibility/](https://emmanuelneri.com.br/2016/11/28/padrao-chain-of-responsibility/)
* “**Design Patterns: Elements of Reusable Object-Oriented Software**”

