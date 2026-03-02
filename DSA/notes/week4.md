# 4.1: Introduction to Graphs  

## **1. Visualizing Relations as Graphs**
A graph is a useful way to visualize a relation.

### **1.1 Example: Teachers and Courses**
*   **Sets:** $T$ is a set of teachers in a college; $C$ is a set of courses being offered.
*   **Relation:** $A \subseteq T \times C$ describes the allocation of teachers to courses.
*   **Notation:** $A = \{ (t,c) \mid (t,c) \in T \times C, \text{ t teaches c} \}$.
*   **Visualization:** Objects of interest are represented as black dots (nodes). In this example, edges (arrows) point from a teacher to a course to indicate who teaches what.

<img width="360" height="357" alt="image" src="https://github.com/user-attachments/assets/4e54ba60-4856-4f67-af39-346234c30bc6" />

### **1.2 Example: Friendship**
*   **Sets:** $P$ is a set of students.
*   **Relation:** $F \subseteq P \times P$ describes which pairs of students are friends.
*   **Notation:** $F = \{ (p,q) \mid p, q \in P, p \neq q, \text{ p is a friend of q} \}$.
*   **Symmetry:** $(p,q) \in F$ iff $(q,p) \in F$. In a symmetric relation, if one person is a friend of another, that person is also their friend.

<img width="350" height="329" alt="image" src="https://github.com/user-attachments/assets/8c7212c3-51be-4d97-a34f-d61b42a1adc7" />

---

## **2. Formal Definition of a Graph**
Formally, a graph is defined as $G = (V, E)$.

*   **Vertices (Nodes):** $V$ is a set of vertices or nodes.
*   **Edges:** $E$ is a set of edges. An edge technically is a binary relation $E \subseteq V \times V$.
*   **Irreflexive Property:** Usually, a vertex is not connected to itself; in relational terminology, the relation is assumed to be **IRREFLEXIVE**. Self-loops are forbidden in many contexts, meaning every edge consists of two distinct vertices.

---

## **3. Types of Graphs**

### **3.1 Directed Graphs**
*   **Definition:** Roles are not interchangeable; the relationship is asymmetric.
*   **Notation:** Each edge $e'$ is an ordered pair $(u, v)$.
*   **Terminology:** $u$ is called the **tail** (incident from) and $v$ is the **head** (incident to). The edge leaves node $u$ and enters node $v$.
*   **Example:** In the teacher-course graph, $(v,v') \in E$ does not imply $(v',v) \in E$, as a course cannot teach a person.

### **3.2 Undirected Graphs**
*   **Definition:** Relationships are symmetric; edges indicate a reciprocity.
*   **Notation:** An edge $e$ is a two-element subset of $V$: $e = \{u, v\}$.
*   **Property:** $(v, v') \in E$ iff $(v', v) \in E$.
*   **Visualization:** Effectively, $(v, v')$ and $(v', v)$ are the same edge. Lines connecting vertices are drawn without arrows.
*   **Analogy:** Bi-directional roads where one can travel in either direction.

---

## **4. Structural Properties**

### **4.1 Paths and Walks**
*   **Path:** A sequence of vertices $v_1, v_2, \dots, v_k$ connected by edges where for $1 \le i < k$, $(v_i, v_{i+1}) \in E$. Normally, a path does not visit a vertex twice.
*   **Walk:** A sequence that re-visits a vertex.
*   **Simple Path:** A path where all vertices are distinct from one another.
<img width="354" height="369" alt="image" src="https://github.com/user-attachments/assets/fc800a59-b50f-427a-811b-2143d07e6f4f" />


### **4.2 Reachability**
*   **Definition:** Vertex $v$ is reachable from vertex $u$ if there is a path from $u$ to $v$.
*   **Example:** Airline routes allow one to determine if a target city is reachable from a source city by following a sequence of flights.
<img width="350" height="408" alt="image" src="https://github.com/user-attachments/assets/831141e3-346a-41eb-8d74-1b140f9f43bc" />

---

## **5. Representative Abstract Problems**
Graphs serve as useful abstract representations for a wide range of problems.

### **5.1 Graph Colouring (Map Colouring)**
*   **Context:** Assign each state on a map a colour such that states sharing a border are coloured differently.
*   **Formal Problem:** Given $G = (V, E)$ and a set of colours $C$, find a colouring function $c: V \rightarrow C$ such that $(u, v) \in E \Rightarrow c(u) \neq c(v)$.
*   <img width="411" height="453" alt="image" src="https://github.com/user-attachments/assets/659d808f-0c06-43bf-9658-a6d3d9f2f9b4" />
*   <img width="370" height="419" alt="image" src="https://github.com/user-attachments/assets/5d3707c3-b55a-4ae2-bee3-766b77c1f49b" />


*   **Four Colour Theorem:** For planar graphs derived from geographical maps, 4 colours suffice.
*   **Application:** Timetable scheduling where overlap in slots (edges) requires different classrooms (colours).
*   <img width="345" height="395" alt="image" src="https://github.com/user-attachments/assets/84a7e22e-d672-4648-9812-b73c5596d4c5" />





### **5.2 Vertex Cover**
*   **Context:** Installing the minimum number of security cameras in a hotel where cameras at intersections monitor all incident corridors.
*   **Graph Model:** $V$ represents intersections; $E$ represents corridor segments.
*   **Problem:** Marking a vertex $v$ "covers" all edges incident on $v$. Find the smallest subset of $V$ that covers all edges.
*   <img width="283" height="308" alt="image" src="https://github.com/user-attachments/assets/7fcc1239-1671-41de-83f6-4c42dda98fd2" />


