# Abstract Factory

O **Abstract Factory** é um padrão de projeto que resolve o problema de criar **famílias inteiras de produtos** que **só funcionam em conjunto**, sem que seja necessário especificar suas classes concretas.

O Abstract Factory define uma **interface para criar todos os produtos** distintos, mas deixa a criação real do produto para classes de fábrica concretas. Cada tipo de fábrica corresponde a uma determinada família de produtos.

Um **Abstract Factory** na prática é apenas uma extensão do **Factory Method**, usando de **vários métodos** em uma classe **especializada em construir objetos de uma mesma família**.

Vamos usar o mesmo exemplo de antes, expandindo para o conceito de família de objetos.

```java
public abstract class Dialogo {
    public abstract Botao criarBotao();
    public abstract Janela criarJanela();
    public abstract CampoFormulario criarCampoFormulario();
}
```

Nesse exemplo, cada janela de Dialogo concreta, que estende de `Dialogo`, deve ter também suas implementações próprias dos componentes que usa, como `Botao`, `Janela` e `CampoFormulario`. Já imaginou uma tela para Linux utilizando botões de Windows e com campos de formulário próprios para o MacOS? Não faz sentido, né?

Pois bem, nesse caso, toda vez que quisermos criar uma janela de Dialogo para um sistema operacional especifico, iremos usar sua própria **Abstract Factory**, como no exemplo abaixo:

```java
public class DialogoLinux {
    public Botao criarBotao() {
        return new BotaoLinux(); // BotaoLinux é uma classe especializada de Botao
    }
    public Janela criarJanela() {
        return new JanelaLinux();
    }
    public CampoFormulario criarCampoFormulario() {
        return new CampoFormularioLinux();
    }
}
```

Dessa maneira, sempr que quiseremos "mudar" a família de objetos, só precisamos usar uma instância da fábrica referente a família desejada \(Linux, Mac ou Windows\).

### Exemplos de Abstract Factory nas APIs nativas do Java

Para citar exemplos de objetos que funcionam apenas com objetos da mesma família, podemos falar dos drivers **JDBC** para bancos \(PostgreSQL, Oracle, etc\). A classe **Connection** funciona como uma fábrica abstrata que cria vários objetos que só funcionam como uma família especifica, relacionada ao banco de dados que se deseja comunicar. Desse modo, quando criamos uma consulta \(`PreparedStatement`\) ou executamos a mesma para obter os resultados \(`ResultSet`\), estamos usando objetos da mesma família que, de outro modo, não funcionariam em conjunto.

