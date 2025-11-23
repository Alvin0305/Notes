- Condensed form of parse tree with non terminal nodes either dropped or replaced by operators
![[AST Parse Tree Difference.png]]

#### SDD to construct syntax tree for expressions

| Production             | Semantic Rule                                              |
| ---------------------- | ---------------------------------------------------------- |
| S -> id = E            | S.node = CreateNode('=', CreateLeaf(id, id.entry), E.node) |
| S -> if (B) S1 else S2 | S.node = CreateNode(ifelse, B.node, S1.node, S2.node)      |
| S -> while (B) S1      | S.node = CreateNode(while, B.node, S1.node)                |
| E -> E1 + E2           | E.node = CreateNode('+', E1.node, E2.node)                 |
| E -> E1 \* E2          | E.node = CreateNode('\*', E1.node, E2.node)                |
| E -> num               | E.node = CreateLeaf(num, num.lexval)                       |
| E -> id                | E.node = CreateLeaf(id, id.entry)                          |
