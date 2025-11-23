# **1. DIVIDE AND CONQUER**

### **Algorithms Explicitly Taught in CLRS**
- [ ] **Merge Sort**
- [ ] **Binary Search**
- [ ] **Strassen’s Matrix Multiplication**
- [ ] **Maximum Subarray Problem** (Divide and Conquer version)
- [ ] **Quicksort** (partition + recursive structure)
- [ ] **Randomized Quicksort**
- [ ] **Finding the Median / Selection (Randomized Select)**
- [ ] **Closest Pair of Points** (Computational Geometry)
### **Exercise / Text Problems (important)**
- [ ] Convolution / Polynomial Multiplication using Divide and Conquer
- [ ] Inversions counting using Merge Sort
- [ ] Karatsuba Multiplication (in exercises)
- [ ] Divide & conquer integer multiplication variants
- [ ] Majority element using divide & conquer
- [ ] Skyline problem (appears later but D&C method is applicable)
---
# **2. GREEDY ALGORITHMS**
(Chapter 16 of CLRS)
### **Main Algorithms**
- [ ] **Activity-Selection Problem** (classic greedy)
- [ ] **Fractional Knapsack**
- [ ] **Huffman Coding Algorithm**
- [ ] **Minimum Spanning Tree Algorithms**
    - [ ] **Prim’s Algorithm**
    - [ ] **Kruskal’s Algorithm**
- [ ] **Dijkstra’s Single Source Shortest Path Algorithm** (non-negative weights only; greedy method)
- [ ] **Task Scheduling / Interval Scheduling Problems**
- [ ] **Matroid Theory Application Examples** (important conceptual topic)
### **Exercise Problems (frequently asked)**
- [ ] Scheduling to **minimize lateness**
- [ ] Scheduling with **deadline and profits** (job sequencing)
- [ ] Optimal caching strategies (page replacement)
- [ ] Making change using greedy vs non-greedy (counterexamples)
- [ ] Greedy for minimum number of platforms at station
- [ ] Greedy for minimizing number of refueling stops
- [ ] Greedy for minimizing sum of weighted completion times
- [ ] Greedy set cover (appears in approximation chapter too)
---
# **3. OPTIMAL SUBSTRUCTURE**
(Scattered concept, but appears in Greedy + DP chapters)
### Algorithms illustrating Optimal Substructure:
- [ ] **Activity Selection**
- [ ] **Shortest Path algorithms** (Dijkstra, Bellman-Ford)
- [ ] **Rod Cutting**
- [ ] **Matrix Chain Multiplication**
- [ ] **Longest Common Subsequence**
- [ ] **Optimal Binary Search Tree**
- [ ] **Knapsack (0–1 and Fractional)**
- [ ] **Edit Distance / Dynamic Time Warping**
- [ ] **Floyd–Warshall** (All-Pairs Shortest Path)
- [ ] **Binary Search Tree optimality proofs**
### Exercises involving optimal substructure:
- [ ] Maximum-weight independent set on path (DP)
- [ ] Bitonic Tour
- [ ] Longest Increasing Subsequence
- [ ] Interval scheduling variants
- [ ] Weighted interval scheduling
---
# **4. DYNAMIC PROGRAMMING**
(Chapter 15 of CLRS + several later chapters)
### Core DP Algorithms in CLRS
- [ ] **Rod Cutting**
- [ ] **Matrix Chain Multiplication**
- [ ] **Longest Common Subsequence (LCS)**
- [ ] **Optimal Binary Search Trees**
- [ ] **0–1 Knapsack**
- [ ] **Subset-Sum**
- [ ] **Edit Distance (Levenshtein)**
- [ ] **Floyd–Warshall** for All Pairs Shortest Path
- [ ] **Bellman–Ford** for Single Source Shortest Path
- [ ] **Bitonic Euclidean TSP (DP version)**
- [ ] **Dynamic Programming Optimal BST Construction**
### Additional DP exercise/problems:
- [ ] Weighted Interval Scheduling
- [ ] Longest Increasing Subsequence
- [ ] Maximum Subarray (Kadane’s algorithm comparison)
- [ ] Coin Change (min coins DP)
- [ ] Word Break (DP)
- [ ] Palindrome Partitioning / Longest Palindromic Subsequence
- [ ] Egg Dropping DP
- [ ] Boolean Parenthesization
- [ ] Cutting Sticks problem
- [ ] DP for polygon triangulation
- [ ] DP for knapsack with repetition
---
# **5. PROBLEM CLASSES & NP COMPLETENESS**
(Chapter 34)
### Standard NP-complete problems listed in CLRS
- [ ] **Circuit-SAT**
- [ ] **SAT**
- [ ] **3-SAT**
- [ ] **Vertex Cover**
- [ ] **Clique**
- [ ] **Independent Set**
- [ ] **Hamiltonian Cycle**
- [ ] **Traveling Salesman Problem (TSP decision version)**
- [ ] **Subset-Sum**
- [ ] **Partition Problem**
- [ ] **Knapsack (decision version)**
- [ ] **Graph Coloring**
- [ ] **Set Cover (decision version)**
- [ ] **Exact Cover**
- [ ] **Hitting Set**
### Exercises include reductions among:
- [ ] 3-SAT → Clique
- [ ] Clique → Independent Set
- [ ] Vertex Cover → Node Cover variants
- [ ] SAT → 3-SAT conversion
- [ ] Subset-Sum → Partition
- [ ] Partition → Scheduling Problems
These are **very often exam questions**: “Show problem X is NP-complete.”
---
# **6. APPROXIMATION ALGORITHMS**
(Chapter 35)
### Algorithms explicitly taught
- [ ] **Vertex Cover 2-Approximation**
- [ ] **Set Cover O(log n)-approximation (Greedy)**
- [ ] **TSP 2-Approximation for Metric TSP using MST doubling**
- [ ] **Christofides Algorithm (3/2-approx)** (mentioned in exercises/notes)
- [ ] **Subset-Sum approximation scheme (pseudo-poly vs FPTAS)**
- [ ] **Knapsack FPTAS**
- [ ] **MAX-SAT approximation**  
    – **Primal–Dual Method** (high level introduction in exercises)
