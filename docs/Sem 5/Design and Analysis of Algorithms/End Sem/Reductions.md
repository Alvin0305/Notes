##### Independent Set - Vertex Cover
- Take the complement of the graph of one's input and give to the other
![[Vertex Cover - Independent Set.png]]
##### Set Cover - Vertex Cover
- 1 -> 2 => construct a graph and find the vertex cover
- 2 -> 1 => construct the sets from the graph and find the set cover
![[Set Cover - Vertex Cover.png]]
##### 3 SAT to Independent Set
- In each clause, there will be only 3 variables
- draw graphs from the input
	- for each clause, draw a triangle graph
	- from each triangle, connect `x` to other triangles if `x'`
		- i.e., if there is x in one triangle, draw a edge to all x' in other triangles
- After this, find the independent set of size k in this
	- In each triangle, we can choose only one vertex (it is connected to the other 2 in the triangle)
	- Since, we connected all pairs of x and x', it ensures that x and x' will never be true together
	- For the existence of an independent set of size k in the graph, there should be one vertex in each triangle => One true in each clause
![[Satifiability.png]]
##### 3 SAT - 3 Colorability
![[3 Coloring - 3 SAT.png]]
![[3 Colorability.png]]
##### SAT to 3 SAT
- for clauses of form (l) -> (l or l or l)
- for clauses of form (l1 or l2) -> (l1 or l2 or l2)
- for clauses with length, k > 3
	- add extra k - 3 variables: y1, y2, ...y(k-3)
	- (l1 or l2 or l3 or ... or ln)
	- update the clauses as
		- $(l_1 \lor l_2 \lor y_1)$
		- $(\lnot y_1 \lor l_3 \lor y_2)$
		- $(\lnot y_2 \lor l_4 \lor y_3)$
		- ...
		- $(\lnot y_{k-3} \lor l_{k-1} \lor l_k)$
		- and take the ANDs of them
	- If any of the variable in a clause is true, we will make both the Ys in that clause as false resulting in making all other clauses true
	- If all other variables in a clause is false, we will make the y in the clause as true, making the whole input true only if any of the other clause is true
##### 3 SAT to SAT
- 3 SAT is a special case of SAT
##### SAT - Clique
- Construct a graph 
	- For each literal in each clause, add a vertex
	- Add edges from vertices of one clause to another such that
		- do not add vertex from x to x'
		- add all other possible edges
- In this graph, inside one clause, no vertices are connected
- And between clauses, all pairs other than (x, x') pairs are connected
- If the CNF is satisfiable, then there will be a clique of size k, because
	- We all edges other than x, x' pair and edges within the clause
	- this means, for a clique of size k to exist, each vertex should be taken from each clause. And since there is no (x, x') pair, these chosen vertices are consistent
![[SAT to Clique.png]]
##### 3 SAT to Vertex Cover


