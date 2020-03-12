## Analisador sintático - Projeto


### Parsing process

Para esse tópico serão usados os [slides](./parsing_process.pdf) dos professores Noam Nisan and Shimon Schocken.

Vejam os seguintes vídeos:

* https://www.coursera.org/learn/nand2tetris2/lecture/0H0Wq/unit-4-5-parser-logic
* https://www.coursera.org/learn/nand2tetris2/lecture/5wwmy/unit-4-7-the-jack-analyzer
* https://www.coursera.org/learn/nand2tetris2/lecture/Q6IZf/unit-4-8-the-jack-analyzer-proposed-implementation

### Gramática


#### Lexical elements 

The Jack language includes five types of terminal elements (tokens):

| Terminal        | Regra   | 
| ------------- |:-------------:|
|keyword |    '**class'** \| '**constructor'** \| '**function'** \|'**method'** \| '**field'** \| '**static'** \| '**var'** \|'**int'** \| '**char'** \| '**boolean'** \| '**void'** \|'**true'** \|'**false'** \| '**null'** \| '**this'** \| '**let'** \| '**do'** \|'**if'** \| '**else'** \| '**while'** \| '**return'**
|symbol| '**{'** \| '**}'** \| '**('** \| '**)'** \| '**['** \| '**]'** \| '**.'** \|'**,'** \| '**;'** \| '**+'** \| '**-'** \| '***'** \| '**/'** \| '**&'** \|'**\|'** \| '**<'** \| '**>'** \| '**='** \| '**~'**
|integerConstant|  A decimal number in the range 0 .. 32767.
|StringConstant|  A sequence of Unicode characters not including double quote or newline.
|identifier| A sequence of letters, digits, and underscore ( '**_'** ) not starting with a digit.


#### Program structure

A Jack program is a collection of classes, each appearing in a separate file. The compilation unit is a class. A class is a sequence of tokens structured according to the following context free syntax:

| Não terminal        | Regra   | 
| ------------- |:-------------:|
|class| '**class'** className '**{'** classVarDec* subroutineDec* '**}'**
|classVarDec| ( '**static'** \| '**field'** ) type varName ( '**,'** varName)* '**;'**
|type|'**int'** \| '**char'** \| '**boolean'** \| className
|subroutineDec| ( '**constructor'** \| '**function'** \| '**method'** ) ( '**void'** \| type) subroutineName '**('** parameterList '**)'** subroutineBody
|parameterList| ((type varName) ( '**,'** type varName)*)?
|subroutineBody|'**{'** varDec* statements '**}'**
|varDec| '**var'** type varName ( '**,'** varName)* '**;'**
|className| identifier
|subroutineName| identifier
|varName| identifier


#### Statements

| Não terminal        | Regra   | 
| ------------- |:-------------:|
|statements| statement*
|statement| letStatement \| ifStatement \| whileStatement \| doStatement \| returnStatement
|letStatement|'**let'** varName ( '**['** expression '**]'** )? '**='** expression '**;'**
|ifStatement| '**if'** '**('** expression '**)'** '**{'** statements '**}'** ( '**else'** '**{'** statements '**}'** )?
|whileStatement| '**while'** '**('** expression '**)'** '**{'** statements '**}'**
|doStatement| '**do'** subroutineCall '**;'**
|ReturnStatement| '**return'** expression? '**;'**


#### Epressions

| Não terminal        | Regra   | 
| ------------- |:-------------:|
|expression| term (op term)|
|term | integerConstant \| stringConstant \| keywordConstant \| varName |
|subroutineCall| subroutineName '**('** expressionList ')' \| (className\|varName) '**.'** subroutineName '**('** expressionList '**)'**|
|expressionList| (expression ( '**,'** expression)* )?
op| '**+'** \| '**-'** \| '**\*** \| '**/'** \| '**&'** \| '**\|'** \| '**<'** \| '**>'** \| '**='**
|unaryOp|'**-**' \| '**~**'
|KeywordConstant|'**true** \| '**false'** \| '**null'** \| '**this'**




### API

A seguir é apresentado as rotinas, construtor e métodos do CompilationEngine. Lembrem-se que esse componente irá utilizar o JackTokenizer implementado anteriormente.

| Rotina                 | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Constructor            |  Given Input stream/file Output stream/file. Creates a new compilation engine with the given input and output. The next routine called must be compileClass() .                                                                                                                                                                                                                                                                                                                            |
| CompileClass           |  Compiles a complete class.                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| CompileClassVarDec     |  Compiles a static declaration or a field declaration.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| CompileSubroutine      |  Compiles a complete method, function, or constructor.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| CompileParameterList   |  Compiles a (possibly empty) parameter list, not including the enclosing "()" .                                                                                                                                                                                                                                                                                                                                                                                                            |
| compileVarDec          |  Compiles a var declaration.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| compileStatements      |  Compiles a sequence of statements, not including the enclosing ‘‘{}’’.                                                                                                                                                                                                                                                                                                                                                                                                                    |
| compileDo              |  Compiles a do statement.                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| compileLet             |  Compiles a let statement.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| compileReturn          |  Compiles a return statement.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| compileIf              |  Compiles an if statement, possibly with a trailing else clause.                                                                                                                                                                                                                                                                                                                                                                                                                           |
| CompileExpression      |  Compiles an expression.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| CompileTerm            |  Compiles a term. This routine is faced with a slight difficulty when trying to decide between some of the alternative parsing rules. Specifically, if the current token is an identifier, the routine must distinguish between a variable, an array entry, and a subroutine call. A single lookahead token, which may be one of ‘‘[’’, ‘‘(’’, or ‘‘.’’ suffices to distinguish between the three possibilities. Any other token is not part of this term and should not be advanced over. |
| CompileExpressionList  |  Compiles a (possibly empty) comma-separated list of expressions.                                                                                                                                                                                                                                                                                                                                                                                                                          |

## Projeto

Desenvolva um analisador sintático, que dado um ou mais arquivos .jack, ele execute a análise sintática e retorne um arquivo XML para cada arquivo .jack compilador. Esse arquivo XML irá representar a árvore sintática (parse tree) das unidades compiladas.

Nesse primeiro projeto, vamos considerar programas com expressões simples, como as dos [seguintes arquivos](../projetos/02)





