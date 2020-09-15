# Padrão State

O padrão State é intimamente relacionado com o conceito de uma [Máquina de Estado Finito](https://pt.wikipedia.org/wiki/M%C3%A1quina_de_estados_finita). Ou ainda, se você já estou **Diagrama de Estados** em **UML**, vai ser mais fácil de entender como esse padrão funciona. 

De qualquer maneira, esse padrão se foca nos problemas relacionados a **troca de estados de um objeto**. Esse é um problema bastante comum. Busque em sua memória e provavelmente você irá lembrar de alguma situação onde você verificou o estado de um objeto qualquer para decidir como um algoritmo deve ser executado.

### Problema

Vamos tomar como exemplo aqui um personagem de jogo, como **Mario** \(do [Super Mario World](https://pt.wikipedia.org/wiki/Super_Mario_World) de Super Nintendo\). Durante o jogo acontecem várias **trocas de estado** com Mario. Ao pegar uma flor de fogo, por exemplo, o mario cresce \(caso esteja pequeno\), e fica com a habilidade de soltar bolas de fogo.

![](../.gitbook/assets/image%20%282%29.png)

Desenvolvendo um pouco mais o pensamento temos um **grande conjunto de possíveis estados**, e **cada transição depende de qual é o estado atual do personagem**. Como falado anteriormente, ao pegar uma flor de fogo podem acontecer quatro ações diferentes, dependendo de qual o estado atual do mario:

* Se Mario estiver pequeno → Mario fica grande e com poder de fogo
* Se Mario estiver grande → Mario fica com poder de fogo
* Se Mario estiver com poder de fogo → Mario ganha 1000 pontos
* Se Mario estiver com capa → Mario fica com poder de fogo

**Todas estas condições devem ser checadas** para realizar esta única troca de estado. Agora imagine os vários estados que são possíveis no jogo e a complexidade para realizar a troca destes estados: Mario pequeno, Mario grande, Mario flor e Mario pena. Um código semelhante ao mostrado abaixo provavelmente seria criado a princípio:

```java
public class Mario {

    private enum MarioState {
        PEQUENO, GRANDE, FOGO, PENA
    }

    private MarioState estadoAtual;

    public void levarDano() {
        if (estadoAtual == MarioState.PEQUENO) {
            // Game over
            return;
        }
        if (estadoAtual == MarioState.GRANDE) {
            // Transformação pra Mario Pequeno
            return;
        }
        if (estadoAtual == MarioState.FOGO) {
            // Transformação pra Mario Grande
            return;
        }
        if (estadoAtual == MarioState.PENA) {
            // Transformação pra Mario Grande
            return;
        }
    }


    public void pegarCogumelo() {
        // Mesmas condicionais de levarDano()
    }


    public void pegarFlor() {
        // Mesmas condicionais de levarDano()
    }


    public void pegarPena() {
        // Mesmas condicionais de levarDano()
    }

}
```

Com certeza **não vale a pena investir tempo e código numa solução que utilize várias verificações para cada troca de estado**. Para não correr o risco de esquecer de tratar algum estado e deixar o código bem mais fácil de manter, vamos usar a proposta do **Padrão State** para resolver o problema e eliminar **todas as condicionais desse exemplo**.

### Solução

No livro “**Design Patterns: Elements of Reusable Object-Oriented Software**”, a intenção declarada do padrão é:

> Permite a um objeto alterar seu comportamento quando o seu estado interno muda. Oobjeto parecerá ter mudado sua classe.

Esse padrão vai **alterar o comportamento de um objeto** quando houver alguma **mudança no seu estado interno** \(atributo `estadoAtual` no exemplo anterior\), como se ele tivesse mudado de classe.

Para implementar a idéia do padrão será necessário criar uma interface básica para todos os estados, contendo os métodos que **podem alterar o estado** do personagem. Como definimos anteriormente o que pode causar alteração nos estados do objeto Mario, estas serão as operações básicas que vão fazer parte da interface. O que era o `enum` **`MarioState`** antes, agora vai ser refatorado para uma interface.

```java
public interface MarioState {

	  MarioState levarDano();
	  MarioState pegarCogumelo();
	  MarioState pegarFlor();
	  MarioState pegarPena();

}
```

Os retornos dos métodos devem ser o mesmo tipo da interface, o porquê disso ficará mais óbvio a frente, mas por hora entenda que ao executar um dos métodos acima, devemos **informar** qual é o novo estado. Vamos implementar um dos exemplos a partir dessa interface:

```java
public class MarioPequeno implements MarioState {
 
    @Override
    public MarioState pegarCogumelo() {
        System.out.println("Mario grande");
        return new MarioGrande();
    }
 
    @Override
    public MarioState pegarFlor() {
        System.out.println("Mario grande com fogo");
        return new MarioFogo();
    }
 
    @Override
    public MarioState pegarPena() {
        System.out.println("Mario grande com capa");
        return new MarioPena();
    }
 
    @Override
    public MarioState levarDano() {
        System.out.println("Mario morto");
        return System.exit(0);
    }
 
}
```

Nesse novo formato, **os estados ficam encapsulados em objetos distintos** e cada classe se preocupa apenas com as funcionalidades que serão executadas quando Mario estiver naquele determinado estado. Vejamos um exemplo da classe que vai usar os objetos de estado

```java
public class Mario {

    private MarioState estadoAtual;

    public Mario() {
        this.estadoAtual = new MarioPequeno();
    }

    public Mario(MarioState estadoAtual) {
        this.estadoAtual = estadoAtual;
    }

    public void levarDano() {
        this.estadoAtual = estadoAtual.levarDano();
    }

    public void pegarCogumelo() {
        this.estadoAtual = estadoAtual.pegarCogumelo();
    }

    public void pegarFlor() {
        this.estadoAtual = estadoAtual.pegarFlor();
    }

    public void pegarPena() {
        this.estadoAtual = estadoAtual.pegarPena();
    }

}
```

Cada novo estado que aparecer também não vai precisar alterar a classe que **usa** os estados, apenas implementar a interface e retornar os estados em que ela pode transitar. A classe `Mario` possui uma referência para um **objeto estado** \(`estadoAtual`\), este estado vai ser atualizado de acordo com as operações de troca de estados, definidas logo em seguida. **Quando uma operação for invocada, o objeto estado vai executar a operação e se atualizará automaticamente**.

Dessa maneira, uma execução como a mostrada abaixo dá a impressão de que o objeto mudou de estado interno, deixando transparente como isso acontece.

```java
Mario mario = new Mario();
mario.pegarCogumelo();
mario.pegarPena();
mario.levarDano();
mario.pegarFlor();
mario.pegarFlor();
mario.levarDano();
mario.levarDano();
mario.pegarPena();
mario.levarDano();
mario.levarDano();
mario.levarDano();
```

Preste atenção no diagrama de classes abaixo, mostrando de maneira visual a organização desse padrão. De uma maneira geral, o que ele faz é usar **composição** para que a responsabilidade de cada uma das ações seja extraída do `Contexto` e **delegada** a objetos de `Estado`, que são implementados nos `EstadosConcretos`, que podem surgir a medida que o projeto precisa crescer.

![](https://lh6.googleusercontent.com/qxeh5x--2RSYb9_Iy73TJa0-ocBvoQnF1PPCSPx3zc3NvLUtXe0rgv6OFpWu4MIMeme7MzPKBI3Avq3nBOaK3d52hvwvA0QG9MtIX99CNNn92jq4wF7jJCFHisZ5nICXphWOz5qzISw)

### **Exemplo completo de código**

O exemplo mostrado aqui pode ser encontrado [**nesse repositório**](https://github.com/ads-ifpb-padroes/exemplo-state). Os estados não estão implementados em classes separadas, mas sim em um `enum` a parte, explore o uso desse recurso da linguagem Java e pondere sobre suas vantagens e desvantagens. Além disso, verifique se isso também é possível em outras linguagens orientadas a objeto, a medida que desenvolve o artigo.

### Referências

O exemplo de problema e solução foi extraído do site do [Marcos Brizeno](https://brizeno.wordpress.com/category/padroes-de-projeto/state/). Algumas adaptações foram feitas para facilitar a explicação pro nosso contexto.

A ilustração no começo dessa página é **propriedade** da **Nintendo**

