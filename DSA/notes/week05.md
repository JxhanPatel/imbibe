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





---





# 5.4: All Pairs Shortest Paths (Floyd-Warshall Algorithm)


## **1. The All-Pairs Shortest Path Problem**
While Single Source Shortest Paths (SSSP) fix a starting point and find paths to every other vertex, the **All-Pairs Shortest Path (APSP)** problem seeks the shortest path between every pair of vertices $i$ and $j$ in a weighted graph.

### **1.1 Conceptual Motivation**
*   **SSSP Analogy:** A warehouse or factory shipping items to all retail outlets.
*   **APSP Analogy:** A travel service or website needing to answer shortest-route queries (in terms of time, cost, or distance) between any two arbitrary cities.

### **1.2 Initial Strategies**
A simple way to solve APSP is to run an SSSP algorithm $n$ times, treating each vertex as a potential source:
*   **Non-negative weights:** Run Dijkstra’s algorithm from each vertex. Using a binary heap, the time is $O(V \cdot E \log V)$; with a Fibonacci heap, it is $O(V^2 \log V + VE)$.
*   **Negative weights:** Run the Bellman-Ford algorithm from each vertex. This takes $O(V^2 E)$, which can be as high as $O(V^4)$ for dense graphs.

**Goal:** Find a more direct way to solve APSP that does not require repeatedly running SSSP algorithms from every starting point.

---

## **2. Foundations: Transitive Closure and Warshall’s Algorithm**
The Floyd-Warshall algorithm is rooted in the concept of **transitive closure**.

*   **Adjacency Matrix ($A$):** Represents paths of length 1.
*   **Matrix Multiplication ($A^2 = A \times A$):** $A^2[i, j] = 1$ if there is a path of length 2 from $i$ to $j$.
*   **Transitive Closure ($A^+$):** $A^+ = A + A^2 + \dots + A^{n-1}$ represents paths of any length.

### **2.1 The "Intermediate Vertex" Approach**
An alternative to measuring path length is to constrain the **choice of vertices** allowed on the path.
*   **Definition:** Let $B^k[i, j] = 1$ if there is a path from $i$ to $j$ via intermediate vertices only from the set $\{0, 1, \dots, k-1\}$.
*   **Warshall’s Update Rule:** $B^{k+1}[i, j] = 1$ if:
    1.  $B^k[i, j] = 1$ (already reachable via vertices $\{0, \dots, k-1\}$).
    2.  $B^k[i, k] = 1$ AND $B^k[k, j] = 1$ (reachable by going from $i \to k$ and $k \to j$ using intermediate vertices $\{0, \dots, k-1\}$).

---

## **3. The Floyd-Warshall Algorithm**
This algorithm adapts Warshall’s transitive closure approach to compute shortest path weights.

### **3.1 Recursive Formulation**
Let $SP^k[i, j]$ (or $d_{ij}^{(k)}$) be the length of the shortest path from vertex $i$ to vertex $j$ for which all intermediate vertices are in the set $\{0, 1, \dots, k-1\}$.

1.  **Base Case ($k=0$):** No intermediate vertices are allowed. The shortest path is simply the weight of the direct edge $W[i, j]$.
    *   $SP^0[i, j] = W[i, j]$ if $(i, j) \in E$, else $\infty$.
2.  **Recursive Step:** For $k \ge 1$:
    $$SP^{k+1}[i, j] = \min(SP^k[i, j], SP^k[i, k] + SP^k[k, j])$$
    *   The first term represents not using vertex $k$ as an intermediate point.
    *   The second term represents using vertex $k$ as an intermediate point (combining the shortest path $i \to k$ and $k \to j$).

3.  **Final Answer:** $SP^n[i, j]$ provides the length of the shortest path overall, as all vertices are now permitted as intermediate points.

