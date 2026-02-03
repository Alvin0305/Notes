>[!info] Form
>- x = y op z
>- x = op y
>- x = y
>- goto L
>- if x goto L
>- if x relop y goto L
>- x = y\[i]
>- x\[i] = y
>- return y
>- param x
>- call p, n
>- x = &y
>- x =  \*y
>- \*x = y
- At most one operator on the right side

#### Address
- A name in the source program
- A constant
- A compiler generated temporary

>[!note] 
>In the implementation, a name is replaced by a pointer to its Symbol Table entry.

>[!example] x = a + b \* c
>- t1 = b \* c
>- t2 = a + t1
>- x = t2

#### Procedure calls

>[!info] p(x1, x2, x3, ..., xn)
>- param x1
>- param x2
>- ...
>- param x3
>- y = call p, n

- return can be done by `return y`

>[!example] x = f(a + b)
>- t = a + b
>- param t
>- t2 = call f, 1
>- x = t2 

#### Representation

- Quardruples - each instruction as a record with four fields: op - code for operator, arg1, arg2 for operands and result.
- Triples - only three fields for each instruction. Result field is not part of the instruction. An instruction i can use the result of instruction j as operand, by keeping a reference to the position of instruction j.
- Indirect Triples - a list of pointers to triples.

#### 3-address code: Quardruples
- Each instruction as a record with four fields: op - code for operator, arg1, arg2 for operands and result.
	- Instructions with unary operators do not use arg2
	- param uses arg1 alone
	- goto instruction keeps target label in result

#### Addressing Array Elements: 1D
>[!example] A\[i] -> index = base + i \* w
>- t1 = i \* w
>- t2 = A\[t1]

#### Addressing Array Elements: 2D
>[!example] A\[i]\[j] -> index = base + (i \* n + j) \* w
>


#### Addressing Array Element: k-Dimensional
>[!info] 
>Address(A[i1][i2]...[ik]) 
>  = BA + ( i1\*(d2\*d3\*...\*dk)
>  + i2 * (d3\*d4\*...\*dk)
>  + ...
>  + ik-1 \* (dk)
>  + ik ) \* w
### Code Generation

| Production             | Semantic Rules                                                                                                                                                                                         |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| P -> S                 | S.next = newLabel();<br>P.code = S.code \|\| label(S.next)                                                                                                                                             |
| S -> S1 S2             | S1.next = newLabel();<br>S2.next = S.next;<br>S.code = S1.code \|\| label(S1.next) \|\| S2.code                                                                                                        |
| S -> id = E            | S.code = E.code \|\| gen(id.entry '=' E.addr)                                                                                                                                                          |
| s -> if (B) S1         | B.true = newLabel();<br>B.false = S1.next = S.next;<br>S.code = B.code \|\| label(B.true) \|\| S1.code                                                                                                 |
| S -> if (B) S1 else S2 | B.true = newLabel();<br>B.false = newLabel();<br>S1.next = S2.next = S.next;<br>S.code = B.code \|\| label(B.true) \|\| S1.code \|\| gen('goto' S.next) \|\| label(B.false) \|\| S2.code               |
| S -> while (B) S1      | begin = newLabel();<br>B.true = newLabel();<br>B.false = S.next();<br>S1.next = begin;<br>S.code = label(begin) \|\| B.code \|\| label(B.true) \|\| S1.code \|\| gen('goto' begin)                     |
| S -> L = E             | S.code = L.code \|\| E.code \|\| gen(L.array.base \[L.addr] '=' E.addr)                                                                                                                                |
| E -> E1 + E2           | E.addr = newTemp();<br>E.code = E1.code \|\| E2.code \|\| gen(E.addr '=' E1.addr '+' E2.addr);                                                                                                         |
| E -> -E1               | E.addr = newTemp();<br>E.code = E1.code \|\| gen(E.addr '=' '-' E1.addr)                                                                                                                               |
| E -> (E1)              | E.addr = E1.addr;<br>E.code = E1.code;                                                                                                                                                                 |
| E -> id                | E.addr = id.entry;<br>E.code = ''                                                                                                                                                                      |
| E -> L                 | E.addr = newTemp();<br>E.code = L.code \|\| gen(E.addr '=' L.array.base \[L.addr])                                                                                                                     |
| L -> id\[E]            | L.array = id.entry;<br>L.type = L.array.type.elemtype;<br>L.addr = newTemp();<br>L.code = E.code \|\| gen(L.addr '=' E.addr \* L.type.width)                                                           |
| L -> L1\[E]            | L.array = L1.array;<br>L.type = L1.type.elemtype;<br>t = newTemp();<br>L.addr = newTemp();<br>L.code = L1.code \|\| E.code \|\| gen(t '=' E.addr '\*' L.type.width) \|\| gen(L.addr '=' L1.addr '+' t) |
| E -> id(A)             | E.code = A.code \|\| gen(call id.entry, A.n)                                                                                                                                                           |
| A -> E, A1             | A.n = 1 + A1.n;<br>A.code = E.code \|\| gen(param E.addr)                                                                                                                                              |
| A -> $\epsilon$        | A.n = 0;<br>A.code = ''                                                                                                                                                                                |
| B -> E1 relop E2       | B.code = E1.code \|\| E2.code \|\| gen('if' E1.addr relop E2.addr 'goto' B.true) \|\| gen('goto' B.false)                                                                                              |
| B -> true              | B.code = gen('goto' B.true)                                                                                                                                                                            |
| B -> false             | B.code = gen('goto' B.false)                                                                                                                                                                           |
| B -> B1 \|\| B2        | B1.true = B.true;<br>B1.false = newLabel();<br>B2.true = B.true;<br>B2.false = B.false;<br>B.code = B1.code \|\| label(B1.false) \|\| B2.code                                                          |
| B -> B1 && B2          | B1.true = newLabel();<br>B1.false = B.false;<br>B2.true = B.true;<br>B2.false = B.false;<br>B.code = B1.code \|\| label(B1.true) \|\| B2.code                                                          |
| B -> !B                | B1.true = B.false;<br>B1.false = B.true;<br>B.code = B1.code                                                                                                                                           |
| B -> (B1)              | B1.true = B.true;<br>B1.false = B.false;<br>B.code = B1.code                                                                                                                                           |
|                        |                                                                                                                                                                                                        |
- B.true, B.false, S.next are the inherited attributes here
- These might not get filled in the first pass. So we pass the code once more.
- second pass can be avoided using Backpatching
	- We keep the list of unfilled labels so it can be filled later instead of passing again
	- We keep truelists and falselists for each B, and nextlist for each sequence of instructions
	- when we leave an instruction with B1.true, we add that instruction to b1.truelist, so that they can be filled later
```
1. if (x < y) goto B1.true
2. goto B1.false
3. if (x < z) goto B2.true
4. goto B2.false
5. if (a < b) goto B1.true
6. goto B2.true
```
here B1.truelist = {1, 5}
B1.falselist = {2}
B2.truelist = {3, 6}
B2.falselist = {4}
