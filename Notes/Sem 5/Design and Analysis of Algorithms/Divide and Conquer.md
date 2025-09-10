### Multiplication of Complex Numbers
---
$$(a + bi) * (c + di) = (ac - bd) + (ad + bc)i$$
- This contains 4 multiplications
- Multiplication is more expensive than addition, So we try to reduce the number of multiplications (which may increase the number of additions)
$$
\begin{aligned}
P_1 &= ac \\
P_2 &= bd \\
P_3 &= (a + b)(c + d) \\
\therefore \text{Real Part} &= P_1 - P_2 \\
\text{Imaginary Part} &= P_3 - P_1 - P_2
\end{aligned}
$$
- By this method, we reduce the number of multiplications from 4 to 3. But the number of additions increased from 2 to 5, which is not much expensive compared to multiplications
### Multiplication of 2 Numbers
---
- Multiplication of two numbers in naive method results in $O(n^2)$ time
- Multiplication of two numbers can also be done efficiently by divide and conquer. In each step we split the number in middle. And we multiply them only if the numbers get reduced to one bit (In practical computers, there is no need to divide them till 1 bit, most of the computers can handle 16-bit or 32-bit multiplications as a single operation)
$$
\begin{aligned}
X &= 2^{n/2}*X_L + X_R \\
Y &= 2^{n/2}*Y_L + Y_R \\
\end{aligned}
$$
- The power of 2, shifts the left part n/2 positions and then the right part is added to it resulting in the original number
##### Naive method
$$
\begin{aligned}
XY &= (2^{n/2}*X_L + X_R) * (2^{n/2}*Y_L + Y_R) \\
&= 2^nX_LY_L + (X_RY_L + X_LY_R)\cdot 2^{n/2} + X_RY_R \\
\end{aligned}
$$
- This needs 4 multiplications
- And each time we are splitting the number in half
- => T(n) = 4T(n / 2) + O(n)
- => T(n) = $O(n^2)$
##### Better method
- Here we need to compute $X_RY_L + X_LY_R$ which can be rewritten as:
$$
\begin{aligned}
X_RY_L + X_LY_R &= (X_L + X_R)\cdot(Y_L + Y_R) - X_LY_L - X_RY_R \\
\end{aligned}
$$
- This looks like we have increased the number of multiplications from 2 to 3, but we can see that, the $X_LY_L$ term and $X_RY_R$ term we have already computed. Which means, we reduced the number of multiplications from a total of 4 to 3
$$
\begin{aligned}
P_1 &= X_LY_L \\
P_2 &= X_RY_R \\
P_3 &= (X_L + X_R)(Y_L + Y_R) \\
X\cdot Y &= 2^nP_1 + 2^{n/2}P_3 + P_2 \\
\end{aligned}
$$
- This way we reduced the number of multiplications from 4 to 3, Also since we are splitting the number is half, the number of bits being multiplied at a time is also reduced

```Python
def multiply(x, y):
	if n == 1:
		return x * y
	X_L = left half of x
	X_R = right half of x
	Y_L = left half of y
	Y_R = right half of Y
	
	P_1 = multiply(X_L, Y_L)
	P_2 = multiply(X_R, Y_R)
	P_3 = multiply(X_L + X_R, Y_L + Y_R)
	
	return 2^n * P_1 + 2^(n/2) * P_3 + P_2
```

- Since, there are 3 multiplications and each at each step we are splitting the number in halves
- => T(n) = 3T(n/2) + O(n)
- => T(n) = $O(n^{\log_{2}(3)}) = O(n^{1.585})$
- Which is better than $O(n^2)$

>[!question] Find the k-th smallest element in an unsorted Array
### Naive Solution
---
- To solve the k-th smallest element solution, first approach would be sorting and taking the k-th element from the sorted array
- But this will take O(nlogn) time since we need to sort the array
### Quick Select
---
- A better way of solving the above question is using Quick select
- The concept of quick select is:
	- We pick a pivot from the array and partition the array so that 
		- All elements in the left is smaller than the pivot
		- All elements in the right is greater than the pivot
	- After partition:
		- If the index of pivot == k, then pivot is the answer
		- If the index of pivot < k, recursively search the right part
		- If the index of pivot > k, recursively search the left part