<img width="1088" height="567" alt="image" src="https://github.com/user-attachments/assets/cef6abaf-407b-4df9-860d-bbce24519815" />
<img width="1078" height="565" alt="image" src="https://github.com/user-attachments/assets/594bc558-5831-42f4-8165-2305cb2275ce" />
<img width="1076" height="566" alt="image" src="https://github.com/user-attachments/assets/0966a021-55bf-46df-bd3e-8bbb6b2040b1" />

---

## **4. Algorithm Implementation**

### **4.1 Implementation Logic**
The shortest path matrix $SP$ is effectively an $n \times n \times (n+1)$ structure.
```python
def floydwarshall(WMat):
    (rows, cols, x) = WMat.shape
    infinity = np.max(WMat) * rows * rows + 1
    SP = np.zeros(shape=(rows, cols, cols + 1))
    
    # Initialization (SP[i, j, 0])
    for i in range(rows):
        for j in range(cols):
            SP[i, j, 0] = infinity
            if WMat[i, j, 0] == 1:
                SP[i, j, 0] = WMat[i, j, 1]
    
    # Main Iteration (Nested Loops)
    for k in range(1, cols + 1):
        for i in range(rows):
            for j in range(cols):
                SP[i, j, k] = min(SP[i, j, k-1], 
                                 SP[i, k-1, k-1] + SP[k-1, j, k-1])
    return(SP[:, :, cols])
```

### **4.2 Space Optimization**
The algorithm can be reduced from $O(n^3)$ space to $O(n^2)$ space.
*   **Reasoning:** To compute slice $SP^k$, we only need the entries from slice $SP^{k-1}$.
*   **Technique:** Maintain only two $n \times n$ matrices (slices), overwriting the older one once the current one is computed.

---

## **5. Complexity and Characteristics**

### **5.1 Performance**
*   **Time Complexity:** $\Theta(n^3)$ due to the triply nested loops.
*   **Graph Representation:** Adjacency lists do **not** help improve complexity. Because the algorithm must update every pair $(i, j)$ at every iteration $k$, it is independent of the number of edges.

### **5.2 Constraints and Features**
*   **Weights:** Works with negative edge weights.
*   **Negative Cycles:** Shortest paths are not defined if negative cycles exist. The algorithm can detect negative cycles by checking if any diagonal entry $SP^n[i, i]$ becomes negative.
*   **Shortest-Path Construction:** To recover the actual paths, a predecessor matrix $\Pi$ is maintained, where $\pi_{ij}^{(k)}$ records the predecessor of $j$ on a shortest path from $i$ using intermediate vertices up to $k$.


> [!IMPORTANT]
> **Key Property:** The Floyd-Warshall algorithm is a classic example of dynamic programming where the "dag" of subproblems is implicit, defined by the order of intermediate vertices added one by one.





---





# 5.5: Minimum Cost Spanning Trees 



## **1. Motivation and Examples**
Spanning trees are used to recover a connected part of a graph while retaining a minimal set of edges.

*   **Road Restoration:** After a cyclone damages roads in a district, the priority is to ensure all parts are reachable. The goal is to choose a set of roads to restore so that every town is reachable from every other town.
*   **Fibre Optic Cables:** An Internet service provider wants to ensure redundancy against cable faults by laying secondary cables. The goal is to find the minimum number of cables to be doubled up so that the entire network remains connected via redundant links.

---

## **2. Problem Definitions**

### **2.1 Spanning Tree**
*   **Definition:** A tree that connects all the vertices in a graph.
*   **Minimally Connected:** A tree is a minimally connected graph. Adding any edge to a tree creates a loop (cycle), and removing any edge disconnects the graph.
*   **Non-Uniqueness:** In general, a graph can have more than one spanning tree.


<img width="924" height="470" alt="image" src="https://github.com/user-attachments/assets/4ce23238-1b38-4a01-b325-7454cbeeb966" />
<img width="980" height="506" alt="image" src="https://github.com/user-attachments/assets/14f161c2-507e-4ed6-afba-f410909aa8bf" />

