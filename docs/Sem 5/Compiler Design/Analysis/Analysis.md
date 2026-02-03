- input -> source language as a stream of bytes
- output -> intermediate representation
- This is considered the front end part

>[!example]
>- sum = num + x;
>```text
>		=
>	  /   \
>	 id1    +
>		  /  \
>		 id2 id3
>```

>[!tip] Steps of Analysis
>Character Stream -> `Lexical Analysis` -> Token Stream -> `Syntax Analysis (parsing)` -> IR + [[Sem 5/Compiler Design/Concepts#Symbol Table|Symbol Table]] -> `Semantic Analysis` -> IR + [[Sem 5/Compiler Design/Concepts#Symbol Table|Symbol Table]]

### Lexical Analysis
---
![[Lexical Analysis]]

### Syntax Analysis
---
![[Syntax Analysis]]

### Semantic Analysis
---
![[Semantic Analysis]]