- Partioning the array takes O(n) time
- Best time complexity: O(n)
- Worst time complexity: $O(n^2)$ => If the pivot we take always lies in the starting or ending of the array after partitioning, then it will take $O(n^2)$ time

```Python
def quick_select(arr, left, right, k):
	if left == right:
		return arr[left]
	
	pivotIndex = partition(arr, left, right)
	
	if pivotIndex == k:
		return arr[pivotIndex]
	elif k < length:
		return quick_select(arr, left, pivotIndex - 1, k)
	else:
		return quick_select(arr, pivotIndex + 1, right, k)
		
def partition(arr, left, right):
	pivot = arr[right]
	i = left
	
	for j in range(left, right):
		if arr[j] <= pivot:
			arr[i], arr[j] = arr[j], arr[i]
			i += 1
	arr[i], arr[right] = arr[right], arr[i]
	return i
```

### Median of Median's Approach
---
- The problem with the previous QuickSelect example was the choosing of pivot. If we choose wrong pivot all the time, we get quadratic time, where as if we choose the right pivot, we get the answer in linear time. 
- Instead of picking a pivot randomly (like QuickSelect does), we **carefully pick a "good pivot"** that ensures the array shrinks fast enough
- Steps:
	- Divide the array into groups of 5 each (last group may contain fewer elements)
	- Find the median of each group (constant time, since each group contains at max 5 elements)
	- Recursively find the median of these medians -> this will be our pivot
	- Partition the array around this pivot (like QuickSelect)
	- Depending on where the pivot lands:
		- if pivot's position == k: return pivot
		- if pivot's position < k: recurse right
		- else: recurse left

>[!question] Why is this efficient?
>- In each group of 5, at least 3 elements will be >= group median and at least 3 elements will be <= group median
>- Since we take the median of these medians, at least half of the medians are >= pivot
>- This means at least 30% of all elements are >= pivot and at least 30% <= pivot
>- So the pivot is never too skewed -> We remove at least 30% of the elements in each partition