### Exercise approximation problems:
- [ ] Approx for scheduling to minimize lateness / tardiness
- [ ] Approx for independent set (greedy)
- [ ] Approx for set packing
- [ ] Approx for bin packing (First Fit and Best Fit Decreasing)
- [ ] Approx Steiner Tree variants
- [ ] Approx Vertex Coloring heuristic
---
# ⭐ **WHAT YOU MUST KNOW FOR YOUR EXAM (CONSOLIDATED LIST)**
### **Divide and Conquer**
- [x] Merge Sort
- [x] Quick Sort
- [x] Maximum Subarray
- [x] Strassen’s Algorithm
- [ ] Randomized Select
- [x] Closest Pair of Points
### **Greedy**
- [x] Activity Selection
- [x] Fractional Knapsack
- [x] Huffman Coding
- [x] Kruskal, Prim
- [x] Dijkstra
- [x] Job Sequencing (with deadlines & profits)
### **Dynamic Programming**
- [ ] Rod Cutting
- [x] Matrix Chain Multiplication
- [x] LCS
- [ ] Optimal BST
- [x] 0–1 Knapsack
- [x] Subset Sum
- [x] Floyd–Warshall
- [x] Bellman–Ford
### **NP Completeness**
- [ ] SAT, 3-SAT
- [ ] CLIQUE
- [ ] Independent Set
- [ ] Vertex Cover
- [ ] Subset Sum
- [ ] Partition
- [ ] Hamiltonian Cycle
- [ ] TSP
### **Approximation**
- [x] 2-approx Vertex Cover
- [x] MST-based TSP approximation
- [x] Greedy Set Cover
- [x] Knapsack FPTAS


