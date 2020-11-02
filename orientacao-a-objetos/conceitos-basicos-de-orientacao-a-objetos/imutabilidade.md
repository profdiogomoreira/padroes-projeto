# Imutabilidade

Objetos imutáveis são aqueles que suas instâncias **não podem ter seu estado alterado** após terem sido criadas e nem mudam durante a aplicação. Exemplos de classes imutáveis se encontram de maneira abrangente nas bibliotecas nativas do Java, como na própria classe `String`:

```java
String nome = "Diogo Moreira ";
System.out.println(nome); // Imprime "Diogo Moreira " com espaço;
nome.trim(); // Esse método "corta" espaços no fim e no começo de uma String
System.out.println(nome); // Imprime "Diogo Moreira " com espaço;
```

O exemplo de código acima parece "não funcionar", mas na verdade, funciona. O que acontece é que `String` é imutável, e nenhum de seus métodos pode alterar o seu estado. Ao invés disso, `trim()` retorna uma nova String a partir das modificações propostas pelo método.

Para que um objeto seja considerado **imutável**, em Java, ela deve seguir as seguintes características:

* Métodos não podem modificar seu estado
* Definida como final
* Atributos devem ser privados e final

Um exemplo de classe imutável em Java:

```java
public final class Aluno {
  private final String nome;
  
  public Aluno(String nome){
    this.nome = nome;
  }
  
  public String getNome() {
    return this.nome;
  }
}
```

Caso sua classe tenha composição com objetos mutáveis, eles devem ter acesso exclusivo pela sua classe, devolvendo [cópias defensivas](http://www.javapractices.com/topic/TopicAction.do?Id=15).





