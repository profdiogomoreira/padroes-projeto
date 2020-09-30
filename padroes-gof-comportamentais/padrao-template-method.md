# Padrão Template Method

O **Padrão Template Method** é um padrão de projeto comportamental que define o esqueleto de um algoritmo na superclasse mas deixa as subclasses sobrescreverem etapas específicas do algoritmo sem modificar sua estrutura.

### Problema

Imagine que sua equipe de desenvolvimento quer construir um **gerador de documentos**. Esse gerador de documentos toma como base um _**template**_ \(modelo\) e define seções específicas como cabeçalho e corpo, como no modelo abaixo.

![](https://lh4.googleusercontent.com/6j4UEiG36xBxK3FjezKnKctJuaiDrKoqICK4rSl9tYf27GNPBO9GL3UfqkuCRd7Js1Y3uqr7THDZRcj90kpOeF3wuelPbdDz4VNHjoK9mR2PeXYO4lAOGkpVsiRJH6huQ5_puJ2Ecio)

Se imaginarmos as instâncias \(como o de Experiência Química acima\) de relatório como classes que buscam reaproveitar código do template, para que implementem apenas os detalhes necessários para o seu relatório, como poderiamos modelar essas classes?

#### Hook methods

Antes de partirmos para a solução, é interessante entender o conceito de _Hook Method_ \(Método Gancho\). **Hook methods** são **métodos que permitem extensão**. A superclasse possui um método principal público que é invocado pelos seus clientes \(qualquer classe que queira utilizá-lo\). Esse método **delega parte de sua execução para o hook method**, que é um método abstrato que deve ser implementado pela subclasse.

![](https://lh4.googleusercontent.com/H6905STMGSKqrCRIVD6gjKCTlmWHVoq3wtxFDZ30azne6MVM5FXZh9dkf0uPdoKX7_Iz-HbxYMUYKTPA9QEb07tpXTlUvvSbhA7q31LtoGc-XFAk7RA379dOxQqP3kqi2dGaaa7meNQ)

Essa técnica e muito utilizada em **frameworks** para permitir que as aplicações possam **especializar seu comportamento** para seus requisitos A aplicação deve estender essa classe e implementar o hook method de forma a **inserir o comportamento especifico de seu domínio**.

{% hint style="info" %}
**Hook method** é uma técnica, diferentemente dos padrões que já vimos até agora. Encare ela como um recurso que a linguagem provê.
{% endhint %}

### Solução

Como dito anteriormente, o Padrão **Template Method** “Define um esqueleto de um algoritmo, delegando alguns passos para suas subclasses.” Um **modelo de documento** define algumas **partes que são fixas e outras partes que devem ser introduzidas quando o documento propriamente dito for criado**. Assim como a intenção do Template Method.

No exemplo abaixo, buscando simplificar, temos uma classe "Template" que define um método `imprimir` que seria comum a todos as classes de relatório, implementa **parte do algoritmo comum a todos** \(nesse caso o cabeçalho e rodapé\) e deixa que as partes variáveis sejam definidas por quem for implementar os métodos `definirIntroducao` e `definirCorpo`, nesse caso qualquer classe que venha a estender `DocumentoTemplate`.

```java
public abstract class DocumentoTemplate {

	public void imprimir() {
		StringBuffer buffer = new StringBuffer();

		// Cabeçalho
		buffer.append("INSTITUTO FEDERAL DA PARAÍBA");
		
		// Introdução
		buffer.append("INTRODUÇÃO");
		buffer.append(definirIntroducao());

		// Corpo do documento
		buffer.append(definirCorpo());

		// Rodapé
		buffer.append(LocalDateTime.now().getYear());
		System.out.println(buffer.toString());
	}

	protected abstract String definirIntroducao();

	protected abstract String definirCorpo();

}
```

**Template Method** é o principal padrão que utiliza hook methods como técnica. É aplicável quando desejamos definir um **algoritmo geral, com N passos e que podem variar**.

O diagrama UML a seguir demonstra a estrutura do padrão.

![](https://lh3.googleusercontent.com/vPqpfq5nuT73r6XiRsbXs9lz0q6kHEnJoCiH8CNs4wkrJA62U_KhaIE5AWxXv_SGEf0OoU4Bm69_VkZ7TyXvljMoJIaHqWMaxzsMZT-2x1peNSzMZB3Xs4tb93s7ia0tWrp1PSoBrrg)

Com o Template Method podemos **reaproveitar o código relativo à parte comum de um algoritmo**, permitindo que cada passo variável possa ser **definido na subclasse**.

Porém, lembre-se que a herança traz alguns problemas de acoplamento.

{% hint style="warning" %}
**Para pesquisar:** Com o recurso de **Default Method**, introduzido no Java 8, seria possível implementar esse padrão sem que fosse necessário utilizar herança?
{% endhint %}