### **2.2 Minimum Cost Spanning Tree (MCST)**
*   **Input:** A connected undirected graph $G = (V, E)$ with edge weights $w_e$.
*   **Output:** A tree $T = (V, E')$, where $E' \subseteq E$, that minimizes the total weight: $weight(T) = \sum_{e \in E'} w_e$.
*   **Objective:** Among all different spanning trees, choose one with the minimum total cost.

---

## **3. Fundamental Facts About Trees**
1.  **Connectivity and Edges:** A tree on $n$ vertices has exactly $n-1$ edges.
2.  **Cycle Formation:** Adding an edge to a tree must create a cycle.
3.  **Connectivity Robustness:** Removing an edge from a cycle cannot disconnect a graph.
4.  **Edge Deletion:** Deleting an edge from a tree splits it into exactly two connected components.
5.  **Unique Paths:** For any two nodes in a tree, there is exactly one simple path between them.

> [!TIP]
> **Tree Equivalence:** Any two of the following properties imply the third: (1) $G$ is connected, (2) $G$ is acyclic, (3) $G$ has $n-1$ edges.

---

## **4. Foundations of Correctness: The Properties**

### **4.1 The Cut Property (Minimum Separator Lemma)**
The cut property provides the rule for recognizing "safe" edges to add to an MST.

*   **Definition of a Cut:** A partition of the vertices into two non-empty groups, $S$ and $V-S$.
*   **Crossing Edge:** An edge crosses the cut if one endpoint is in $S$ and the other is in $V-S$.
*   **Theorem:** Let $V$ be partitioned into two non-empty sets $U$ and $W = V \setminus U$. Let $e = (u, w)$ be the minimum cost edge with $u \in U, w \in W$. Every MCST must include $e$.
*   **Proof Sketch (Exchange Argument):** If an MST $T$ does not contain $e$, it must contain some other path $P$ between $u$ and $w$. This path must cross the cut at some edge $f$. Since $e$ is the minimum weight edge crossing the cut, $w(e) < w(f)$. Replacing $f$ with $e$ in $T$ yields a spanning tree with a smaller total weight, contradicting the optimality of $T$.


### **4.2 The Cycle Property**
The cycle property provides a rule for recognizing edges that are "safe" to eliminate.

*   **Theorem:** Assume all edge costs are distinct. Let $C$ be any cycle in $G$, and let edge $e = (v, w)$ be the most expensive edge belonging to $C$. Then $e$ does not belong to any minimum spanning tree of $G$.
*   **Proof Sketch:** If an MST $T$ contains the most expensive edge $e$, deleting $e$ partitions $T$ into two components $S$ and $V-S$. Since $e$ was part of cycle $C$, there must be another edge $e'$ in $C$ that also crosses the cut. Since $e$ is the most expensive edge on $C$, $w(e') < w(e)$. Replacing $e$ with $e'$ creates a cheaper spanning tree.

---

## **5. General Algorithmic Strategies**
Based on the properties above, two natural greedy strategies emerge:

| Strategy | Algorithm Name | High-Level Logic |
| :--- | :--- | :--- |
| **Grow a Tree** | **Prim’s Algorithm** | Start with a single vertex or the smallest edge and "grow" the tree one edge at a time by picking the smallest edge connecting the tree to an outside vertex. |
| **Connect Components**| **Kruskal’s Algorithm** | Start with $n$ components (isolated vertices). Scan all edges in ascending order of weight and include an edge if it merges two disjoint components without forming a cycle. |

> [!NOTE]
> **Uniqueness:** If all edge weights in a graph are distinct, the minimum spanning tree is unique. If edge weights repeat, there may be a large number of possible minimum cost spanning trees.






---



# 5.6: Minimum Cost Spanning Trees (Prim's Algorithm)


## **1. Introduction to Prim's Algorithm**
Prim’s algorithm is a greedy strategy used to find a minimum cost spanning tree (MCST) in a connected, undirected graph $G = (V, E)$ with a weight function $w: E \rightarrow \mathbb{R}$.

### **1.1 The High-Level Strategy**
The algorithm has the property that the edges in the set $A$ always form a single tree.
*   **Initialization:** Start with an arbitrary root vertex $r$ (or a smallest weight edge overall).
*   **Incremental Growth:** The tree grows until it spans all the vertices in $V$.
*   **Greedy Choice:** In each iteration, add the "lightest" edge connecting the current tree to a vertex not yet in the tree.
*   **Constraint:** Adding an edge that connects two parts already in the tree would create a cycle and is forbidden.


---

## **2. Correctness: The Minimum Separator Lemma**
The correctness of Prim's method follows from the **Cut Property** (also known as the Minimum Separator Lemma).

*   **Lemma Statement:** Let $V$ be partitioned into two non-empty sets $U$ and $W = V \setminus U$. Let $e = (u, w)$ be the minimum cost edge with $u \in U$ and $w \in W$. Every MCST must include $e$.
*   **Application to Prim's:** At any point, the vertices already in the tree ($TV$) and the vertices outside the tree ($V \setminus TV$) partition the graph. By picking the smallest edge connecting these two partitions, the algorithm is guaranteed to make a correct choice that belongs to every MCST.
*   **Corollary:** The smallest weight edge leaving *any* vertex must belong to every MCST.

<img width="1107" height="535" alt="image" src="https://github.com/user-attachments/assets/d8c5a2cd-1ac4-46ca-aade-e0f557886009" />

---

## **3. Algorithmic Implementation**

### **3.1 Priority Queue Logic (Cormen)**
To implement Prim's algorithm efficiently, all vertices not yet in the tree reside in a min-priority queue $Q$ based on a `key` attribute.
*   **$v.key$**: The minimum weight of any edge connecting $v$ to a vertex in the tree ($v.key = \infty$ if no such edge exists).
*   **$v.\pi$**: The parent of $v$ in the tree.

#### **MST-PRIM(G, w, r) Pseudocode**
```text
1  for each u ∈ G.V
2      u.key = ∞
3      u.π = NIL
4  r.key = 0
5  Q = G.V
6  while Q ≠ ∅
7      u = EXTRACT-MIN(Q)
8      for each v ∈ G.Adj[u]
9          if v ∈ Q and w(u, v) < v.key
10             v.π = u
11             v.key = w(u, v)
```

### **3.2 Adjacency List Implementation (Mukund)**
This version maintains `nbr[v]` to track the nearest neighbor of $v$ currently in the tree.
```python
def primlist2(WList):
    infinity = 1 + max([d for u in WList.keys() for (v,d) in WList[u]])
    (visited, distance, nbr) = ({}, {}, {})
    for v in WList.keys():
        (visited[v], distance[v], nbr[v]) = (False, infinity, -1)
    
    visited = True # Start with vertex 0
    for (v, d) in WList:
        (distance[v], nbr[v]) = (d, 0)
        
    for i in range(1, len(WList.keys())):
        # Find unvisited vertex with minimum distance
        nextd = min([distance[v] for v in WList.keys() if not visited[v]])
        nextvlist = [v for v in WList.keys() if (not visited[v]) and distance[v] == nextd]
        if nextvlist == []:
            break
        nextv = min(nextvlist)
        visited[nextv] = True
        
        # Update distances of neighbours
        for (v, d) in WList[nextv]:
            if not visited[v] and d < distance[v]:
                (distance[v], nbr[v]) = (d, nextv)
```

---

## **4. Complexity Analysis**
The running time of Prim’s algorithm depends heavily on the priority queue implementation.

| Implementation | Extract-Min | Decrease-Key | Total Complexity |
| :--- | :--- | :--- | :--- |
| **Array** | $O(V)$ | $O(1)$ | $O(V^2)$ |
| **Binary Heap** | $O(\log V)$ | $O(\log V)$ | $O(E \log V)$ |
| **Fibonacci Heap**| $O(\log V)$ (amortized) | $O(1)$ (amortized) | $O(E + V \log V)$ |

*   **Adjacency List Bottleneck:** Even with adjacency lists, an $O(V)$ scan to find the minimum distance vertex in each of the $V$ iterations results in $O(V^2)$.
*   **Heaps:** Using a binary heap improves performance for sparse graphs ($E < V^2 / \log V$). Fibonacci heaps are faster for dense graphs.

---

## **5. Step-by-Step Example Execution**
Consider a 5-node graph with weights: $(1, 3)=6, (2, 4)=8, (0, 1)=10, (1, 2)=20$.
1.  **Start with vertex 0:** Initial tree is $\{0\}$.
2.  **Edge (0, 1) weight 10:** Added to tree. $TV = \{0, 1\}$.
3.  **Explore neighbors of 1:** Edges are $(1, 3)=6$ and $(1, 2)=20$.
4.  **Pick smallest:** Add $(1, 3)$ weight 6. $TV = \{0, 1, 3\}$.
5.  **Next smallest edge to $TV$:** Edge $(1, 2)$ weight 20 and $(3, 4)$ (assumed edge).
6.  **Progress:** Continue picking the smallest available edge that connects to an unvisited vertex until all vertices are in $TV$.
7.  
<img width="1142" height="540" alt="image" src="https://github.com/user-attachments/assets/76457c6a-7b4a-4fb1-8ba9-e4f35ff5567b" />


---

## **6. Comparison with Dijkstra's Algorithm**
Prim’s algorithm is strongly reminiscent of Dijkstra’s algorithm, but they differ in their objective.
*   **Dijkstra Update:** `v.key = min(v.key, u.d + w(u, v))` — seeks the shortest path weight from a *source* to $v$.
*   **Prim Update:** `v.key = min(v.key, w(u, v))` — seeks the shortest distance from the *current tree* to $v$.

> [!NOTE]
> **Uniqueness:** If all edge weights are distinct, the minimum spanning tree is unique. If weights repeat, different choices of minimum edges can lead to multiple valid spanning trees.







---








# 5.7: Minimum Cost Spanning Trees (Kruskal's Algorithm) 


## **1. Overview and Strategy**
Kruskal's algorithm is a greedy strategy for finding a Minimum Cost Spanning Tree (MCST) in a connected, undirected graph $G = (V, E)$ with a weight function $w: E \rightarrow \mathbb{R}$. Unlike Prim's algorithm, which grows a single tree from a root, Kruskal's algorithm builds the MCST "bottom-up" by merging disjoint components.

### **1.1 The High-Level Logic**
1.  **Start Small:** Begin with $n$ components, where each vertex is an isolated component (a singleton set).
2.  **Order Edges:** Sort all edges in the graph in ascending order of their weights.
3.  **Greedy Selection:** Scan the sorted edges. For each edge $e = (u, v)$:
    *   If $u$ and $v$ belong to **different components**, adding the edge will not create a cycle. Add $e$ to the MCST and merge the components of $u$ and $v$.
    *   If $u$ and $v$ are already in the **same component**, adding the edge would create a cycle (loop). Discard the edge.
4.  **Termination:** Continue until all vertices are connected in a single component (which will require exactly $n-1$ edges).

<img width="1128" height="519" alt="image" src="https://github.com/user-attachments/assets/743146c9-5039-4629-8751-41a2a0776d8f" />
<img width="1130" height="520" alt="image" src="https://github.com/user-attachments/assets/cd6bc3db-96cf-4bbc-aa21-249ce12c085f" />

---

## **2. Correctness: The Minimum Separator Lemma**
The correctness of Kruskal’s method is justified by the **Cut Property** (Minimum Separator Lemma).

*   **Lemma Statement:** Let $V$ be partitioned into two non-empty sets $U$ and $W = V \setminus U$. Let $e = (u, w)$ be the minimum cost edge with $u \in U$ and $w \in W$. Every MCST must include $e$.
*   **Application to Kruskal's:** Consider an edge $e = (u, v)$ being added. Let $U$ be the component containing $u$ and $W$ be all other vertices ($V \setminus U$). Because edges are processed in ascending order, $e$ is the cheapest edge connecting $U$ to any vertex not in $U$ that hasn't been used yet. Since no edge connecting $U$ to $W$ has been encountered yet, $e$ is the absolute cheapest edge crossing this partition and must belong to the MCST.

---

## **3. Implementation and Data Structures**

### **3.1 Modeling the Algorithm State**
The algorithm's state is modeled as a **collection of disjoint sets**, where each set contains the nodes of a particular connected component.

*   **`makeset(x)`**: Creates a singleton set containing just $x$.
*   **`find(x)`**: Returns the name/representative of the set containing $x$. Two nodes $u$ and $v$ are in the same component if `find(u) == find(v)`.
*   **`union(u, v)`**: Merges the sets containing $u$ and $v$ into a single new set.

### **3.2 Naive Implementation (Python)**
A simple implementation uses a dictionary to track the component ID of each vertex.

```python
def kruskal(WList):
    (edges, component, TE) = ([], {}, [])
    for u in WList.keys():
        # Weight as first component for easy sorting
        edges.extend([(d, u, v) for (v, d) in WList[u]])
        component[u] = u
    
    edges.sort() # O(m log m)
    
    for (d, u, v) in edges:
        if component[u] != component[v]: # If in different components
            TE.append((u, v))
            # Merge components: rename all nodes in u's component to v's component
            old_comp = component[u]
            for w in WList.keys():
                if component[w] == old_comp:
                    component[w] = component[v]
    return(TE)
```

---

## **4. Complexity Analysis**

| Operation | Complexity (Naive) | Complexity (Efficient) |
| :--- | :--- | :--- |
| **Sorting Edges** | $O(m \log m)$ | $O(m \log m)$ |
| **Set Lookups (`find`)** | $O(1)$ | $O(\alpha(n))$ |
| **Merging Sets (`union`)** | $O(n)$ (scan all vertices) | $O(\alpha(n))$ |
| **Total Running Time** | **$O(n^2)$** or **$O(m \log n + n^2)$** | **$O(m \log n)$** |

*   **The Bottleneck:** In the naive implementation, merging components takes $O(n)$ time. Since we perform $n-1$ successful unions, the total time for unions is $O(n^2)$.
*   **Improvement:** Using an efficient **Union-Find** data structure with "union by rank" and "path compression" reduces the cost of set operations to almost constant time—specifically $O(m \alpha(n))$, where $\alpha$ is the inverse Ackermann function.
*   **Final Bound:** With efficient Union-Find, the complexity is dominated by sorting the edges: **$O(m \log m)$**, which is equivalent to **$O(m \log n)$** since $m \le n^2$.

---

## **5. Uniqueness and Multiple trees**
*   **Distinct Edge Weights:** If all edge weights are unique, the minimum separator lemma implies there is only one possible MCST.
*   **Duplicate Edge Weights:** If weights repeat, there may be multiple valid MCSTs. The algorithm chooses one based on how ties are broken during sorting or selection.
*   **Example:** In a triangle graph where all edges have weight 10, any two edges form an MCST. There are three possible trees.

> [!IMPORTANT]
> **Greedy Choice Property:** Kruskal’s is a greedy algorithm because at each step it adds the edge of least possible weight that contributes to the final goal without looking ahead.

> [!NOTE]
> **Forest vs. Tree:** Throughout the execution, the set of edges $A$ forms a **forest** (a collection of trees). Only upon termination does it become a single **spanning tree**.
