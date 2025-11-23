##### Rod Cutting
##### Matrix Chain Multiplication
$$
\text{Total number of possible parathesisation = }\frac{\binom{2n}{n}}{n + 1}
$$
```Java
static int memo(int[] dim, int i, int j, int[][] dp) {
        if (i == j) return 0;
        
        if (dp[i][j] != -1) return dp[i][j];
        
        int cost = Integer.MAX_VALUE;
        for (int k = i; k < j; k++) {
            int currentCost = memo(dim, i, k, dp)
                + memo(dim, k + 1, j, dp)
                + dim[i - 1] * dim[k] * dim[j];
            
            cost = Math.min(cost, currentCost);
        }
        
        return dp[i][j] = cost;
    }
    
    static int tabulation(int[] dim) {
        int n = dim.length;
        
        int[][] dp = new int[n][n];
        
        for (int L = 2; L < n; L++) {
            for (int i = 1; i < n - L + 1; i++) {
                int j = i + L - 1;
                
                dp[i][j] = Integer.MAX_VALUE;
                
                for (int k = i; k < j; k++) {
                    dp[i][j] = Math.min(
                        dp[i][j], 
                        dp[i][k] + dp[k + 1][j] + dim[i - 1] * dim[k] * dim[j]
                    );
                }
            }
        }
        
        return dp[1][n - 1];
    }
    
    static int matrixMultiplication(int arr[]) {
        // code here
        int n = arr.length;
        int[][] dp = new int[n][n];
        
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }
        
        return tabulation(arr);
        // return memo(arr, 1, n - 1, dp);
    }
```
##### LCS
```Java
static int memo(String s1, String s2, int i, int j, int[][] dp) {
        if (i < 0 || j < 0) return 0;
        
        if (dp[i][j] != -1) return dp[i][j];
        
        if (s1.charAt(i) == s2.charAt(j)) {
            return dp[i][j] = memo(s1, s2, i - 1, j - 1, dp) + 1;
        }
        
        int left = memo(s1, s2, i - 1, j, dp);
        int right = memo(s1, s2, i, j - 1, dp);
        
        return dp[i][j] = Math.max(left, right);
    }
    
    static int tabulation(String s1, String s2, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        
        return dp[m][n];
    } 
    
    static int lcs(String s1, String s2) {
        // code here
        int m = s1.length();
        int n = s2.length();
        
        // int[][] dp = new int[m][n];
        // for (int[] row: dp) {
        //     Arrays.fill(row, -1);
        // }
        // return memo(s1, s2, m - 1, n - 1, dp);
        
        return tabulation(s1, s2, m, n);
    }
```
##### Optimal BST
##### 0-1 Knapsack
```Java
public int memo(int[] val, int[] wt, int rem, int i, int[][] dp) {
        if (i < 0) return 0;
        if (rem <= 0) return 0;
        
        if (dp[i][rem] != -1) return dp[i][rem];
        
        int take = Integer.MIN_VALUE;
        if (wt[i] <= rem) {
            take = val[i] + memo(val, wt, rem - wt[i], i - 1, dp);    
        }
        
        int notTake = memo(val, wt, rem, i - 1, dp);
        
        return dp[i][rem] = Math.max(take, notTake);
    }
    
    public int tabulation(int[] val, int[] wt, int W) {
        int n = val.length;
        int[][] dp = new int[n + 1][W + 1];
        
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= W; j++) {
                int take = 0;
                if (wt[i - 1] <= j) {
                    take = val[i - 1] + dp[i - 1][j - wt[i - 1]];
                }
                int notTake = dp[i - 1][j];
                
                dp[i][j] = Math.max(take, notTake);
            }
        }
        
        return dp[n][W];
    }
    
    public int knapsack(int W, int val[], int wt[]) {
        // code here
        // int n = val.length;
        // int[][] dp = new int[n][W + 1];
        
        // for (int[] row: dp) {
        //     Arrays.fill(row, -1);
        // }
        
        // return memo(val, wt, W, n - 1, dp);
        return tabulation(val, wt, W);
    }
```

>[!tip] Time Complexity
>O(nW)
##### Subset Sum
```Java
static Boolean memo(int[] arr, int i, int rem, int[][] dp) {
        if (i < 0 || rem < 0) return rem == 0;
        if (rem == 0) return true;
        
        if (dp[i][rem] != -1) return dp[i][rem] == 1;
        
        boolean take = memo(arr, i - 1, rem - arr[i], dp);
        boolean notTake = memo(arr, i - 1, rem, dp);
        
        dp[i][rem] = take || notTake ? 1 : 0;
        return take || notTake;
    }
    
    static Boolean tabulation(int[] arr, int sum) {
        int n = arr.length;
        boolean[][] dp = new boolean[n + 1][sum + 1];
        
        for (int i = 0; i < n; i++) {
            dp[i][0] = true;
        }
        
        for (int i = 1; i <= n; i++) {
            for (int s = 1; s <= sum; s++) {
                boolean notTake = dp[i - 1][s];
                boolean take = false;
                
                if (s - arr[i - 1] >= 0) take = dp[i - 1][s - arr[i - 1]];
                
                dp[i][s] = take || notTake;
            }
        }
        
        return dp[n][sum];
    }

    static Boolean isSubsetSum(int arr[], int sum) {
        // code here
        // int n = arr.length;
        // int[][] dp = new int[n + 1][sum + 1];
        // for (int[] row: dp) {
        //     Arrays.fill(row, -1);
        // }
        
        // return memo(arr, n - 1, sum, dp);
        return tabulation(arr, sum);
    }
```

>[!tip] Time Complexity
>O(n^2)
##### Floyd Warshall
```Java
public void floydWarshall(int[][] dist) {
	int n = dist.length;
	int INF = 100_000_000;
	
	for (int k = 0; k < n; k++) {
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				if (dist[i][k] == INF || dist[k][j] == INF) continue;
				dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
			}
		}
	}
}
```

>[!tip] Time Complexity
>O(n^3)
##### Bellman Ford
```Java
public int[] bellmanFord(int V, int[][] edges, int src) {
	int INF = 100_000_000;
	
	int[] dist = new int[V];
	Arrays.fill(dist, INF);
	dist[src] = 0;
	
	for (int i = 0; i < V - 1; i++) {
		for (int[] edge: edges) {
			int u = edge[0], v = edge[1], weight = edge[2];
			
			if (dist[u] != INF && dist[u] + weight < dist[v]) {
				dist[v] = dist[u] + weight;
			}
		}
	}
	
	for (int[] edge: edges) {
		int u = edge[0], v = edge[1], weight = edge[2];
			
		if (dist[u] != INF && dist[u] + weight < dist[v]) {
			return new int[] {-1};
		}
	}
	
	return dist;
}
```

>[!tip] Time Complexity
>O(VE)

