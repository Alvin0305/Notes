- Construct parse tree bottom-up, starting from leaves
- Parent node A added to a set of nodes matching the body $\alpha$ of production $A \to \alpha$
- Equivalent to do a right most derivation in reverse
- Top down parsers can only parse LL(1) (if there are k look-aheads, it can parse LL(k)) grammars. 
- Top down parsers are less powerful but are simple
- Bottom up parsers are more powerful, but is more complex

>[!example] 
>E -> E + T | T
>T -> id
>String = id1 + id2

![[Bottom Up parsing example 1.png]]

- the right most derivation of the same is:
- E => E + T => E + id => T + id => id + id
- We can see, the reverse of the right most derivation is same as the image
- in the image, 
	- id + id => T + id => E + id => E + T => E
- when we replace the body of a production by its head, we call it reduction
	- e.g., when we replaced id1 by T, it is called ***[[Sem 5/Compiler Design/Concepts#Reduction|Reduce]] id to T***
- each step replaces a substring matching the body of a production by its head
- The parsing process reduces the input string w to the start symbol
	- can be viewed as building a parse tree bottom-up
	- the process is called Handle Pruning

>[!example] 
>E -> E + T | T
>T -> T \* F | F
>F -> (E) | id
>
>String1: id \* id
>- Right most derivation is:
>- E => T => T \* F => T \* id => F \* id => id \*id
>
>String2: (id + id) \* id
>- Right most derivation is: 
>- E => T => T \* F => T \* id => F \* id => (E) \* id => (E + T) \* id => (E + F) \* id => (E + id) \* id => (T + id) \* id => (F + id) \* id => (id + id) \* id
#### Shift Reduce Parsing
- A form of Bottom up parsing
- A stack holds grammar symbols and an input buffer holds the rest of the symbols to be parsed
- Parsing starts with $ as the only symbol in stack
- Parser shifts input symbols on to the stack until it gets a handle α in the stack. It then reduces α to the head of the appropriate production
- The parser repeats the above step until the stack contains the start symbol and the input is empty (accept the string) or until it detects an error

>[!example] 
>E -> E + T | T
>T -> id
>
>String: id1 + id2

| Stack  | Input Buffer | Action            |
| ------ | -----------: | ----------------- |
| $      |     id1+id2$ | shift             |
| $id1   |        +id2$ | Reduce T -> id    |
| $T     |        +id2$ | Reduce E -> T     |
| $E     |        +id2$ | shift             |
| $E+    |         id2$ | shift             |
| $E+id2 |            $ | Reduce T -> id    |
| $E+T   |            $ | Reduce E -> E + T |
| $E     |            $ | accept            |
>[!tip] 
>Four possible actions:
>- shift -> shift the next input symbol to the top of the stack
>- reduce -> a set of topmost symbols constitute a handle A -> α. The right end of α is on top of the stack, Pop α and push A
>- accept -> stack contains the start symbol + $ and the input is empty ($ only)
>- reject / error -> report syntax error or call error recovery routine

>[!example] 
>E -> E + T | T
>T -> T \* F | F
>F -> (E) | id
>
>String: id1 \* id2

| Stack   | Input Buffer | Action           |
| ------- | -----------: | ---------------- |
| $       |  id1 \* id2$ | shift            |
| $id1    |       \*id2$ | Reduce F -> id   |
| $F      |       \*id2$ | Reduce T -> F    |
| ==$T==  |   ==\*id2$== | ==shift==        |
| $T\*    |         id2$ | shift            |
| $T\*id2 |            $ | Reduce F -> id   |
| $T\*F   |            $ | Reduce T -> T\*F |
| $T      |            $ | Reduce E -> T    |
| $E      |            $ | accept           |
- Here, the top of the stack is at the right end
- Here in the highlighted row, we can do two actions, either a shift, or a reduce by E -> T. This is a shift reduce conflict. And if we had chosen the reduce action, then we won't be able to successfully parse the given string. Since we chose the shift action, we parsed the string

>[!example] 
>S -> L = R
>L -> id
>R -> num
>
>String: id = num

| Stack  | Input Buffer | Action            |
| ------ | -----------: | ----------------- |
| $      |      id=num$ | shift             |
| $id    |        =num$ | Reduce L -> id    |
| $L     |        =num$ | shift             |
| $L=    |         num$ | shift             |
| $L=num |            $ | Reduce R -> num   |
| $L=R   |            $ | Reduce S -> L = R |
| $S     |            $ | accept            |
- Decision on shift/reduce/accept/reject, by which production is based on the stack contents and the next input symbol
	- locating a handle may require examining the set of top most symbols, sometimes scanning the entire stack
	- This is not efficient
	- So we keep information about, what the parser has seen till now

| Stack  | Input Buffer | Action            | What the parser has seen      |
| ------ | -----------: | ----------------- | ----------------------------- |
| $      |      id=num$ | shift             | nothing                       |
| $id    |        =num$ | Reduce L -> id    | substring id                  |
| $L     |        =num$ | shift             | a string derivable from L     |
| $L=    |         num$ | shift             | a string derivable from L=    |
| $L=num |            $ | Reduce R -> num   | a string derivable from L=num |
| $L=R   |            $ | Reduce S -> L = R | a string derivable from L=R   |
| $S     |            $ | accept            | a string derivable from S     |
- This means, we are maintaining the parser state to keep track of the parsing process
- A state indicated how much of a production is already seen
- The parser makes shift/reduce decisions based on the current state and the next input symbol
- For that, we push state symbols along with grammar symbols into the stack
- The current state will be in the top of the stack
- So, the parser can make a decision based on the current state(top symbol) and next input symbol -> no need to scan the entire stack, only one symbol(state) is enough to be checked

| Stack      | Input Buffer | Action            |
| ---------- | -----------: | ----------------- |
| $0         |      id=num$ | shift             |
| $0id1      |        =num$ | Reduce L -> id    |
| $0L2       |        =num$ | shift             |
| $0L2=3     |         num$ | shift             |
| $0L2=3num4 |            $ | Reduce R -> num   |
| $0L2=3R5   |            $ | Reduce S -> L = R |
| $0S6       |            $ | accept            |
- For implementing this, we use [[Sem 5/Compiler Design/Concepts#Item|Items]]
- For finding the set of items, we need to find the [[Sem 5/Compiler Design/Concepts#Closure of set of items|Closure]]

| Stack      | Input Buffer | Action               | Set of Items           |
| ---------- | -----------: | -------------------- | ---------------------- |
| $0         |      id=num$ | shift                | {S -> .L=R, L -> .id}  |
| $0id1      |        =num$ | Reduce by L -> id    | {L -> id.}             |
| $0L2       |        =num$ | shift                | {S -> L.=R}            |
| $0L2=3     |         num$ | shift                | {S -> L=.R, R -> .num} |
| $0L2=3num4 |            $ | Reduce by R -> num   | {R -> num.}            |
| $0L2=3R5   |            $ | Reduce by S -> L = R | {S -> L=R.}            |
| $0S6       |            $ | accept               |                        |
- For this, we first [[Sem 5/Compiler Design/Concepts#Augmented Grammar|Augment the Grammar]], and then construct the [[Sem 5/Compiler Design/Concepts#LR(0) Automaton|LR(0) Automaton]] using [[Sem 5/Compiler Design/Concepts#Closure of set of items|Closure]] and [[Sem 5/Compiler Design/Concepts#Goto|Goto]] sets
#### LR(k) parsing
- L -> left to right scanning of input
- R -> Right most derivation (in reverse)
- k -> k symbol look ahead

- This method is more powerful that LL(k) parsing technique
	- LL(k): must predict the production based on k look-ahead symbols
	- LR(k): reduction after seeing the entire right hand side of a production and k look-ahead symbols

- LR(0) grammars are too weak to be useful

###### LR(0) parser generation
---
>[!info] Computing canonical collection of sets of LR(0) items
>Input -> An augmented grammar with start symbol S'
>Output -> the canonical collection of sets of LR(0) items
>```Python
>C = Closure({S' -> .S})
>repeat 
>	for each set I in C:
>		for each item A -> α.Xβ in I:
>			J = Goto(I, X)
>			C = C ∪ J
>until no new sets are added to C
>```

- Compute C = $I_0, I_1, I_2, ...I_n$, the canonical collection of sets of LR(0) items
- Each set $I_i$ in C, represents a parser state i
- The initial state of the parser is $I_0$ = Closure({S' -> .S})
- For each state i:
	- for each terminal a, if Goto($I_i$, a) = $I_j$, set parser action as shift a and move to state j
	- for each non-terminal A, if Goto($I_i$, A) = $I_j$, set parser action as move to state j
	- if $I_i$ has an item, A -> α., A != S', set parser action as reduce α to A
	- if $I_i$ has an item, S' -> S., set parser action as accept for input symbol $

Using this, we create a [[Sem 5/Compiler Design/Concepts#LR(0) Parsing Table|LR(0) Parsing Table]]
#### SLR (Simple LR parser)
- More powerful than LR(0) parser
- Reduction A -> α only when look ahead is in FOLLOW(A)

>[!example] 
>0: S' -> S
>1: S -> id = R
>2: R -> id + id
>3: R -> id
>
>FOLLOW(S') = {\$}
>FOLLOW(S) = {\$}
>FOLLOW(R) = {\$}

| State | id  | =   | +   | $   | S   | R   |
| ----- | --- | --- | --- | --- | --- | --- |
| 0     | s1  |     |     |     | 6   |     |
| 1     |     | s2  |     |     |     |     |
| 2     | s3  |     |     |     |     | 7   |
| 3     |     |     | s4  | r3  |     |     |
| 4     | s5  |     |     |     |     |     |
| 5     |     |     |     | r2  |     |     |
| 6     |     |     |     | acc |     |     |
| 7     |     |     |     | r1  |     |     |
- Previously, we were having a shift reduce conflict at T\[3, +], but now, it is gone
- So this grammar is SLR(1) or just SLR

>[!example] 
>0: S' -> S
>1: S -> L = R
>2: L -> id
>3: R -> num
>
>FOLLOW(S') = {\$}
>FOLLOW(S) = {\$}
>FOLLOW(L) = {=}
>FOLLOW(R) = {\$}

| State | id  | =   | num | $   | L   | R   | S   |
| ----- | --- | --- | --- | --- | --- | --- | --- |
| 0     | s3  |     |     |     | 2   |     | 1   |
| 1     |     |     |     | acc |     |     |     |
| 2     |     | s4  |     |     |     |     |     |
| 3     |     | r2  |     |     |     |     |     |
| 4     |     |     | s6  |     |     | 5   |     |
| 5     |     |     |     | r1  |     |     |     |
| 6     |     |     |     | r3  |     |     |     |

>[!example] 
>0: E' -> E
>1: E -> T + E
>2: E -> T
>3: T -> id
>
>FOLLOW(E') = {\$}
>FOLLOW(E) = {\$}
>FOLLOW(T) = {+, \$}
- If we were doing LR(0),

| State | id  | +     | $   | E   | T   |
| ----- | --- | ----- | --- | --- | --- |
| 0     | s1  |       |     | 5   | 2   |
| 1     | r3  | r3    | r3  |     |     |
| 2     | r2  | s3/r2 | r2  |     |     |
| 3     | s1  |       |     | 4   | 2   |
| 4     | r1  | r1    | r1  |     |     |
| 5     |     |       | acc |     |     |
- When we do SLR

| State | id  | +   | $   | E   | T   |
| ----- | --- | --- | --- | --- | --- |
| 0     | s1  |     |     | 5   | 2   |
| 1     |     | r3  | r3  |     |     |
| 2     |     | s3  | r2  |     |     |
| 3     | s1  |     |     | 4   | 2   |
| 4     |     |     | r1  |     |     |
| 5     |     |     | acc |     |     |

>[!example] 
>0: S' -> S
>1: S -> L = R
>2: S -> R
>3: L -> \*R
>4: L -> id
>5: R -> L
>
>FOLLOW(S') = {\$}
>FOLLOW(S) = {\$}
>FOLLOW(R) = {\$, =}
>FOLLOW(L) = {=, $}
- If we do LR(0)

| State | id  | \*  | =     | $      | S   | L   | R   |
| ----- | --- | --- | ----- | ------ | --- | --- | --- |
| 0     | s5  | s4  |       |        | 1   | 2   | 3   |
| 1     |     |     |       | accept |     |     |     |
| 2     | r5  | r5  | r5/s6 | r5     |     |     |     |
| 3     | r2  | r2  | r2    | r2     |     |     |     |
| 4     | s5  | s4  |       |        |     | 8   | 7   |
| 5     | r4  | r4  | r4    | r4     |     |     |     |
| 6     | s5  | s4  |       |        |     | 8   | 9   |
| 7     | r3  | r3  | r3    | r3     |     |     |     |
| 8     | r5  | r5  | r5    | r5     |     |     |     |
- When we do SLR

| State | id  | \*  | =     | $      | S   | L   | R   |
| ----- | --- | --- | ----- | ------ | --- | --- | --- |
| 0     | s5  | s4  |       |        | 1   | 2   | 3   |
| 1     |     |     |       | accept |     |     |     |
| 2     |     |     | r5/s6 | r5     |     |     |     |
| 3     |     |     |       | r2     |     |     |     |
| 4     | s5  | s4  |       |        |     | 8   | 7   |
| 5     |     |     | r4    | r4     |     |     |     |
| 6     | s5  | s4  |       |        |     | 8   | 9   |
| 7     |     |     | r3    | r3     |     |     |     |
| 8     |     |     | r5    | r5     |     |     |     |
| 9     |     |     |       | r1     |     |     |     |
- Even though we tried SLR, we still have a shift reduce conflict

>[!example] 
>0: S' -> S
>1: S -> L = L
>2: S -> id
>3: L -> id
>
>FOLLOW(S') = {\$}
>FOLLOW(S) = {\$}
>FOLLOW(L) = {=, \$}

| State | id  | =   | $      | S   | L   |
| ----- | --- | --- | ------ | --- | --- |
| 0     | s3  |     |        | 1   | 2   |
| 1     |     |     | accept |     |     |
| 2     |     | s4  |        |     |     |
| 3     |     | r3  | r2/r3  |     |     |
| 4     | s6  |     |        |     | 5   |
| 5     |     |     | r1     |     |     |
| 6     |     | r3  | r3     |     |     |
- Since the $ is in both FOLLOW(S) and FOLLOW(L), there is a reduce reduce conflict in T\[3, \$]
- So, this is not SLR
#### LR(1)
- LR parsing with one symbol look ahead. 
- more powerful that LR(0) and SLR
- also known as canonical LR or just LR
- uses [[Sem 5/Compiler Design/Concepts#LR(1) Items|LR(1) Items]]

>[!example] 
>0: S' -> S
>1: S -> AA
>2: A -> aA
>3: A -> b

| State | a   | b   | $   | A   | S   |
| ----- | --- | --- | --- | --- | --- |
| 0     | s5  | s2  |     | 3   | 1   |
| 1     |     |     | acc |     |     |
| 2     | r3  | r3  |     |     |     |
| 3     | s8  | s4  |     | 7   |     |
| 4     |     |     | r3  |     |     |
| 5     | s5  | s2  |     | 6   |     |
| 6     | r2  | r2  |     |     |     |
| 7     |     |     | r1  |     |     |
| 8     | s8  | s4  |     | 9   |     |
| 9     |     |     | r2  |     |     |
>[!example] 
>0: S' -> S
>1: S -> L = R
>2: S -> R
>3: L -> \*R
>4: L -> id
>5: R -> L

| State | \*  | id  | =   | $   | S   | L   | R   |
| ----- | --- | --- | --- | --- | --- | --- | --- |
| 0     | s4  | s5  |     |     | 1   | 2   | 3   |
| 1     |     |     |     | acc |     |     |     |
| 2     |     |     | s6  | r5  |     |     |     |
| 3     |     |     |     | r2  |     |     |     |
| 4     | s4  | s5  |     |     |     | 8   | 7   |
| 5     |     |     | r4  | r4  |     |     |     |
| 6     | s11 | s12 |     |     |     | 10  | 9   |
| 7     |     |     | r3  | r3  |     |     |     |
| 8     |     |     | r5  | r5  |     |     |     |
| 9     |     |     |     | r1  |     |     |     |
| 10    |     |     |     | r5  |     |     |     |
| 11    | s11 | s12 |     |     |     | 10  | 13  |
| 12    |     |     |     | r4  |     |     |     |
| 13    |     |     |     | r3  |     |     |     |
#### LALR Parsing
- Look ahead LR parsing
- it has fewer number of states compared to LR(1) parser
- it combines the LR(1) states having set of items with the same first component(core)
- most of the programming languages can be expressed using LALR grammar
- We identify the states with same core but different look ahead in LR(1) and merge them
- Merging states might result in conflicts
	- After merging, there won't be shift-reduce conflict
	- But there can be reduce-reduce conflict
- for correct inputs, LALR works same as LR(1)
- for erroneous inputs, LALR may not find the error as fast as LR(1), but will eventually find the error
