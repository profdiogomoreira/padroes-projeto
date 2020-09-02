# SRP - Princípio de Responsabilidade Única

Uma das bases da programação orientada a objetos é a **Coesão**. Uma classe é **coesa** quando ela é focada em uma responsabilidade bem definida, assim como também não tem responsabilidades além da sua. Se uma classe deve gerar uma nota fiscal, por exemplo, essa deve ser a única responsabilidade que ela deve ter e tudo além disso vai diminuir a sua coesão.

É importante termos classes coesas em um projeto orientado a objeto, pois elas apresentam uma série de vantagens, tais como:

* **Facilidade de manutenção**. Uma vez que temos classes com uma única responsabilidade, saberemos quais classes afetam ou não uma funcionalidade, qual o escopo dessa classes e onde a manutenção e correções de bugs de uma determinada funcionalidade devem ser feitas.
* **Menos código**. Com menos código, a legibilidade fica mais fácil e podemos ganhar tempo no entendimento do sistema
* **Reuso**. Uma vez que uma classe cuida de uma única responsabilidade, é fácil reusar sempre que quisermos essa mesma responsabilidade/funcionalidade no sistema, sem que hajam efeitos colaterais.

O **Princípio de Responsabilidade Única** reforça o uso da coesão, afirmando que se uma classe tem muitas responsabilidades, aumentam as possibilidades de ocorrerem _bugs_ ao alterar uma de suas responsabilidades, sem que possamos perceber. **Responsabilidade** aqui pode ser definida como um "motivo para mudança" e **Robert Martin** conclui que "**uma classe ou módulo deve ter um, e somente um, motivo para ser alterada**" \(ou reescrita\).

Como exemplo, considere uma classe que **compila** \(coleta dados, realiza processamento, etc.\) e **imprime** um relatório.

```java
class Relatorio {
  public void compilarRelatorio() {
    // Lógica de compilar o relatório
  }

  public void imprimirRelatorio() {
    // Lógica para montar o relatório no formato de impressão escolhido
  }
}
```

Essa classe pode ser alterada por 2 motivos:

1. O conteúdo e as fontes para compilação do relatório podem mudar
2. O formato do relatório impresso pode mudar

Essas duas motivações para alterações acontecem por causas muito distintas. Uma por questões de lógica e outro por questões estéticas. O **Princípio de Responsabilidade Única** diz que esses dois aspectos, na prática, são duas responsabilidades separadas, por mais que estejamos trabalhando com a **geração de um relatório**. Sendo assim, devem estar em classes ou módulos separados, e seria uma má decisão de projeto de classes que elas estivessem juntas uma vez que mudam por diferentes motivos em momentos diferentes.

{% hint style="info" %}
Você deve estar se perguntando **"existe uma** _**regra**_ **pra fazer essa separação?"** e a resposta é **não**. O importante é que ao se projetar uma classe, analisar os momentos e os motivos que levariam você a alterar essa classe no futuro. Com um tempo essa tarefa vai ficar mais intuitiva e fácil de realizar
{% endhint %}





