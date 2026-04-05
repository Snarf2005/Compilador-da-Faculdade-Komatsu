# Projeto_Compilador_Faculdade

UNIC – Universidade de Cuiabá
Relatório Técnico: Especificação da Gramática do Analisador Léxico
Data: 23/03/2026
Professor: Edson Komatsu
Integrantes: Chefe-Emmanuel, Sandro, Leandro
Cidade: Cuiabá-Mato Grosso

# Compilador Simples em Go

## EBNF da Linguagem

```
Programa    ::= { Instrução }
Instrucao   ::= Atribuição | Print | If | While
Atribuicao  ::= Ident "=" Expressão
Expressao   ::= Termo [ Operator Termo ]
Termo       ::= Number | Ident
Print       ::= "print" "(" Ident ")"
If          ::= "if" "(" Condicao ")" "{" { Instrução } "}" [ "else" "{" { Instrução } "}" ]
While       ::= "while" "(" Condição ")" "{" { Instrução } "}"
Condicao    ::= Termo Operator Termo
Ident       ::= [a-zA-Z_][a-zA-Z0-9_]*
Number      ::= [0-9]+
Operator    ::= "=" | "+" | "-" | "*" | "/" | ">" | "<" | "!"
```

- **Tipos de dados**: Apenas inteiros (`Number`).
- **Operações**: Aritméticas e relacionais básicas.
- **Declaração de variáveis**: Implícita na atribuição.
- **Estruturas de controle**: `if`/`else`, `while`.
- **Operadores**: `=`, `+`, `-`, `*`, `/`, `>`, `<`, `!`

## Exemplo de Código (teste.txt)

```
x = 10
y = 20
if (x > y) {
    print(x)
} else {
    print(y)
}
while (x < 100) {
    x = x + 1
}
```

# Regras
- Cada instrução deve estar em uma linha separada ou delimitada por chaves/blocos.
- Espaços em branco são ignorados.
- Variáveis são criadas na primeira atribuição.
- O compilador faz análise léxica, sintática e exibe tokens e erros léxicos.

## Execução
I. Compile com `go run main.go`.
II. O analisador léxico exibirá todos os tokens válidos e reportará erros léxicos, se houver.
III. O parser exibirá o número de instruções raiz encontradas.

1. Introdução: Este documento descreve a especificação formal da linguagem de programação desenvolvida para a disciplina de Compiladores. A implementação utiliza um analisador sintático descendente (Top-Down) baseado na biblioteca Participle para a linguagem Go. O sistema é capaz de processar instruções de atribuição, saída de dados e estruturas de controlo de fluxo.

2. Especificação Léxica (Tokens): A análise léxica é definida por um conjunto de regras de expressões regulares (Regex) que identificam os símbolos básicos da linguagem.
.   
                            Tabela de Tokens
Token          |         Padrão (Regex)       |            Descrição
Keyword	       |           `\b(if	          |             else)\b`
Ident	       |     [a-zA-Z_][a-zA-Z0-9_]*	  |   Identificadores de variáveis.
Number         |         	\d+	              |   Literais numéricos inteiros.
Operator	   |         [=+\-*/><!-]	      |   Operadores de atribuição, aritméticos e lógicos.
Punct          |           [{}()]	          |   Delimitadores de blocos e expressões.

3. Gramática Formal (EBNF): A sintaxe da linguagem segue o padrão ISO/IEC 14977. Abaixo está a representação das produções que compõem a Árvore Sintática Abstrata (AST):

EBNF
(* Estrutura Principal *)
Programa = { Instrucao } ;
Instrucao = Atribuicao
          | Print
          | If
          | While ;

(* Regras de Produção *)
Atribuição = Ident , "=" , Expressão ;
Print = "print" , "(" , Ident , ")" ;
If = "if" , "(" , Condição , ")" , "{" , { Instrução } , "}" , [ "else" , "{" , { Instrução } , "}" ] ;
While = "while" , "(" , Condição , ")" , "{" , { Instrução } , "}" ;

Condição = Termo , Operator, Termo ;
Expressão = Termo , [ Operator , Termo ] ;
Termo = Number | Ident ;

4. Descrição das Estruturas Programadas:
I. Raiz do Parser: Corresponde ao ponto inicial da análise sintática, sendo composta por uma lista de instruções que estruturam o programa.
II. If / Else: Estrutura condicional que permite a execução de blocos alternativos de código, conforme a avaliação de uma condição lógica.
III. While: Estrutura de repetição que executa um bloco de instruções de forma iterativa, enquanto uma determinada condição lógica for satisfeita.
IV. Expressão: Suporta operações matemáticas simples entre números ou variáveis (termos), possibilitando a construção de cálculos básicos.
V. Condição: Responsável por realizar a comparação entre dois termos (números ou variáveis), utilizando operadores lógicos ou relacionais.
VI. Termo: Unidade básica utilizada em cálculos e comparações, podendo representar um literal numérico ou um identificador (variável).

5. Exemplo de Implementação (Go)Abaixo, um fragmento do código que demonstra a integração do Lexer com o Parser:Go// Trecho do ficheiro main.go
parser, err := participle.Build[Programa](
    participle.Lexer(meuLexer),
    participle.Elide("Whitespace"),
) 
// ... processamento do ficheiro teste.txt
