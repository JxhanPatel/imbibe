# 5.1:  Shortest Paths in Weighted Graphs


## **1. Introduction to Weighted Graphs**
In many applications, abstractions such as simple graphs are insufficient to capture the whole truth. While Breadth-First Search (BFS) explores a graph level by level and computes the shortest path in terms of the **number of edges** to every reachable vertex, it does not account for specific values assigned to edges.

### **1.1 Definition**
A **weighted graph** is defined as $G = (V, E)$ with a weight function $W: E \rightarrow \mathbb{R}$. 
*   Weights can represent various metrics such as **cost, time, distance, penalties, or loss**—essentially any quantity that accumulates linearly along a path and that one would want to minimize.

<img width="1059" height="410" alt="image" src="https://github.com/user-attachments/assets/600c19e4-c21f-4a43-8e81-8f4bab79ed13" />

---

## **2. Shortcomings of BFS in Weighted Contexts**
BFS treats all edges as having the same unit length. However, in a weighted graph, the **weighted shortest path** (the one with the minimum total cost) need not have the minimum number of edges.

*   **Example:** In a specific graph, the shortest path from vertex 0 to vertex 2 might be via vertex 1 (two edges) rather than a direct edge from 0 to 2 (one edge) because the sum of weights along the two edges is less than the weight of the single direct edge.
*   **Real-world Analogy:** A direct flight (one hop) might be more expensive than a flight with stopovers (multiple hops) because it is more convenient for the passenger.

---

## **3. Representing Weighted Graphs**
To operate on weighted graphs, representations must be adapted to store the weight $w(u, v)$ for each edge $(u, v) \in E$.

### **3.1 Weighted Adjacency Matrix**
An $n \times n$ matrix $A$ where:

$$
A[i, j] = \begin{cases} \text{weight of edge } (i, j) & \text{if } (i, j) \in E \\\ 0, \infty, \text{ or NIL} & \text{if there is no edge} \end{cases}
$$

*   Entries capture edge weights.
*   In Python implementations, each cell may store a pair: `(edge_information, weight_information)`.

### **3.2 Weighted Adjacency List**
Each node $v$ has a record containing a list of adjacent nodes and their corresponding edge weights.
*   **Example format:** `0: [(1, 10), (2, 80)]` means vertex 0 has edges to vertex 1 with weight 10 and to vertex 2 with weight 80.

---

## **4. Path Length and Shortest Path Problems**

### **4.1 Definition of Path Length**
In a weighted graph, the **length (or weight)** of a path $p = \langle v_0, v_1, \dots, v_k \rangle$ is the **sum of the weights** of its constituent edges:
$$w(p) = \sum_{i=1}^{k} w(v_{i-1}, v_i)$$.

### **4.2 Problem Variants**
1.  **Single Source Shortest Paths:** Find the shortest path from a fixed designated vertex (source) to every other vertex in the graph.
    *   *Application:* A courier company delivering items from a central distribution center to multiple addresses.
2.  **All-Pairs Shortest Paths:** Find shortest paths between every pair of vertices $i$ and $j$ in the graph.
    *   *Application:* Finding optimal airline or road routes between all cities in a travel service.

---

## **5. Negative Edge Weights and Cycles**

### **5.1 Meaning of Negative Weights**
Negative weights can be meaningful in specific contexts, such as a taxi driver heading home at the end of the day:
*   **Positive weight:** Roads with few customers (cost of driving empty).
*   **Negative weight:** Roads with many customers (potential for profit).

### **5.2 Negative Cycles**
A **negative cycle** is a directed cycle where the sum of the edge weights is negative.
*   **Impact:** By repeatedly traversing a negative cycle, the total path cost keeps decreasing to $-\infty$.
*   **Result:** If a graph has a negative cycle reachable from the source, shortest-path weights are not well-defined.

### **5.3 Summary of Feasibility**
*   **Dijkstra's Algorithm:** Requires all edge weights to be non-negative.
*   **Bellman-Ford Algorithm:** Works even with negative edge weights, provided there are **no negative cycles**.

> [!CAUTION]
> **Ill-posed Problems:** The shortest-path problem is ill-posed in graphs containing negative cycles because one can go around the cycle arbitrarily many times to reduce the cost indefinitely.

