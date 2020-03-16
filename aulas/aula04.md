
## Análise léxica: descrição do projeto

### Referências

Nos slides e vídeos abaixo, vocês poderão acessar o excelente material que estou baseando este curso. Contudo, no curso nand2tetris os autores não separaram a análise léxica da sintática. Os dois conceitos são trabalhados na mesma aula.

* [Slides - nand2tetris](https://drive.google.com/file/d/1ujgcS7GoI-zu56FxhfkTAvEgZ6JT7Dxl/view?usp=sharing)
* [Vídeos aulas : sintático e léxico-  nand2tetris]([https://www.coursera.org/learn/nand2tetris2/home/week/3])
* [Vídeos aulas : léxico-  nand2tetris](https://www.coursera.org/learn/nand2tetris2/lecture/QM0lZ/unit-4-2-lexical-analysis)
* [Implementação do analisador léxico e sintático](https://www.coursera.org/learn/nand2tetris2/lecture/Q6IZf/unit-4-8-the-jack-analyzer-proposed-implementation)

### Visão Geral

O objetivo deste projeto é dado um programa .jack:

```
if (x < 0) {
// prints the sign
let sign = "negative";
}
```

Retorne o seguinte arquivo de XML, já com as tags que descrevem as categorias de cada token: 


```
<tokens>
<keyword> if </keyword>
<symbol> ( </symbol>
<identifier> x </identifier>
<symbol> &lt; </symbol>
<intConst> 0 </intConst>
<symbol> ) </symbol>
<symbol> { </symbol>
<keyword> let </keyword>
<identifier> sign </identifier>
<symbol> = </symbol>
<stringConst> negative </stringConst>
<symbol> ; </symbol>
<symbol> } </symbol>
</tokens>
```

> strings são impressas sem aspas duplas

> Os símbolos `<`, `>`, `"`, e `&` são impressos como `&lt;`, `&gt;`, `&quot;`, e `&amp;`. Isso é devido que estes símbolos já possuem significado dentro da linguagem XML.




***

### Categorias de tokens

Os tokens são classificados em cinco categorias:


* Palavras chaves: 
```
'class' | 'constructor' | 'function' |
'method' | 'field' | 'static' | 'var' | 'int' |
'char' | 'boolean' | 'void' | 'true' | 'false' |
'null' | 'this' | 'let' | 'do' | 'if' | 'else' |
'while' | 'return’
```

* Símbolos:
```
 '{' | '}' | '(' | ')' | '[' | ']' | '. ' | ', ' | '; ' | '+' | '-' | '*' |
'/' | '&' | '|' | '<' | '>' | '=' | '~'
```

* Inteiros: um número decimal inteiro entre 0 ... 32767
* Strings: "uma sequência de caracteres Unicode"

* Identificadores: uma sequência de letras, digitos e *undescore* ( '_' ) não iniciando com um dígito.

***

### Implementação

O objetivo é então implementar um analisado léxico, aqui denominado `JackTokenizer`. 

Definir uma API padrão é muito difícil, dado os diferentes paradigmas e tipos de linguagem. Porém, seguindo o projeto, podemos propor a seguinte API:

| Rotina        | Argumentos         | Retorno | Funcionalidade                                                                                                                                     |
| ------------- | ------------------ | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Constructor   | Arquivo de entrada | -       | Abre um arquivo .jack                                                                                                                              |
| hasMoreTokens | _                  | Boolean | Retorna verdadeiro caso exista mais tokens                                                                                                         |
| advance       | _                  |         | Avança para o próximo token, atualizando assim a variável tokenCorrente. Esse método só pode ser executado caso hasMoreTokens retornar verdadeiro. |
| tokenType     | _                  | TokenClass |    Retorna o tipo do token corrente (keyword,symbol, identier ... ) Usualmente implementado por um enumerate em linguagens estáticas  |
| keyWord       |                    | KeywordType  | Retorna o keyword como uma constant, usualmente um enumerado. Só pode ser chamado quando tokenType é Keyword |
| symbol        |                    | char        |  Retorna  o caracter que o toke corrente      |
| identifier    |                    |  string       | Retorna o identificador como uma string|
| intVal        |                    |    int     |    Retorna a constante inteira como um número|
| stringVal     |                    |    string     |  Retorna a constante string sem as aspas duplas  |


> Caso utilize uma linguagem dinamicamente tipada, os métodos keyword, symbol, identifier, intVal e stringVal podem ser resumido apenas a um único método getToken. 

| Rotina        | Argumentos      |    Funcionalidade                                                                                                                                     |
| ------------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Constructor   | Arquivo de entrada  | Abre um arquivo .jack                                                                                                                              |
| hasMoreTokens | _                  | Retorna verdadeiro caso exista mais tokens                                                                                                         |
| advance       | _                  | Avança para o próximo token, atualizando assim a variável tokenCorrente. Esse método só pode ser executado caso hasMoreTokens retornar verdadeiro. |
| tokenType     | _                  | Retorna o tipo do token corrente (keyword,symbol, identier ... ), podendo ser no formato de string ou uma constante na linguagem |
| getToken     | _                  | Retorna o token corrente |


Como usualmente não existem tipos enumerados em linguagens dinâmicas, uma boa prática é criar constantes para representar os tipos:

```python
TK_KEYWORD = 0
TK_SYMBOL = 1
TK_INTCONST = 2
```
Desse modo, poderá depois utilizar essas constantes em todo código, evitanto possíveis erros de digitação. Em Python poderia usar assim:

```python
from enum import Enum
class Token(Enum):
  KEYWORD = 0
  SYMBOL = 1
  INTCONST = 2
```





### Usando a API

Um pseudocódigo que mostra como poderiamos usar a API JackTokenizer.

```
tknz = new JackTokenizer ("Prog.jack")
tknz.advance();
print "<tokens>"
while tknz.hasMoreTokens() {
	tokenClass = tipo do token corrente
	print "<" + tokenClass + ">"
	print token corrent
	print "</" + tokenClass + ">"
	print newline
	tknz.advance()
}
print "</tokens>"
```

***

### Projeto

Cada aluno deve desenvolver um analisador léxico para a linguagem Jack. Para isso, pode-se usar qualquer linguagem de alto nível, como Ruby, Python, Gol, Kotlin, C, C+, Java, Scala, Haskell ...
 
O analisador léxico deve reconhecer os tokens da linguagem Jack. O analisador deve ser implementado de acordo com a API proposta aqui, podendo fazer algumas adaptações para se adequar melhor a linguagem utilizada. Lembre-se que esta API será usado pelo analisador sintático. 

Cada aluno deverá criar um repositório no github ou gitlab com o nome jackcompiler. Nesse momento o projeto deverá ter no mínimo uma classe JackTokenizer, e uma outra chamada JackCompiler. Essa última será o "ponto de partida" do nosso programa. 

No envio da atividade, deverá ser enviado o código e o link para o github ou gitlab.

O link para o github deverá ser enviado no grupo da disciplina no whatsapp. 

Qualquer dúvida, poderá ser dirigida no grupo do whatsapp. Por isso é importante manter o codigo no github ou gitlab.

Para testar o analisador, utiliza o seguinte [código em Jack](../projetos/01/Main.jack), que deverá resultar neste [arquivo XML](../projetos/01/Main.xml).
