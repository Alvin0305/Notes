### LL(1)
---
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