> [!NOTE]
> **Subpaths Property:** Shortest-path algorithms typically rely on the property that any subpath of a shortest path is also a shortest path.




---


# 5.2: Single Source Shortest Paths (Dijkstra's Algorithm)


## **1. The Problem Definition**
The single-source shortest-paths problem is to find a shortest path from a designated source vertex $s \in V$ to each vertex $v \in V$ in a weighted, directed graph $G=(V, E)$ with a weight function $w: E \rightarrow \mathbb{R}$.

### **1.1 Formal Setup**
*   **Path Weight:** The weight $w(p)$ of path $p = \langle v_0, v_1, \dots, v_k \rangle$ is the sum of the weights of its constituent edges: $w(p) = \sum_{i=1}^{k} w(v_{i-1}, v_i)$.
*   **Shortest-Path Weight:** Defined as $\delta(u, v) = \min \{w(p) : u \leadsto v\}$ if a path exists, otherwise $\infty$.
*   **Constraint:** Dijkstra’s algorithm specifically requires that all edge weights are **non-negative** ($w(u, v) \ge 0$ for each edge $(u, v) \in E$).

---

## **2. Conceptual Intuition**

### **2.1 The Burning Pipeline Analogy**
*   Imagine vertices as oil depots and edges as pipelines.
*   Set fire to the oil depot at source vertex $s$ at time $t=0$.
*   Fire travels at a uniform speed along each pipeline. The "burn time" for each depot corresponds to its shortest-path distance from the source.
*   The first oil depot to catch fire after $s$ is the nearest vertex; the next is the second nearest, and so on.
<img width="763" height="426" alt="image" src="https://github.com/user-attachments/assets/50438dec-f93f-4f3c-b21f-753097b0a1ea" />



### **2.2 The Water Pipe / Wavefront Analogy**
*   Suppose edges are pipes filled with water. A droplet falls at $s$, starting a wave.
*   The expanding sphere of the wavefront reaches nodes in increasing order of their distance from $s$. The order of discovery is exactly the order used by Dijkstra's algorithm.

---

## **3. The Algorithm Strategy**

Dijkstra's algorithm is a **greedy algorithm**. it maintains a set $S$ of vertices whose final shortest-path weights from the source have already been determined.

### **3.1 High-Level Logic**
1.  **Initialize:** Set $S = \{s\}$ and $d(s) = 0$. For all other nodes $v$, $d(v) = \infty$.
2.  **Greedy Selection:** In each iteration, select a node $v \notin S$ that is "closest" to the explored part. Specifically, minimize the quantity:
    $$d'(v) = \min_{e=(u,v): u \in S} (d(u) + l_e)$$.
3.  **Explore:** Add $v$ to $S$ and define $d(v) = d'(v)$.
4.  **Repeat:** Continue until $S = V$.

<img width="772" height="445" alt="image" src="https://github.com/user-attachments/assets/c81d6d09-4320-4e00-aecc-14d7379f18e1" />
<img width="793" height="423" alt="image" src="https://github.com/user-attachments/assets/61b940b1-6c19-4a5d-900e-c861419450e7" />

---

## **4. Pseudocode**

### **4.1 General Implementation (Cormen)**
```Python
DIJKSTRA(G, w, s)
1  INITIALIZE-SINGLE-SOURCE(G, s)
2  S = ∅
3  Q = G.V
4  while Q ≠ ∅
5      u = EXTRACT-MIN(Q)
6      S = S ∪ {u}
7      for each vertex v ∈ G.Adj[u]
8          RELAX(u, v, w)
```

### **4.2 The Relaxation Step**
Relaxing an edge $(u, v)$ tests whether the shortest path to $v$ found so far can be improved by going through $u$.
```text
RELAX(u, v, w)
1  if v.d > u.d + w(u, v)
2      v.d = u.d + w(u, v)
3      v.π = u
```


---

## **5. Proof of Correctness**

Dijkstra’s algorithm works because each new shortest path discovered extends an earlier one.

