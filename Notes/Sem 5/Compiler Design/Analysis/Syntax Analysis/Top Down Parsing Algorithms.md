- Construct the parse tree, starting from the root and creating nodes in preorder
- Tree Nodes:
	- Non terminal: choose production, add children
	- Terminal: match with the next input token
- This procedure is equivalent of finding a [[Sem 5/Compiler Design/Concepts#Left most derivation|Left most derivation]]

>[!example]
>S -> cAd
>A -> ab
>Input: cabd

![[Parsing Example 1.png]]

#### Method 1: Recursive Descent Parser
---
- A procedure(function) will be defined for each non-terminal
- For the above example: 

>[!example]
>S -> cAd
>A -> ab
>
>```C
>S() {
>	match('c');
>	A();
>	match('d');
>}
>
>A() {
>	match('a');
>	match('b');
>}
>```

- The match(x) function compares x with the look-ahead symbol(the next input symbol)
	- If both match, advances the input pointer to the next input symbol
	- If mismatch, reports error and stops
#### Method 2: Recursive Descent with Backtracking
---
- Same as previous one. But, when a mismatch occurs, we backtrack and take the other available production rule

>[!example]
>S -> (A)
>A -> ab | b
>Input: (b)
>
>at internal node A
>- If A -> b is chosen, parsing is successful
>- If A -> ab is chosen, mismatch occurs in the next step
>- => we backtrack and try A -> b

##### Disadvantage
- Not practical, not efficient
- To make it efficient, we should choose the production rule based on the look ahead symbol. This is called [[#Method 3 Predictive Parsing|Predictive Parsing]]
#### Method 3: Predictive Parsing
---
- If we have A -> $a\alpha | b\beta$ 
	- If the look ahead is a, we choose A -> $a\alpha$
	- If the look ahead is b, we choose A -> $b\beta$

```C
A() {
	switch(lookahead) {
		case 'a': choose(aα)
		case 'b': choose(bβ)
	}
}
```

>[!example]
>S -> (A)
>A -> ab | b
>
>```C
>S() {
>	match('(');
>	A();
>	match(')');
>}
>
>A() {
>	switch(lookahead) {
>		case 'a': 
>			match('a');
>			match('b');
>			break;
>		case 'b':
>			match('b');
>			break;
>	}
>}
>```

- For making this algorithm robust. We use FIRST and FOLLOW
- Confusion to choose which production arise when there are multiple productions, like A -> α | β. In such situations, if FIRST(α) and FIRST(β) are disjoint, then we can make a decision based on the look ahead symbol
- Choose the production where the FIRST of its body equals the look ahead symbol
##### Building a parsing table from FIRST sets

>[!important]
>The algorithm is:
>```
>for each production A -> α:
>	for each terminal a ∈ FIRST(α):
>		add A -> α in entry T[A, a]
>```

>[!exampmle]
>S -> id = E
>E -> id + id | num | -id
>
>FIRST(id=E) = {id}
>FIRST(id+id) = {id}
>FIRST(num) = {num}
>FIRST(-id) = {-}
>FIRST(E) = {id, num, -}
>FIRST(S) = {id}

|     | id           | num      | -        | $     |
| --- | ------------ | -------- | -------- | ----- |
| S   | S -> id = E  | error    | error    | error |
| E   | E -> id + id | E -> num | E -> -id | error |

>[!note]
>Blank entries or "error" entries means error

>[!example]
>S -> aAb
>A -> B | C
>B -> bd
>C -> c
>
>FIRST(aAb) = {a}
>FIRST(B) = {b}
>FIRST(C) = {c}
>FIRST(A) = {b, c}

|     | a        | b       | c      | d   | $   |
| --- | -------- | ------- | ------ | --- | --- |
| S   | A -> aAb |         |        |     |     |
| A   |          | A -> B  | A -> C |     |     |
| B   |          | B -> bd |        |     |     |
| C   |          |         | C -> c |     |     |

>[!example]
>S -> (A) | \[A]
>A -> id + E | num + E
>E -> id | num
>
>FIRST((A)) = {(}
>FIRST(\[A]) = {\[}
>FIRST(id+E) = {id}
>FIRST(num+E) = {num}
>FIRST(id) = {id}
>FIRST(num) = {num}
>FIRST(E) = {id, num}
>FIRST(A) = {id, num}
>FIRST(A) = {(, \[}

|     | (        | )   | \[        | ]   | +   | id        | num        | $   |
| --- | -------- | --- | --------- | --- | --- | --------- | ---------- | --- |
| S   | S -> (A) |     | S -> \[A] |     |     |           |            |     |
| A   |          |     |           |     |     | A -> id+E | A -> num+E |     |
| E   |          |     |           |     |     | E -> id   | E -> num   |     |
- But if the grammar contains ε productions, then [[Sem 5/Compiler Design/Concepts#FIRST|FIRST]] is not enough to populate the table, we need [[Sem 5/Compiler Design/Concepts#FOLLOW|FOLLOW]] as well

>[!important] 
>Algorithm to populate the parsing table using FIRST and FOLLOW sets
>
>For each production A -> α
>1. for each terminal a ∈ FIRST(α), add A -> α to T\[A, a]
>2. if ε ∈ FIRST(α), for each terminal b in FOLLOW(A), add A -> α to T[A, b]
>3. if ε ∈ FIRST(α), and \$ ∈ FOLLOW(A), add A -> α to T\[A, \$]

>[!example] 
>S -> (A)
>A -> Cd
>C -> ab | ε 
>
>FIRST((A)) = {(}
>FIRST(Cd) = {a, ε}
>FIRST(ab) = {a}
>FIRST(ε) = ε
>FIRST(C) = {a, ε}
>FIRST(A) = {a, ε}
>FIRST(A) = {(}
>
>FOLLOW(A) = {)}
>FOLLOW(C) = {d}
>FOLLOW(S) = {$}

|     | (        | )   | a       | b   | d       | $   |
| --- | -------- | --- | ------- | --- | ------- | --- |
| S   | S -> (A) |     |         |     |         |     |
| A   |          |     | A -> Cd |     | A -> Cd |     |
| C   |          |     | C -> ab |     | C -> ε  |     |
>[!example] 
>S -> (E)
>E -> idA
>A -> +id | ε
>
>FIRST((E)) = {(}
>FIRST(idA) = {id}
>FIRST(+id) = {+}
>FIRST(ε) = {ε}
>FIRST(E) = {id}
>FIRST(A) = {+, ε}
>FIRST(S) = {(}
>
>FOLLOW(S) = {$}
>FOLLOW(E) = {)}
>FOLLOW(A) = {)}

|     | (        | )      | id       | +        | $   |
| --- | -------- | ------ | -------- | -------- | --- |
| S   | S -> (E) |        |          |          |     |
| E   |          |        | E -> idA |          |     |
| A   |          | A -> ε |          | A -> +id |     |
>[!example] 
>E -> TE'
>E' -> +TE' | ε 
>T -> id
>
>FIRST(TE') = {id}
>FIRST(+TE') = {+}
>FIRST(id) = {id}
>FIRST(ε) = {ε}
>FIRST(T) = {id}
>FIRST(E') = {+, ε}
>FIRST(E) = {id}
>
>FOLLOW(E) = {\$}
>FOLLOW(T) = {+, \$}
>FOLLOW(E') = {\$}

|     | +         | id       | $      |
| --- | --------- | -------- | ------ |
| E   |           | E -> TE' |        |
| T   |           | T -> id  |        |
| E'  | E' -> +TE |          | E -> ε |

>[!example] 
>S -> AB
>A -> aA | ε
>B -> bB | ε
>
>FIRST(AB) = {a, b, ε}
>FIRST(aA) = {a}
>FIRST(bB) = {b}
>FIRST(B) = {b, ε}
>FIRST(A) = {a, ε}
>FIRST(S) = {a, b, ε}
>
>FOLLOW(S) = {\$}
>FOLLOW(A) = {b, \$}
>FOLLOW(B) = {\$}

|     | a       | b       | $       |
| --- | ------- | ------- | ------- |
| S   | S -> AB | S -> AB | S -> AB |
| A   | A -> aA | A -> ε  | A -> ε  |
| B   |         | B -> bB | B -> ε  |

>[!example] 
>S -> (A)
>A -> BC
>B -> b | ε
>C -> c | ε
>
>FIRST((A)) = {(}
>FIRST(BC) = {b, c, ε}
>FIRST(b) = {b}
>FIRST(c) = {c}
>FIRST(C) = {c, ε}
>FIRST(B) = {b, ε}
>FIRST(A) = {b, c, ε}
>FIRST(S) = {(}
>
>FOLLOW(S) = {\$}
>FOLLOW(A) = {)}
>FOLLOW(B) = {c, )}
>FOLLOW(C) = {)}

|     | (        | )       | b       | c       | $   |
| --- | -------- | ------- | ------- | ------- | --- |
| S   | S -> (A) |         |         |         |     |
| A   |          | A -> BC | A -> BC | A -> BC |     |
| B   |          | B -> ε  | B -> b  | B -> ε  |     |
| C   |          | C -> ε  |         | C -> c  |     |

>[!example] 
>A -> BC
>B -> b | ε
>C -> c | ε
>
>FIRST(BC) = {b, c, ε}
>FIRST(b) = {b}
>FIRST(c) = {c}
>FIRST(C) = {c, ε}
>FIRST(B) = {b, ε}
>FIRST(A) = {b, c, ε}
>
>FOLLOW(A) = {\$}
>FOLLOW(B) = {c, \$}
>FOLLOW(C) = {\$}

|     | b       | c       | $       |
| --- | ------- | ------- | ------- |
| A   | A -> BC | A -> BC | A -> BC |
| B   | B -> b  | B -> ε  | B -> ε  |
| C   |         | C -> c  | C -> ε  |
### Method 4: Non Recursive Predictive Parsing
---
- Predictive Parsing is of two types
	- Using recursion 
	- Without recursion (using stack)

>[!tip] 
>Input -> string w and parsing table T for Grammar G
>Output -> if w ∈ L(G), productions in a [[Sem 5/Compiler Design/Concepts#Left most derivation|Left most derivation]] of w; otherwise error indication

- In this method, we keep a stack of input symbols and an input buffer
- The stack contains S\$ initially (S is on top)
- Input buffer contains w\$

>[!question] 
>E -> TE'
>E' -> +TE' | ε 
>T -> id

|     | id       | +         | $      |
| --- | -------- | --------- | ------ |
| E   | E -> TE' |           |        |
| T   | T -> id  |           |        |
| E'  |          | E' -> +TE | E -> ε |
From, the parser table

| Stack | Input Buffer | Action    |
| ----: | -----------: | --------- |
|    E$ |       id+id$ | E -> TE'  |
|  TE'$ |       id+id$ | T -> id   |
| idE'$ |       id+id$ | match id  |
|   E'$ |         +id$ | E' -> +TE |
|  +TE$ |         +id$ | match +   |
|   TE$ |          id$ | T -> id   |
|  idE$ |          id$ | match id  |
|   E\$ |           \$ | E -> ε    |
|    \$ |           \$ | accept    |
(stack top is the first character is the stack column)

>[!important] 
>- Predictive parsing is suitable for [[Grammar Types#LL(1)|LL(1) Grammar]] only
>- Predictive parsers cannot be used for [[Sem 5/Compiler Design/Concepts#Left Recursive Grammar|Left recursive grammars]]. To parse a [[Sem 5/Compiler Design/Concepts#Left Recursive Grammar|Left recursive grammars]], it should be first [[Sem 5/Compiler Design/Concepts#Conversion of Left Recursive to Right Recursive Grammar|Converted to right recursive grammar]]

