>[!important] 
> OUT(ENTRY) = $\phi$
> OUT(B) = $U, \forall B \ne ENTRY$
> OUT(B) = $gen_B \cup(IN(B) - kill_B)$
> IN(B) = $\cap_{P}\{OUT(P)\}, \text{P are the predecessors of B}$

#### Reaching Definitions

> [!info]
> OUT(B) = $gen_B \cup(IN(B) - kill_B)$
> IN(B) = $\cup_{P}\{OUT(P)\}, \text{P are the predecessors of B}$

