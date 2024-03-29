# Padrão Command

O Padrão **Command** encapsula uma solicitação \(chamada de método\) como um objeto, o que lhe permite parametrizar outros objetos com diferentes solicitações, enfileirar ou registrar solicitações e implementar recursos de cancelamento de operações.

A principal motivação do uso do padrão Command é que algumas vezes é necessário emitir solicitações para objetos sem nada saber sobre a operação que está sendo solicitada ou sobre quem será o seu receptor.

### Problema

Imagine que queremos criar um **sistema automatizado** de **casa inteligente** onde seja possível: ligar a TV ao chegar em casa, Ligar o sistema de som em um horário programado, ligar o ar-condicionado ao entrar em um ambiente, etc. Acontece que nesse contexto, uma vez que cada um dos aparelhos vem de uma marca diferente, a **variedade de interfaces** com as quais precisamos nos comunicar é maior.

![Ilustra&#xE7;&#xE3;o de um sistema de casa inteligente](../.gitbook/assets/command.png)

Imagine nosso sistema e cada aparelho como uma classe distinta, apenas para fins didáticos. Cada aparelho tem métodos diferentes, com parâmetros diferentes, o que faria com que nosso sistema criasse um **alto acoplamento** com cada um dos aparelhos. O exemplo de código abaixo facilmente seria implementado nesse cenário.

```java
public class Invocador {
    
    private TV tv;
    private CondicionadorDeAr condicionadorDeAr;
    private SistemaDeSom sistemaDeSom;
    
    public void ligar() {
        if (tv != null) {
            tv.ligarTV();
        }
        if (condicionadorDeAr != null) {
            condicionadorDeAr.ligar(26);
        }
        if (sistemaDeSom != null) {
            sistemaDeSom.ligar(5);
        }
    }
```

Porém, queremos que nosso sistema seja flexível para que possa se comunicar com novos aparelhos e também com aparelhos de outras marcas com **interfaces diferentes**.

Como implementar um sistema com estas características?

* Que evite acoplamento entre o invocador \(**sistema automatizado**\) e o objeto que, de fato, implementa a operação \(**aparelho de TV, som, etc**\).
* Que seja possível **adicionar novos dispositivos** sem que o sistema automatizado crie um **novo acoplamento**

### Solução

Se queremos que nosso sistema seja extensível e suporte outros tipos de dispositivos, temos que enxergar eles da mesma forma, abstraindo os detalhes e passando a lidar apenas com **módulos de alto nível**.

O **Padrão Command** tem como intenção "Encapsular uma **requisição como um objeto**, permitindo que clientes parametrizem diferentes requisições, filas ou requisições de log, e suportar operações reversíveis". Sendo assim, nosso Invocador só precisa se relacionar com esse objeto que representa uma requisição. Vamos ao código

```java
public interface Comando {
	
	void executar();

}
```

Criamos a interface para os objetos de requisição, ou **comandos**, como vamos chamar aqui. A partir daqui, cada objeto vai encapsular uma das chamadas como esta abaixo.

![](../.gitbook/assets/command2.png)

Mas e como iremos saber qual o parâmetro correto pra cada uma das requisições? E se eu quiser trocar a temperatura do condicionador de ar ou o canal que deve ser definido para a TV? Vamos visualizar como esses comandos serão implementados.

```java
public class LigarTV implements Comando { 

	private TV tv;
	private String canal;
	
	public LigarTV(TV tv, String canal) {
		this.tv = tv;
		this.canal = canal;
	}
	
	public void executar() {
		this.tv.ligarTV(this.canal);
	}

}
```

Repare nos parâmetros que recebemos no **construtor**, o objeto do tipo `TV` que será invocado e os **parâmetros necessários** para essa invocação, no caso o `canal`.

Agora podemos refatorar o `Invocador` para que ele não precise mais criar acoplamento com esses dispositivos, e enxergue apenas a interface `Comando`.

```java
public class Invocador {
	
	private List<Comando> comandos = new LinkedList<Comando>();
	
	public void addComando(Comando comando) {
		this.comandos.add(comando);
	}
	
	public void remComando(Comando comando) {
		this.comandos.remove(comando);
	}
	
	public void ligar() {
		for (Comando comando : ligar) {
			comando.executar();
		}
	}
	// ...
}
```

E quando quisermos adicionar novos dispositivos, essa classe não precisará ser alterada para estar pronta para as novas funcionalidades. Criaremos apenas um novo comando, que irá receber o objeto **Receptor** \(TV, Som, etc...\) e seus parâmetros.

```java
public class LigarArCondicionado implements Comando {

    private int temperatura;
    private CondicionadorDeAr condicionador;

    public LigarArCondicionado(CondicionadorDeAr condicionador, int temperatura) {
        this.condicionador = condicionador;
        this.temperatura = temperatura;
    }

    public void executar() {
        condicionador.ligar(this.temperatura);
    }
}
```

Observe o diagrama de classes abaixo. O **`Client`** é qualquer entidade de software que queira utilizar o Invocador \(`Invoker` no diagrama\), que por sua vez só conhece a interface `Command`. Os comandos são de fato implementados nas classes `ConcreteCommand`, no nosso exemplo seriam `LigarArCondicionado` e `LigarTV`. Por fim, o `Receiver` é qualquer classe concreta dos dispositivos, que pode ter interfaces diversas, mas que no final só serão invocadas pelos comandos.

![](https://lh5.googleusercontent.com/Rv8NXQsl8ouHYTTuWKpxB2JfMawiEuuj7GFPT9XG29pNH_ahgPwa2x33QShFksK5wGHbsRxFVfD5lo1Ejhb3IINfnNZwL8e985PcSTGG2PjnByeI3qP1Rxh5gu60J7OUhN8ImSaOles)

### Exemplos de Command nas APIs nativas do Java

Nas APIs nativas do Java, podemos encontrar um exemplo do padrão Command nos componentes que implementam a interface `Runnable`, como é o caso de `TimerTask`. No exemplo abaixo, cada implementação de `TimerTask`, feito por meio de classes anônimas executa um método `run()` que encapsula uma solicitação que deve acontecer de tempos em tempos.

O componente `Timer` por sua vez, agenda esses objetos do tipo `TimerTask` para que sejam executados repetidamente dentro de um espaço de tempo \(linhas 19 e 20\), mas sem saber qual é a operação que de fato vai ser executada, uma vez que conhece apenas a abstração `TimerTask`.

```java
public static void main(String[] args) {
    Timer timer = new Timer();
    TimerTask primeiraTimerTask = new TimerTask() {

        @Override
        public void run() {
            System.out.println("Esse código será executado diversas vezes");
        }

    };

    TimerTask segundaTimerTask = new TimerTask() {
        @Override
        public void run() {
            System.out.println("Essa também, mas com menos frequência");
        }
    };

    timer.schedule(primeiraTimerTask, 0,480);
    timer.schedule(segundaTimerTask, 5, 1000);
}
```

 Referências

GAMMA, Erich et al. Padrões de Projeto: Soluções reutilizáveis de software orientado a objetos.

