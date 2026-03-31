# 8.1: Algorithms and Data Structures/1: Algorithms and Complexity Analysis



## **1.Overview**

### **1.1. Recap**
*   Reviewed Application Programs across various sectors.
*   Understood typical three-tier architectures, their classification (1-tier to n-tier), and historical evolution.
*   Familiarized with fundamental web technologies (URLs, HTML, HTTP) and technologies like scripting and servlets.
*   Learnt to use SQL from a programming language and build Python Web Applications with PostgreSQL using `psycopg2`.
*   Understood Rapid Application Development (RAD) and issues in application performance and security.
*   Learnt the distinctive features of Mobile Apps.

### **1.2. Objectives**
*   To define **Algorithms** and identify their difference with **Programs**.
*   To analyze algorithms for performance in terms of **time, space, power, etc.**.
*   To introduce **Asymptotic notation** for the representation of complexity.
*   To consider the complexity of **common algorithms**.

---

## **2. Algorithms and Programs**

### **2.1. Algorithm**
*   An algorithm is a finite sequence of well-defined, computer-implementable instructions, typically to solve a class of specific problems or to perform a computation.
*   Algorithms are always unambiguous and are used as specifications for performing calculations, data processing, automated reasoning, and other tasks.
*   **Termination:** An algorithm **must terminate**; given an input, it must produce an output in finite time.

### **2.2. Program**
*   A computer program is a collection of instructions that can be executed by a computer to perform a specific task.
*   A computer program is usually written by a computer programmer in a programming language.
*   A program **implements an algorithm**.
*   A program may or may not terminate (e.g., an Operating System).

---

## **3. Analysis of Algorithms**

### **3.1. Motivation: Why analyze?**
*   **Practical Reasons:** Resources are scarce, there is a greed to do more with less, and a need to avoid performance bugs.
*   **Core Issues:**
    *   **Predict performance:** Determine how much time a specific algorithm (e.g., binary search) takes.
    *   **Compare algorithms:** Evaluate which algorithm is faster (e.g., how quick is Quicksort).
    *   **Provide guarantees:** Ensure operations stay within certain bounds (e.g., Red-Black tree inserts in $O(\log n)$).
    *   **Understand theoretical basis:** For example, knowing that sorting by comparison cannot do better than $\Omega(n \log n)$.

### **3.2. Factors: What to analyze?**
The core issue is that one cannot control what cannot be measured. Analysis typically measures resources used as a function of **input size ($n$)**.
*   **Time:** The most common analysis factor; representative of related factors like power, bandwidth, and processors.
*   **Space:** Widely explored and important for handheld devices.

#### **Examples of Analysis:**
*   **Sum of Natural Numbers:**
    *   *Algorithm:* `int sum(int n) { int s = 0; for(; n > 0; --n) s = s + n; return s; }`.
    *   **Time:** $T(n) = n$ (additions).
    *   **Space:** $S(n) = 2$ (variables $n, s$).
*   **Find a Character in a String:**
    *   *Algorithm:* `int find(char *str, char c) { for(int i = 0; i < strlen(str); ++i) if (str[i] == c) return i; return 0; }`.
    *   **Time:** If $n = strlen(str)$, $T(n) = n(compare) + n * T(strlen(str)) \approx n + n^2 \approx n^2$.
    *   **Space:** $S(n) = 3$ (str, c, i).
*   **Minimum of a Sequence of Numbers:**
    *   **Time:** $T(n) = n - 1$ (comparison of value).
    *   **Space:** Depends on implementation (e.g., $S(n) = n + 3$ if using an array, or $S(n) = 3$ if reading one by one).

### **3.3. Methodologies: How to analyze?**
*   **Counting Models:** Total running time = Sum of (cost $\times$ frequency) for all operations.
    *   *Machine Model:* Random Access Machine (RAM) Computing Model.
*   **Asymptotic Analysis:** Comparing the **growth rate** or how time/space increases with input size, rather than actual times.
*   **Recursive Algorithms:** Analyzed using **Generating Functions** or the **Master Theorem**.

---

## **4. Counting Models and Asymptotic Analysis**

