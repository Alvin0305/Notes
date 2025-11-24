- Machine Independent Optimizations
	- Code Improving Transformations done in the intermediate code to generate better machine code (that runs faster / or takes less space)
- Machine Dependent Optimizations - transformations done in the target code

#### Local and Global Optimizations
- Local Optimizations - restricted to portion of code known as basic blocks (a sequence of instructions such that flow of control always enters the first instruction and exits only after executing the last instruction).
- Global Optimization - extend beyond basic blocks, but confined to an individual procedure - uses data flow analysis to retrieve the required information.

![[Common Sub-Expression Elimination]]

![[Copy Propagation]]

![[Dead Code Elimination]]

![[Simple Transformations]]

![[Constant Propagation]]

![[Constant Folding]]

