# Encapsulamento

Encapsulamento vem de encapsular, que em programação orientada a objetos significa separar o programa em partes, o mais isoladas possível. A ideia é tornar o software mais flexível, fácil de modificar e de criar novas implementações.

O encapsulamento protege o acesso direto aos atributos de uma instância vindo de fora da classe onde estes foram declarados. Esta proteção consiste em se usar modificadores de acesso mais restritivos \(como `private`\) sobre os atributos definidos na classe. Depois devem ser criados métodos para manipular de forma indireta os atributos da classe. É por isso que fazemos `get` e `set`.

Encapsular atributos também auxilia a **garantir que o estado e o comportamento de um objeto se mantenha coeso**. Imagine a situação onde você tem a seguinte classe.

```java
public class Pessoa {

    private List<String> numerosContato;
    
    public List<String> getNumerosContato() {
        return this.numerosContato;
    }
    public void setNumerosContato(List<String> numerosContato) {
        this.numerosContato = numerosContato;
    }
}
```

Esse código demonstra como o encapsulamento é importante. Ao gerarmos um `get` e um `set` \(comuns, como geralmente as IDEs sugerem\) para uma coleção estamos **quebrando a proteção contra acesso direto aos atributos**, ou seja, quebramos o encapsulamento.

{% hint style="warning" %}
## **Problemas ao quebrar o encapsulamento**

Imagine que um componente do sistema realizou uma chamada da seguinte maneira: `getNumerosContato().add("1729817982")`. Estamos **mudando o estado do objeto `Pessoa`** sem que ele tenha **conhecimento** disso, uma vez que a adição não passou por um método do mesmo. 

Indo além, podemos imaginar um cenário onde dois componentes \(`X` e `Y`\) usam o mesmo objeto `Pessoa` e em um **primeiro** momento, `X` solicita `getNumerosContato()` para executar alguma lógica de negócios. Se `Y` realiza uma chamada `setNumerosContato(...)` em um **segundo momento**, alterando completamente a coleção, significa dizer que a **lógica executada por `X` passa a ser inconsistente**.

Vários cenários podem ser explorados quando falamos de quebra de encapsulamento, principalmente quando falamos de [programação concorrente](https://pt.wikipedia.org/wiki/Programa%C3%A7%C3%A3o_concorrente).
{% endhint %}

