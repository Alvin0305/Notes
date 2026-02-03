- Third phase of compiler
- It checks whether the program is meaningful according to the language's rules - even if it is syntactically correct
- The parser ensures the structure(syntax), while semantic analyzer ensures meaning(semantics)
- The program that performs this task is called semantic analyzer
#### Input and output
- Input -> Parse Tree / AST(from Syntax Analyzer) + Symbol Table(from lexical analyzer)
- Output -> Annotated AST(with type information, scope etc) + Updated Symbol Table
#### Responsibilities
1. Type Checking
	- Ensure operands are type compatible
2.  Scope Resolution
	- Ensure variables are declared before use
3. Function Check
	- Verify function calls matches function signature
4. Array and pointer check
	- Checks if we are trying to access elements out if index in an array