### **5.3 Independent Set**
*   **Context:** A dance school organizing a cultural program where each dancer performs at most once. Dancers overlap across different group dances.
*   **Graph Model:** $V$ represents dances; $E$ indicates that two dances share a set of dancers.
*   **Problem:** Find a subset of vertices such that no two are connected by an edge. This identifies the maximum number of dances possible.
*   <img width="339" height="324" alt="image" src="https://github.com/user-attachments/assets/1ba6be4e-a5a0-4bd6-91b7-d00425ecaddb" />


### **5.4 Matching**
*   **Context:** Allocating class projects to groups of one or two people who must be friends.
*   **Formal Problem:** In an undirected graph $G = (V, E)$, a matching is a subset $M \subseteq E$ of mutually disjoint edges.
*   **Objective:** Find a maximal matching or a **perfect matching** (which covers all vertices).
*   <img width="305" height="181" alt="image" src="https://github.com/user-attachments/assets/12ed2d08-9528-4838-a2bd-75fce600ea7d" />


> [!TIP]
> **Abstraction:** If we distort the visual representation of the graph, the underlying problem remains unchanged.

> [!NOTE]
> **Connectivity in Directed Graphs:** Vertices $i$ and $j$ are **strongly connected** if there is a path from $i$ to $j$ and a path from $j$ to $i$. Directed graphs can be decomposed into **strongly connected components (SCCs)**.




---





# 4.2: Representing Graphs


---

## 1. Introduction to Graph Representation

To work with a graph, we need to transform a visual picture into something that we can manipulate in an algorithm. While a human being can visually identify connectivity or paths, an algorithm must represent the graph in a more concrete way and manipulate this representation to solve problems without relying on visual intuition.

### Vertex Representation

We typically choose a very simple representation for vertices. Regardless of their original names (e.g., names of people, courses, or cities), we label them as integers.

- If there are $n$ vertices, we label them $0, 1, 2, \dots, n-1$.
- We think of the vertex names as numbers between $0$ and $n-1$.

### Edge Representation

Since vertices are numbers, an edge is represented as a pair of numbers $(i, j)$.

- **No Reflexive Relations:** We do not allow an edge to start at $i$ and end at $i$.
- For any edge $(i, j)$, both $i$ and $j$ must be between $0$ and $n-1$, and $i \neq j$.

<img width="1138" height="563" alt="image" src="https://github.com/user-attachments/assets/d91833b7-8881-496c-8999-5b0ec109cfdb" />

---

## 2. Adjacency Matrix

The most obvious way to represent a graph is to record which edges are present and which are absent in a table or matrix called an **Adjacency Matrix**.

### Formal Definition

For a graph with $n$ vertices, the adjacency matrix is an $n \times n$ matrix where:

- The entry at row $i$ and column $j$ is **1** if there is an edge from $i$ to $j$.
- The entry is **0** otherwise.
- This is a zero-one matrix where ones represent existing edges.

### Python Implementation (using NumPy)

To create an adjacency matrix for a graph with 10 vertices (0 to 9):

1. Initialize a $10 \times 10$ matrix of all zeros.
2. Scan through the list of edges.
3. For every pair $(i, j)$ in the edge list, set the entry `matrix[i][j] = 1`.

<img width="565" height="455" alt="image" src="https://github.com/user-attachments/assets/24129b27-9927-433c-a45e-2fb9ec7be6ec" />

### Properties

- **Undirected Graphs:** If the graph is undirected, the matrix is **symmetric** across the main diagonal. If $(i, j)$ is an edge, $(j, i)$ is also an edge. Therefore, the entry at row $i$, column $j$ will be the same as the entry at row $j$, column $i$.
- <img width="1219" height="544" alt="image" src="https://github.com/user-attachments/assets/5c364e40-bd14-4b17-8dd0-5a94dca1d4a6" />

- **Directed Graphs:** * **Rows** represent outgoing edges (edges from $i$).

  - **Columns** represent incoming edges (edges into $j$).
  - In general, there is no symmetry; an edge from $i$ to $j$ does not imply an edge from $j$ to $i$.
  - <img width="1180" height="590" alt="image" src="https://github.com/user-attachments/assets/58a8dced-d885-48cc-980e-666a1c2c9e61" />


---

## 3. Operations on Adjacency Matrices

### Finding Neighbors

In a picture, neighbors are found by looking at outgoing arrows. In an adjacency matrix, this corresponds to looking along a **row**.

**Python Function to find neighbors:**

```python
def neighbors(adj_matrix, i):
    # adj_matrix is a numpy array, i is the vertex
    neighbor_list = []
    (rows, cols) = adj_matrix.shape 
    for j in range(cols):
        if adj_matrix[i][j] == 1:
            neighbor_list.append(j)
    return neighbor_list

```

- **Example:** For vertex 7 in the sample graph, row 7 contains ones at columns 4, 5, and 8. The function returns `[4, 5, 8]`.

### Degree

- **Undirected Graph:** The degree of a vertex is the number of ones in its row (or column).
- **Directed Graph:**

  - **Out-degree:** Number of ones in row $i$ (edges going out).
  - **In-degree:** Number of ones in column $i$ (edges coming in).
  - Out-degree and In-degree are not necessarily equal.

---

## 4. Reachability and Graph Exploration

