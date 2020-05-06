# Diseño de Lenguajes (Reto)

Modifica la gramática corrigiendo los errores que veas, de manera que genere frases como estas:

1. `a[4][b+c][5] = c[2][3]`
2. `a.b.c = a[x]`
3. `a.b[x*y].d = c.d[3][1].e(temp)`

* Procura que exprese bien la precedencia de operadores (vigila la asignación)

* La variable `<value>` no se describe (y debería describirse)

## Grammar

```ruby
<program> ::= <block>

<block> ::= '{' <statement>* '}'  // Modified Casiano

<statement> ::=
               <declaration> |
              "if" <parenthesis> <block> ("else" "if" <block>)* ('else' <block>)? |
              "while" <parenthesis> <block> |
              'function' <word> '(' <word> (',' <word>)* ')' <block> |
              <expr> ";"
              
<declaration> ::= 'var' WORD ('=' <expr>)?


<expr> :: <variable> ('=' <assgination>)? // Redefinimos expresion para que sea una variable a la que se le puede asignar valor.


<variable> :: = WORD (. WORD | '[' <array> ']')* // En caso de que se puedan anidar corchetes
<variable> :: = WORD (. WORD | '[' <expr> ']')* // Si no se pueden anidar corchetes este es el correcto.

<assignation> ::= <term> (('==', '!=', '>', '>=', '<', '<=', '=') <term>)* // Asignación que se puede hacer a la variable.

<term> ::= <sum> (('+', '-') <sum>)*

<sum> ::= <value> (('*', '/') <value>)*

<apply> ::= '(' <expr> (',' <expr>)* ')'<apply> | '.'<word><apply> | empty

<array> ::= '[' ']' | '[' <expr> (',' <expr> )*']' // Added by Casiano

<parenthesis> ::= '(' <expr> ')'

// Definimos value.
<value> ::= (VALUE | WORD | <array> | <parenthesis>) (. WORD | '[' <expr> ']' | <apply>)*  // Added by: Casiano
```

## Tokens

```Ỳacc
WORD = [a-zA-Z_]\w*
VALUE = STRING | NUMBER
STRING = "( [^"\\]   #any character except " and escape
           | \\.     #any character before an escape
           )*"
NUMBER = ([-+]?\d+     #entero
          (\.\d+)?    #flotante
          ([eE][-+]?\d+)?  #con exponente
          )
```
