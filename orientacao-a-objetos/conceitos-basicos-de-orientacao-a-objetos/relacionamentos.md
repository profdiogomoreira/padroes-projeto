# Relacionamentos

## Associação

É o tipo de relacionamento mais comum e mais importante em um sistema orientado a objetos. É um relacionamento ou ligação entre duas classes permitindo que objetos criados a partir destas possam se comunicar, podendo essa comunicação ser uni ou bidirecional.

As associações podem ser de dois tipos: **Agregação** ou **Composição**.

#### Agregação

É um tipo de associação onde tenta-se demonstrar que as informações de um objeto \(chamado objeto-todo\) precisam ser complementados pelas informações contidas em um ou mais objetos de outra classe \(chamados objetos-parte\).

Os objetos-parte nesse tipo de associação tem fraca dependência para o objeto-todo e **podem existir separadamente**.

## Composição

Pode-se dizer que composição é uma variação da agregação. Uma composição tenta representar também uma relação todo - parte. No entanto, na composição o objeto-todo é responsável por criar e destruir suas partes. Em uma composição um mesmo objeto-parte não pode se associar a mais de um objeto-todo.

Se o objeto-todo for destruído, as classes da agregação de composição serão destruídas juntamente, já que as mesmas fazem parte da outra.

\*\*\*\*

