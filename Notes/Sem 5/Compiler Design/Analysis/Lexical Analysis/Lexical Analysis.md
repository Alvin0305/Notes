- It takes the character stream (raw source code text) as input
- Groups the characters into meaningful sequences called lexemes
- Add attribute or name to lexeme if needed forming tokens
- Sends these tokens to the syntax analysis (parser)

>[!tip]
>- input -> Character Stream
>- output -> Token Stream

>[!example]
>sum = num + 10;
>`<id, sum>`, `<assign>`, `<id, num>`, `<op, +>`, `<const, 10>`, `<punct, =>`

| name        | attribute |
| ----------- | --------- |
| identifier  | sum       |
| assign      |           |
| identifier  | num       |
| op          | +         |
| literal     | 10        |
| punctuation | =         |

>[!example]
>if (x <= y) small = x; else small = y;
>Lexemes are:
>
>- if, (, x, <=, y, ), small, =, x, ;, else, small, =, y, ;
>
>Tokens are: 

| name 1  | attribute 1 | name 2 | attribute 2 | name 3 | attribute 3 |
| ------- | ----------- | ------ | ----------- | ------ | ----------- |
| keyword | if          | if     |             |        |             |
| punct   | (           | brace  | (           | lbrace |             |
| id      | x           |        |             |        |             |
| relop   | <=          |        |             |        |             |
| id      | y           |        |             |        |             |
| punct   | )           | brace  | )           | rbrace |             |
| id      | small       |        |             |        |             |
| assign  |             |        |             |        |             |
| id      | x           |        |             |        |             |
| punct   | ;           |        |             |        |             |
| keyword | else        | else   |             |        |             |
| id      | small       |        |             |        |             |
| assign  |             |        |             |        |             |
| id      | y           |        |             |        |             |
| punct   | ;           |        |             |        |             |

>[!example]
>printf("sum=%d", sum);
>lexemes are: 
>- printf, (, "sum=%d", sum, ), ;

| name    | attribute |
| ------- | --------- |
| id      | printf    |
| lbrace  |           |
| literal | "sum=%d"  |
| punct   | ,         |
| id      | sum       |
| rbrace  |           |
| punct   | ;         |
### Recognition of Tokens
---
- For identifying the lexemes and creating the tokens, we use [[Regular Definitions]]
- When we start the process, we keep a lexeme begin pointer and forward pointer at the starting of the input character stream
- we iterate the forward pointer, until we get a matching lexeme between the lexeme begin pointer and forward pointer
- When we get a match, we update the lexeme begin pointer to the forward pointer and continue the process
- For some tokens like "id", we have to move the forward pointer an extra byte forward to confirm the "id" has ended, this results in a need of retraction (i.e., move the forward pointer backward)
- A retraction will be denoted by a star in the transition diagram

#number

![[Number.png]]

#Identifier

![[Identifier.png]]

#relop

![[Relop.png]]

- We can create a complete lexical analyzer by combining transition diagrams of each of the lexemes in the grammar