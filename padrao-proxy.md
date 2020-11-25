# Padrão Proxy

O Padrão de Projeto Proxy possui **três principais finalidades**, sendo elas:

* Prover um **substituto para um outro objeto** controlar seu acesso.
* Usar um **nível extra de indireção para fornecer acesso distribuído, controlado ou inteligente**.
* Adicionar um **agregador e delegador para proteger o componente real de complexidade indevida**.

### Problema

Imagine uma situação onde temos uma conexão com bancos de dados por meio de um componente `DAO` e as operações providas por esse DAO só podem ser realizadas por um grupo específico de usuários.

```java
public class DAOPessoaImpl implements DAOPessoa {

    public void salvar(Pessoa p) { ... }
    public void excluir(Pessoa p) { ... }
    public Pessoa consultar(Long pessoaId) { ... }
    public List<Pessoa> listar() { ...}

}
```

Como podemos fazer essa verificação? Uma vez que adicionar essa regra no próprio componente faria com que estivessemos quebrando o [Princípio de Responsabilidade Única](principios-solid/principios-solid.md).

### Solução

O Padrão Proxy tem como objetivo "**fornecer um substituto ou marcador da localização de outro objeto para controlar o acesso a esse objeto**". Se esse objeto Proxy visa ser um substituto, devemos ter a mesma interface do objeto que se pretende substituir, ou seja, `DAOPessoa` no nosso exemplo.

```java
public class DAOPessoaProxy implements DAOPessoa {

    private DAOPessoa daoPessoaBase;
    
    public DAOPessoaProxy(DAOPessoa dao) {
        this.daoPessoaBase = dao;
    }

    public void salvar(Pessoa p) {
        // Verificar se é permitido realizar essa operação
        this.daoPessoaBase.salvar(p);
    }
    
    public void excluir(Pessoa p) {
        // Verificar se é permitido realizar essa operação
        this.daoPessoaBase.excluir(p);
    }
    
    public Pessoa consultar(Long pessoaId) {
        // Verificar se é permitido realizar essa operação
        this.daoPessoaBase.consultar(pessoaId);
    }
    
    public List<Pessoa> listar() {
        // Verificar se é permitido realizar essa operação
        this.daoPessoaBase.listar();
    }

}
```

Veja que temos os mesmos métodos, mas ao invés de implementá-los como um `DAO` padrão, fazemos apenas as verificações necessárias antes de executar de fato o método do objeto que estamos envolvendo \(`daoPessoaBase`\). Dessa maneira, esse Proxy pode envolver e controlar o acesso de qualquer classe concreta que venha a partir da interface `DAOPessoa`.

### Um pouco mais sobre Proxy

Embora esse exemplo acima seja extremamente simples, apenas pra fins didáticos, é preciso comentar um pouco sobre os **tipos de proxy** que podem ser utilizados:

* **Protection Proxy**: esse é o tipo de proxy que utilizamos no exemplo. Eles controlam o acesso aos objetos, por exemplo, verificando se quem chama possui a devida permissão.
* **Virtual Proxy**: mantem informações sobre o objeto real, adiando o acesso/criação do objeto em si. 
* **Remote Proxy**: fornece um representante local para um objeto em outro espaço de endereçamento. 
* **Smart Reference:** este proxy é apenas um susbtituto simples para executar ações adicionais quando o objeto é acessado, por exemplo para implementar mecanismos de sincronização de acesso ao objeto original.

Cada proxy implicaria em um design diferente. A principal vantagem de utilizar o Proxy é que, ao utilizar um substituto, podemos fazer desde operações otimizadas até proteção do acesso ao objeto. No entanto isto também pode ser visto como um problema, pois, como a responsabilidade de um proxy não é bem definida é necessário conhecer bem seu comportamento para decidir quando utilizá-lo ou não.



