# Padrão Builder

Antes de começar a entender o padrão Builder, é interessante que veja o assunto de [Imutabilidade](../orientacao-a-objetos/conceitos-basicos-de-orientacao-a-objetos/imutabilidade.md).

O **Builder** é um padrão de projeto criacional que permite a você construir objetos complexos **passo a passo**. Ele permite que você produza **diferentes tipos e representações de um objeto usando o mesmo código de construção**.

### Problema

Imagine um objeto complexo que necessite de uma inicialização passo a passo trabalhosa de muitos campos e objetos agrupados. Tal código de inicialização fica geralmente enterrado dentro de um construtor monstruoso com vários parâmetros. Ou pior: **espalhado por todo o código cliente**.

Considere uma classe `Usuário` que tenha vários atributos, sendo uns **obrigatórios**, outros **opcionais**. Além disso, queremos criar um objeto [imutável](../orientacao-a-objetos/conceitos-basicos-de-orientacao-a-objetos/imutabilidade.md), ou seja, todos os seus atributos devem ser marcados como **`final`**.

```java
public class Usuario {

    private final String nome;
    private final String sobrenome;
    private final Integer idade;
    private final String telefone;
    private final String endereco;

    // Gets e sets
    
}
```

Uma vez que usamos construtores para criar objetos dessa classe, como poderiamos trazer essa abordagem de atributos obrigatórios e opcionais? Pare e pense por um momento e verá que criar **vários construtores provavelmente não vai resolver esse cenário**.

### Solução

O padrão Builder sugere que você extraia o código de construção do objeto para fora de sua própria classe e mova ele para objetos separados chamados _builders_. “**Builder**” significa “**construtor**”, mas não usaremos essa palavra para evitar confusão com os construtores de classe.

O padrão organiza a construção de objetos em uma série de etapas \(`comNome`, `comSobrenome`, etc.\). Para criar um objeto você executa uma série de etapas em um objeto **builder**. A parte importante é que você não precisa chamar todas as etapas. Você chama apenas aquelas etapas que **são necessárias para a produção de uma configuração específica de um objeto**, no nosso caso, os atributos que são obrigatórios. Observe o exemplo abaixo:

```java
public class UsuarioBuilder {

    private String nome;
    private String sobrenome;
    private Integer idade;
    private String telefone;
    private String endereco;

    public UsuarioBuilder() {
    }

    public UsuarioBuilder comNome(String nome) {
        this.nome = nome;
        return this;
    }

    public UsuarioBuilder comSobrenome(String sobrenome) {
        this.sobrenome = sobrenome;
        return this;
    }

    public UsuarioBuilder comIdade(Integer idade) {
        this.idade = idade;
        return this;
    }

    public UsuarioBuilder comTelefone(String telefone) {
        this.telefone = telefone;
        return this;
    }

    public UsuarioBuilder comEndereco(String endereco) {
        this.endereco = endereco;
        return this;
    }

    public Usuario toUsuario() throws UsuarioBuilderException {
        validarUsuario();
        return new Usuario(nome, sobrenome, idade, telefone, endereco);
    }

    private void validarUsuario() throws UsuarioBuilderException {

        if(nome == null || nome.isEmpty()) {
            throw new UsuarioBuilderException();
        }
        if(sobrenome == null || sobrenome.isEmpty()) {
            throw new UsuarioBuilderException();
        }
        if(idade < 18)
            throw new UsuarioBuilderException();
    }
}
```

Ao finalizar o **processo de construção**, o método `toUsuario()` é chamado. Aqui, ainda acrescentamos uma validação para saber se o processo de construção foi bem executado, mas o mesmo poderia ter sido distribuido a medida que os métodos das etapas fossem chamados.

Assim, nosso processo de construção será semelhante ao exemplo abaixo.

```java
UsuarioBuilder builder = new UsuarioBuilder();
builder.comNome("Diogo").comSobrenome("Moreira");
Usuario novoUsuario = builder.toUsuario();
```

O padrão Builder permite que você construa objetos passo a passo, usando apenas aquelas etapas que você realmente precisa. Após implementar o padrão, você não vai mais precisar amontoar dúzias de parâmetros em seus construtores. O exemplo completo está [nesse repositório](https://github.com/ads-ifpb-padroes/exemplo-builder).

{% hint style="info" %}
No exemplo acima, utilizamos uma "técnica" para construir os métodos chamada de **Fluent Interface**. Veja uma explicação [aqui](https://pt.stackoverflow.com/questions/106955/o-que-%C3%A9-fluent-interface).
{% endhint %}

### Exemplos de Builder na API nativa do Java

A classe `StringBuilder` é um exemplo claro do padrão **Builder** sendo usado para construir objetos imutáveis, nesse caso `String`.