>[!important] Complexity Analysis
>- Step 1: Splitting the array into groups of 5: O(n)
>- Step 2: Finding the median of each group: O(n) (constant time per group)
>- Step 2: Recursively find the medians: T(n/5)
>- Step 3: Partition around pivot: O(n)
>- Step 4: Recurse into at most 70% of the array: T(7n/10)
>
>So, T(n) = T(n/5) + T(7n/10) + O(n)
>=> T(n) <= T(n/5) + T(7n/10) + dn for some d > 0
>We assume T(n) <= cn for some constant c and try to prove it by induction
>=> $T(n) \le c\frac{n}{5} + c\frac{7n}{10} + dn$
>=> $T(n) \le cn(\frac{1}{5} + \frac{7}{10} + \frac{d}{c})$
>=> $T(n) \le cn(\frac{2}{10} + \frac{7}{10} + \frac{d}{c})$
>=> $T(n) \le cn(\frac{9}{10} + \frac{d}{c})$
>Take c >= 10d
>=> $T(n) \le cn(\frac{9}{10} + \frac{1}{10})$
>=> $T(n) \le cn\frac{10}{10}$
>=> $T(n) \le cn$
>=> taking appropriate value for d gives as T(n) = O(n)
>
>Base case:
>- T(5) <= 10 i.e., T(n) <= 5c for all c >= 2
>Time complexity can be proved using [[Useful Theorems#Extended Masters Theorem|Extended Masters Theorem]] also

### Maximum sub-array sum
---
Given an array of integers (can contain positives and negatives), find the contiguous sub-array with the maximum sum
#### Brute-force method
- Find all sub-arrays of the given array
- Find the maximum sum among the sub-arrays
- Generating all sub-arrays need $O(n^3)$ time
- Therefore complexity is $O(n^3)$
#### Divide and conquer approach
- We split the array into two equal halves
- The maximum sub array can lie in the following three possible places
	- Entirely in the left sub-array
	- Entirely in the right sub-array
	- Partly in the left and partly in the right sub-array
- maxSubArraySum(arr, l, r) = max(maxSubArraySum(arr, l, mid), maxSubArraySum(arr, mid + 1, r), maxCrossingSum(arr, l, mid, r))

Steps:
- Base case: If the array has only one element: return that element
- Divide: Find the middle index "mid".
- Conquer: Recursively find the maximum sub-array sum in the left half and right half
- Combine: Find the maximum sub-array sum crossing the middle
	- Compute maximum sum starting from mid and going left
	- Compute maximum sum starting from mid + 1 and going right
	- Add them to get the crossing sum
- Finally, take the maximum among the three

```Python
def maxSubArraySum(arr, l, r):
	if l == r:
		return arr[l]
		
	mid = (l + r) // 2
	
	left_sum = maxSubArraySum(arr, l, mid)
	right_sum = maxSubArraySum(arr, mid + 1, r)
	cross_sum = maxCrossingSum(arr, l, mid, r)
	
	return max(left_sum, right_sum, cross_sum)
	
def maxCrossingSum(arr, l, mid, r):
	sum = 0
	left_max = -inf
	for i = mid downTo 1:
		sum += arr[i]
		left_max = max(left_max, sum)
		
	sum = 0
	right_max = -inf
	for i = mid + 2 upto r:
		sum += arr[i]
		right_max = max(right_max, max)
		
	return left_max + right_max
```

>[!info] Time Complexity
>- T(n) = 2T(n/2) + O(n) 
>	- 2 T(n/2) comes from the recursion for left and right sub arrays
>	- O(n) comes is for calculating the maximum crossing sum
>	- From the recurrence, we get T(n) = O(nlogn)
>- Each level of recursion takes O(n) time for finding max crossing sum
>- There is logn number of recursive calls
>- Therefore, total time complexity is O(nlogn)

> [!tip]
> Divide and conquer is giving O(nlogn) time whereas kadane's algorithm gives result in O(n) time
### Counting Inversions
---
Given an array, we want to count the number of inversions
An inversion is a pair of indices (i, j) such that
**i < j and arr\[i] > arr\[j]**

>[!example]
>\[2, 4, 1, 3, 5]
>Inversions:
>- (2, 1), (4, 1), (4, 3) => (Here the numbers are not indices, they are the values)
>- Therefore, Total = 3 inversions 

#### Brute force solution
- Compare every pair of (i, j)
- If arr\[i] > arr\[j] and i < j, increment count
- Since we need to check all the possible pairs of (i, j), it takes $O(n^2)$ time

```Python
def countInverse(arr):
	n = len(arr)
	count = 0
	for i = 0 to n - 2:
		for j = i + 1 to n - 1:
			if arr[i] > arr[j]:
				count += 1
	return count
```

#### Divide and conquer Solution
- We divide the array into left and right half
- Inversion can come in three types:
	- Both indices in left half
	- Both indices in right half
	- One index in left and other in right half

```Python
def countInversions(arr, l, r):
	if l >= r:
		return 0
		
	mid = (l + r) // 2
	
	left_inv = countInversions(arr, l, mid)
	right_inv = countInversions(arr, mid + 1, r)
	cross_inv = mergeAndCount(arr, l, mid, r)
	
	return left_inv + right_inv + cross_inv
	
def mergeAndCount(arr, l, mid, r):
	left = arr[l..mid]
	right = arr[mid + 1..r]
	i = 0
	j = 0
	k = 1
	inv_count = 0
	
	while i < len(left) and j < len(right):
		if left[i] <= right[j]:
			arr[k] = left[i]
			i += 1
		else:
			arr[k] = right[j]
			j += 1
			inv_count += (length(left) - i)
		k += 1
	
	while i < len(left): 
		arr[k] = left[i]
		i += 1
		k += 1
	while j < len(right):
		arr[k] = right[j]
		j += 1
		k += 1
	
	return inv_count
```

- The concept is: when we mergeAndCount, we merge the array in sorted manner so that, when we compare an element in left with an element in right and finds that it is an inversion, then all the elements in the left array after that number will be inversions
- example: 
- left = \[2, 4], right = \[1, 3, 5]
- since (2, 1) is an inversion (4, 1) will also be an inversion, which is not needed to be checked since the arrays will be sorted.
- T(n) = 2T(n/2) + O(n)
- => T(n) = O(nlogn)
