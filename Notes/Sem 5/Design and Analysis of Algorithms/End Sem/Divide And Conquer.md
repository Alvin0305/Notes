##### Closest Pair of Points
```java
closestPair(X, Y) {
	n = X.length;
	
	if (n == 2) return dist(X[1], X[2]);
	if (n == 3) min(dist(X[1], X[2]), dist(X[2], X[3]), dist(X[1], X[3]));
	
	mid = X[n / 2];
	
	dl = closestPair(X[1...mid], Y);
	dr = closestPair(X[mid + 1...N], Y);
	d = min(dl, dr);
	
	S = points in Y whose x-coordinates are in the range of [mid.x - d, mid.x + d];
	for i = 1 to S.length:
		for j = 1 to j = 7:
			d = min(d, dist(S[i], S[i + j]))
			
	return d;
}
```

- X => List of Points sorted by X coordinates
- Y => List of Points sorted by Y coordinates

>[!tip] Time Complexity
>O(nlogn)
##### Merge Sort
```Java
public void mergeSort(int[] arr, int low, int high) {
	if (low < high) {
		int mid = (low + high) / 2;
		mergeSort(arr, low, mid);
		mergeSort(arr, mid + 1, high);
		
		merge(arr, low, mid, high);
	}
}

public void merge(int[] arr, int low, int mid, int high) {
	int n1 = mid - low + 1;
	int n2 = high - mid;
	
	int[] left = new int[n1];
	int[] right = new int[n2];
	
	System.arraycopy(arr, low, left, 0, n1);
	System.arraycopy(arr, mid + 1, right, 0, n2);
	
	int i = 0, j = 0, k = low;
	
	while (i < n1 && j < n2) {
		if (left[i] < right[j]) {
			arr[k++] = left[i++];
		} else {
			arr[k++] = right[j++];
		}
	}
	
	while (i < n1) {
		arr[k++] = left[i++];
	}
	
	while (j < n2) {
		arr[k++] = right[j++];
	}
}
```

>[!tip] Time Complexity
>O(nlogn)

##### Quick sort
```Java
public void quickSort(int[] arr, int low, int high) {
	if (low < high) {
		int pivot = partition(arr, low, high);
		
		quickSort(arr, low, pivot - 1);
		quickSort(arr, pivot + 1, high);
	}
}

public int partition(int[] arr, int low, int high) {
	for (int i = low; i < high; i++) {
		if (arr[low] < arr[high]) {
			int temp = arr[i];
			arr[i] = arr[low];
			arr[low] = temp;
			low++;
		}
	}
	
	int temp = arr[low];
	arr[low] = arr[high];
	arr[high] = temp;
	
	return low;
}
```

>[!tip] Time Complexity
>O(nlogn)

##### Strassen's Matrix Multiplication
Base case => 2 \* 2 matrix => Calculate the resulting 2 \* 2 matrix in constant time
divide the matrix into 4 halves and recurse
```txt
MM(A, B, n):
	if n == 2:
		do 2 * 2 multiplication of matrices
	
	C11 = MM(A11, B11, n/2) + MM(A12, B21, n/2)
	C12 = MM(A11, B12, n/2) + MM(A12, B22, n/2)
	C21 = MM(A21, B11, n/2) + MM(A22, B21, n/2)
	C22 = MM(A21, B12, n/2) + MM(A21, B22, n/2)
	
	return C
```

- Here each addition is matrix addition => O(n^2)
- i.e., total complexity is T(n) = 8T(n/2) + O(n^2) => O(n^3)
- Strassen optimized the number of multiplications from 8 to 7 resulting in
	- T(n) = 7T(n/2) + O(n^2) => O(n^2.81)
##### Maximum subarray sum
```Java
public int maxSubarraySum(int[] arr) {
	return solve(arr, 0, arr.length - 1);
}

public int solve(int[] arr, int low, int high) {
	if (low == high) return arr[low];
	
	int mid = (low + high) / 2;
	int left = solve(arr, low, mid);
	int right = solve(arr, mid + 1, high);
	int middle = computeMiddle(arr, low, mid, high);
	
	return Math.max(left, Math.max(right, middle));
}

public int computerMiddle(int[] arr, int low, int mid, int high) {
	int leftBest = Integer.MIN_VALUE;
	int sum = 0;
	
	for (int i = mid; i >= low; i--) {
		sum += arr[i];
		leftBest = Math.max(leftBest, sum);
	}
	
	sum = 0;
	int rightBest = Integer.MIN_VALUE;
	
	for (int i = mid; i < high; i++) {
		sum += arr[i];
		rightBest = Math.max(rightBest, sum);
	}
	
	return leftBest + rightBest;
}
```

