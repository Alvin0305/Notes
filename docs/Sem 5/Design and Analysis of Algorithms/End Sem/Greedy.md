##### Activity Selection
```Java
List<Activity> activitySelection(List<Activity> activities) {
	activities.sort(Comparator.compareInt(a -> a.finish));
	
	Activity last = activites.get(0);
	List<Activity> result = new ArrayList<>();
	result.add(last);
	
	for (int i = 1; i < activities.size(); i++) {
		Activity current = activities.get(i);
		
		if (current.start >= last.finish) {
			result.add(current);
			last = current;
		}
	}
	
	return result;
}
```

>[!tip] Time Complexity
>O(nlogn)
##### Fractional Knapsack
```Java
public double fractionalKnapsack(List<Obj> list, int capacity) {
        int n = list.size();
        
        list.sort((a, b) -> Double.compare(
            (double) b.value / b.weight,
            (double) a.value / a.weight
            ));
        
        double result = 0;
        for (int i = 0; i < n; i++) {
            Obj current = list.get(i);
            
            if (current.weight < capacity) {
                result += current.value;
                capacity -= current.weight;
            } else {
                result += ((double) current.value / current.weight) * capacity;
                break;
            }
        }
        
        return result;
    }
```

>[!tip] Time Complexity
>O(nlogn)
##### Huffman Coding
```Java
public Node huffmanCodes(String S, int f[], int N) {
        PriorityQueue<Node> pq = new PriorityQueue<>((a, b) -> a.freq - b.freq);
        
        for (int i = 0; i < N; i++) {
            pq.offer(new Node(f[i], S.charAt(i)));
        }
        
        while (pq.size() > 1) {
            Node left = pq.poll();
            Node right = pq.poll();
            
            Node merged = new Node(left.freq + right.freq, left, right);
            pq.offer(merged);
        }
        
        return pq.poll();
    }
```

>[!tip] Time Complexity
>O(nlogn)
##### Minimum Spanning Tree
\#Vertices in Input = V
\#Edges in Input = E
\#Vertices in MST = V
\#Edges in MST = V - 1
\#Spanning Trees = $\binom{E}{V-1}$ - \#cycles in graph

###### PRIMS
```Java
public int prims(List<List<Pair>> adjList, int V) {
        boolean[] visited = new boolean[V];
        PriorityQueue<Pair> pq = new PriorityQueue<>((a, b) -> a.weight - b.weight);
        
        pq.offer(new Pair(0, 0));
        
        int result = 0;
        
        while (!pq.isEmpty()) {
            Pair current = pq.poll();
            
            int node = current.v;
            int weight = current.weight;
            
            if (visited[node]) continue;
            
            visited[node] = true;
            result += weight;
            
            for (Pair next : adjList.get(node)) {
                if (!visited[next.v]) {
                    pq.offer(next);
                }
            }
        }
        
        return result;
    }
```

###### KRUSKALS
```Java
public int kruskals(int[][] edges, int E, int V) {
        Arrays.sort(edges, (a, b) -> a[2] - b[2]);
        
        int result = 0;
        int usedEdges = 0;
        
        DSU dsu = new DSU(V);
        
        for (int i = 0; i < E && usedEdges < V - 1; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];
            
            if (dsu.parent(u) != dsu.parent(v)) {
                dsu.union(u, v);
                result += w;
                usedEdges++;
            }
        }
        
        return result;
    }
    
    public static class DSU {
        int[] parent;
        int[] rank;
        
        public DSU(int V) {
            parent = new int[V];
            rank = new int[V];
            
            for (int i = 0; i < V; i++) {
                parent[i] = i;
                rank[i] = 0;
            }
        }
        
        public int parent(int x) {
            if (parent[x] != x) {
                parent[x] = parent(parent[x]);
            }
            
            return parent[x];
        }
        
        public void union(int x, int y) {
            int px = parent(x);
            int py = parent(y);
            
            if (px == py) return;
            
            if (rank[px] < rank[py]) {
                parent[px] = py;
            } else if (rank[px] > rank[py]) {
                parent[py] = px;
            } else {
                parent[py] = px;
                rank[px]++;
            }
        }
    }
```
##### Dijkstra
```Java
public int[] dijkstra(int V, int[][] edges, int src) {
        // code here
        List<List<Pair>> adjList = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adjList.add(new ArrayList<>());
        }
        
        for (int i = 0; i < edges.length; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            int w = edges[i][2];
            
            adjList.get(u).add(new Pair(v, w));
            adjList.get(v).add(new Pair(u, w));
        }
        
        int[] distance = new int[V];
        Arrays.fill(distance, Integer.MAX_VALUE);
        distance[src] = 0;
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        
        pq.offer(new int[] {src, 0});
        
        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            
            int node = current[0];
            int currentDistance = current[1];
            
            if (currentDistance > distance[node]) continue;
            
            for (Pair edge : adjList.get(node)) {
                int next = edge.v;
                int weight = edge.w;
                
                if (distance[node] + weight < distance[next]) {
                    distance[next] = distance[node] + weight;
                    pq.offer(new int[] {next, distance[next]});
                }
            }
        }
        
        return distance;
    }
```
##### Job Sequencing
```Java
public ArrayList<Integer> jobSequencing(int[] deadline, int[] profit) {
        // code here
        int n = deadline.length;
        
        int[][] jobs = new int[n][2];
        for (int i = 0; i < n; i++) {
            jobs[i][0] = profit[i];
            jobs[i][1] = deadline[i];
        }
        
        Arrays.sort(jobs, (a, b) -> Integer.compare(b[0], a[0]));
        
        int maxDeadline = Integer.MIN_VALUE;
        
        for (int d : deadline) maxDeadline = Math.max(maxDeadline, d);
        
        int[] slot = new int[maxDeadline];
        Arrays.fill(slot, -1);
        
        int maxProfit = 0;
        int count = 0;
        
        for (int i = 0; i < n; i++) {
            int d = jobs[i][1] - 1;
            
            while (d >= 0) {
                if (slot[d] == -1) {
                    slot[d] = i;
                    maxProfit += jobs[i][0];
                    count++;
                    break;
                }
                d--;
            }
        }
        
        ArrayList<Integer> result = new ArrayList<>();
        result.add(count);
        result.add(maxProfit);
        
        return result;
    }
```
##### Maximum Subarray Sum
```Java
public int maxSubarraySum(int[] arr) {
	int currentSum = arr[0];
	int maxSum = arr[0];
	
	for (int i = 1; i < arr.length; i++) {
		currentSum = Math.max(currentSum + arr[i], arr[i]);
		maxSum = Math.max(currentSum, maxSum);
	}
	
	return maxSum;
}
```