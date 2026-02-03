#### Syntax Directed Translation Scheme (SDT)

- a Context Free Grammar with semantic actions embedded within production bodies
- the order of evaluation of semantic rules is explicitly specified
```mathematica
E → {print(′ +′ )} E + T
E → T
T → 1 {print(′ 1′ )}
```
#### Computing Types and their Widths

- D -> T id {enter(id.lexeme, T.type, T.width)}
- T -> int {T.type = int; T.width = 4}
- T -> float {T.type = float; T.width = 8}

#### Sequence of Declarations

- P -> {offset = 0} D
- D -> D; D
- D -> T id {enter(id.lexeme, T.type, offset); offset += T.width}
- T -> int {T.type = int; T.width = 4}
- T -> float {T.type = float; T.width = 8}

#### Arrays

>[!info] Type expression for arrays
>int\[2] x -> array(2, int) -> width = 2 \* size(int) = 8
>int\[5]\[4] x -> array(5, array(4, int)) -> width = 5 \* 4 \* size(int) = 80

##### 1D Integer Array

T -> int\[num] {T.type = array(num.lexval, int); T.width = num.lexval * 4}

##### General Array Declaration

T -> B {type = B.type; width = B.width} C
B -> int {B.type = int; B.width = 4}
B -> float {B.type = float; B.width = 8}
C -> \[num] C1 {C.type = array(num.lexval, C1.type); C.width = num.lexval \* C2.width}
C -> $\epsilon$ {C.type = type; C.width = width}