To find if there is a path from a source vertex to a target vertex (e.g., finding an airline route from city 9 to city 0):
<img width="590" height="518" alt="image" src="https://github.com/user-attachments/assets/bd613ae5-9d75-425a-ae07-65f090ae41ec" />

1. **Mark the source:** Start at vertex 9 and mark it as reachable.
2. **Systematic Exploration:** Look at all places reachable from 9 (its neighbors). If 8 is a neighbor, mark 8 as reachable.
3. **Iterate:** Look at neighbors of 8 (e.g., 5 and 7) and mark them.
4. **Termination:** Stop when the target (0) is marked or no more new nodes can be marked.

> [!IMPORTANT]
> To avoid infinite loops (going from 8 back to 9 and 9 back to 8), we must ensure we do not re-explore already marked nodes.
>
>

**Exploration Strategies:**

- **Breadth First Search (BFS):** Explores layer by layer (1 step away, 2 steps away, etc.).
- **Depth First Search (DFS):** Follows a path as far as possible until stuck, then backtracks.

---

## 5. Adjacency List

In many realistic situations, graphs are **sparse**. While $n$ vertices allow for $O(n^2)$ possible edges, the actual number of edges is often closer to $O(n)$. An adjacency matrix for such a graph would be mostly zeros.

### Formal Definition

An **Adjacency List** records only the existing edges. For every vertex $i$, we store a list of vertices $j$ such that $(i, j)$ is an edge.

### Python Representation
<img width="778" height="471" alt="image" src="https://github.com/user-attachments/assets/50822e72-2acd-479e-aebc-66c1b6e28c67" />

The simplest way to build this in Python is using a **dictionary**:

- **Keys:** Vertex names ($0$ to $n-1$).
- **Values:** A list of neighbors for that vertex.

**Example Construction:**
If edges are $(0,1)$ and $(0,4)$, then `adj_list[0] = [1, 4]`. It is often convenient to keep these lists in ascending order.

<img width="553" height="596" alt="image" src="https://github.com/user-attachments/assets/9d7bdee7-5138-48b7-ace4-c5c893b01155" />

---

## 6. Comparison of Representations

| Feature | Adjacency Matrix | Adjacency List |
| --- | --- | --- |
| **Space Complexity** | $O(n^2)$ | O(n+number of edges) |
| **Checking if (i,j) is an edge** | O(1) (Direct lookup at `matrix[i][j]`) | O(degree of i) (Must scan the list) |
| **Finding all neighbors of i** | O(n) (Must scan the entire row) | O(degree of i) (Only scan existing neighbors) |
| **Efficiency for Sparse Graphs** | Less efficient (mostly zeros) | Highly efficient (compact) |


**Summary:** Adjacency lists are generally better for the types of problems we explore because they require less space and allow faster neighbor discovery in sparse graphs.





---





# 4.3: Breadth First Search (BFS)


## 1. Introduction to Systematic Exploration

Breadth First Search (BFS) is our first strategy to systematically explore a graph. The core objective is **reachability**:

- Start from a vertex and mark it as reachable.
- Systematically mark all the neighbors of already known reachable markings.
- Look at the neighbors of those neighbors and mark them as reachable.
- Stop when the target is marked.
- **Avoid Redundancy:** To prevent going around and redoing work, we must keep track of which vertices are already marked.

<img width="402" height="432" alt="image" src="https://github.com/user-attachments/assets/a9487420-a6c9-45b4-ad06-4aa02522e38c" />
<img width="327" height="359" alt="image" src="https://github.com/user-attachments/assets/2402d3f2-0720-44ac-9f01-3c5015ec8f9b" />


### Comparison: BFS vs. DFS

- **Breadth First Search (BFS):** Propagates one level at a time.

  - Start with a vertex reachable in 0 steps.
  - Find all vertices reachable in 1 step.
  - From 1-step vertices, find all reachable in 2 steps, and so on.
  - The set of nodes grows broader and broader.
- **Depth First Search (DFS):** Picks one neighbor and explores as far as possible.

  - If a neighbor is reached, move to its neighbor immediately.
  - Keep exploring until stuck, then backtrack to find unexplored neighbors.

---

## 2. BFS Mechanism and Data Structures

In BFS, we explore the graph level by level (1 step away, then 2 steps away, etc.). Whenever we reach a vertex for the first time, it must be **explored** (looking at its neighbors). We must ensure we do not visit a vertex twice to avoid infinite loops.

### Tracking Information

We maintain two states for vertices:

1. **Visited:** The vertex has been seen for the first time.
2. **Explored:** The neighbors of the visited vertex have been processed.

We use a **dictionary or function** to mark each vertex (0 to $n-1$) as `True` or `False` for its visited status. Initially, everything is `False`.

### The Queue Data Structure

A **Queue** is used to manage vertices that are visited but pending exploration. It follows the "First-In, First-Out" (FIFO) principle.

- **Entry:** Join the queue at one end (append).
- **Exit:** Move out from the other end (head of the queue).

#### Python Implementation of a Queue Class

```python
class Queue:
    def __init__(self):
        self.queue = []

    def addq(self, v):
        self.queue.append(v)

    def delq(self):
        v = None
        if not self.isempty():
            v = self.queue[0]
            self.queue = self.queue[1:]
        return v

    def isempty(self):
        return self.queue == []

    def __str__(self):
        return str(self.queue)

```

<img width="600" height="208" alt="image" src="https://github.com/user-attachments/assets/0ff99e94-2ef4-4295-829c-e910bf8e3481" />

---

## 3. The BFS Algorithm

### Basic Step

For a neighbor $j$ of vertex $i$:

