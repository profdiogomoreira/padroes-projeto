# Padrão Null Object

{% hint style="info" %}
O **Padrão Null Object** não faz parte do catálogo Gang of Four, mas se encaixa nas definições dos padrões comportamentais, por isso está nesse módulo da disciplina.
{% endhint %}

`null`, `nil` ou `nothing`, dependendo da linguagem em uso, pode ser visto como um **valor que indica dados ausentes ou desconhecidos em um campo**. Exemplo: Se um campo “**Telefone**” de uma tabela “**Pessoa**” é `null`, pode-se dizer que a pessoa **não tem telefone ou este é desconhecido**.

Podemos usar valores null em expressões e inserir valores null em campos dos quais as informações são desconhecidas. É importante notar que: **null é diferente de zero** \(0\); é diferente de uma **string vazia** \(“”\). Enfim, **null** é alguma coisa **indefinida**.

### Problema

Mais comumente em **Java**, uma exceção `NullPointerException` é lançada quando tentamos acessar uma referência **nula**. Isso faz com que em vários momentos durante o desenvolvimento do nosso projeto, precisamos utilizar uma **checagem para saber se o objeto é nulo**

```java
if(objeto != null)
    objeto.metodo();
```

Só que a **repetição** desse código **não é desejada**. Como podemos evitar a replicação de código para verificar a existência de null?

Dado que uma referência de objeto pode ser **opcionalmente nula**, e que o resultado de uma verificação nula é **não fazer nada ou usar algum valor padrão**, como isso pode ser tratado de forma transparente?

### Solução

Na nossa solução, uma classe deseja **tratar um objeto que não faz nada da mesma forma que trata aquele que realmente fornece comportamento**. Em outras palavras, teremos **objetos que representam um nulo** \(sem comportamento\) e eles devem ser vistos da mesma forma dos objetos que provêm comportamentos.

O padrão **Null Object** simplifica o uso de dependências que **podem não estar definidas**, possuindo assim um valor **null**. Ao invés de referências nulas, o padrão utiliza uma **classe concreta que implementa uma interface conhecida mas que retorna um valor vazio \(ou esperado\) ou ainda, um comportamento fixo, ao invés de null**. O exemplo anterior ficaria como mostrado abaixo, uma vez que independente de qual objeto estejamos usando, ele nunca vai trazer efeitos colaterais, o que retira a necessidade de checagem de nulo.

```java
objeto.metodo();
```

Observe o exemplo abaixo. Os dois tipos \(com implementação e nulo\) são **compatíveis** entre si já que atendem o mesmo contrato \(`DependencyBase`\), implementam as mesmas interfaces / estendem o mesmo tipo.

![](https://lh5.googleusercontent.com/2iEAzDK1FTN-I69mG6uw7vMDnh_znLSEruFPA9wRHY0rvdc1SgZdP2_4FMpZ7NLS9jNl9cG9Rwr_zo7nY_L5eNkRZPpDO24U32i8cgiLYP6o_6v2hUGf20zsorZYYCZHl9HbZv4_HC8)

Onde precisarmos de uma **referência nula** de `Dependency` para transmitir a ausência de uma informação, usamos um objeto que implementa uma interface esperada, mas cujo método está vazio, ou com uma implementação esperada \(`NullObject`\). A vantagem deste método em relação à implementação padrão é que **um objeto nulo é previsível e não tem efeitos colaterais**, pois ele **não faz nada que já não seja esperado**.

{% hint style="warning" %}
**Para pesquisar:** Com o recurso de **Optional**, introduzido no Java 8, seria possível implementar/usar esse padrão de outra forma?
{% endhint %}