### **5.1 The Inductive Argument**
*   **Base Case:** $|S|=1$. We have $S=\{s\}$ and $d(s)=0$, which is correct.
*   **Inductive Step:** Suppose for each $u \in S$, $d(u)$ is the true shortest-path distance. When we add $v$ to $S$ via edge $(u, v)$, we claim $d(v)$ is the true shortest-path distance.
*   **Contradiction:** Suppose there is some other path $P$ from $s$ to $v$. If $P$ leaves the set $S$ to some node $y$, then at the time $v$ was selected, the path to $y$ was already at least as long as the path to $v$ (because Dijkstra's picks the minimum). Since edge lengths are non-negative, any further extension of the path to $y$ cannot make it shorter than the path already found to $v$.

<img width="1042" height="563" alt="image" src="https://github.com/user-attachments/assets/bec4fb10-7989-4c19-9cac-5e937d6e7d1f" />

---

## **6. Complexity and Implementation**

The running time depends heavily on the data structure used for the priority queue.

### **6.1 Complexity Comparison Table**
| Implementation | Selection/ExtractMin | DecreaseKey | Total Running Time |
| :--- | :--- | :--- | :--- |
| **Array** | $O(V)$ | $O(1)$ | $O(V^2)$ |
| **Binary Heap** | $O(\log V)$ | $O(\log V)$ | $O((V+E)\log V)$ |
| **Fibonacci Heap**| $O(\log V)$ (amortized) | $O(1)$ (amortized) | $O(V \log V + E)$ |

### **6.2 Python implementation note**
Even using adjacency lists, the $O(n)$ bottleneck remains to find the next vertex to visit unless a better data structure (like a heap) is used to identify and remove the minimum from the collection.
<img width="1125" height="549" alt="image" src="https://github.com/user-attachments/assets/6f8caaeb-e841-4b02-8ed0-9eb357e1916b" />

---

## **7. Limitations: Negative Edge Weights**
Dijkstra’s algorithm **does not work** if the graph contains negative edges.

*   **The Reason:** The greedy assumption (that the shortest path to $v$ must pass through nodes closer than $v$) breaks down. A path starting with an expensive edge might eventually become cheaper by using subsequent negative-weight edges.
*   **Example:** If $s \rightarrow a$ has weight 3, but $s \rightarrow b \rightarrow a$ has weights 4 and -2, Dijkstra will incorrectly freeze the distance to $a$ as 3.

> [!CAUTION]
> Adding a large constant to every edge weight to make them positive **does not** solve the problem. This changes the identity of the shortest path because paths with more edges are penalized more than paths with fewer edges.

> [!TIP]
> For graphs with negative edge weights (but no negative cycles), use the **Bellman-Ford Algorithm** instead.





---





# 5.3: Single Source Shortest Paths with Negative Weights (Bellman-Ford Algorithm)


## **1. Motivation: Why Dijkstra’s Algorithm Fails**
Dijkstra’s algorithm is a greedy strategy that assumes non-negative edge weights.
*   **The "Burning" Analogy:** In Dijkstra’s, once a vertex "burns" (is visited), its shortest path distance is frozen.
*   **The Breakdown:** If negative edge weights are allowed, a shorter path to a vertex could be discovered *after* it has already been burned. Because Dijkstra does not reconsider distances once a vertex is marked, it can produce incorrect results.
*   **Example Case:** If $s \rightarrow v$ has weight 5, but $s \rightarrow w \rightarrow v$ has weights 7 and -3, Dijkstra will pick 5 and freeze it, missing the path with total weight 4.

--- 📸 **INSERT IMAGE**: [Graph showing Dijkstra's failure with nodes $s, A, B$ and weights $s \rightarrow A = 3, s \rightarrow B = 4, B \rightarrow A = -2$ | Dasgupta Figure 4.12, page 128] ---

---

## **2. Negative Edge Weights and Negative Cycles**
While we can extend algorithms to handle negative edge weights, **negative cycles** present a fundamental problem.
*   **Definition of Negative Cycle:** A cycle where the sum of the edge weights is negative.
*   **Consequence:** By repeatedly traversing a negative cycle, the total path cost keeps decreasing to $-\infty$.
*   **The Rule:** Shortest paths are not defined in a graph containing a negative cycle reachable from the source.
*   **Path Assumption:** Without negative cycles, the shortest route to every vertex is a simple path (no loops). Because the graph has $n$ vertices, a simple path can have at most $n-1$ edges.

---

## **3. The Bellman-Ford Strategy**
Instead of the greedy "freeze" rule, the Bellman-Ford algorithm uses **Dynamic Programming** to iteratively update distances.

### **3.1 The Principle of Optimality**
Every prefix of a shortest path must itself be a shortest path.
*   If the minimum weight path from 0 to $k$ follows $l$ edges: $0 \xrightarrow{w_1} j_1 \xrightarrow{w_2} \dots \xrightarrow{w_l} k$.
*   Once the shortest path to $j_{l-1}$ is discovered, the next update will fix the shortest path to $k$.

### **3.2 The Core Observation**
After $l$ updates, all shortest paths using $\le l$ edges have stabilized. Since any shortest path has at most $n-1$ edges, we only need to perform $n-1$ rounds of updates.

---

## **4. The Bellman-Ford Algorithm**

### **4.1 Initialization (Source vertex 0)**
*   $D(j)$ = minimum distance known so far to vertex $j$.
*   $D(0) = 0$.
*   $D(j) = \infty$ for all $j \neq 0$.

### **4.2 Processing (Iterative Updates)**
```text
Repeat n-1 times:
    For each vertex j:
        For each edge (j, k) in E:
            D(k) = min( D(k), D(j) + W(j, k) )
```

### **4.3 Python Implementation (Adjacency Matrix)**
```python
def bellmanford(WMat, s):
    (rows, cols, x) = WMat.shape
    infinity = np.max(WMat) * rows + 1
    distance = {}
    for v in range(rows):
        distance[v] = infinity
    distance[s] = 0
    
    for i in range(rows): # Update loop (n times)
        for u in range(rows):
            for v in range(cols):
                if WMat[u, v, 0] == 1:
                    distance[v] = min(distance[v], distance[u] + WMat[u, v, 1])
    return(distance)
```

---

## **5. Example Execution Trace**
Consider a directed graph with nodes $0..7$:
1.  **Initialize:** $D(0)=0$, all others $\infty$.
2.  **Pass 1:** Update neighbors of 0. $D(1)=10, D(7)=8$.
3.  **Pass 2:** Use new values for 1 and 7 to update their neighbors. $D(5)=12, D(6)=9$.
4.  **Pass 3:** $D(1)$ updates via node 6 (weight -4). New $D(1) = 9-4=5$. $D(2)$ updates via node 5 (weight -2). New $D(2)=10$.
5.  **Passes 4–7:** Continue until values stabilize.

<img width="1041" height="551" alt="image" src="https://github.com/user-attachments/assets/f974c3bf-56ae-4378-bb7e-6be5457c7651" />
<img width="499" height="186" alt="image" src="https://github.com/user-attachments/assets/1f120be7-2af9-4c6e-a2ba-0fa55c54871d" />

---

## **6. Complexity Analysis**
The complexity depends on the graph representation.

| Feature | Adjacency Matrix | Adjacency List |
| :--- | :--- | :--- |
| **Initializing Infinity** | $O(n^2)$ | $O(m)$ |
| **Number of Outer Passes** | $O(n)$ | $O(n)$ |
| **Work per Pass** | $O(n^2)$ (scan all rows/cols) | $O(m)$ (scan all edges) |
| **Total Running Time** | **$O(n^3)$** | **$O(mn)$** |

> [!NOTE]
> Adjacency lists provide a significant speedup for **sparse graphs**, where $m \approx n$.

---

## **7. Negative Cycle Detection**
The Bellman-Ford algorithm can be extended to detect negative cycles.
*   **The Method:** Run the update loop one extra time (the $n$-th time).
*   **The Logic:** If the graph has no negative cycles, all shortest paths must have stabilized after $n-1$ iterations.
*   **The Criterion:** If any distance $D(v)$ reduces during the $n$-th pass, the graph contains a negative cycle.

---

## **8. Summary Comparison**
*   **Dijkstra's Algorithm:** Requires non-negative edge weights; uses a greedy "burn and freeze" strategy; faster complexity ($O(n^2)$ or $O(m \log n)$).
*   **Bellman-Ford Algorithm:** Allows negative edge weights but not negative cycles; uses an iterative "blind update" strategy; slower complexity ($O(mn)$); can detect negative cycles.


