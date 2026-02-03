- Token Specification: The formal description of tokens in a programming language using regular definitions or regular expressions, which guides the lexical analyzer in recognizing valid tokens.
- Regular Expression: A pattern that directly describes a set of strings (e.g., \[0-9]+ for numbers)
- Regular Definition: A shorthand where names are assigned to regular expressions, allowing complex token patterns to be built step by step (e.g., digit -> \[0-9], then num -> digit+)

#### Language corresponding to a regex and its properties
---
L(r) = Language for regex r
L(0) = {0}
L(0|1) = {0, 1}
L(01) = {01}
$L(r_1, r_2) = L(r_1).L(r_2)$
$L(r_1|r_2) = L(r_1)\cup L(r_2)$
$L(r*) = [L(r)]*$
#### Useful Regular Definitions
---
- (0|1)* -> all binary numbers including **ε**
- digit -> 0|1|2|3|4|5|6|7|8|9
- number -> (digit)(digit)*
- float -> number | number.number
- letter -> A|B| ... |Z|a|b| ... |z
- id -> (letter)(digit|letter)*

Strings are also called as sentence or word in this context
#### Extensions
---
- a+ is equivalent to (a)(a)\*, i.e., a can appear one or more times
- a? is equivalent to a | ε, i.e., a can either appear once or no
- \[a-z] is equivalent to a|b|...|z, i.e., all characters from a to z in lexicographic order
- \[abc] is equivalent to a|b|c, i.e., all the characters inside the brackets

>[!tip]
Using the extensions, we can define
>- digit -> \[0-9]
>- digits -> \[0-9]+
>- number -> (digit+)(.digit+)? or number -> (digits)(.digits)?
>- letter -> \[A-Za-z]
>- id -> letter\[letter digit]*

- Keywords are treated as tokens whose patters are fixed string literals, defined separately.
- e.g., if -> if, while -> while, ...

>[!example]
>stmt -> if expr then stmt else stmt | id = expr
>expr -> expr relop expr | id | number
>
>- Here, tokens are if, then, else, id, =, relop, number,
>- expr, stmt are not tokens
>  
>  Regular definitions needed are
>  - if -> if
>  - then -> then
>  - else -> else
>  - id -> \[A-Za-z]\[A-Za-z0-9]*
>  - assign -> =
>  - relop -> \[> < <= >= != \==]
>  - number -> \[0-9]+(.\[0-9]+)?