- If `visited[j]` is `False`:

  - Mark `visited[j] = True`.
  - Add $j$ to the Queue (to explore its neighbors later).

### Initialization

- `visited` is `False` for every $v$.
- Queue is empty.
- Start BFS at vertex $j$: mark `visited[j] = True` and add $j$ to the Queue.

### Python Code (Adjacency Matrix Version)

```python
def BFS(AMat, v):
    (rows, cols) = AMat.shape
    visited = {}
    for i in range(rows):
        visited[i] = False
    
    q = Queue()
    visited[v] = True
    q.addq(v)
    
    while not q.isempty():
        j = q.delq()
        for k in neighbors(AMat, j):
            if not visited[k]:
                visited[k] = True
                q.addq(k)
                
    return visited

```
<img width="857" height="368" alt="image" src="https://github.com/user-attachments/assets/19feafa3-6c18-4dd4-a9dc-27cc37c5fa12" />
<img width="864" height="463" alt="image" src="https://github.com/user-attachments/assets/84e1d601-c4d3-462f-a358-c6a09468ee13" />

---

## 4. Complexity Analysis

Let $n$ be the number of vertices and $m$ be the number of edges.

### Adjacency Matrix Representation

- **Initialization:** $O(n)$ to set `visited` to `False`.
- **Exploration:** Each reachable vertex enters and leaves the queue exactly once ($n$ vertices).
- **Neighbor Scanning:** For each vertex $j$, we scan the entire row in the matrix ($n$ entries) to find neighbors.
- **Total Complexity:** $n \times n = O(n^2)$.

### Adjacency List Representation (Amortized Analysis)

- **Amortized Analysis:** Instead of looking at individual operations, we look at the cumulative work.
- **Sum of Degrees:** The sum of degrees across all vertices is $2m$ (each edge $(i, j)$ contributes to the degree of both $i$ and $j$).
- **Exploration:** The total time spent exploring neighbors across all $n$ vertices is proportional to the sum of their degrees.
- **Total Complexity:** $O(n + m)$.

  - $n$ for initialization/visiting.
  - $m$ for exploring all edges.

> [!IMPORTANT]
> $O(n + m)$ is considered **linear time** for a graph. Since any serious problem requires examining the entire graph (all vertices and edges), this is the best possible complexity.
>
>

---

## 5. Enhancing BFS: Paths and Levels

### Recovering the Path (Parent Tracking)

To know how we reached a vertex $k$, we record its **parent** $j$ (the vertex from which $k$ was first visited).

- Maintain a `parent` dictionary.
- When marking `visited[k] = True`, set `parent[k] = j`.
- Trace back from the target to the source using parent links.

### Finding Shortest Paths (Level Tracking)

Since BFS explores level by level, the first time we see a vertex, we have reached it via the shortest possible path (in terms of number of edges).

- Maintain a `level` dictionary.
- `level[source] = 0`.
- If vertex $k$ is reached from $j$, `level[k] = level[j] + 1`.

### Python Code (Adjacency List with Levels and Parents)

```python
def BFS_Level_Parent(AList, v):
    level = {}
    parent = {}
    for i in AList.keys():
        level[i] = -1  # -1 indicates unvisited
        parent[i] = -1
    
    q = Queue()
    level[v] = 0
    q.addq(v)
    
    while not q.isempty():
        j = q.delq()
        for k in AList[j]:
            if level[k] == -1:
                level[k] = level[j] + 1
                parent[k] = j
                q.addq(k)
                
    return (level, parent)

```

<img width="878" height="407" alt="image" src="https://github.com/user-attachments/assets/86cae399-8202-4a8a-b61d-fe1411574a7f" />
<img width="888" height="446" alt="image" src="https://github.com/user-attachments/assets/d6dad15d-5b26-4f83-85b1-e9954ed824b9" />
<img width="854" height="452" alt="image" src="https://github.com/user-attachments/assets/8819905e-e8cb-40de-8815-277216eef28f" />

---

## 6. Summary

- **BFS** explores graphs systematically level by level.
- **Reachability:** Use a `visited` dictionary.
- **Efficiency:** Adjacency List yields $O(n + m)$, whereas Adjacency Matrix yields $O(n^2)$.
- **Path Recovery:** Use a `parent` dictionary to trace the path.
- **Shortest Path:** In unweighted graphs, `level` information provides the shortest path length (number of edges).
- **Note:** In weighted graphs (where edges have costs/distances), BFS alone is not sufficient for the shortest path; this is covered in later lectures.






---



# 4.4: Depth First Search (DFS)

## 1. Introduction to Depth First Search

Depth First Search (DFS) is one of the two fundamental strategies to search and find reachability in graphs. In contrast to Breadth First Search (BFS), which explores all neighbors at one level before moving to the next, DFS follows a single path of exploration as far as possible.

### Core Strategy:

- Start from some vertex.
- Look at any one unexplored neighbor.
- Instead of coming back to look at more unexplored neighbors of the original vertex, proceed directly from the neighbor.
- Follow a long path until getting "stuck" (reaching a vertex with no unexplored neighbors).
- Backtrack systematically to the most recent vertex that has unexplored neighbors and continue the process.

> [!NOTE]
> It is like reading a web page: instead of reading the rest of the text, you click the first link you see, go to a new page, click the first link there, and continue until you hit a dead end, then backtrack.
>
>

---

## 2. Data Structure: The Stack

While BFS uses a **Queue** (First-In, First-Out) to process vertices in the order they are discovered, DFS uses a **Stack** (Last-In, First-Out).

