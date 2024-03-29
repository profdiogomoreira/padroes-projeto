# Padrão Strategy

Ao longo desse semestre, os tópicos serão semelhantes a esse, onde iremos apresentando um **problema** e uma descrição da **solução** por etapas. A medida que o assunto for mudando, o contexto dos problemas também mudam para facilitar o entendimento. Sintam-se a vontade para solicitar novas explicações sempre que necessário

### **Problema**

Considere que você e sua equipe decidem criar uma aplicação de navegação, no estilo do **Google Maps**. Nessa aplicação você busca prover um **planejamento de rotas**, por diferentes meios \(a pé, de carro, de bicicleta, de transporte público, etc.\), como o exemplo mostrado abaixo, onde na parte superior esquerda ele oferece a opção de mudarmos o meio de transporte..

![](../.gitbook/assets/screen-shot-2020-09-09-at-14.42.18.png)

A primeira versão da aplicação podia apenas construir rotas sobre **rodovias**, para quem viaja/transita de carro. Vamos imaginar a construção de uma classe para realizar esse processamento. No método `gerarRota` passamos dois objetos do tipo `Ponto` e ele deverá nos retornar um objeto `Rota`. Os detalhes desses objetos não são importantes pra compreensão do problema.

```java
public class GeradorRota {
    
    public Rota gerarRota(Ponto inicio, Ponto chegada) {
        // Lógica de gerar a rota por rodovias
    }
}
```

Porém, algum tempo depois do lançamento do aplicativo, você percebeu que nem todo mundo dirige em suas férias e que seus usuários estavam insatisfeitos com as opções disponíveis. Então com a próxima atualização você pretende adicionar uma opção de **rotas de caminhada**. Assim sendo, uma condicional foi feita.

```java
public class GeradorRota {
    
    public enum TipoRota {
        RODOVIA, PEDESTRE
    }

    public Rota gerarRota(Ponto inicio, Ponto chegada, TipoRota tipoRota) {
        if (tipoRota == TipoRota.RODOVIA) {
            // Lógica de gerar a rota por rodovias
        } else {
            // Lógica de gerar a rota para pedestre
        }
    }
}
```

 Logo após isso você adicionou outra opção para permitir que as pessoas usem o **transporte público**. Contudo, isso foi apenas o começo. Mais tarde você planejou adicionar um construtor de rotas para **ciclistas**. E mais tarde, outra opção para construir rotas até todas as atrações turísticas de uma cidade.

Embora da perspectiva de negócio a aplicação tenha sido um sucesso, a parte técnica vai nos causar muitos problemas. Cada vez que adicionarmos um **novo algoritmo de roteamento**, a classe principal `GeradorRota` aumentava de tamanho e se tornou algo muito difícil de se manter.

Qualquer mudança a um dos algoritmos, seja uma simples correção de bug ou um pequeno ajuste no valor das ruas, afetava toda a classe, aumentando a chance de inserir um novo bug no código já existente.

{% hint style="info" %}
Até aqui, estamos **mudando** uma classe para adicionar novas funcionalidades, mas não é isso que o [OCP - Princípio de Aberto/Fechado](../principios-solid/ocp-principio-de-aberto-fechado.md) nos diz. A medida que a disciplina for avançando, um bom conhecimento dos princípios já vão nos dar impressões de que **algo está errado** antes mesmo de declararmos isso explicitamente
{% endhint %}

### **Solução**

A solução sugerida pelo padrão **Strategy** nos sugere pegar uma classe que faz algo específico de diversas maneiras diferentes e extrair todos esses algoritmos para classes separadas chamadas **estratégias**.

A classe principal, que aqui chamamos de `GeradorRota` deve ter uma referência para uma dessas estratégias, delegando o trabalho para um objeto estratégia ao invés de executá-lo por conta própria. Já que queremos expandir nossas funcionalidades sempre que possível, vamos criar uma interface chamada `EstrategiaGeradorRota`

```java
public interface EstrategiaGeradorRota {
    Rota gerarRota(Ponto inicio, Ponto chegada);
}
```

E nossa classe `GeradorRota` ficaria como o código abaixo. Muito mais simples, não é?

```java
public class GeradorRota {

    public EstrategiaGeradorRota estrategiaGeradorRota;
    
    // Construtores

    public Rota gerarRota(Ponto inicio, Ponto chegada) {
        return estrategiaGeradorRota.gerarRota(inicio, chegada);
    }
    
    // Gets e sets

}
```

A classe principal **não é responsável por selecionar um algoritmo apropriado para o trabalho**. Ao invés disso, passamos a estratégia desejada. Na verdade, ela também não sabe muito sobre as estratégias concretas \(como calcular pra rodovia ou pra ciclistas, por exemplo\). Ela trabalha com todas elas através de uma **interface genérica** \(`EstrategiaGeradorRota`\), que somente expõe um **único método** para acionar o algoritmo encapsulado dentro da estratégia selecionada. Desta forma, `GeradorRota` se torna independente das estratégias concretas, então você pode **adicionar novos algoritmos ou modificar os existentes sem modificar o código do GeradorRota ou o de outras estratégias**.

![Diagrama de classes do resultado](../.gitbook/assets/strategy.jpg)

Mesmo dando os mesmos argumentos, cada classe de roteamento pode **construir uma rota diferente**, sem que seja necessário realizar a verificação de qual tipo de rota deve ser feito, logo, eliminando todos os `ifs` anteriores. A classe principal pode ter um método para trocar a estratégia ativa de rotas \(um método `set` por exemplo\), então seus clientes, bem como os botões na interface de usuário, podem substituir o comportamento de rotas selecionado por um outro.

{% hint style="warning" %}
**Para pensar:**  
Quais outros princípios do **SOLID** estamos em acordo ao usarmos a solução proposta do Strategy? Revisite o material e tente fazer a conexão do resultado final do nosso código com as premissas de cada um dos princípios
{% endhint %}

### Exemplo de Strategy nas APIs nativas do Java

Um dos exemplos mais claros de aplicação do padrão **Strategy** na API nativa do Java é a interface `Comparable`, usada quando queremos definir regras de como comparar e ordenar objetos em coleções. Ao **realizar** essa interface, temos que implementar o seguinte método.

```java
public interface Comparable {
    int compareTo(T outro);
}
```

Ao implementar o método, devemos realizar nossa lógica de comparação e retornar um dos seguintes valores:

* **-1 ou qualquer int negativo**, caso `this` seja "menor" ou "anterior" ao objeto que estamos recebendo como parâmetro.
* **0**, caso `this` seja igual ou deva estar na mesma posição do objeto que estamos recebendo como parâmetro.
* **1 ou qualquer int positivo**, caso `this` seja "maior" ou "posterior" ao objeto que estamos recebendo como parâmetro.

Na prática, o que `Comparable` faz é prover uma interface para que possamos definir nossa própria **estratégia** de comparação, fazendo com que métodos como `Collections.sort(lista)` não precisem implementar ou prever todos os tipos de comparação possível, mas sim deleguem isso para os objetos que estão sendo comparados

### **Exemplo completo de código**

O exemplo mostrado aqui pode ser encontrado [**nesse repositório**](https://github.com/ads-ifpb-padroes/exemplo-strategy-20201).

### Referências

O exemplo de problema e solução foi extraído [do Refactoring.Guru](https://refactoring.guru/pt-br/design-patterns/strategy). Algumas adaptações foram feitas, como a inclusão do código-fonte.

