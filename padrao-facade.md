# Padrão Facade

Antes de falar do Padrão Facade, vamos falar um pouco sobre **arquitetura de software**. Especialmente sobre **Camadas físicas** e **lógicas**.

**Camada física** se refere a parte estrutural dos componentes com foco na infraestrutura \(hardware, sistema operacional e serviços/servidores\) e **suas responsabilidades no sistema**. Já a **Camada lógica** se refere a organização dos componentes do sistema ou aplicação, os módulos, pacotes, etc.

Uma boa prática que se prega no projeto da arquitetura de software é que cada **camada lógica** só deve interagir com a\(s\) camada\(s\) imediatamente adjacente\(s\), visando a maior separação de atribuições \(coesão\) e o baixo acoplamento.

### Problema

Imagine o uso de um **subsistema de compras**, onde o uso de várias classes são necessárias para realizar uma compra. Se a classe **Cliente** depende de várias classes deste subsistema \(como na figura abaixo\), significa que ele sabe muitos detalhes, e consequentemente, **está acoplado**.

**Como diminuir o acoplamento entre estas camadas**?

![](https://lh5.googleusercontent.com/-Ggve1Y7XNn0pGzaNlG0oHigJvW6-cknoDNoHtR-z4DAAy3F6p25VeifozqUNoniRlFvfO1f1PwU0dfzyTcZ3g3sAKUJDYtjFxadte0cj7RCkG5G63A_EI_oSAH9sZXK3EN8llYvWsg)

### Solução

A intenção do padrão:

> "Fornecer uma interface unificada para um conjunto de interfaces em um subsistema. Facade d**efine uma interface de nível mais alto que torna o subsistema mais fácil de ser usado**."

Pela intenção é possível notar que o padrão pode ajudar bastante na resolução do nosso problema. O conjunto de interfaces seria exatamente o conjunto de subsistemas. Um subsistema é análogo a uma classe, **uma classe encapsula estados e operações, enquanto um subsistema encapsula classes**.

Nesse sentido o Facade vai **definir operações a serem realizadas com estes subsistemas**. Assim, é possível definir uma **operação padrão** para o subsistema de compras, evitando a necessidade de chamar os métodos separadamente. Nesse cenário, os **detalhes** do subsistema ficam isolados, podendo ser trocados a qualquer momento.

![](https://lh6.googleusercontent.com/MRSsj_ALM4QGYvDRhB_h2cczP6ceDVCdRaBQ2G-gIQR8cxeiGnUqFHSVCaTpI_qhlBNd_Gy8GiLcvpxvsFR_9gJO0Yl5y3UnBqgsBhZHxrWn0RnAxtqUAEPhyBZ0f82NpQW-lmERptY)

O Facade **desacopla o sistema**, favorecendo a separação de responsabilidades. Um sistema bem projetado tem que ser um sistema desacoplado, com suas partes independentes uma das outras. Um sistema com fraco acoplamento possui uma **série de benefícios.**