### **4.1. Factorial Example (Counting Model)**
*   **Recursive Factorial:**
    *   Time $T(n) = n - 1$ (multiplications).
    *   Space $S(n) = n + 1$ (due to $n$ in recursive call stack).
*   **Iterative Factorial:**
    *   Time $T(n) = n$ (multiplications).
    *   Space $S(n) = 2$ (variables $n, t$).

### **4.2. Function Approximation ($\sim$ Notation)**
*   Estimate running time/memory as a function of input size $N$, ignoring lower-order terms when $N$ is large.
*   $f(n) \sim g(n)$ means $\lim_{N \to \infty} \frac{f(n)}{g(n)} = 1$.

### **4.3. Asymptotic Notation (Big-O)**
*   $O(g(n)) = \{f(n) : \text{there exist positive constants } c \text{ and } n_0 \text{ such that } 0 \le f(n) \le cg(n), \text{ for all } n > n_0\}$.
*   It provides an **upper bound** on a function within a constant factor.
*   Saying a running time is $O(n^2)$ means the **worst-case** running time is bounded from above by a quadratic function.

---

## **5. Complexity Classifications**

### **5.1. Common Order-of-Growth Classifications**
The following set of functions suffices to describe the growth of most common algorithms:

| Order of Growth | Name | Typical Code Framework | Example | $T(2N)/T(N)$ |
| :--- | :--- | :--- | :--- | :--- |
| **1** | Constant | Statement | Add two numbers | 1 |
| **$\log N$** | Logarithmic | Divide in half | Binary Search | $\sim 1$ |
| **$N$** | Linear | Single loop | Find maximum | 2 |
| **$N \log N$** | Linearithmic | Divide and conquer | Mergesort | $\sim 2$ |
| **$N^2$** | Quadratic | Double loop | Check all pairs | 4 |
| **$N^3$** | Cubic | Triple loop | Check all triples | 8 |
| **$2^N$** | Exponential | Exhaustive search | Check all subsets | $T(N)$ |

<img width="826" height="331" alt="image" src="https://github.com/user-attachments/assets/4879ccce-c046-4c35-8f39-85f6861febeb" />

---

## **6. Scenarios: Where to analyze?**

Analysis identifies data configurations or scenarios:
*   **Best Case:** Minimum running time on an input.
*   **Worst Case:** Running time guarantee for any input of size $n$.
*   **Average Case:** Expected running time for a random input of size $n$.
*   **Probabilistic Case:** Expected running time of a randomized algorithm.
*   **Amortized Case:** Worst-case running time for any sequence of $n$ operations.

---

## **7. Complexity Cheat Sheets**

### **7.1. Common Data Structure Operations (Average Case)**
*   **Array:** Access $O(1)$, Search $O(n)$, Insertion $O(n)$, Deletion $O(n)$.
*   **Hash Table:** Search $O(1)$, Insertion $O(1)$, Deletion $O(1)$.
*   **Binary Search Tree:** Access $O(\log n)$, Search $O(\log n)$, Insertion $O(\log n)$, Deletion $O(\log n)$.
*   **B-Tree:** Access $O(\log n)$, Search $O(\log n)$, Insertion $O(\log n)$, Deletion $O(\log n)$.

### **7.2. Array Sorting Algorithms**
| Algorithm | Time (Best) | Time (Average) | Time (Worst) | Space (Worst) |
| :--- | :--- | :--- | :--- | :--- |
| **Quicksort** | $\Omega(n \log n)$ | $\Theta(n \log n)$ | $O(n^2)$ | $O(\log n)$ |
| **Mergesort** | $\Omega(n \log n)$ | $\Theta(n \log n)$ | $O(n \log n)$ | $O(n)$ |
| **Heapsort** | $\Omega(n \log n)$ | $\Theta(n \log n)$ | $O(n \log n)$ | $O(1)$ |
| **Insertion Sort**| $\Omega(n)$ | $\Theta(n^2)$ | $O(n^2)$ | $O(1)$ |






---





# 8.2: Algorithms and Data Structures/2: Data Structures