- **Stack Logic:** Imagine a stack of books. The book you take out is the one you put on last.
- **Backtracking:** The stack ensures that when we backtrack, we return to the most recent vertex from which a choice was made.
- **Implicit Stack:** In many implementations, the stack is maintained implicitly through **recursion**.

---

## 3. DFS Execution Example

Consider a graph starting from vertex 4:

1. **Start at 4:** Mark 4, pick its smallest neighbor, say **0**. Suspend 4 and move to 0. (Stack: `[4]`)
2. **Move to 0:** Neighbors are 1 and 2. Pick **1**. Suspend 0 and move to 1. (Stack: `[4, 0]`)
3. **Move to 1:** Neighbors are 0 and 2. 0 is visited, so pick **2**. Suspend 1 and move to 2. (Stack: `[4, 0, 1]`)
4. **Stuck at 2:** All neighbors of 2 are already explored. Backtrack to 1.
5. **Backtrack to 1:** No more unexplored neighbors for 1. Backtrack to 0.
6. **Backtrack to 0:** Even though 0 has neighbor 2, 2 has already been visited via the path through 1. Backtrack to 4.
7. **Backtrack to 4:** Pick the next neighbor, **3**. Suspend 4 and move to 3. (Stack: `[4]`)
8. **Move to 3:** Neighbors are 5 and 6. Pick **5**. Suspend 3 and move to 5. (Stack: `[4, 3]`)
9. **Move to 5:** Pick **6**. Suspend 5 and move to 6. (Stack: `[4, 3, 5]`)
10. **Stuck at 6:** Neighbors 3 and 5 are marked visited. Backtrack to 5.
11. **Backtrack to 5:** Vertex 5 has another neighbor, **7**. Suspend 5 and move to 7. (Stack: `[4, 3, 5]`)
12. **Move to 7:** Pick **8**. Suspend 7 and move to 8. (Stack: `[4, 3, 5, 7]`)
13. **Move to 8:** Pick **9**. Suspend 8 and move to 9. (Stack: `[4, 3, 5, 7, 8]`)
14. **Dead end at 9:** Backtrack through 8, 7, 5, 3, and finally 4. The stack is now empty.

<img width="1178" height="584" alt="image" src="https://github.com/user-attachments/assets/5c38cbb6-3815-4636-91c3-80f4ecb6a334" />
<img width="1168" height="608" alt="image" src="https://github.com/user-attachments/assets/22de36c0-03cf-4d0c-b20b-4e5f526778c3" />

---

## 4. Implementation in Python

### 4.1. Recursive DFS (Internal State)

This version passes the `visited` and `parent` dictionaries as parameters.

```python
def DFSInit(AMat):
    # initialization
    (rows, cols) = AMat.shape
    visited = {}
    parent = {}
    for i in range(rows):
        visited[i] = False
        parent[i] = -1
    return (visited, parent)

def DFS(AMat, v, visited, parent):
    visited[v] = True
    for k in neighbors(AMat, v):
        if not visited[k]:
            parent[k] = v
            (visited, parent) = DFS(AMat, k, visited, parent)
    return (visited, parent)

```
> For DFS we don't have to construct The **stack** like we had to construct The **queue** for BFS Because for DFS **Recursion** does the work
>
> 
### 4.2. Global Recursive DFS (Adjacency Matrix)

Using global dictionaries to avoid passing them repeatedly.

```python
visited = {}
parent = {}

def DFSInitGlobal(AMat):
    for i in range(AMat.shape[0]):
        visited[i] = False
        parent[i] = -1

def DFSGlobal(AMat, v):
    visited[v] = True
    for k in neighbors(AMat, v):
        if not visited[k]:
            parent[k] = v
            DFSGlobal(AMat, k)

```

### 4.3. Global Recursive DFS (Adjacency List)

Preferred for better efficiency in sparse graphs.

```python
visited = {}
parent = {}

def DFSInitListGlobal(AList):
    for i in AList.keys():
        visited[i] = False
        parent[i] = -1

def DFSListGlobal(AList, v):
    visited[v] = True
    for k in AList[v]:
        if not visited[k]:
            parent[k] = v
            DFSListGlobal(AList, k)

```
<img width="606" height="523" alt="image" src="https://github.com/user-attachments/assets/442f3783-097d-4219-9f79-6c5afd4f2dfe" />

---

## 5. Complexity Analysis

| Representation | Time Complexity |
| --- | --- |
| **Adjacency Matrix** | $O(n^2)$ |
| **Adjacency List** | $O(m+n)$ |


- **Vertex Processing:** Each vertex is visited once and explored once ($O(n)$).
- **Edge Processing:**

  - In an **Adjacency Matrix**, deciding which neighbors to process takes $O(n)$ for each vertex, leading to $O(n^2)$.
  - In an **Adjacency List**, we scan the degree of each vertex, totaling $O(m)$ edges, leading to $O(m + n)$.

---

## 6. Comparison: DFS vs. BFS

- **Shortest Paths:** Unlike BFS, DFS **does not** generally find the shortest path. It follows a "roundabout" route based on the order of exploration.
- **Data Structure:** BFS uses a **Queue**; DFS uses a **Stack** (often implicit via recursion).
- **Utility:** While DFS lacks the shortest-path advantage, it is often more informative about the **structure of a graph** (e.g., detecting cycles, strongly connected components).

> [!TIP]
> DFS is often the primary way of exploring a graph because it provides deep structural information that BFS cannot easily capture.
>
>





