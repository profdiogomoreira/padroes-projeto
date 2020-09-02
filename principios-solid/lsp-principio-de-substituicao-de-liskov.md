# LSP - Princípio de Substituição de Liskov

[Barbara Liskov](https://pt.wikipedia.org/wiki/Barbara_Liskov) introduziu o **Princípio de substituição de Liskov** como uma definição particular para o conceito de subtipo. O princípio foi definido de [forma resumida](http://reports-archive.adm.cs.cmu.edu/anon/1999/CMU-CS-99-156.ps) como:

_Se_![q\(x\)](https://wikimedia.org/api/rest_v1/media/math/render/svg/c38bbafe34a043d284f19231b946a76c0a4b16b4)_é uma propriedade demonstrável dos objetos_![x](https://wikimedia.org/api/rest_v1/media/math/render/svg/87f9e315fd7e2ba406057a97300593c4802b53e4) _de tipo_ ![T](https://wikimedia.org/api/rest_v1/media/math/render/svg/ec7200acd984a1d3a3d7dc455e262fbe54f7f6e0)_. Então_![{\displaystyle q\(y\)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/46049a30deb0e2a1d751cab6457c5204d7ee82a9) _deve ser verdadeiro para_![y](https://wikimedia.org/api/rest_v1/media/math/render/svg/b8a6208ec717213d4317e666f1ae872e00620a0d) _de tipo_![S](https://wikimedia.org/api/rest_v1/media/math/render/svg/4611d85173cd3b508e67077d4a1252c9c05abca2) _onde_![S](https://wikimedia.org/api/rest_v1/media/math/render/svg/4611d85173cd3b508e67077d4a1252c9c05abca2) _é um subtipo_ ![T](https://wikimedia.org/api/rest_v1/media/math/render/svg/ec7200acd984a1d3a3d7dc455e262fbe54f7f6e0)_._

Falando de outra maneira, a visão de "**subtipo**" defendida por Liskov é baseada na noção da **substituição**; isto é, se **S** é um subtipo de **T**, então os objetos do tipo **T**, em um programa, podem ser substituídos pelos objetos de tipo **S** sem que seja necessário alterar as propriedades deste programa.

Suponha que uma parte do sistema está utilizando uma determinada **funcionalidade**. Se essa funcionalidade precisar ser trocada por outra, por meio de polimorfismo dinâmico ou estático, a outra deverá devolver o mesmo tipo de informação, caso contrário, o sistema quebrará.

Neste contexto, para garantir que a classe **S** tenha o mesmo comportamento que a classe base **T**, é imprescindível a utilização de um contrato \(interface ou classe abstrata\), contendo as definições de métodos necessárias para que as classes que a herdarem sejam obrigadas a implementá-las.

Um exemplo clássico para demonstrar uma violação desse princípio é a modelagem de classes `Quadrado` e `Retangulo`. Todo quadrado **é um** retângulo? **Sim**, por definição. Se **é um**, provavelmente conseguiriamos utilizar herança para modelar essas classes.

 