## **Objectives**
*   Introduction to Data Structures.
*   Review of linear data structures: array, list, stack, and queue.
*   Review of search: linear and binary.



## **1. Data Structure Overview**

### **1.1. Definition**
*   **Data Structure:** A data structure specifies the way of organizing and storing in-memory data that enables efficient access and modification of the data.
*   Broadly classified into two categories:
    1.  **Linear Data Structures**.
    2.  **Non-linear Data Structures**.

### **1.2. Key Operations in Data Management**
For applications relating to data management, a data structure typically acts as a container for data and supports five key operations:
1.  **Create:** Initialize the structure.
2.  **Insert:** Add a data element.
3.  **Delete:** Remove a data element.
4.  **Find / Search:** Locate a data element by value.
5.  **Close:** Wrap up and release the data structure when no longer required.


---

## **2. Linear Data Structures**

### **2.1. Definition**
*   A linear data structure has data elements arranged in a linear or sequential manner such that each member element is connected to its previous and next element.
*   Elements are traversable through a single run.
*   Examples include Array, Linked List, Queue, and Stack.

### **2.2. Array**
*   **Storage:** Data elements are stored at contiguous memory locations.
*   **Access:** Allows random access using an index, which is fast with a cost of $O(1)$.
*   **Flexibility:** Arrays have fixed sizes and are not flexible. If created too large, memory is wasted; if too small, some elements cannot be accommodated.
*   **Insertion and Removal:**
    *   Adding or removing from the **end** of an array is easy ($O(1)$).
    *   Adding or removing at an **any arbitrary position** is costly ($O(n)$) because elements must be moved to maintain consecutive memory locations.

<img width="896" height="140" alt="image" src="https://github.com/user-attachments/assets/7e4605f8-921a-4bf8-aae0-2f7e9eba7a85" />

### **2.3. Linked List**
*   **Storage:** Elements are not required to be stored at contiguous locations. A new element can be stored anywhere in memory where free space is available.
*   **Mechanism:** Each element is stored in a **node** consisting of two parts:
    1.  **Info:** Stores the actual element.
    2.  **Link:** Stores the location (pointer or reference) of the next node.
*   **Header:** A link pointing to the first node of the list.
*   **Access:** Random access is not possible; elements must be accessed sequentially ($O(n)$).
*   **Flexibility:** Size is flexible; it grows or shrinks as elements are inserted or deleted.
*   **Insertion and Removal:**
    *   Efficient ($O(1)$) for arbitrary positions provided the location of the node is already known, as no elements need to be physically moved.

### **2.4. Queue and Stack**
*   **Queue:** A **FIFO** (First In First Out) data structure. The element inserted first is removed first.
*   **Stack:** A **LIFO** (Last In First Out) data structure. The element inserted last is removed first.
*   The definition of these structures is based on their **operations** (e.g., push/pop for stacks) rather than the underlying container (which could be an array or a linked list).

---

## **3. Search Algorithms**

### **3.1. Linear Search**
*   **Process:** The algorithm starts with the first element and compares it with the given key value. If they match, it returns "yes." If not, it proceeds sequentially through the list until a match is found or the full list is traversed.
*   **Example:**
    *   Input: `inputArr = ['a', 'c', 'a', 'd', 'e', 'm', 'i', 'c', 's']`
    *   Search Key: `'i'`
    *   Result: Match found at Step 7.
*   **Complexity:** $O(n)$.

### **3.2. Binary Search**
*   **Constraint:** Requires a **sorted list** as input.
*   **Process:**
    1.  Compare key $k$ with the middle element.
    2.  If matches, return the index.
    3.  If $k >$ middle, repeat on the right sub-list.
    4.  If $k <$ middle, repeat on the left sub-list.
*   **Complexity:** $O(\log n)$.
    *   *Derivation:* The search space is divided by 2 at each step. For $n=16$, it takes 4 steps ($\log_2 16 = 4$) to reach a list of size 1.

<img width="1237" height="609" alt="image" src="https://github.com/user-attachments/assets/79878b83-cc0b-42ad-a932-507e23b40924" />

---

## **4. Performance Summary and Comparison**

