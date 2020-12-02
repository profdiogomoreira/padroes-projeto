# Padrão Composite

O **Composite** é um padrão de projeto estrutural que permite compor objetos em uma estrutura semelhante a uma árvore e trabalhar com eles como se fosse um objeto singular.

### Problema

Em certas estruturas, necessitamos de **métodos e funções que já estão especializadas em um determinado objeto,** necessitando apenas a ligação entre os dois para reutilizar o código. No nosso problema vamos trabalhar com um **Editor de texto** \(como o Microsoft Word, por exemplo\) que pode misturar **texto** e **gráficos** usando várias opções de formatação \(negrito, itálico, alinhamento centralizado, etc.\)

O primeiro problema de design que queremos atacar é **como representar a estrutura do documento.** Essa estrutura afeta o resto da aplicação já que a edição, formatação, análise textual, etc. deverão acessar a representação do documento.

![Exemplo de representa&#xE7;&#xE3;o do documento](.gitbook/assets/image%20%288%29.png)

Vamos encarar um documento como um **arranjo de elementos gráficos básicos:** caracteres, linhas, polígonos e outras figuras. O usuário normalmente não pensa em termos desses elementos gráficos mas em termos de **estruturas físicas**, tais como linhas, colunas, figuras, tabelas e outras subestruturas. Essas subestruturas, por sua vez, podem ser compostas de outras subestruturas, etc.

### Composição recursiva

Uma forma comum de representar informação estruturada hierarquicamente é através da **Composição Recursiva.** Ela permite construir **elementos complexos a partir de elementos simples** – aqui, a composição recursiva vai permitir **compor um documento a partir de elementos gráficos simples.**

Começamos formando linhas a partir de elementos gráficos simples \(caracteres e gráficos\), múltiplas linhas formam colunas, múltiplas colunas formam páginas, múltiplas páginas formam documentos, etc.

![](.gitbook/assets/image%20%2810%29.png)

O **Padrão Composite** se baseia nessa idéia para compor objetos em uma estrutura semelhante a uma árvore. Imagine os elementos do exemplo acima dispostos da seguinte maneira:

![](.gitbook/assets/image%20%2811%29.png)

### Solução

**Composite** é um padrão de projeto que permite que **um objeto seja constituído de outros objetos semelhantes a ele formando uma hierarquia parte-todo**. Semelhantes, aqui, significam objetos que implementam um contrato comum.

Em resumo, vamos tratar **composições e unidades uniformemente**. Faremos isso por meio da interface `TipoDaHierarquia` no exemplo abaixo. Os elementos que podem ser compostos, como linhas e colunas, são subtipos de `Composto`, a medida que os elementos que não podem ser compostos \(como imagens e caracteres\) são subtipos de `Primitivo`.

![](.gitbook/assets/image%20%2812%29.png)

A ideia desse padrão é montar uma árvore onde tanto as folhas \(primitivos ou **objetos individuais**\) quanto os compostos \(**grupos de objetos**\) sejam tratados de maneira igual. Em termos de orientação a objetos, isso significa aplicarmos **polimorfismo** para chamar métodos de um objeto na árvore **sem nos preocuparmos se ele é uma folha ou um composto**.

### Self Composite e Composite na API nativa do Java

Em algumas situações especiais não existe um objeto primitivo na hierarquia ou a sua interface não se distingue da interface do objeto composto. Neste caso o objeto que representa o tipo de hierarquia é **simultaneamente o objeto composto e o objeto primitivo**.

Em Java, o objeto `File` implementa o padrão **self-composite**. O objeto representa simultaneamente um arquivo e uma pasta de arquivos num sistema de arquivos \(que tem uma estrutura em árvore\).

