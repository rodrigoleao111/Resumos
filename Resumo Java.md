# Resumo Java

[TOC]



## Declaração de Classe, Método e Modificadores de Acesso

```java
//Classe pública
public class App{		//PRIMEIRA LETRA MAIÚSCULA
    public static void main(String[] args) throws Exception{
        System.out.println("Hello World.");		//TODO COMANDO TERMINA COM ;
    }
}

//Outro exemplo de classe
public class Pedido{		// O termo "public" informa que a classe é pública
    //Declaração de atributos
    private String produto;		//O termo "private" indica que o atributo é privado
    private float valorTotalDoPedido; // Esse atributo é visivel apenas dentro desta classe
    protected int numeroDaComanda;	// "protected" implica que o atributo é visto apenas no pacote
    
    // Declaração de métodos
    // Método construtor - Definir como uma classe será instanciada (construida)
    public Pedido(String produto, float valorTotalDoPedido, int numeroDaComanda){
        this.produto = produto;
        this.valorTotalDoPedido = valorTotalDoPedido;
        this.numeroDaComanda = numeroDaComanda;
    }
    //Definem comportamentos e ações da classe
    public String adicionaNaComanda(){        
    }
}
```



## Estruturas condicionais

### If - else

```java
int valor = 80;
if(valor < 100){
    valor += 20;
} else {
    valor += 50;
}
```

### Switch-case

```java
int valor;
switch (valor){
    case 1:
        System.out.println("Deu 1");
    case 2:
        System.out.println("Deu 2");
    defaut:
        System.out.println("nenhuma das condições");
}
```



## Estruturas de Repetição

### While

```java
while(condicaoVerdadeira){
    //código
}
```

### Do-while

```java
//dessa forma é garantido que o bloco de código será executado ao menos uma vez
do {
    //código
} while(condicaoVerdadeira)
```

### For

```java
//for(inicialização, condição, incremento)
for(int i=0; i<10; i++){
    //código
}

//ENHANCED FOR
public String[] items; // declaração da lista items
for(String i : items){	// i assume o valor contido dentro da lista items.
    System.out.println(i)	//O laço percorre toda a lista items
}
```



## Javadoc

### Tags

- @author - especifica o autor da classe ou método
- @deprecated - classe ou método obsoleto
- @link - definição de um link para documento local ou remoto (URL)
- @param - descreve um parâmetro que será passado a um método
- @return - descreve qual o tipo de retorno de um método
- @see - associa a outras classes ou métodos
- @since - descreve desde quando uma classe ou método foi adicionado
- @throws - descreve os tipos de excessões que podem ser lançadas por um método
- @version - descreve a versão da classe ou método

```java
/**		(começar com dois asterísticos)
* @author Seu Nome Aqui
* @version 1.0.0
* @todo Criar método para....
*/
```



## Programaçaõ Orientada a Objetos

### Herança

```java
public class Pai {		//CLASSE PAI
	//atributos:
    public String nomePai;
    //construtor:
    public Pai(String nomePai){
        this.nomePai=nomePai;
    }
    //método:
    public void nome(){
        System.out.println("O nome do pai é '" + nomePai + "'.");
    }
}

public class Filha extends Pai {	//CLASSE FILHA - absorve os atributos e métodos do pai
	//atributo específico da filha
    private String nomeFilha;
    //construtor da filha
    public Filha(String nomeFilha, String nomePai){
        super(nomePai);		//precisa deixar de forma explicita no construtor, utilizando "super"
        this.nomeFilha = nomeFilha;
    }
    //métodos da filha
    @Override
    public void nome(){
        System.out.println("O nome da filha é '" + this.nomeFilha + "', e do pai '"+nomePai+"'.");
    }
}
```

### Polimorfismo

