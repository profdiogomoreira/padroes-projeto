# Padrão State

O padrão **State** é intimamente relacionado com o conceito de uma [Máquina de Estado Finito](https://pt.wikipedia.org/wiki/M%C3%A1quina_de_estados_finita), ou se você estudou **Diagrama de Estados** em UML, o conceito é bem semelhante.

A ideia principal é que, em **qualquer dado momento**, há um número **finito** de **estados** que um programa possa estar. Dentro de qualquer estado único, o programa se **comporta de forma diferente**, e o programa pode ser trocado de um estado para outro instantaneamente. Contudo, **dependendo do estado atual, o programa pode ou não trocar para um determinado estado**. Essas regras de troca, chamadas **transições**, também são finitas e pré determinadas.

Você também pode aplicar essa abordagem para objetos. Imagine que nós temos uma classe `Documento`. Um documento pode estar em um de três estados: `Rascunho`, `Moderação` e `Publicado`. O método `publicar` do documento funciona um pouco diferente em cada estado:

* No `Rascunho`, ele move o documento para a moderação.
* Na `Moderação` ele torna o documento público, mas apenas se o usuário atual é um administrador.
* No `Publicado` ele não faz nada.

![Poss&#xED;veis estados de um objeto documento](https://refactoring.guru/images/patterns/diagrams/state/problem2-pt-br.png)

Possíveis estados e transições de um objeto documento.

Máquinas de estado são geralmente implementadas com muitos operadores de condicionais \(`if` ou `switch`\) que selecionam o comportamento apropriado dependendo do estado atual do objeto. Geralmente esse “estado” é apenas um conjunto de valores dos campos do objeto. Mesmo se você nunca ouviu falar sobre máquinas de estado finito antes, você provavelmente já implementou um estado ao menos uma vez. A seguinte estrutura de código lembra alguma coisa para você?

```text
class Document is
    field state: string
    // ...
    method publish() is
        switch (state)
            "draft":
                state = "moderation"
                break
            "moderation":
                if (currentUser.role == 'admin')
                    state = "published"
                break
            "published":
                // Não fazer nada.
                break
    // ...
```

A maior fraqueza de uma máquina de estados baseada em condicionais se revela quando começamos a adicionar mais e mais estados e comportamentos baseados em estados para a classe `Documento`. A maioria dos métodos irá conter condicionais monstruosas que selecionam o comportamento apropriado de um método de acordo com o estado atual. Um código como esse é muito difícil de se fazer manutenção porque qualquer mudança na lógica de transição pode necessitar de mudanças de condicionais de estado em todos os métodos.

O problema tende a ficar maior a medida que o projeto evolui. É muito difícil prever todos os possíveis estados e transições no estágio inicial de projeto. Portanto, uma máquina de estados enxuta, construída com um número limitado de condicionais pode se tornar uma massa inchada e disforme com o tempo.

