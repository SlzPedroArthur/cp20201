


# Compilador: Linguagem de alto nível

Nos slides e vídeos abaixo, vocês poderão acessar o excelente material que estou baseando este curso.

* [Slides - nand2tetris](https://drive.google.com/file/d/1rbHGZV8AK4UalmdJyivgt0fpPiD1Q6Vk/view)
* [Vídeos aulas - nand2tetris](https://www.coursera.org/learn/nand2tetris2/home/week/3)

* [Exemplo de código que escrevi para "brincar" com a linguagem jack](https://github.com/profsergiocosta/MemoryGame)

## Introdução

* Neste curso será utilizada a linguagem de programação denominada Jack, desenvolvida no contexto do projeto Nand2Tetris. Essa é uma linguagem que possui vários elementos que facilitam o desenvolvimento de um compilador. 
* Algumas características:
  * Sintaxe inspirado no Java
  * Baseada em classe, mas sem recursos como herança e polimorfismo
  * Uso geral
  * Aprendizado fácil
  * Não existe precedencia de operadores
  * Variáveis de instância são sempre privadas
  * Métodos são sempre públicos
  * Fracamente tipada
  * Não possui um tipo de dados equivalente ao `float`
    
  
  
***


## Estrutura básica

* A unidade básica de um programa Jack é a classe. Estas são definida em um arquivo  único e compilada separadamente.

* Um programa Jack é uma coleção de uma ou mais classes, e uma classe é uma coleção de uma ou mais subrotinas. Uma classe em Jack tem a seguinte estrutura:

```
    class ClassName {
        variáveis de classe
        variáveis de instância
        
        construtor
        funções (método de classe) 
        métódos (método de instância)
        
    }
``` 
 

* Sendo que para ser executado, é necessário que uma das classes seja nomeada como “Main”, e nela possua uma função nomeada de “main”, como no programa mínimo abaixo:

 ```
    class Main { 
      function void main () {
        do Output.printString("Ola Mundo!!"); 
        do Output.println(); 
        return;
       } 
    } 
```

***

## Hello world !!

* Baixem [aqui](https://drive.google.com/file/u/1/d/1KcFPj8KQ_QAHheFmLCqs5iqC_0NCndvs/view?usp=sharing) as ferramentas disponibilizadas pelo projeto nand2tetris.
* Documentação das ferramentas, estão disponíveis no seguinte [site](https://www.nand2tetris.org/software).

* Ferramentas necessárias para testar o "Hello World"
  * Compilador, que dado um arquivo na linguagem Jack formato `jack`, converte para o formato intermediário, que poderá ser executado por um emulador de maquina virtual
  * VM Emulator, é a máquina virtual, depois de compilado, basta abrir e executar o código no formato `.vm`


## Tipos de dados básicos

* A linguagem Jack é uma linguagem estaticamente tipada, ou seja, toda variável tem que ser declarada previamente e possuir um tipo de dado. 
  
* As variáveis podem ser declarada com um dos três tipos de dados primitivos:

| Tipo    | Valores                           | 
| ------- | --------------------------------- |
| int     | 2 bytes (-32768 até 32767))       |
| boolean | true e false                      |
| char    | unicode (‘a’, ‘x’, ‘+’, ‘%’, ...) |

***

## Tipos de dados compostos

* Os tipos de dados compostos são criados a partir da especificação de novas classes.
* A linguagem Jack suporta o uso de cadeia de caracteres (string) e coleções (arrays) através da sua biblioteca padrão:
  * String, representando uma cadeia de caracteres demarcada por aspas duplas. Por exemplo: “Ola mundo”
  * Array, representando uma estrutura de dados contígua e homogênea. 

* Na Seção “Biblioteca Padrão” apresenta alguns exemplos da utilização destes tipos de dados. Além destes, toda classe é tratada como um tipo de dado. Ou [click aqui](../arquivos/jack_api.pdf) para visualizar em detalhes a biblioteca padrão.

***

## Escopo de variáveis

* As variáveis podem ser declaradas no escopo de classes ou de subrotinas.
  * As subrotinas aqui serão sinomino a métodos, construtores e funçoes.

* No escopo de classe as variáveis podem ser:
  1. Variáveis de classe
  2. variáveis de instância, também usadas 
   
* No escopo de subrotinas podem ser
  1. variáveis locais e 
  2. parâmetros formais. 
   
***

## Variáveis: escopo de classe

   * As variáveis de classe, similar ao Java, são definidas através da palavra reservada “static”. Elas são são *únicas por classe*, sendo compartilhadas por todas instâncias.
   * As variáveis de instância são *únicas por objetos*, e diferentemente de Java, aqui elas são explicitamente declaradas através a palavra reservada `field`:
  
```java
    class Point { 
        field int x, y; 
        static int pointCount; 
        ...
    }
```
 *  As variáveis de instância são sempre privadas, e precisam de métodos gets e sets.

***

## Variáveis: escopo de subrotinas

* Nas subrotinas, as variáveis podem aparecer como parâmetros formais:

```java
method void setX(int x)
```
* ou declaradas como variáveis locais dentro das subrotinas usando a palavra reservada `var`:

```java
function void main() { 
    var int n; 
    var int i, sum; 
    let length = Keyboard.readInt(”Entre com um inteiro:”); 
    ...
```
* As declaraçãoes sempre ocorrem no início da subrotina, e não é possível atribuir valores da declaração. Por exemplo:

```java
function void main() { 
    var int sum = 0; // erro sintático
    ...
```

ou

```java
function void main() { 
    var int sum; 
    let sum = 0;
    var int i; // erro sintático
    ...
```


***

## Construtor

* Um construtor na linguagem Jack é definido a partir da palavara reservada `constructor`. 
* Um construtor é identificado como `new`  e é único por classe:
  
```java
class Point { 
    field int x, y; 
    static int pointCount; 
    /** definição do construtor */ 
    constructor Point new(int ax, int ay) { 
        let x = ax; let y = ay; 
        let pointCount = pointCount + 1; 
        return this; 
    } 
    ... 
}

```

* Um construtor é chamado da seguinte maneira:
```
let pt = Point.new(10, 20); 
```

***

## Métodos e funções

* Os métodos de instância são definidos através da palavra reservada `method` e são sempre públicos:

```java
method int getX() {
    return x;
}
```

As funções podem ser comparadas aos métodos de classe na linguagem Java, e similarmente é defindo através da palavra reservada `static`. Por exemplo, a função `main()` 

```java
class Main {
    function void main() {
        var Array a;
        var int length;
        var int i, sum;
        let length = Keyboard.readInt(”How many numbers? ”);
        let a = Array.new(length); // constructs the array
        let i = 0;
        while (i < length) {
            let a[i] = Keyboard.readInt(”Enter a number: ”);
            let sum = sum + a[i];
            let i = i + 1;
        }
        do Output.printString(”The average is ”);
        do Output.printInt(sum / length);
        return;
    }
}
```

Todo método ou função tem então a seguinte estrutura:

```
function tipo nomeDaFuncao ( lista de parâmetros e tipos) ) {
      variáveis locais
      comandos
}
```

***

## Expressões

* Literais
* Operadores
* Chamadas de funções
* Variáveis

***

## Expressões: Literais ou constantes



* "Ola", cadeias de caracteres
* 1256, inteiro sem sinal
* true e false, valores lógicos
* null, que significa uma referência não definida
  

***

## Expressões: Operadores

Para facilitar o desenvolvimento do compilador, todos operadores são formados por apenas um caracter:

* Operadores matemáticos binários: `+`, `-`, `*`, `/`
* Operador matemático unário: `-`
* Operadores lógicos binário: `&` (e) , `|`(ou)
* Operadores lógico unário: `~` (não) 
* Operadores relacionais: `<` , `>` e `=`


***

## Expressões: chamadas de funções

Uma expressão pode ser composta também por uma ou mais chamadas de funções. Por exemplo, uma expressão válida:

```java
4 + 5 * Math.sqrt (16)
```

***
## Comandos

* Atribuição
* Chamadas de procedimentos
* Seleção
* Iteração

## Comandos: Atribuição

A atribuição é o comando mais comum em qualquer linguagem imperativa. Aqui toda atribuição inicia com a palavra reservada `let`:

```java
let a = 10;
```

***

##  Comandos: Chamadas de procedimentos

Procedimentos podem ser entendidos como uma coleção de comandos que executa uma computação parametrizável. Na linguagem Jack os procedimentos são distintos das funções, dado que os procedimentos não fazem parte de expressões.

Um procedimento é executado usando a palavra reservada `do`

```java
do Output.printString(”The average is ”);
```



***

##  Comandos: Seleção

A unica estrutura de seleção existente é o `if`. Diferentemente de outras linguagens, os comandos dentro do `if` e `else`, sempre devem estar entre a abertura e o fechamento de chaves:

```java
class Main {
    function int maior (int a, int b) {
        if (a > b) {
            return a;
        } else {
            return b;
        }
    }

    function void main() {
        do Output.printInt( Main.maior (10,20));
        return;
    }
}
```


***

## Comandos: Iteração

A única estrutura de iteração existente é o `while`:

```java
    function int fatorial (int n) {
        var int p;
        var int i;
        let p = 1;
        let i = 1;
        let n = n + 1;
        while (i < n) {
            let p = p * i;
            let i = i + 1;
        }
        return p;
    }
```

***

## Biblioteca padrão 

A biblioteca padrão possui funções para:

* Operações matemáticas
* Cadeia de caracteres
* Array
* Operações de saída de texto
* Operações de saída gráfica
* Operações de entrada de dados por teclado
* Operações de gerência de memória

[Acesse aqui](jack_api.pdf) a descrição destas operações.


