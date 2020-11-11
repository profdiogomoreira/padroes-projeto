# Padrão Adapter

A utilização de frameworks e biblioteca de terceiros é muito comum durante o desenvolvimento de qualquer software, devido a **comodidade e facilidade de utilização**.

### Problema

Vamos considerar o seguinte exemplo: é preciso fazer **um sistema que manipule imagens**, para isto será utilizado uma biblioteca que oferece essas funcionalidades. Suponhamos que será necessário ter um método para carregar a imagem de um arquivo e outro para exibir a imagem na tela. Para fins didáticos, considere o exemplo abaixo

```java
public class ManipuladorImagemBibliotecaA {

    public void carregarImagem(String arquivo) {
        System.out.println("Imagem " + arquivo + " carregada.");
    }
 
    public void desenharImagem(int posicaoX, int posicaoY) {
        System.out.println("Image desenhada");
    }

}
```

Porém, existem outras alternativas de bibliotecas que oferecem a mesma funcionalidade, porém com nomes de métodos diferentes.

```java
public class ManipuladorImagemBibliotecaB {

    public void carregarDados(String arquivo) {
        System.out.println("Imagem " + arquivo + " carregada.");
    }
 
    public void exibirImagem(int largura, int altura, int posicaoX,
            int posicaoY) {
        System.out.println("Imagem exibida");
    }

}
```

Independente da biblioteca que escolhermos, teremos um cenário onde **nosso sistema depende da biblioteca**, uma vez que realizamos chamadas de métodos para essa biblioteca.

Como podemos então construir o sistema de maneira que ele fique independente de qual biblioteca será utilizada? De maneira que, caso seja decidido mudar a biblioteca por outra no futuro, nosso sistema não seja afetado?

### Solução

O Adapter atua como um "wrapper", **envolvendo dois objetos**. Sua intenção é:

"**Converter** a **interface de uma classe em outra interface, esperada pelo cliente**. O Adapter permite que **interfaces incompatíveis trabalhem em conjunto** – o que, de outra forma, seria impossível."

Ou seja, dado um conjunto de classes **com mesma responsabilidade, mas interfaces diferentes,** utilizamos o **Adapter** para unificar o acesso a qualquer uma delas. Precisamos então, inicialmente, fornecer uma interface comum para o cliente, oferencendo o comportamento que ele necessita

```java
public interface ManipuladorImagem {

    public void carregarImagem(String arquivo);
    public void desenharImagem(int largura, int altura, 
        int posicaoX, int posicaoY);

}
```

Dessa maneira, toda vez que precisarmos utilizar um **manipulador de imagem**, iremos usar apenas objetos dessa interface. Essas implementações, por sua vez, **estendem** ou **usam** objetos do Manipulador A ou B, delegando e adaptando as chamadas.

```java
public class ManipuladorImagemA extends ManipuladorImagemBibliotecaA 
    implements ManipuladorImagem {

    public void carregarImagem(String arquivo) {
        super.carregarImagem(arquivo);
    }
    
    public void desenharImagem(int largura, int altura, 
        int posicaoX, int posicaoY) {
        super.desenharImagem(posicaoX, posicaoY);
    }

}
```

De posse dos adaptadores, nosso cliente fica então independente de qual biblioteca será utilizada, utilizando apenas a interface comum definida no início.

```java
public static void main(String[] args) {
    ManipuladorImagem imagem = new ManipuladorImagemA();
    imagem.carregarImagem("teste.png");
    imagem.desenharImagem(0, 0, 10, 10);
 
    imagem = new ManipuladorImagemB();
    imagem.carregarImagem("teste.png");
    imagem.desenharImagem(0, 0, 10, 10);
}
```



