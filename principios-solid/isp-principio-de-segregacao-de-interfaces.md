# ISP - Princípio de Segregação de Interfaces

Na orientação a objetos, quando falamos de **interface**, estamos falando do conjunto de métodos que um objeto expõe, ou seja, das maneiras como nós podemos interagir com esse objeto \(como vimos em [Objetos e classes](../orientacao-a-objetos/conceitos-basicos-de-orientacao-a-objetos/objetos-e-classes.md)\). Toda mensagem \(ou chamada de método\) que um objeto recebe constitui uma interface.

A interface funciona como um **contrato**: nós definimos o comportamento da interface na forma de diferentes métodos que ela possui. Cada classe que desejar compartilhar o comportamento dessa interface precisa **implementar** os métodos dela, ou seja, declarar como esses métodos serão executados. Quando a classe utiliza uma interface, na prática ela **assina o contrato** dizendo que **irá implementar todos os métodos dessa interface**.

Pensando num contexto de geração de relatórios, imagine uma interface como a que é mostrada abaixo.

```java
public interface Relatorio {

    void imprimirCabecalho();
    void imprimirCorpo();
    void imprimirTabela();
    void imprimirFigura();
    void imprimirRodape();
    
}
```

Todos os métodos acima fazem sentido para o contexto de relatórios? **Sim.** Eles deveriam estar na mesma interface? **Não**. Isso porque ao implementar essa interface talvez não haja a necessidade de implementar todos os métodos. Existem relatórios que não teriam Figuras, assim como outros não teriam Tabelas ou até mesmo Rodapé.

Isso acontece porque **tentamos generalizar demais uma interface**. Separar essa interface em outras mais específicas deixaria essa interface em acordo com **Princípio de Segregação de Interfaces**, que diz que "Nenhum cliente deve ser forçado a depender de métodos que ele não usa.", por Cliente aqui, entenda **classe que implementa**.

Sendo assim, uma proposta de interface mais adequada seria

```java
public interface RelatorioPadrao {

    void imprimirCorpo();
    
}
```

Uma vez que todo relatório tem um corpo, poderiamos deixar isso em uma interface `RelatorioPadrao`, enquanto as especificidades ficariam em `RelatorioComCabecalho`, `RelatorioComTabela` e assim por diante.

```java
public interface RelatorioComCabecalho {

    void imprimirCabecalho();
    
}
```

Uma vez que um determinado cliente precise de um conjunto dessas interfaces, é possível [**realizar**](../orientacao-a-objetos/conceitos-basicos-de-orientacao-a-objetos/realizacao.md) mais do que uma e implementar apenas os métodos que serão necessários.

