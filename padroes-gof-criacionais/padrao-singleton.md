# Padrão Singleton

O padrão Singleton permite criar **objetos únicos** para os quais **há apenas uma instância**. Este padrão oferece um **ponto de acesso global**, assim como uma variável global, porém sem as desvantagens das variáveis globais. Para entendermos e usarmos bem o padrão de Projeto Singleton é necessário apenas dominar bem as **variáveis e métodos de classe estáticos** além dos **modificadores de acesso**.

### Problema

É importante para algumas aplicações ter **uma e somente uma instância de uma determinada classe**, independente do número de requisições que a mesma recebe para criá-lo a partir de seu construtor. Alguns exemplos são:

* Somente um **spooler de impressão** \(fila de impressão dos documentos a serem impressos\)
  * Embora possam existir muitas impressoras num ambiente, um único spooler deve ser instanciado
* Somente um objeto que contém a **configuração de um programa**
  * Garante que as alterações nas configurações sejam realizadas num único ponto centralizado

Considere o exemplo abaixo, onde temos uma suposta classe de configuração que define um nome de usuário. Essa classe pode ser instanciada várias vezes uma vez que seu construtor é **público** \(`public`\).

```java
public class Configuracao {

    private String nomeUsuario;

    public Configuracao(String nomeUsuario) {
        this.nomeUsuario = nomeUsuario;
    }
    
    public String getNomeUsuario() {
        return nomeUsuario;
    }

    public void setNomeUsuario(String nomeUsuario) {
        this.nomeUsuario = nomeUsuario;
    }
}
```

Se esses objetos devem ser construídos para que só exista uma única instância e todas as requisições sejam processadas pelo mesmo objeto, como podemos garantir que os clientes dessa classe não instanciem vários objetos?

### Solução

A solução proposta pelo **Padrão Singleton** passa por tornar a própria classe responsável por manter o controle da sua única instância. A classe pode e deve garantir que nenhuma outra instância seja criada, além de fornecer meios para acessar sua única instância.

Veja o exemplo abaixo. Para que a própria classe mantenha o controle de sua única instância, usa-se o atributo `singleton`, que é do tipo da sua própria classe e é estático, nessa variável tem-se a única instância da classe.

```java
public class Configuracao {

    private String nomeUsuario;

    private static Configuracao singleton;

    private Configuracao(String nomeUsuario) {
        this.nomeUsuario = nomeUsuario;
    }

    public static Configuracao getInstance(String nomeUsuario) {
        if(singleton == null) {
            singleton = new Configuracao(nomeUsuario);
        }
        return singleton;
    }

    public String getNomeUsuario() {
        return nomeUsuario;
    }

    public void setNomeUsuario(String nomeUsuario) {
        this.nomeUsuario = nomeUsuario;
    }
}
```

Nos métodos pode-se observar a presença do construtor da classe `Configuracao(nomeUsuario)` que é **PRIVADO**. Ou seja, um **construtor privado não permite que a classe seja instanciada a não ser que seja feito por ela mesmo** na qual será instanciada pelo método `getInstance()` que é estático e assim pode ser acessado de qualquer outra classe sem precisar instanciar `Configuracao`.

Dessa forma, toda vez que uma classe precisar da instância de `Configuracao`, terá que invocar o método `getInstance`, que retorna a mesma instância sempre.

{% hint style="info" %}
O método `getInstance` se utiliza de uma técnica chamada **Lazy Loading**. Você pode ler mais sobre isso [aqui](https://pt.wikipedia.org/wiki/Lazy_loading).
{% endhint %}

### Referências

{% embed url="https://www.devmedia.com.br/padrao-de-projeto-singleton-em-java/26392" %}

[https://brizeno.wordpress.com/category/padroes-de-projeto/singleton/](https://brizeno.wordpress.com/category/padroes-de-projeto/singleton/)

