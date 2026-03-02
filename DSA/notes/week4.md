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


