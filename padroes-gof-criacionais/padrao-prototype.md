# Padrão Prototype

O Padrão **Prototype** é um padrão de projeto criacional que permite **copiar objetos existentes sem fazer seu código ficar dependente de suas classes**.

### Problema

Digamos que você tenha um objeto, e você **quer criar uma cópia exata dele**. Como você o faria? Primeiro, você tem que **criar um novo objeto da mesma classe**. Então você terá que ir por todos os campos do objeto original e copiar seus valores para o novo objeto.

Legal! Mas tem um problema: nem todos os objetos podem ser copiados dessa forma porque alguns **campos de objeto podem ser privados e não serão visíveis fora do próprio objeto**.

Há ainda mais um problema com a abordagem direta. Uma vez que você precisa **saber a classe do objeto para criar uma cópia**, seu código se torna **dependente** daquela classe. Além da dependência, que é algo extremamente indesejável, algumas vezes você só sabe a interface que o objeto segue, mas não sua classe concreta.

Sendo assim, como criar uma cópia de um objeto de maneira simples e sem criar dependências desnecessárias?

### Solução

O padrão Prototype delega **o processo de clonagem para o próprio objeto que está sendo clonado**. O padrão declara um interface comum para todos os objetos que suportam clonagem. Essa interface permite que você clone um objeto sem acoplar seu código à classe daquele objeto. Geralmente, tal interface contém apenas um único método `clonar()`. Veja o exemplo abaixo.

```java
public class Pessoa {
	
	private Long altura;
	private Long peso;
	private String nome;
	
	public Pessoa() {
		
	}

	// Gets e sets

	@Override
	public Pessoa clonar() {
		Pessoa pClone = new Pessoa();
		pClone.setAltura(this.altura);
		pClone.setPeso(this.peso);
		pClone.setNome(this.nome);
		return pClone;
	}

}
```

A implementação do método `clonar` é muito parecida em todas as classes. O método **cria um objeto da classe atual e carrega todos os valores de campo para do antigo objeto para o novo**. Você pode até mesmo copiar campos privados porque a maioria das linguagens de programação permite objetos acessar campos privados de outros objetos que pertençam a mesma classe.

Também podemos refatorar esse método para usar um construtor especializado para clonagem.

```java
public class Pessoa {
	
	private Long altura;
	private Long peso;
	private String nome;
	
	public Pessoa() {
		
	}
	
	private Pessoa(Pessoa objetoOrigem) {
		this.altura = objetoOrigem.getAltura();
		this.peso = objetoOrigem.getPeso();
		this.nome = objetoOrigem.getNome();
	}

	// Gets e sets

	@Override
	public Pessoa clonar() {
		return new Pessoa(this);
	}

}
```

Um objeto que suporta clonagem é chamado de um _**protótipo**_. Quando seus objetos têm dúzias de campos e centenas de possíveis configurações, cloná-los pode servir como uma alternativa à subclasses. Você cria um ou mais objetos, configurados de diversas formas. Quando você precisa um objeto parecido com o que você configurou, você **apenas clona um protótipo** ao invés de construir um novo objeto a partir do nada.

### Exemplo de Prototype nas APIs nativas do Java

O padrão Prototype está disponível e pronto para uso em Java com a interface `Cloneable`. Qualquer classe pode implementar essa interface para se tornar **clonável**.





