# Introdução

Em Engenharia de Software, um padrão de projeto \(do inglês _design pattern_\) é uma **solução geral para um problema que ocorre com frequência dentro de um determinado contexto** no projeto de software. Esses padrões não se tratam de um projeto finalizado que pode ser transcrito diretamente em código fonte, ele é uma **descrição** ou **modelo** de como resolver um problema que pode ser **usado em muitas situações diferentes**.

Padrões de Projeto, em outras palavras, são **melhores práticas formalizadas** que o engenheiro de software pode usar para resolver problemas comuns quando projetar uma aplicação ou sistema. Existem padrões dos tipos mais variados possíveis, para análise, para **projeto** \(que é o caso desses que veremos\), padrões para testes, etc.

Sendo mais específico a nossa disciplina, os **Padrões de projeto orientados a objeto** normalmente mostram **relacionamentos e interações entre classes ou objetos, sem especificar as classes ou objetos da aplicação que estão envolvidas**. Frequentemente estaremos tratando alguns "participantes" de um padrão que precisarão ser compreendidos dentro do contexto do projeto em que você estiver trabalhando.

### Do que consiste um padrão?

A maioria dos padrões são descritos formalmente para que as pessoas possam **reproduzi-los em diferentes contextos**. Aqui estão algumas seções que **geralmente** estão presentes em uma descrição de um padrão:

* **Propósito** do padrão descreve brevemente o **problema** e a **solução**.
* **Motivação**, explicando a fundo o problema e a solução que o padrão torna possível.
* **Estruturas** de classes, que mostram cada parte do padrão e como se relacionam.
* **Exemplos de código** em uma das linguagens de programação populares tornam mais fácil compreender a ideia por trás do padrão. Aqui, usaremos **Java**.

Nessa disciplina veremos os Padrões que conhecemos como **GoF** ou **Gang of Four**. Isso porque eles foram publicados a mais de 20 anos atrás pelos 4 autores **Erich Gamma, Richard Helm, Ralph Johnson,** e **John Vlissides** no livro “**Design Patterns: Elements of Reusable Object-Oriented Software**” \(que tem versão em português\).

![](../.gitbook/assets/image%20%281%29.png)

Poucos livros da área de Engenharia de Software mantém sua relevância ao longo de tantos anos como esse. Porém, alguns de seus conceitos aparentemente são utéis apenas como idéias nos dias de hoje. Ao longo do semestre iremos explorar como os padrões de projeto **moldaram** as linguagens de programação atuais e as decisões de arquitetura de grandes sistemas.

### Categorias

Etc etc

* **Padrões de criação**: Os padrões de criação são aqueles que abstraem e/ou adiam o processo criação dos objetos. Eles ajudam a tornar um sistema independente de como seus objetos são criados, compostos e representados. Um padrão de criação de classe usa a herança para variar a classe que é instanciada, enquanto que um padrão de criação de objeto delegará a instanciação para outro objeto. Os padrões de criação tornam-se importantes à medida que os sistemas evoluem no sentido de dependerem mais da composição de objetos do que a herança de classes. O desenvolvimento baseado na composição de objetos possibilita que os objetos sejam compostos sem a necessidade de expor o seu interior como acontece na herança de classe, o que possibilita a definição do comportamento dinamicamente e a ênfase desloca-se da codificação de maneira rígida de um conjunto fixo de comportamentos, para a definição de um conjunto menor de comportamentos que podem ser compostos em qualquer número para definir comportamentos mais complexos. Há dois temas recorrentes nesses padrões. Primeiro todos encapsulam conhecimento sobre quais classes concretas são usadas pelo sistema. Segundo ocultam o modo como essas classes são criadas e montadas. Tudo que o sistema sabe no geral sobre os objetos é que suas classes são definidas por classes abstratas. Conseqüentemente, os padrões de criação dão muita flexibilidade no que é criado, quem cria, como e quando é criado. Eles permitem configurar um sistema com objetos “produto” que variam amplamente em estrutura e funcionalidade. A configuração pode ser estática \(isto é, especificada em tempo de compilação\) ou dinâmica \(em tempo de execução\).
* **Padrões estruturais**: Os padrões estruturais se preocupam com a forma como classes e objetos são compostos para formar estruturas maiores. Os de classes utilizam a herança para compor interfaces ou implementações, e  os de objeto descrevem maneiras de compor objetos para obter novas funcionalidades. A flexibilidade obtida pela composição de objetos provém da capacidade de mudar a composição em tempo de execução o que não é possível com a composição estática \(herança de classes\).
* **Padrões comportamentais**: Os padrões de comportamento se concentram nos algoritmos e atribuições de responsabilidades entre os objetos. Eles não descrevem apenas padrões de objetos ou de classes, mas também os padrões de comunicação entre os objetos. Os padrões comportamentais de classes utilizam a herança para distribuir o comportamento entre classes, e os padrões de comportamento de objeto utilizam a composição de objetos em contrapartida a herança. Alguns descrevem como grupos de objetos que cooperam para a execução de uma tarefa que não poderia ser executada por um objeto sozinho.