### **4.1. Comparison Table (Linear Data Structures)**

| Operation | Array (Unordered) | Array (Ordered) | Linked List (Unordered) | Linked List (Ordered) |
| :--- | :--- | :--- | :--- | :--- |
| **Access** | $O(1)$ | $O(1)$ | $O(n)$ | $O(n)$ |
| **Insert** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Delete** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Search** | $O(n)$ | $O(\log n)$ | $O(n)$ | $O(n)$ |


### **4.2. Complexity of Operations (Average and Worst Case)**

| Data Structure | Average Access | Average Search | Average Insertion | Average Deletion | Worst Access | Worst Search |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Array** | $\Theta(1)$ | $\Theta(n)$ | $\Theta(n)$ | $\Theta(n)$ | $O(1)$ | $O(n)$ |
| **Stack** | $\Theta(n)$ | $\Theta(n)$ | $O(1)$ | $\Theta(1)$ | $O(n)$ | $O(n)$ |
| **Queue** | $\Theta(n)$ | $\Theta(n)$ | $O(1)$ | $\Theta(1)$ | $O(n)$ | $O(n)$ |
| **Singly-Linked List**| $\Theta(n)$ | $\Theta(n)$ | $\Theta(1)$ | $\Theta(1)$ | $O(n)$ | $O(n)$ |
| **Doubly-Linked List**| $\Theta(n)$ | $\Theta(n)$ | $\Theta(1)$ | $\Theta(1)$ | $O(n)$ | $O(n)$ |

<img width="1216" height="552" alt="image" src="https://github.com/user-attachments/assets/684ff02e-5954-4c49-8c47-ba5e34a363fb" />



---






# 8.3: Algorithms and Data Structures/3: Data Structures/2


## **Objectives**
*   Introducing Non-linear Data Structures - graph, tree, hash table.
*   Exploring Binary Search Tree.
*   Comparing Linear and Non-Linear Data Structures.



## **2. Non-linear Data Structures**

### **2.1. Motivation: Why Non-linear?**
From the study of Linear data structures, we observe:
*   All have space complexity $O(n)$, which is optimal.
*   Linked lists have a space overhead (approximately 100% in terms of pointers).
*   Complexity is often identical for Worst as well as Average case.
*   They offer satisfactory complexity for some operations while being unsatisfactory on others.

**Comparison of Linear Data Structures:**
| Operation | Array (Unordered) | Array (Ordered) | Linked List (Unordered) | Linked List (Ordered) |
| :--- | :--- | :--- | :--- | :--- |
| **Access** | $O(1)$ | $O(1)$ | $O(n)$ | $O(n)$ |
| **Insert** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Delete** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Search** | $O(n)$ | $O(\log n)$ | $O(n)$ | $O(n)$ |


**Conclusion:** Non-Linear data structures can be used to trade-off between these extremes and achieve a balanced good performance for all operations.

<img width="790" height="255" alt="image" src="https://github.com/user-attachments/assets/c62b927f-3840-4cf2-a877-3e4dc4e93f15" />

### **2.2. General Definition**
*   Data items are **not arranged in a sequence**.
*   Each element may have **multiple paths** to connect to other elements.
*   Traversing through the elements is not possible in one run.

### **2.3. Common Non-Linear Structures**

#### **2.3.1. Graph**
*   A collection of vertices $V$ (storing elements) and connecting edges (links) $E$: $G = <V, E>$ where $E \subseteq V \times V$.
*   **Variants:** Undirected/Directed, Cyclic/Acyclic, Unweighted/Weighted, Disconnected/Connected.
*   **Examples:** ER Diagrams, Networks (Electrical/Water), Facebook Friendships, Knowledge Graphs.

#### **2.3.2. Tree**
*   A **connected acyclic graph** representing hierarchical relationships.
*   **Variants:** Rooted/Unrooted, Binary/n-ary, Balanced/Unbalanced.
*   **Examples:** Composite Attributes, Family Genealogy, Search Trees.

#### **2.3.3. Hash Table**
*   Implements an **associative array** abstract data type.
*   Uses a **hash function** to compute an index (hash code) into an array of buckets or slots.
*   **Examples:** Associative arrays, Database indexing, Caches.

