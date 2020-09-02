# LSP - Princípio de Substituição de Liskov

[Barbara Liskov](https://pt.wikipedia.org/wiki/Barbara_Liskov) introduziu o **Princípio de substituição de Liskov** como uma definição particular para o conceito de subtipo. O princípio foi definido de [forma resumida](http://reports-archive.adm.cs.cmu.edu/anon/1999/CMU-CS-99-156.ps) como:

_Se_![q\(x\)](https://wikimedia.org/api/rest_v1/media/math/render/svg/c38bbafe34a043d284f19231b946a76c0a4b16b4)_é uma propriedade demonstrável dos objetos_![x](https://wikimedia.org/api/rest_v1/media/math/render/svg/87f9e315fd7e2ba406057a97300593c4802b53e4) _de tipo_ ![T](https://wikimedia.org/api/rest_v1/media/math/render/svg/ec7200acd984a1d3a3d7dc455e262fbe54f7f6e0)_. Então_![{\displaystyle q\(y\)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/46049a30deb0e2a1d751cab6457c5204d7ee82a9) _deve ser verdadeiro para_![y](https://wikimedia.org/api/rest_v1/media/math/render/svg/b8a6208ec717213d4317e666f1ae872e00620a0d) _de tipo_![S](https://wikimedia.org/api/rest_v1/media/math/render/svg/4611d85173cd3b508e67077d4a1252c9c05abca2) _onde_![S](https://wikimedia.org/api/rest_v1/media/math/render/svg/4611d85173cd3b508e67077d4a1252c9c05abca2) _é um subtipo_ ![T](https://wikimedia.org/api/rest_v1/media/math/render/svg/ec7200acd984a1d3a3d7dc455e262fbe54f7f6e0)_._

Falando de outra maneira, a visão de "**subtipo**" defendida por Liskov é baseada na noção da **substituição**; isto é, se **S** é um subtipo de **T**, então os objetos do tipo **T**, em um programa, podem ser substituídos pelos objetos de tipo **S** sem que seja necessário alterar as propriedades deste programa.

Suponha que uma parte do sistema está utilizando uma determinada **funcionalidade**. Se essa funcionalidade precisar ser trocada por outra, por meio de polimorfismo dinâmico ou estático, a outra deverá devolver o mesmo tipo de informação, caso contrário, o sistema quebrará.

Neste contexto, para garantir que a classe **S** tenha o mesmo comportamento que a classe base **T**, é imprescindível a utilização de um contrato \(interface ou classe abstrata\), contendo as definições de métodos necessárias para que as classes que a herdarem sejam obrigadas a implementá-las.

Um exemplo clássico para demonstrar uma violação desse princípio é a modelagem de classes `Quadrado` e `Retangulo`. Todo quadrado **é um** retângulo? **Sim**, por definição. Se **é um**, portanto é lógico e intuitivo modelar uma classe `Quadrado` como sendo derivada da classe `Retangulo`. Aqui provavelmente conseguiriamos utilizar herança para modelar essas classes. Então teriamos as duas classes da seguinte maneira

```java
public class Retangulo {
    
    private double altura;
    private double largura;

    public Retangulo(double altura, double largura) {
        this.altura = altura;
        this.largura = largura;
    }

    public double getAltura() {
        return altura;
    }

    public void setAltura(double altura) {
        this.altura = altura;
    }

    public double getLargura() {
        return largura;
    }

    public void setLargura(double largura) {
        this.largura = largura;
    }
}
```

E a classe `Quadrado`. Perceba que sobrescrevemos os métodos `set` para garantir que ele continua sendo um quadrado, ou seja, tenho os lados iguais.

```java
public class Quadrado extends Retangulo {

    public Quadrado(double altura, double largura) {
        super(altura, largura);
    }

    @Override
    public void setAltura(double altura) {
        super.setAltura(altura);
        super.setLargura(altura);
    }

    @Override
    public void setLargura(double largura) {
        super.setAltura(largura);
        super.setLargura(largura);
    }
}
```

Com isso, a classe derivada **viola uma regra estabelecida na classe base**: a de que **altura e comprimento variam independentemente**. E quando isso vai se mostrar um problema? Vejamos o exemplo a seguir

```java
public void metodoXPTO(Retangulo retangulo) {
   retangulo.setAltura(retangulo.getAltura() * 2);
   retangulo.setComprimento(retangulo.getComprimento * 4);
   // Realiza alguma operação com os novos dados
}
```

O desenvolvedor facilmente pode assumir que estava lidando com um `Retangulo` \(afinal, é o tipo do parâmetro do método\) e aplicou um cálculo que variasse suas dimensões. No entanto, caso o método receba, em tempo de execução, um objeto do tipo `Quadrado` teremos um comportamento inesperado: o cálculo para redimensionar o retângulo o transformará em um quadrado, isto é, **ao quadruplicar o comprimento, inadvertidamente, a altura também foi quadruplicada**!

O problema acima só existe porque, como dito anteriormente, a classe derivada n**ão respeita a regra da classe base de variar os lados de forma independente**. Sendo assim, do ponto de vista computacional, Quadrado não é um Retangulo, pois ambos possuem comportamentos diferentes em relação a alteração de seus lados.

Esse cenário ocorre muitas vezes pela nossa intuição de que **Herança** sempre ocorre quando temos um cenário de "**é** **um"**, quando na verdade isso não é verdadeiro. Essa idéia ajuda na maioria das oportunidades de Herança, mas é sempre bom ter em mente o **Princípio de Substituição de Liskov** para verificarmos se realmente faz sentido.

