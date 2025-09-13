- Universal Algorithm => CYK Algorithm
	- Can parse any grammar in $O(n^3)$ time
- Compilers commonly use top down or bottom up parsing methods

## Top Down Parsing
---
![[Top Down Parsing Algorithms]]
## Bottom Up Parsing
---
- Construct parse tree bottum-up, starting from leaves
- Parent node A added to a set of nodes matching the body $\alpha$ of production $A \to \alpha$
- 