---

## **3. Tree Terminology and Properties**

### **3.1. Key Definitions**
*   **Root:** The node at the top; only one per tree.
*   **Parent:** The predecessor of any node. Every node except the Root has a unique parent.
*   **Child:** The descendant of a node.
*   **Leaf:** A node with no children.
*   **Internal Node:** A node with at least one child.
*   **Siblings:** Nodes having the same parent.
*   **Arity (Degree):** Number of children of a node. The maximum arity of a node defines the arity of the tree.
*   **Levels:** Root is at Level 0. Level is the distance from the root.
*   **Height:** The maximum level in a tree.

### **3.2. Binary Tree Facts**
*   A tree with $n$ nodes has $n-1$ edges.
*   The maximum number of nodes at level $l$ is $2^l$.
*   If $h$ is the height of a binary tree of $n$ nodes:
    *   $h + 1 \le n \le 2^{h+1} - 1$
    *   $\lceil \log(n+1) \rceil - 1 \le h \le n - 1$
    *   $O(\log n) \le h \le O(n)$
*   For a $k$-ary tree, height is $O(\log_k n) \le h \le O(n)$.

---

## **4. Binary Search Tree (BST)**

### **4.1. Concept: Adaptation of Binary Search**
*   Binary search splits an array; conceptually, the **Middle Element** is the Root, and the left/right sub-arrays are sub-trees.
*   By progressing recursively, we form a Binary Search Tree.

<img width="1043" height="637" alt="image" src="https://github.com/user-attachments/assets/e04d1805-740b-4a40-9a48-000cc7dd88c4" />

### **4.2. Definition**
*   **BST Rule:** For any node with value $X$:
    *   Every node in the **left sub-tree** has a value $< X$.
    *   Every node in the **right sub-tree** has a value $> X$.
*   **Node Structure:** Each node consists of an element, a link to the left child (LC), and a link to the right child (RC).

### **4.3. Building a BST**
**Example Sequence:** E, L, P, H, A, N, T
1.  Insert **E**: Becomes Root.
2.  Insert **L**: $L > E$, goes to Right Child.
3.  Insert **P**: $P > E, P > L$, goes to Right Child of L.
4.  Insert **H**: $H > E, H < L$, goes to Left Child of L.
5.  Insert **A**: $A < E$, goes to Left Child of E.
6.  Insert **N**: $N > E, N > L, N < P$, goes to Left Child of P.
7.  Insert **T**: $T > E, T > L, T > P$, goes to Right Child of P.

<img width="1104" height="592" alt="image" src="https://github.com/user-attachments/assets/fd75f321-7925-4606-9e97-5f2021e01994" />
<img width="951" height="366" alt="image" src="https://github.com/user-attachments/assets/84c4ab94-262a-4db7-a240-8ac3c43cfcc3" />
<img width="1036" height="539" alt="image" src="https://github.com/user-attachments/assets/59b1bf0f-84bd-4c43-b50c-e1947f4f586f" />

### **4.4. Searching a Key**
*   **Algorithm:** Compare key with root. If equal, return node. If smaller, recurse left; if larger, recurse right.
*   **Complexity:** $O(h)$ where $h$ is the height.
*   **Worst Case:** $O(n)$. Occurs if keys are inserted in sorted order, creating a **skewed binary search tree** ($h = n-1$).
*   **Best Case:** $O(\log n)$. Occurs if the tree is **balanced**.

---

## **5. Comparison of Linear and Non-Linear Data Structures**

| Feature | Linear Data Structure | Non-Linear Data Structure |
| :--- | :--- | :--- |
| **Arrangement** | Linear/Sequential order; attached to previous/next. | Hierarchical or networked manner. |
| **Levels** | Single level involved. | Multiple levels involved. |
| **Implementation** | Easy. | Complex. |
| **Traversal** | Single way only. | Multiple ways (Depth-First, Breadth-First, Inorder, etc.). |
| **Examples** | Array, Stack, Queue, Linked List. | Trees, Graphs, Skip List, Hash Map. |


