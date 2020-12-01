# Padrão Bridge

O **Bridge** é um padrão de projeto estrutural que divide a lógica de negócio ou uma enorme classe em hierarquias de classe separadas que podem ser desenvolvidas ou variadas independentemente.

Uma dessas hierarquias \(geralmente chamada de **Abstração**\) obterá uma referência a um objeto da segunda hierarquia \(**Implementação**\). A abstração poderá delegar algumas \(às vezes, a maioria\) de suas chamadas para o objeto de implementações. Como todas as implementações terão uma interface comum, elas seriam intercambiáveis dentro da abstração.

### Problema

Suponha agora que é necessário fazer um programa que vá funcionar em várias plataformas, por exemplo, Windows, Linux, Mac, etc. O programa fará uso de diversas **abstrações de janelas gráficas**, por exemplo, janela de diálogo, janela de aviso, janela de erro, etc. Essas abstrações são conceitos de janelas compostas por vários componentes como botões, rótulos e painéis.

![Exemplo de janela de di&#xE1;logo](.gitbook/assets/image%20%285%29.png)

Como podemos representar esta situação? A utilização de um [Adapter](padrao-adapter.md) para adaptar as janelas para as diversas plataformas parece ser boa, criaríamos um Adapter para Windows, Linux e Mac e então utilizaríamos de acordo com a necessidade. No entanto teríamos que utilizar o adaptador de cada uma das plataforma para cada um dos tipos de abstrações de janelas. Por exemplo, para uma janela de diálogo, teríamos um Adapter para Windows, Linux e Mac, da mesma forma para as outras janelas.

Esta solução tem problemas:

* Torna o **código do cliente dependente de plataforma**, o que dificulta sua **portabilidade** para outras plataformas
* **Amarra na mesma hierarquia a abstração e a implementação**
* Elas **não podem variar de forma independente**
* **Dificulta o reuso da abstração e o reuso da implementação**

### **Solução**

O Padrão **Bridge** tem como intenção “Desacoplar uma abstração da sua implementação, de modo que as duas possam variar independentemente”. Ou seja, o Bridge fornece um nível de abstração maior que o Adapter, pois são **separadas as implementações e as abstrações, permitindo que cada uma varie independentemente**.

![](.gitbook/assets/image%20%286%29.png)

Para o exemplo as implementações seriam as classes de Janela das plataformas. Vamos iniciar construindo elas, de maneira bem simples. A primeira classe será a interface comum a todas as implementações, chamadas de `ComponentesJanela`:

```java
public interface Janela {

    void desenharJanela(String titulo);
    void desenharBotao(String titulo);
    
}
```

Ou seja, nessa classe definimos que **todas as janelas desenham uma janela e um botão**. Poderiamos adicionar outros componentes, mas deixamos apenas esses para simplificar

Vamos ver agora a classe concreta que desenha a janela na plataforma Windows:

```java
public class JanelaWindows implements Janela {
 
    @Override
    public void desenharJanela(String titulo) {
        System.out.println(titulo + " - Janela Windows");
    }
 
    @Override
    public void desenharBotao(String titulo) {
        System.out.println(titulo + " - Botão Windows");
    }
 
}
```

Também uma classe bem simples, apenas vamos exibir uma mensagem na saída padrão para saber que tudo correu bem.

Vamos então para as abstrações. Elas **são abstrações pois não definem uma janela específica, como a JanelaWindows, no entanto utilizarão os métodos destas janelas concretas para construir suas janelas**.

Vamos então iniciar a construção da classe abstrata que vai fornecer uma interface de acesso comum para as abstrações de janelas:

```java
public abstract class JanelaAbstrata {
 
    protected Janela janela;
 
    public JanelaAbstrata(Janela j) {
        janela = j;
    }
 
    public void desenharJanela(String titulo) {
        janela.desenharJanela(titulo);
    }
 
    public void desenharBotao(String titulo) {
        janela.desenharBotao(titulo);
    }
 
    public abstract void desenhar();
 
}
```

Essa classe possui uma referência para a interface das janelas implementadas, com isso conseguimos variar a implementação de maneira bem simples. Agora veja o exemplo da classe de JanelaDialogo, que abstrai uma janela de diálogo para todas as plataformas:

```java
public class JanelaDialogo extends JanelaAbstrata {
 
    public JanelaDialogo(Janela j) {
        super(j);
    }
 
    @Override
    public void desenhar() {
        desenharJanela("Janela de Diálogo");
        desenharBotao("Botão Sim");
        desenharBotao("Botão Não");
        desenharBotao("Botão Cancelar");
    }
 
}
```

Uma janela de diálogo exibe sempre três botões: **Sim**, **Não** e **Cancelar**. Ou seja, independente de qual plataforma se está utilizando, a abstração é sempre a mesma. Para uma janela de aviso por exemplo, bastaria um botão Ok, então sua implementação seria algo do tipo:

```java
public class JanelaAviso extends JanelaAbstrata {
 
    public JanelaAviso(Janela j) {
        super(j);
    }
 
    @Override
    public void desenhar() {
        desenharJanela("Janela de Aviso");
        desenharBotao("Ok");
    }
 
}
```

Ao final, nosso exemplo está exatamente como o diagrama UML abaixo, quando podemos visualizar que criar duas hierarquias e ligá-las por meio de uma ponte \(_**Bridge**_\) faz com que nosso código seja independente de plataforma, que possamos variar abstrações e implementações separadamente, facilitando o reuso.

![](.gitbook/assets/image%20%287%29.png)