---






# 4.5: Applications of BFS and DFS

## 1. Beyond Reachability

Graph exploration using BFS and DFS primarily focuses on:

- **Reachability:** Determining if a path exists between vertices.
- **Path Recovery:** Finding the actual path between vertices.
- **Distance (BFS only):** Recovering the distance in terms of the number of edges, as BFS works level by level to discover shortest paths.

Beyond these, BFS and DFS are used to discover structural properties of graphs, such as connected components and cycles.

---

## 2. Connected Components (Undirected Graphs)

A graph may consist of multiple parts that are not connected to each other.

- **Connected Graph:** You can get from every vertex to every other vertex.
- **Disconnected Graph:** There are parts which cannot be reached from other parts.
- **Connected Component:** A maximal set of vertices that are connected. "Maximal" means everything in the set is connected, and no more vertices can be added to the set while keeping it connected.

### Example: Finding Components

In a disconnected graph, we identify components through reachability:

1. Assign each vertex a **component number**.
2. Start with vertex 0 (or the smallest unvisited node).
3. Run BFS or DFS to explore everything reachable from that vertex.
4. Assign all reached vertices the current `comp_id` (e.g., 0).
5. Find the smallest unvisited node, increment the `comp_id` (e.g., 1), and repeat the process.
6. Continue until all vertices have been visited.

<img width="284" height="421" alt="image" src="https://github.com/user-attachments/assets/5bcf86db-33a6-459b-99af-469b7d4263e7" />


### BFS Implementation for Connected Components

The following Python-style logic uses an adjacency list to find component IDs:

```python
def find_components(AList):
    component = {}
    for i in AList.keys():
        component[i] = -1 # Initialize all to -1 (unvisited)
    
    comp_id = 0
    seen = 0
    n = len(AList.keys())
    
    while seen < n:
        # Find the smallest vertex not yet visited
        start_v = min([i for i in AList.keys() if component[i] == -1])
        
        # Run BFS from start_v
        visited = BFS(AList, start_v) 
        
        # Mark all vertices reachable in this round
        for node in visited.keys():
            if visited[node]:
                seen += 1
                component[node] = comp_id
        
        comp_id += 1 # Increment for the next component
        
    return component

```

---

## 3. Detecting Cycles

A **cycle** is a walk that starts and ends at the same vertex.

- **Simple Cycle:** A cycle that does not repeat intermediate vertices or edges.
- **Acyclic Graph:** A graph with no cycles.

<img width="832" height="413" alt="image" src="https://github.com/user-attachments/assets/6882edeb-4472-44b7-ae1b-d5198a3b10b9" />


### Cycle Detection using BFS

When BFS explores a graph, the edges used to reach unvisited neighbors form a **BFS Tree** (or a **Forest** if the graph is disconnected).

- **Tree Edges:** Edges used by BFS to visit new nodes.
- **Non-tree Edges:** Edges in the graph that were discarded because the target vertex was already visited.
- **Property:** In an undirected graph, **any non-tree edge corresponds to a cycle**.

<img width="919" height="488" alt="image" src="https://github.com/user-attachments/assets/4f2d5e92-abe5-40cd-806a-e8f5f341e10e" />

---

## 4. DFS Numbering: Pre and Post Numbers

DFS can record the progress of exploration using a counter to assign two numbers to each vertex:

1. **Pre-number:** The counter value when exploration of the vertex *starts*.
2. **Post-number:** The counter value when exploration of the vertex *finishes*.

### DFS with Pre/Post Implementation

```python
def DFS(AList, v, count, visited, pre, post):
    visited[v] = True
    pre[v] = count
    count += 1
    
    for k in AList[v]:
        if not visited[k]:
            count = DFS(AList, k, count, visited, pre, post)
            
    post[v] = count
    count += 1
    return count

```


<img width="897" height="440" alt="image" src="https://github.com/user-attachments/assets/30d3da41-137d-47ed-a2e2-4dd269fa8af4" />


---

## 5. Classification of Edges (Directed Graphs)

In directed graphs, cycles are more complex because they must follow the direction of the arrows. Non-tree edges in a **DFS Tree** are classified into three flavors based on their pre and post intervals:

| Edge Type | Description | Interval Property (u→v) |
| --- | --- | --- |
| **Tree/Forward Edge** | From ancestor to descendant. | v's interval is subsumed by u's (\[pre_v​,post_v​\]⊂\[pre_u​,post_u​\]). |
| **Back Edge** | From descendant to ancestor. | u's interval is sitting inside v's (\[pre_u​,post_u​\]⊂\[pre_v​,post_v​\]). |
| **Cross Edge** | Between disjoint branches. | Intervals of u and v are disjoint. |

> [!IMPORTANT]
> In a directed graph, **only back edges generate cycles**. Forward and cross edges do not complete a cycle following the arrows.
>
>

<img width="892" height="423" alt="image" src="https://github.com/user-attachments/assets/5033bab1-f57c-42b1-8660-c67648d7ef9b" />
<img width="914" height="393" alt="image" src="https://github.com/user-attachments/assets/c823ec07-4b01-47b3-8ec6-ef650b8d1461" />

---

## 6. Summary of Applications

- **BFS/DFS:** Reachability, finding parents/paths.
- **BFS:** Shortest path distance (unweighted).
- **Connected Components:** Identifying isolated subgraphs.
- **Cycles:** Found by identifying non-tree edges (specifically back edges in directed graphs).
- **Strongly Connected Components (SCC):** Subsets where every vertex is reachable from every other vertex (requires bidirectional connectivity).
- **Structural Features:** Identifying **articulation points** (cut vertices) and **bridges** (cut edges) using DFS numbering.

