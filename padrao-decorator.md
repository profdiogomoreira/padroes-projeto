# Padrão Decorator

O **Padrão Decorator** é um padrão de projeto estrutural que permite **adicionar responsabilidades a um objeto de maneira dinâmica, criando algo similar a uma extensão do objeto**. Geralmente isso é feito através de camadas aninhadas.

Geralmente este padrão é composto por alguns elementos:

1. Definição abstrata de um componente de software;
2. Implementação concreta deste componente;
3. Definição abstrata de um _decorator_ para este componente;
4. Uma ou mais implementações concretas do _decorator_ do componente;

### Problema

Imagine um componente do sistema que calcula um salário de um funcionário. Esse cálculo envolve vários condições, como:

* **Horas trabalhadas**
* Horas extras \(que tem acréscimo de 20% do valor na hora trabalhada\)
* Se existem descontos na folha por plano de saúde/odontológico.

Em um primeiro momento, podemos pensar em várias condicionais para o nosso código, verificando cada um desses aspectos e calculando o valor de acordo. Porém, essas condicionais tendem a crescer e deixar o código cada vez mais complicado de ser entendido. Além disso, alterar essa classe para acrescentar uma nova regra, quebra o [princípio de aberto/fechado](principios-solid/ocp-principio-de-aberto-fechado.md), como já vimos anteriormente.

Então, como podemos acrescentar novas condicionais dinamicamente e a medida que for necessário?

### Solução

A solução proposta pelo **Decorator** passa pela separação de cada uma das responsabilidades "opcionais" em classes que vão **envolver** uma calculadora de salário, adicionando essas responsabilidades por demanda. Começaremos criando uma classe base de Calculadora de salário, em forma de interface. Vamos ao exemplo em código:

```java
public interface CalculadoraSalario {

    double calcularSalario(int horasTrabalhadas);

}
```

De um lado, teremos uma calculadora de salário **base**, que sempre será utilizado, independente das condicionais.

```java
public class CalculadoraSalarioFuncionario implements CalculadoraSalario {

    public double calcularSalario(int horasTrabalhadas) {
        final double valorHora = 15d;
        return valorHora * horasTrabalhadas;
    }
}
```

De outro lado, temos as classes que podem **envolver/decorar** essa `CalculadoraSalarioFuncionario`, adicionando novas funcionalidades. Como queremos permitir que sejam criadas novos decoradores a medida que for necessário, criaremos uma classe abstrata que **tem um** objeto do tipo `CalculadoraSalario`. Isso será a base de nosso **Decorator**.

```java
public abstract class CalculadoraSalarioDecorator implements CalculadoraSalario {

    protected CalculadoraSalario calculadoraSalarioBase;

    public CalculadoraSalarioDecorator(CalculadoraSalario 
        calculadoraSalarioBase) {
        this.calculadoraSalarioBase = calculadoraSalarioBase;
    }
}

```

As classes que implementam esse **Decorator** definem os fragmentos de algoritmos que serão acrescentados a calculadora base. Nesse caso, verificamos se as horas informadas passam das 160 horas normais \(em um **mês**\) e calculamos o excedente com o valor da hora extra.

```java
public class CalculadoraHoraExtraDecorator extends CalculadoraSalarioDecorator {

    public CalculadoraHoraExtraDecorator(CalculadoraSalario 
        calculadoraSalarioBase) {
        super(calculadoraSalarioBase);
    }

    public double calcularSalario(int horasTrabalhadas) {
        double pagamentoHorasExtras = 0d;
        final double valorHoraExtra = 18d;
        if (horasTrabalhadas > 160) {
            int horasExtras = horasTrabalhadas - 160;
            pagamentoHorasExtras = horasExtras * valorHoraExtra;
            return this.calculadoraSalarioBase.calcularSalario(
                horasTrabalhadas - horasExtras) + pagamentoHorasExtras;
        }
        return this.calculadoraSalarioBase.calcularSalario(horasTrabalhadas);
    }
}
```

 Assim sendo, ao utilizarmos a calculadora base com o nosso decorador, fica abstraído para quem usa os detalhes de **quantos decoradores** existem, uma vez que só iremos utilizar a abstração de `CalculadoraSalario`.

```java
CalculadoraSalario calculadoraSalario = new CalculadoraSalarioFuncionario();
calculadoraSalario = new CalculadoraHoraExtraDecorator(calculadoraSalario);
double salarioARecebeer = calculadoraSalario.calcularSalario(170);
System.out.println(salarioARecebeer);
```

 Podemos adicionar quantos decorators forem necessários, a medida que o algoritmo para calcular o salário aumenta e cria mais complexidade.

### Exemplos de Decorator na API nativa do Java

O Java também utiliza bastante o padrão de projeto Decorator. A API `java.io` é amplamente baseada nesse padrão de projeto. Os objetos da API `java.io` que tipicamente usam esse objeto são os bem conhecidos `LineNumberInputStream`, `BufferedInputStream` e `FileInputStream`. Abaixo veja como eles estão decorados:  


![Organiza&#xE7;&#xE3;o t&#xED;pica do padr&#xE3;o decorator para os objetos acima](http://videos.web-03.net/artigos/Higor_Medeiros/PadraoDecorator_Java/PadraoDecorator_Java2.jpg)

Na figura acima temos que `LineNumberInputStream` é um decorador concreto que tem como função contar as linhas de um determinado arquivo. O `BufferInputStream` também é um decorador concreto que tem como objetivo colocar a entrada em buffer para melhorar o desempenho e também oferece o método `readLine()` que lê a entrada baseada em caracteres linha por linha. Por fim `FileInputStream` é quem está sendo decorado e oferece um componente básico através do qual os bytes serão lidos.





