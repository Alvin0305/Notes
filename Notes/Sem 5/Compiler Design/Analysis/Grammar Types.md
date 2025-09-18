### LL(1)

A grammar for which a predictive parsing table can be built with one symbol look-ahead, is said to belong to a class of grammars known LL(1) grammars.
- First L -> input it scanned from left to right
- Second L -> does left most derivation
- 1 -> one symbol look ahead

>[!tip] 
>LL(k) has k look ahead symbols

>[!question] Grammar which is not LL(1)

E -> id + id | id \* id
FIRST(id + id) = {id}
FIRST(id * id) = {id}
FIRST(E) = {id}

|     | id                            |
| --- | ----------------------------- |
| E   | E -> id + id<br>E -> id \* id |
Since, there is two productions in the same cell, this grammar is not suitable for predictive parsing

>[!tip] 
>Conditions for LL(1):
>For productions A -> α | β
>- FIRST(α) and FIRST(β) should be disjoint
>- If ε ∈ FIRST(α), then FIRST(β) should not contain any terminal in FOLLOW(A)
>- If ε ∈ FIRST(β), then FIRST(α) should not contains any terminal in FOLLOW(A)

>[!example] 
>S -> iEtSeS | iEtS | a
>E -> b
>
>FIRST(iEtSeS) = {i}
>FIRST(iEtS) = {i}
>FIRST(a) = {a}
>
>since FIRST(iEtSeS) and FIRST(iEtS) are not disjoint, the grammar is not suitable for predictive parsing

>[!example] 
>S -> iEtSS' | a
>S' -> eS | ε
>E -> b
>
>FIRST(iEtSS') = {i}
>FIRST(eS) = {e}
>FIRST(b) = {b}
>FIRST(E) = {b}
>FIRST(S') = {e, ε}
>FIRST(S) = {i, a}
>
>FOLLOW(E) = {t}
>FOLLOW(S) = {\$, e}
>FOLLOW(S') = {\$, e}
>
>ε ∈ FIRST(ε) and FIRST(eS) and FOLLOW(S') contains e is common => The grammar is not LL(1)

|     | i           | e                   | t   | a      | b      | ε       |
| --- | ----------- | ------------------- | --- | ------ | ------ | ------- |
| S   | S -> iEtSS' |                     |     | S -> a |        |         |
| S'  |             | S' -> eS<br>S' -> ε |     |        |        | S' -> ε |
| E   |             |                     |     |        | E -> b |         |
#### LR(0)
---
- A grammar G belongs to class LR(0), if an [[Sem 5/Compiler Design/Concepts#LR(0) Parsing Table|LR(0) parsing table]] can be built for G(without any conflicting entries in the parsing table)

>[!example] 
>0: E' -> E
>1: E -> E + T
>2: E -> T
>3: T -> id

| State | id  | +   | $   | E   | T   |
| ----- | --- | --- | --- | --- | --- |
| 0     | s3  |     |     | 1   | 2   |
| 1     |     | s4  | acc |     |     |
| 2     | r2  | r2  | r2  |     |     |
| 3     | r3  | r3  | r3  |     |     |
| 4     | s3  |     |     |     | 5   |
| 5     | r1  | r1  | r1  |     |     |

>[!example] 
>0: S' -> S
>1: S -> (L)
>2: S -> x
>3: L -> S
>4: L -> L, S

| State | (   | )   | x   | ,   | $   | S   | L   |
| ----- | --- | --- | --- | --- | --- | --- | --- |
| 0     | s1  |     | s2  |     |     | 3   |     |
| 1     | s1  |     | s2  |     |     | 4   | 5   |
| 2     | r2  | r2  | r2  | r2  | r2  |     |     |
| 3     |     |     |     |     | acc |     |     |
| 4     | r3  | r3  | r3  | r3  | r3  |     |     |
| 5     |     | s6  |     | s7  |     |     |     |
| 6     | r1  | r1  | r1  | r1  | r1  |     |     |
| 7     | s1  |     | s2  |     |     | 8   |     |
| 8     | r4  | r4  | r4  | r4  | r4  |     |     |
- A LR(0) parser can fail, if there is a [[Sem 5/Compiler Design/Concepts#Shift Reduce Conflict|Shift Reduce Conflict]] or [[Sem 5/Compiler Design/Concepts#Reduce Reduce Conflict|Reduce Reduce Conflict]]. 

#### SLR
---
- A grammar G belongs to class SLR, if an [[Sem 5/Compiler Design/Concepts#SLR Parsing Table|SLR parsing table]] can be built for G(without any conflicting entries in the parsing table)


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
|       |     |     |     |     |     |