**Conclusion:** While BFS is simpler for distances, **DFS is the preferred tool** for uncovering the deep structure of a graph because its numbering scheme provides significantly more information.




---




# 4.6: Directed Acyclic Graphs (DAGs)



Formally, we have a directed acyclic graph (DAG) $G=(V,E)$, which is a directed graph without directed cycles.

### Topological Sorting

One of the fundamental problems we want to solve using a DAG is topological sorting.

- **Definition**: Enumerate $V=\{0, 1, \dots, n-1\}$ such that for any $(i,j) \in E$, $i$ appears before $j$.
- **Purpose**: This represents a **feasible schedule**.
- **Example Application**: Imagine the DAG represents prerequisites between courses.

  - If task $i$ must appear before $j$, and $j$ must appear before $i$, this is impossible!.
- **Claim**: Every DAG can be topologically sorted.

<img width="915" height="415" alt="image" src="https://github.com/user-attachments/assets/c6ef2e6a-b2cb-43d8-98f2-17a26204cf73" />

## Tasks and Dependencies

DAGs are useful for telling us about things like tasks and their dependencies.

### Case Study: Construction Schedule

Consider the following constraints on a sequence:

- Lay conduits before tiles and plastering.

<img width="870" height="410" alt="image" src="https://github.com/user-attachments/assets/34cf1972-dba7-4fef-874c-9334eb1ebc43" />

- **Objective**: Find a schedule.
- **Method**: Enumerate $V=\{0, 1, \dots, n-1\}$ such that for any $(i,j) \in E$, $i$ appears before $j$.

## Longest Paths in DAGs

A common question in project management is: "How long will the work take?". This can be solved by finding the longest path in the DAG.

### Longest Path Algorithm

To compute the longest path to a vertex $i$:

- If $indegree(i) = 0$, $longest\text{-}path\text{-}to(i) = 0$.
- If $indegree(i) > 0$, the longest path to $i$ is 1 more than the longest path to its incoming neighbours.
- **Formula**: $longest\text{-}path\text{-}to(i) = 1 + \max \{longest\text{-}path\text{-}to(j) \mid (j,i) \in E\}$.

### Systematic Computation

1. Compute the $indegree$ of each vertex.

   - This is done by scanning each column of the adjacency matrix.
2. Initialize $longest\text{-}path\text{-}to$ to $0$ for all vertices.
3. List a vertex with $indegree$ 0 and remove it from the DAG.
4. Update $indegrees$ and $longest\ path$ values for the remaining vertices.
5. Repeat until all vertices are listed in topological order.

### Using Topological Ordering

Let $i_0, i_1, \dots, i_{n-1}$ be a topological ordering of $V$.

- In this list, all neighbors of $i_k$ appear before it.
- From left to right, compute $longest\text{-}path\text{-}to(i_k)$ as $1 + \max \{longest\text{-}path\text{-}to(i_j) \mid (i_j, i_k) \in E\}$.



---




# 4.7 Topological Sorting 

---

## **1. Directed Acyclic Graphs (DAGs)**
A directed graph $G = (V, E)$ without directed cycles is known as a **Directed Acyclic Graph**, or a **DAG** for short.

### **1.1 Conceptual Framework**
*   **Tasks and Dependencies:** DAGs are a natural way to represent dependencies. 
*   **Precedence Relations:** Vertices represent tasks, and an edge $(t, u)$ exists if task $t$ has to be completed before task $u$.
*   **Feasible Schedule:** A topological sort provides a valid sequence to complete a program, respecting all dependencies.

<img width="414" height="316" alt="image" src="https://github.com/user-attachments/assets/566a4129-77b6-45f4-b2ee-0135176302d2" />

---

## **2. Definition: Topological Sorting**
A topological sort of a directed graph $G$ is an ordering of its nodes as $v_1, v_2, \dots, v_n$ so that for every edge $(v_i, v_j)$, we have $i < j$. In other words, all edges point "forward" or "left to right" in the ordering.

### **2.1 Impossibility in Cyclic Graphs**
A graph with directed cycles cannot be sorted topologically.
*   **Reasoning:** A path $i \leadsto j$ means $i$ must be listed before $j$.
*   **Contradiction:** In a cycle, there are paths $i \leadsto j$ and $j \leadsto i$. This implies $i$ must appear before $j$ AND $j$ must appear before $i$, which is impossible.

> [!TIP]
> **Theorem:** If $G$ has a topological ordering, then $G$ is a DAG.
> **Converse:** If $G$ is a DAG, then $G$ has a topological ordering.

---

## **3. The "Indegree 0" Algorithm**

### **3.1 Fundamental Fact**
**Claim:** Every DAG has at least one vertex with **indegree 0**.
*   **Definition:** Indegree is the number of edges coming into a vertex. A vertex with no dependencies has $indegree(v) = 0$.
*   **Proof by Contradiction:** Suppose every vertex has $indegree > 0$. Start at any vertex and follow an edge back to a predecessor. Repeat this $n$ times. Since there are only $n$ vertices, we must eventually hit a vertex seen before, implying a cycle, which is impossible in a DAG.

### **3.2 General Strategy**
1.  First list vertices with no dependencies ($indegree = 0$).
2.  Delete the listed vertex and all edges originating from it.
3.  What remains is again a DAG!.
4.  Find another vertex with $indegree = 0$ in the remaining graph.
5.  Repeat until all vertices are listed.

