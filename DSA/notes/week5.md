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
$$A[i, j] = \begin{cases} \text{weight of edge } (i, j) & \text{if } (i, j) \in E \\ 0, \infty, \text{ or NIL} & \text{if there is no edge} \end{cases}$$
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
