#### Grammar with ε production - LR(0) items
- For a production A -> ε, add item A -> .
- Item A -> . indicates, a reduction by A -> ε
	- push A to stack without popping anything

>[!example] 
>0: A' -> A
>1: A -> aA
>2: A -> ε
#### Parser Configuration
A configuration is a pair ($X_1X_2X_3...X_m, a_ia_{i+1}...a_n$), where $X_1, X_2, ... X_m$ are the symbols in the stack (from bottom to top) and $a_i, a_{i+1}...a_n$ are the input symbols remaining to be read

>[!example] 
>E -> E + id
>E -> id
>i.e., for a string id + id
>E => E + id => id + id

| State |  id  |  +  |  $  |  E  |
| :---: | :--: | :-: | :-: | :-: |
| **0** |  s2  |     |     |  1  |
| **1** |      | s3  | acc |     |
| **2** |      | r2  | r2  |     |
| **3** |  s4  |     |     |     |
| **4** |      | r1  | r1  |     |

- here the initial configuration is {0, id+id$}
- then we push id to the stack moving to state 2 => {0 id 2, +id$}
- then we reduce id by E => {0 E 1, +id$}
- then we push + to the stack moving to state 3 => {0 E 1 + 3, id$}
- then we push id to the stack moving to state state 4 => {0 E 1 + 3 id 4, $}
- now we reduce E + id by E => we pop E, 1, +, 3, id, 4 and push E => {0 E 1, $}
- this results in an accept => final configuration is {0 E 1, $}

| Stack | Input Buffer | Action                |
| ----- | -----------: | --------------------- |
| $     |       id+id$ | Shift                 |
| $id   |         +id$ | Reduce by E -> id     |
| $E    |         +id$ | Shift                 |
| $E+   |          id$ | Shift                 |
| $E+id |            $ | Reduce by E -> E + id |
| $E    |            $ | accept                |
- We can see that if we concatenate the Stack content with the input buffer at any stage, we get the corresponding right sentential form
- Stack contains a prefix of a right sentential form known as viable prefix

