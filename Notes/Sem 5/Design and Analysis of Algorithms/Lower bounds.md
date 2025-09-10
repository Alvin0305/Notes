Uses of lower bounds:
- Helps in design algorithms
- Rules out many algorithms

### Comparison Model
---
- Only operation allowed is comparison
- Result of operation is "yes" or "no"
##### Decision Tree
- Binary tree
- Internal nodes are single comparison
- Leaf nodes are possible outcomes

### Sorting
---
- For an input array of n elements, there are n! possible solutions

>[!example]
>If the array is: \[a1, a2, a3]
>Then, possible solutions include
>- \[a1, a2, a3]
>- \[a1, a3, a2]
>- \[a2, a1, a3]
>- \[a2, a3, a1]
>- \[a3, a1, a2]
>- \[a3, a2, a1]
>- => For 3 element array, there are 6 possibilities => 3!

![[Sorting.canvas]]
- Number of possible solutions = n!
- Numbers of leafs >= n!
- height = Ω(log(n!))
- = Ω(log(n) + log(n - 1) + log(n - 1) + ... + log(2) + log(1))
- By sterling approximation (log(n!) > nlogn - n)
- => height = Ω(nlogn)

### Find maximum in an array
---
##### Naive solution
- Sort the array and take the last element
##### Optimized Approach
- Using Decision Tree
- We take an array A which contains the list of possible results
- At each internal node, we compare two values say a and b, If a < b is true, then we remove a from A. If a < b is false, then we remove b from A
- At each level, we eliminate one element from the result array
- For an array of n elements, we need n - 1 comparisons => Height of tree is n - 1 = Ω(n)

#### Finding both minimum and maximum together in an array
---
##### First approach
```Python
def findMinMax(arr):
	min = max = arr[0]
	for num in arr:
		if num < min:
			min = num
		elif num > max:
			max = num
			
	return (min, max)
```

- The problem here is, there is a loop running n - 1 times and in each loop we do 2 comparisons. Therefore we have minimum 2n - 2 comparisons.
- Asymptotically its Ω(n). But we can reduce the number of comparisons
##### Better Approach
- We keep a MIN array and MAX array each containing all the elements of the input array
- We take pairs of consecutive numbers a and b from the array. 
- Then compare the two numbers
- If a < b:
	- then, a will never be the maximum in the array
	- So, pop out a from MAX
	- Also, b will never be the minimum in the array
	- So, pop out b from MIN
- If a > b:
	- then a will never be the minimum in the array
	- So, pop out a from MIN
	- Also, b will never be the maximum in the array
	- So, pop out b from MAX
- Do this repeatedly. 
- After all the pairs are compared, the MIN and MAX arrays will have n/2 elements each.
- But till now we have done n/2 comparisons only.
- From the MIN array, now we can find the minimum by the first approach with n/2 - 1 comparisons
- Also, from the MAX array, we can find the maximum by the first approach with n/2 - 1 comparisons
- In this way, we can find the result in n/2 + (n/2 - 1) + (n/2 - 1) comparisons
- => 3n/2 - 2 comparisons which is less than 2n - 2

```Python
def findMinMax(arr, n):
	MIN[n/2]
	MAX[n/2]
	k = 0
	
	for i = 0 to n - 1 step 2:
		if arr[i] < arr[i+1]:
			MIN[k] = arr[i]
			MAX[k] = arr[i+1]
		else:
			MIN[k] = arr[i+1]
			MAX[k] = arr[i]
		k += 1
	
	min_val = MIN[0]
	for num in MIN:
		if num < min_val:
			min_val = num
			
	max_val = MAX[0]
	for num in MAX:
		if num > max_val:
			max_val = num
			
	return (min_val, max_val)
```
### Adversary Argument Approach
---
- Algorithm can query the adversary. 
- Adversary need not give the correct answers, but the answers should agree each other. i.e., previous answer should not contradict the next answer. 
- We count the number of queries asked as a measure of time complexity
- Algorithm can asks only yes or no questions
- When the adversary has two options to answer, it will always answer the one which will require more steps to reach the leaf (find the solution)
- When adversary has two options to asks, it will ask the query which will result in reaching the leaf node (finding the solution) in least steps