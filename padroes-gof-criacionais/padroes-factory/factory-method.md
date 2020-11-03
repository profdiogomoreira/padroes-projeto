# Factory Method

O **Factory Method** é um padrão criacional de projeto que fornece uma interface para criar objetos em uma superclasse, mas permite que as subclasses alterem o tipo de objetos que serão criados.

O exemplo de código desse padrão é exatamente o que o seu nome sugere: um **método fábrica**, ou seja, que **cria** um objeto. Imagine uma classe abstrata `Dialogo` que será estendida para criar janelas de diálogo para diferentes tipos de sistema operacionais \(Windows, Linux e Mac\).

```java
public abstract class Dialogo {
    public abstract Botao criarBotao();
}
```

Essa classe **declara** que cria um botão no seu método `criarBotao`, mas deixa que suas classes concretas decidam qual instância de `Botao` criar, podendo ser `BotaoWindows` ou `BotaoLinux`, por exemplo.

Isso caracteriza um **Factory Method**.

