# 4.1: Introduction to Graphs  

---

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