---

## **4. Step-by-Step Algorithm Execution**
Consider a DAG with $V=\{0, 1, 2, 3, 4, 5, 6, 7\}$ and edges representing dependencies.

### **4.1 Initialization**
Compute the indegree of each vertex by scanning columns of the adjacency matrix or iterating through the adjacency list.

| Vertex | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Initial Indegree** | 0 | 0 | 2 | 1 | 1 | 2 | 1 | 4 |

<img width="473" height="343" alt="image" src="https://github.com/user-attachments/assets/8319ab92-e28a-45d6-97cb-ae9d7360cc4b" />
<img width="470" height="416" alt="image" src="https://github.com/user-attachments/assets/2f66fbba-b94d-4a99-87dd-3cc2470674bc" />
<img width="482" height="379" alt="image" src="https://github.com/user-attachments/assets/afe9950e-0f7c-42e0-8843-b0d4512ef214" />



### **4.2 Processing Trace**
1.  **Enumerate 1:** Indegree is 0. List **1**. Update neighbors (2, 7).
    *   New Indegrees: $2 \rightarrow 1, 7 \rightarrow 3$.
2.  **Enumerate 0:** Indegree is 0. List **1, 0**. Update neighbors (2, 3, 4).
    *   New Indegrees: $2 \rightarrow 0, 3 \rightarrow 0, 4 \rightarrow 0$.
3.  **Choices:** Vertices 2, 3, and 4 now all have indegree 0.
4.  **Enumerate 3:** List **1, 0, 3**. Update neighbor 5.
    *   New Indegree: $5 \rightarrow 1$.
5.  **Enumerate 2:** List **1, 0, 3, 2**. Update neighbor 5.
    *   New Indegree: $5 \rightarrow 0$.
6.  **Enumerate 5:** List **1, 0, 3, 2, 5**. Update neighbor 6.
    *   New Indegree: $6 \rightarrow 0$.
7.  **Enumerate 6:** List **1, 0, 3, 2, 5, 6**.
8.  **Enumerate 4:** List **1, 0, 3, 2, 5, 6, 4**. Update neighbor 7.
    *   New Indegree: $7 \rightarrow 0$.
9.  **Enumerate 7:** List **1, 0, 3, 2, 5, 6, 4, 7**.

**Final Topologically Sorted Sequence:** `1, 0, 3, 2, 5, 6, 4, 7`.

> [!NOTE]
> **Non-Uniqueness:** More than one topological sort is possible depending on the choice of which vertex with indegree 0 to list next.

---

## **5. Implementations and Complexity**

### **5.1 Adjacency Matrix Implementation**
```python
def toposort(AMat):
    (rows,cols) = AMat.shape
    indegree = {}
    toposortlist = []
    
    for c in range(cols): # Initialize indegrees to 0
        indegree[c] = 0
        for r in range(rows): # Scan columns for incoming edges
            if AMat[r,c] == 1:
                indegree[c] = indegree[c] + 1
                
    for i in range(rows):
        # Identify next vertex to enumerate
        j = min([k for k in range(cols) if indegree[k] == 0])
        toposortlist.append(j)
        indegree[j] = indegree[j] - 1 # Mark as processed
        
        # Updating indegrees of neighbors
        for k in range(cols):
            if AMat[j,k] == 1:
                indegree[k] = indegree[k] - 1
                
    return(toposortlist)
```
**Complexity Analysis ($O(n^2)$):**
*   Initializing indegrees requires scanning the $n \times n$ matrix: $O(n^2)$.
*   The outer loop runs $n$ times.
*   Identifying the next vertex and updating neighbors both take $O(n)$ within the loop.
*   Total: $O(n^2 + n \times n) = O(n^2)$.

### **5.2 Adjacency List Implementation (Queue-based)**
This approach uses a queue to explicitly keep track of all vertices eligible to be enumerated.

```python
def toposortlist(AList):
    (indegree, toposortlist) = ({}, [])
    for u in AList.keys():
        indegree[u] = 0
    
    # Compute indegrees
    for u in AList.keys():
        for v in AList[u]:
            indegree[v] = indegree[v] + 1
            
    zerodegreeq = Queue() # Maintain queue of indegree 0 vertices
    for u in AList.keys():
        if indegree[u] == 0:
            zerodegreeq.addq(u)
            
    while (not zerodegreeq.isempty()):
        j = zerodegreeq.delq()
        toposortlist.append(j)
        
        for k in AList[j]: # Update neighbors
            indegree[k] = indegree[k] - 1
            if indegree[k] == 0:
                zerodegreeq.addq(k)
                
    return (toposortlist)
```
**Complexity Analysis ($O(m+n)$):**
*   Initializing and computing indegrees requires scanning all vertices and edges: $O(m+n)$.
*   The loop to enumerate vertices runs $n$ times.
*   Updating indegrees is done once for each edge: Amortized $O(m)$.
*   Total: $O(m+n)$.

---

## **6. Alternative Strategy: DFS-Based**
A simple algorithm to topologically sort a DAG using Depth-First Search:
1.  Call $DFS(G)$ to compute finishing times $v.f$ for each vertex $v$.
2.  As each vertex is finished, insert it onto the front of a linked list.
3.  The list will contain vertices in decreasing order of their finishing times.

**Time Complexity:** $\Theta(V+E)$.

<img width="1422" height="975" alt="image" src="https://github.com/user-attachments/assets/40be95d6-5bf6-4d2d-9202-801fb588828e" />
