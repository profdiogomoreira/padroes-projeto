# OCP - Princípio de Aberto/Fechado

Software é um **produto evolutivo**, e raramente é feito uma única vez e nunca mais será modificado. Sendo assim, nosso software deve conseguir evoluir a medida que as **demandas por funcionalidade** crescem. Porém, como essas evoluções devem vir, por meio de alterações ou extensões?

O **Princípio de Aberto/Fechado** diz que "Objetos ou entidades devem estar **abertas para extensão**, mas **fechadas para modificação**".

Ou seja, as entidades de software como classes, módulos, funções, etc, devem atender a essa afirmação. Sendo assim, uma classe **deve ser** facilmente extensível sem a necessidade de alterar o seu código.

A **extensibilidade** é uma das chaves da orientação a objetos. Quando um novo comportamento ou funcionalidade precisa ser adicionado, espera-se que as existentes sejam **estendidas** e não alteradas, dessa forma o código original permanece **intacto e confiável** enquanto as novas são implementadas através de extensibilidade.

Tendo em mente o princípio do aberto/fechado, o **impacto das mudanças reduz drasticamente**. Veja o exemplo de código abaixo

```java
public class Debito {
    
    public enum TipoDebito {
        CONTA_CORRENTE, POUPANÇA
    }

    public void debitar(BigDecimal valor, TipoDebito tipo) {
        if (tipo == TipoDebito.CONTA_CORRENTE) {
            // Lógica de débito em conta-corrente...
            return;    
        }
        if (tipo == TipoDebito.POUPANÇA) {
            // Lógica de débito em poupança...
            return;
        }
    }
}
```

É uma classe `Debito` que valida o tipo da conta para aplicar a regra de negócio correta para conta corrente ou para conta poupança. Agora vamos supor que surgiu um novo tipo de débito \(conta **digital**, por exemplo\), logo seria necessário modificar a classe, acrescentando um `if` a mais.

Mas qual é o problema de um `if` a mais? Estamos falando de um caso simples, onde apenas uma mudança está sendo requerida, mas quando não pensamos em extensibilidade, essa classe pode sofrer inúmeras mudanças ao longo da vida do sistema, se tornando um aninhado imenso de ifs, onde nenhum desenvolvedor tem segurança em mudar, com receio de adicionar um bug

Além de ter que testar todos os tipos de débito em conta, um bug introduzido nesta modificação não pararia apenas o débito em conta digital mas poderia causar que todos os tipos de débitos parassem de funcionar.

Se dividirmos a responsabilidade, aplicando o **Princípio de Aberto/Fechado**, teremos um cenário semelhante a esse abaixo.

```java
public abstract class Debito {
    public abstract void debitar(BigDecimal valor, TipoDebito tipo);
}
```

E as lógicas de negócio? Onde ficam? Nas classes que herdam de `Debito`, obviamente. Um exemplo é a classe `DebitoContaCorrente` abaixo. Perceba como o código fica mais legível, sem tantas verificações e com uma abordagem que facilita a correção de possíveis bugs em um tipo determinado de Débito, uma vez que as lógicas estão separadas.

```java
public class DebitoContaCorrente extends Debito {

    public void debitar(BigDecimal valor, TipoDebito tipo) {
        // Lógica de débito em conta-corrente...
        return;
    }
}
```

Veja que possuímos agora uma abstração \(`Debito`\) bem definida, onde todas as extensões implementam suas próprias regras de negócio **sem necessidade de modificar uma funcionalidade devido mudança ou inclusão de outra**.

Assim, podemos criar software **flexível**, **robusto** e que **aceita as mudanças**, ao invés de brigar com elas.

