# Princípios e conceitos do Kotlin para desenvolvimento Android



[TOC]



## Estrutura da linguagem

- Não utiliza ponto e vírgula
- Não possui operador ternário tradicional
- Type Safe e null Safe
- Inferência de tipos
- Interoperável com Java

## Declaração de variáveis

```kotlin
const val NUM = 15	// Usado para variáveis globais (maiúsculas)
val x = 12	// Val -> valor imutável
var y:Float		// Var -> valor mutável
var day:Int? = null		// Para atribuit null, precisa da '?'
```

## Operadores In e range

- Se valor está presente em uma lista ou uma faixa de valores
- Range cria um intervalo de valores que inicia no primeiro parâmetro e acaba no segundo

```kotlin
val bingo = listOf(8,5,12,20,15)
val number = (1..22).random()

println(number)
println(number in bingo)	// Retorna True ou False


const val MIN_AGE = 16
const val MAX_AGE = 68
var age = (10..100).random()
println(age in MIN_AGE..MAX_AGE)	// Retorna True ou False
```

## Manipulação de Strings

- Concatenaçõ com plus/+
- Tratado como array de Char

```kotlin
val nome = "ana"
val wellcome = "Bem vinda"

println("$wellcome, ${nome.capitalize()}!")
```

![image-20220706075609974](C:\Users\Rodrigo Leão\AppData\Roaming\Typora\typora-user-images\image-20220706075609974.png)

## Empty x Blank: Validação de conteúdo

- Métodos de comparação → retornam um booleano
- String vazia (isEmpty) = tamanho (length) é zero
- Se o tanho for > 0, mas todos os caracteres são espaços em branco → está **blank** mas não **empty**

```kotlin
val empty = ""
val blank = "   "

println(empty.isEmpty() and empty.isBlank())	// true
println(blank.isEmpty())	// false
println(blank.isBlank())	// true
```

## Funções

```kotlin
// [encapsulamento] fun nomeDaFuncao(nomeParametro:Tipo):TipoRetorno{}
private fun getFullName(firstName:String, lastName:String):String{
    var fullname = "$firstName $lastName"
    return fullname
}

// single line function
private fun getFullName(firstName:String, lastName:String) = "$firstName $lastName"
```

### Funções de ordem superior (High Level Functions)

```kotlin
fun main(){
    val z:Int
    
    z = calculate(34, 90){a,b->
               println("vou calcular")
               (a+b)*2
               }
}

fun calculate(n1:Int, n2:Int, operation:(Int,Int)->Int) = operation(n1,n2)
```

### Criando extenções

```kotlin
fun String.randomMapitalizedLetter() = 
	this[(0..this.length-1).random()].toUpperCase()
```

## Estruturas de controle

```kotlin
if(expressao){
    //bloco de código
} else if(expressao2){
    //bloco de código
} else{
    //bloco de código
}
```

```kotlin
// IF COM MAIS DE 3 CONDIÇÕES
when {
    case1 -> {}
    case2 -> {}
    case3 -> {}
    else -> {}
}
```

```kotlin
// VERIFICAR SE VALOR É NULO E FORNECER UMA ALTERNATIVA
val a:Int? = null
var number = a ?: 0	// se a não é nulo, atribua a. Se não, atribua 0
```

## Estruturas de repetição

```kotlin
while(condição){}
//-------------------------------------
do{
    //bloco
} while(condição)
//--------------------------------------
for(i in 0..20 step 2){
    println(i)
}
```

- in: conta do valor inicial até o valor final estabelecido
- until: conta do atual até o estabelecido -1
- downTo: conta decrescente
- step: passo da contagem

```kotlin
// VARRER UMA LISTA
for(l in valArray){
    println(l)
}
```

*Obs.: Para listas, ver função `.forEach`
