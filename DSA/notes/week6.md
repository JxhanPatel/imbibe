# 6.1: Union-find data structure

## 6.1.1: Kruskal's Algorithm for Minimum Cost Spanning Tree
Kruskal's algorithm for minimum cost spanning tree (MCST) processes edges in ascending order of cost. If edge $(u, v)$ does not create a cycle, add it. 

*   $(u, v)$ can be added if $u$ and $v$ are in different components.
*   Adding edge $(u, v)$ merges these components.
*   How can we keep track of components and merge them efficiently?.

Kruskal's algorithm built up a tree bottom up. We started with disconnected nodes, and then we connected them by adding the shortest edge possible. Initially, it is all disjoint because it is a bottom up algorithm; everything is a singleton. As we go along and edges are added, the endpoints of the edge $u$ and $v$ belonged earlier to different components. Having now connected these two components together, we must merge the components as we go along.


<img width="881" height="682" alt="image" src="https://github.com/user-attachments/assets/db28cb77-09dc-423f-abe5-6504c8ab9631" />


## 6.1.2: Disjoint Set Partitioning
Components partition vertices, forming a collection of disjoint sets. Every vertex belongs to one of the partitions, and no vertex belongs to two partitions. 

*   A set $S$ is partitioned into components $\{C_{1}, C_{2}, ..., C_{k}\}$.
*   $C_{1} \cup C_{2} \cup ... \cup C_{k}$ is actually all of $S$.
*   Each $s \in S$ belongs to exactly one $C_{j}$.

## 6.1.3: Union-Find Operations
The data structure must support the following operations:
*   **MakeUnionFind(S)**: Set up initial singleton components $\{s\}$, for each $s \in S$.
*   **Find(s)**: Return the component containing $s$.
*   **Union(s, s')**: Merges components containing $s, s'$.

## 6.1.4: Naive Implementation
Assume $S = \{0, 1, ..., n-1\}$. Set up an array/dictionary `Component`. 

> [!IMPORTANT]
> **Basic Naive Operations**:
> *   **MakeUnionFind(S)**: Set `Component[i] = i` for each `i`.
> *   **Find(i)**: Return `Component[i]`.
> *   **Union(i, j)**:
>   ```python
>   c_old = Component[i]
>   c_new = Component[j]
>   for k in range(n):
>       if Component[k] == c_old:
>           Component[k] = c_new
>   ```
>  

### Complexity of Naive Implementation
*   **MakeUnionFind(S)**: $O(n)$.
*   **Find(i)**: $O(1)$.
*   **Union(i, j)**: $O(n)$.
*   A sequence of $m$ `Union()` operations takes time $O(mn)$.

## 6.1.5: Improved Implementation
Use another array/dictionary `Members`. 
*   For each component $c$, `Members[c]` is a list of its members.
*   `Size[c] = length(Members[c])` is the number of members.


### Operations for Improved Implementation
*   **MakeUnionFind(S)**:
    *   Set `Component[i] = i` for all $i$.
    *   Set `Members[i] = [i]`, `Size[i] = 1` for all $i$.
*   **Find(i)**: Return `Component[i]`.
*   **Union(i, j)**:
  ```python
  c_old = Component[i]
  c_new = Component[j]
  for k in Members[c_old]:
      Component[k] = c_new
      Members[c_new].append(k)
      Size[c_new] = Size[c_new] + 1
  ```
 

## 6.1.6: Optimization - Weighted Union
`Members[c_old]` allows us to merge `Component(i)` into `Component[j]` in time $O(Size[c\_old])$ rather than $O(n)$.

**How to make use of `Size[c]`**: Always merge the smaller component into the larger one.
*   If `Size[c] < Size[c']` relabel $c$ as $c'$, else relabel $c'$ as $c$.

> [!WARNING]
> Individual merge operations can still take time $O(n)$ if both `Size[c]` and `Size[c']` are about $n/2$.

## 6.1.7: Amortized Complexity Analysis
For each $i$, the size of `Component[i]` at least doubles each time it is relabelled.
*   After $m$ `Union()` operations, at most $2m$ elements have been “touched”.
*   The size of `Component[i]` is at most $2m$.
*   Since the size of `Component[i]` grows as $1, 2, 4, ...$, $i$ changes component at most $\log m$ times.

**Over $m$ updates**:
*   At most $2m$ elements are relabelled.
*   Each one is relabelled at most $O(\log m)$ times.
*   Overall, $m$ `Union()` operations take time $O(m \log m)$.
*   The amortised complexity of `Union()` is $O(\log m)$.

## 6.1.8: Complexity in Kruskal's Algorithm
1.  **Sort** $E=\{e_{0}, e_{1}, ..., e_{m-1}\}$ in ascending order: $O(m \log m)$, which is equivalently $O(m \log n)$ since $m \le n^{2}$.
2.  **MakeUnionFind(V)** — each vertex $j$ is in component $j$: $O(n)$.
3.  **Process edges** $e_{k}=(u,v)$:
    *   Check `Find(u) != Find(v)`: $O(1)$.
    *   Merge components: `Union(Component[u], Component[v])`.
    *   The tree has $n-1$ edges, so there are $O(n)$ `Union()` operations.
    *   Total amortised cost for all unions is $O(n \log n)$.

**Overall Time Complexity**: $O((m+n) \log n)$.

## 6.1.9: Summary and Tree-Based Implementation
*   Implement Union-Find using arrays/dictionaries `Component`, `Member`, `Size`.
*   `MakeUnionFind(S)` is $O(n)$.
*   `Find(i)` is $O(1)$.
*   Across $m$ operations, amortised complexity of each `Union()` operation is $\log m$.

> [!TIP]
> **Tree-Based Optimization**:
> `Members[k]` can also be maintained as a tree rather than as a list.
> *   `Union()` becomes $O(1)$ by taking the root of one tree and putting it as a child of the root of the other tree.
> *   With clever updates like path compression, `Find()` has amortised complexity very close to $O(1)$.




---





# 6.2: Priority Queues

## 6.2.1: Dealing with Priorities - Job Scheduler
A job scheduler maintains a list of pending jobs with their priorities. When the processor is free, the scheduler picks out the job with maximum priority in the list and schedules it. New jobs may join the list at any time. How should the scheduler maintain the list of pending jobs and their priorities?.

> [!NOTE]
> **Queue vs. Priority Queue**: In a normal queue, it is first-in, first-out (FIFO). In a priority queue, you look at all the elements that are there and you take out the one with the highest priority. A good analogy is a crowded temple where people stand in line, but VIPs with higher priority can jump the queue.

## 6.2.2: Priority Queue Operations
A priority queue needs to maintain a collection of items with priorities to optimize the following operations:
*   **delete_max()**: Identify and remove the item with the highest priority. Note: The highest priority item need not be unique; if there are duplicates, you can pick any one.
*   **insert()**: Add a new item to the collection/list.

Dually, one could also take out the one with the lowest priority, in which case the operation is **delete_min()**.


## 6.2.3: Implementing Priority Queues with One-Dimensional Structures
If the collection is maintained as a simple sequence (array or list), we have two main naive options:

### 1. Unsorted List
*   **insert()**: Is $O(1)$ because you can just put it anywhere, such as at the end of the list.
*   **delete_max()**: Is $O(n)$ because you have to scan the whole list to identify the maximum.

### 2. Sorted List
*   **insert()**: Is $O(n)$ because you have to find the correct place to insert and shift elements to maintain the sorted property.
*   **delete_max()**: Is $O(1)$ because the maximum is always at one end of the list.

### Performance Summary for 1D Structures
Processing $n$ items (doing $n$ inserts and $n$ deletions) requires $O(n^{2})$ time because one of the two operations will always be linear.

## 6.2.4: Moving to Two Dimensions - The Square Grid
To improve performance, we can move from a one-dimensional list to a two-dimensional grid. 

**The $\sqrt{N} \times \sqrt{N}$ Array Strategy**:
*   Assume $N$ processes enter/leave the queue.
*   Maintain a $\sqrt{N} \times \sqrt{N}$ array.
*   Each row in the array is kept in sorted order from left to right.
*   Keep track of the size of each row separately in a size column.

<img width="1131" height="383" alt="image" src="https://github.com/user-attachments/assets/7d8117c5-100b-4fe3-ae09-055dbcd59666" />

<img width="1127" height="477" alt="image" src="https://github.com/user-attachments/assets/c608911d-a978-41b7-8c07-6644e01f2fc4" />


### 2D Insert Operation
To insert a new value (e.g., Insert 15):
1.  Scan the size column to locate the first row that has free space.
2.  Use the size of the row to determine where to insert.
3.  Insert the element into the correct position in that row to maintain sorted order.
4.  Update the size of that row in the size column.

**Complexity of 2D Insert**: $O(\sqrt{N})$
*   Scanning the size column takes $O(\sqrt{N})$.
*   Inserting into a row of size $\approx \sqrt{N}$ takes $O(\sqrt{N})$.

### 2D Delete_max Operation
To identify and remove the maximum:
1.  The maximum in each row is its last element.
2.  The position of each row's maximum is available through the size column.
3.  Identify the overall maximum among these last entries (at most $\sqrt{N}$ values to check).
4.  Logically delete it by reducing the size count for that row.

**Complexity of 2D Delete_max**: $O(\sqrt{N})$
*   Finding the maximum among last entries takes $O(\sqrt{N})$.
*   Deletion and updating size takes $O(1)$.

## 6.2.5: Complexity Comparison and Summary

| Implementation Structure | Insert Complexity | Delete_max Complexity | Processing $N$ Items |
| :--- | :--- | :--- | :--- |
| **Unsorted List** | $O(1)$ | $O(N)$ | $O(N^{2})$ |
| **Sorted List** | $O(N)$ | $O(1)$ | $O(N^{2})$ |
| **2D $\sqrt{N} \times \sqrt{N}$ Array** | $O(\sqrt{N})$ | $O(\sqrt{N})$ | $O(N\sqrt{N})$ |
| **Heap (Target)** | $O(\log N)$ | $O(\log N)$ | $O(N \log N)$ |

> [!TIP]
> **Moving Forward**: While the 2D grid improves complexity from $N^{2}$ to $N\sqrt{N}$, we can do much better by using a special binary tree called a **heap**, which reduces operation costs to $O(\log N)$. Unlike the fixed $N$ required for the grid, heaps are flexible and do not require fixing $N$ in advance.







---




# 6.3: Heaps

## 6.3.1: Binary Trees
Values are stored as nodes in a rooted tree.
*   Each node has up to two children: **Left child** and **right child**.
*   Order is important.
*   Other than the root, each node has a unique parent.
*   **Leaf node**: A node with no children.
*   **Size**: Total number of nodes in the tree.
*   **Height**: Number of levels (level 0 is the root).

<img width="1015" height="523" alt="image" src="https://github.com/user-attachments/assets/dc861500-5c1d-44e8-91db-035ee3778232" />


## 6.3.2: Heap Definition and Properties
A heap is a binary tree implementation of priority queues with two additional constraints:

1.  **Structural Constraint**: The binary tree is filled level by level, from top to bottom and left to right.
2.  **Value Constraint (max-heap)**: The value at each node is at least as big as the values of its children.

> [!IMPORTANT]
> **The Root Property**: Because of the max-heap property, the root always has the largest value in the tree. By induction, as you go down any level $j$, you can only go down in the value chain.

## 6.3.3: Non-examples of Heaps
A binary tree is not a heap if it violates either the structural or value properties:
*   **Structural violations**: No "holes" are allowed. You cannot leave a level incomplete or leave a gap in the middle of a level.
*   **Value violations**: If a node has a value smaller than one of its children (in a max-heap), the heap property is violated.

<img width="1142" height="523" alt="image" src="https://github.com/user-attachments/assets/7081bf57-56bd-4875-b13a-34163cf283db" /> <img width="641" height="526" alt="image" src="https://github.com/user-attachments/assets/53937fb3-71a4-4760-90f4-edff59c7b76e" />


## 6.3.4: The Insert Operation
To insert an arbitrary item into a heap while maintaining its properties:
1.  **Structural Growth**: Add a new node at the only position dictated by the heap structure (the leftmost available spot in the bottom-most level).
2.  **Restore Value Property**: If the new value is bigger than its parent, swap it with the parent. Continue this process along the path walking upwards until the value is correctly placed with respect to its children and parent.

> [!NOTE]
> When moving a value up, you do not need to check the other child of the parent. If $77 > 74$ and $74 > 27$, then $77$ is automatically bigger than $27$.


<img width="1151" height="506" alt="image" src="https://github.com/user-attachments/assets/5066c33d-fc75-4ed1-ad82-490c25cbcbe8" />
<img width="1142" height="465" alt="image" src="https://github.com/user-attachments/assets/23665e0b-ebea-4d63-94e1-792585038468" />
<img width="1101" height="431" alt="image" src="https://github.com/user-attachments/assets/9fd1f55a-0c23-4101-9bc9-dda6f22377b2" />



## 6.3.5: Complexity of Insert
*   In the worst case, the new element must walk up from the leaf to the root.
*   The number of levels in a tree with $N$ nodes is at most $1 + \log N$.
*   Number of nodes at level $j$ is $2^j$.
*   Filling $k$ levels results in $2^0 + 2^1 + \dots + 2^{k-1} = 2^k - 1$ nodes.
*   **Complexity**: `insert()` is $O(\log N)$.

## 6.3.6: The Delete_max Operation
1.  **Identify Max**: The maximum value is always at the root.
2.  **Shrink Tree**: The node to delete must be the rightmost node at the lowest level to maintain the structural property.
3.  **Move Homeless Value**: Move the value from the deleted leaf node to the empty root.
4.  **Restore Property Downwards**: Compare the new root with its two children. To restore the heap, swap the root with the **bigger** of the two children. Continue this single path down the tree until the heap property is restored.


<img width="1155" height="506" alt="image" src="https://github.com/user-attachments/assets/ae84131d-dfb4-4c9d-82d5-dcc0aef365e2" />
<img width="1116" height="458" alt="image" src="https://github.com/user-attachments/assets/6940c916-361c-439a-8dc8-ae4ca9c50189" />
<img width="1110" height="559" alt="image" src="https://github.com/user-attachments/assets/7a8b24c2-8bd2-4b9d-88a4-6b7ebb4b8f0c" />



## 6.3.7: Complexity of Delete_max
*   The operation follows a single path from the root down to the leaves.
*   The height of the tree is logarithmic in the size.
*   **Complexity**: `delete_max()` is $O(\log N)$.





## 6.3.8: Implementation (Array/List Representation)
A heap does not require a complex graph structure with pointers. Due to its rigid level-by-level growth, it can be stored as a list $H = [h_0, h_1, h_2, \dots]$.

**Index Arithmetic**:
*   Number the nodes top to bottom, left to right.
*   **Children of $H[i]$**: Located at $H[2i + 1]$ and $H[2i + 2]$.
*   **Parent of $H[i]$**: Located at $H[(i - 1) // 2]$ for $i > 0$.

<img width="1129" height="457" alt="image" src="https://github.com/user-attachments/assets/cf5be30e-f644-4c65-a595-6e4439dae1bc" />





## 6.3.9: Building a Heap (heapify)
Converting an unordered list $[v_0, v_1, \dots, v_N]$ into a heap.

### Strategy 1: Naive Strategy
*   Start with an empty heap and repeatedly apply `insert(v_j)` for each element.
*   **Complexity**: $O(N \log N)$.

### Strategy 2: Better heapify()
*   Treat the list as a tree structure.
*   **Leaves**: The slice $L[mid:]$ where $mid = len(L) // 2$ contains only leaf nodes, which already satisfy the heap condition.
*   **Process**: Work backwards from the right of the list to the left (bottom-up in the tree). Fix the heap property downwards for the second-last level, then the third-last, up to the root.
*   **Cost Analysis**:
    *   Number of nodes at each level halves as you move up, while the max work per node (swaps) increases by 1.
    *   Total cost $\approx \frac{n}{4} \times 1 + \frac{n}{8} \times 2 + \frac{n}{16} \times 3 + \dots$
*   **Complexity**: `heapify()` is $O(N)$.

## 6.3.10: Min-heaps
The heap condition can be inverted:
*   **Property**: Each node is smaller than its children.
*   **Root**: Contains the smallest value.
*   **Operation**: Supports `delete_min()` rather than `delete_max()`.
*   **Application**: Used in Dijkstra's and Prim's algorithms to pick the smallest distance or cost.






---



# 6.4: Using Heaps in Algorithms

## 6.4.1: Priority Queues and Heaps Recap
Priority queues support two basic operations: `insert()` and `delete_max()` (or `delete_min()`). In a priority queue, each element has a priority; when removing an element, you must remove the one with the highest priority rather than the one inserted earliest. This differs from a normal queue, which is first-in, first-out (FIFO).

Heaps are a tree-based implementation of priority queues where `insert()` and `delete_max()` are both $O(\log n)$. A heap can be represented as a list or array using simple index arithmetic to find the parent and children of a node:
*   **Children of $H[i]$**: Located at $H[2i + 1]$ and $H[2i + 2]$.
*   **Parent of $H[i]$**: Located at $H[(i - 1) // 2]$ for $i > 0$.

Additionally, `heapify()` can build a heap from a list or array in $O(n)$ time.

## 6.4.2: Dijkstra's Algorithm and the Bottleneck
Dijkstra's algorithm maintains two dictionaries with vertices as keys: `visited` (initially `False`) and `distance` (initially infinity, with `distance[s] = 0`). The algorithm repeats the following until all reachable vertices are visited:
1.  Find an unvisited vertex `nextv` with the minimum distance.
2.  Set `visited[nextv]` to `True`.
3.  Recompute `distance[v]` for every neighbour `v` of `nextv`.

<img width="620" height="547" alt="image" src="https://github.com/user-attachments/assets/71a0a1fe-ce95-43d2-8e42-db546129e4f4" />


### The Bottleneck
The primary bottleneck is finding the unvisited vertex $j$ with the minimum distance. A naive implementation requires an $O(n)$ scan across all vertices to check which ones are not visited and find the minimum distance. Over $n$ iterations, this results in an $O(n^2)$ worst-case complexity.

> [!IMPORTANT]
> **Optimization Strategy**: By maintaining unvisited vertices in a **min-heap**, identifying and removing the minimum distance vertex (`delete_min()`) takes only $O(\log n)$ time.

## 6.4.3: Updating Values in a Min-Heap
While a min-heap optimizes finding the minimum, Dijkstra's algorithm also requires updating the distances of neighbours. These unvisited neighbours' distances are already inside the min-heap. However, updating a value in the middle of a heap is not a basic heap operation.

### Mechanism for Updating Arbitrary Values
To update an arbitrary value in a min-heap, two types of violations must be handled:
1.  **Reducing a value**: If a distance is reduced (e.g., changing 54 to 35), it may create a violation with its parent if the new value is now smaller. To restore the heap property, **swap upwards** (bubble up), similar to the `insert()` operation.
2.  **Increasing a value**: If a value is increased (e.g., changing 29 to 71), it may create a violation with its children. To restore the heap property, **swap downwards** (bubble down), similar to the `delete_min()` operation.

Both update operations take $O(\log n)$ time.

<img width="1125" height="498" alt="image" src="https://github.com/user-attachments/assets/6bf32bd9-fef0-447e-89f1-b79c6d7add38" />
<img width="1085" height="488" alt="image" src="https://github.com/user-attachments/assets/20dc54b5-2d53-44b4-bd08-8e1187c8ae64" />
<img width="1114" height="498" alt="image" src="https://github.com/user-attachments/assets/e14440cb-6baa-4fdf-90c7-878f80774b29" />


### Locating the Node to Update
A challenge exists in locating a specific vertex within the heap structure, as elements move around based on heap operations. Finding a vertex by value would require an exhaustive $O(n)$ search. To avoid this, two additional dictionaries are maintained:
*   **VtoH**: Maps vertices $\{0, 1, \dots, n-1\}$ to their current **heap positions**.
*   **HtoV**: Maps current **heap positions** $\{0, 1, \dots, n-1\}$ back to the **vertices** stored there.

> [!NOTE]
> These dictionaries must be updated every time values are swapped within the heap during any heap operation to ensure the mapping remains consistent.

<img width="1118" height="547" alt="image" src="https://github.com/user-attachments/assets/60e462c0-c129-4998-b030-5cdbf6de0577" />

## 6.4.4: Complexity Analysis
With the extended heap (supporting updates), the complexity of graph algorithms improves:

### Dijkstra's Algorithm Complexity
*   **Identifying the next vertex**: $n$ iterations of `delete_min()` take $O(n \log n)$.
*   **Updating distances**: Each edge $(u, v)$ is processed once; updating a distance in the heap takes $O(\log n)$. For $m$ edges, this takes $O(m \log n)$.
*   **Overall Time**: $O((m + n) \log n)$.




### Prim's Algorithm Complexity
Prim's algorithm for Minimum Cost Spanning Tree has the same structure as Dijkstra's, differing only in the distance update calculation (using edge cost instead of cumulative distance). Using the min-heap optimization, Prim's algorithm also achieves $O((m + n) \log n)$.

## 6.4.5: Heap Sort
Heaps can also be used to sort a list of $n$ elements.
1.  **Build a Heap**: Use `heapify()` to convert an unordered list into a max-heap in $O(n)$ time.
2.  **Extract Elements**: Call `delete_max()` $n$ times to extract elements in descending order. Each `delete_max()` takes $O(\log n)$, totaling $O(n \log n)$.
3.  **In-place Storage**: After each `delete_max()`, the heap shrinks by one. The vacated position at the end of the current heap (index $n-1, n-2, \dots$) can be used to store the deleted maximum value.

This results in an **in-place $O(n \log n)$ sort**. This is an improvement over Merge Sort, which also takes $O(n \log n)$ but requires extra space for merging.







---








# 6.5: Search Trees

## 6.5.1: Context - Dynamic Sorted Data
Sorting is useful for efficient searching. However, problems arise if the data is changing dynamically, where items are periodically inserted and deleted. 

*   In a sorted list, both **insert** and **delete** take time $O(n)$. 
*   To handle dynamic data efficiently, we move to a tree structure, similar to how heaps are used for priority queues.

## 6.5.2: Binary Search Tree (BST) Definition
A binary search tree is a rooted binary tree where for each node with value $v$:
*   All values in the left subtree are $< v$.
*   All values in the right subtree are $> v$.
*   No duplicate values are allowed in the tree.

<img width="1086" height="388" alt="image" src="https://github.com/user-attachments/assets/d9b606d6-1de1-47b5-965b-46f699a80da7" />



## 6.5.3: Implementing a Binary Search Tree
Each node in a BST has a value and pointers to its children. 

<img width="1114" height="512" alt="image" src="https://github.com/user-attachments/assets/86cbfc02-0013-45aa-a6f0-a5271adae79d" />


> [!NOTE]
> **Frontier Strategy**: To make implementation easier, we add a frontier with empty nodes. An empty tree is represented as a single empty node, and leaf nodes point to empty nodes. This structure makes it easier to implement operations recursively.
>
> 

<img width="1128" height="555" alt="image" src="https://github.com/user-attachments/assets/ccb8202e-bfd8-4a24-b95d-679da956bf4f" />





## 6.5.4: The Class Tree
The implementation uses a class called `Tree` with three local fields: `value`, `left`, and `right`. 

*   **Value None**: Used for an empty value.
*   **Empty Tree**: An empty tree has all fields set to `None`.
*   **Leaf Node**: A leaf has a non-empty value and empty `left` and `right` children.

### Implementation Code
```python
class Tree:
    # Constructor:
    def __init__(self, initval=None):
        self.value = initval
        if self.value:
            self.left = Tree()
            self.right = Tree()
        else:
            self.left = None
            self.right = None
        return

    # Only empty node has value None
    def isempty(self):
        return (self.value == None)

    # Leaf nodes have both children empty
    def isleaf(self):
        return (self.value != None and 
                self.left.isempty() and 
                self.right.isempty())
```



## 6.5.5: Inorder Traversal
An inorder traversal lists the left subtree, then the current node, then the right subtree. 

*   **Sorted Output**: This traversal lists values in sorted order.
*   **Printing**: It can be used as the string representation to print the tree.

### Implementation Code
```python
class Tree:
    # Inorder traversal
    def inorder(self):
        if self.isempty():
            return([])
        else:
            return(self.left.inorder() + 
                   [self.value] + 
                   self.right.inorder())

    # Display Tree as a string
    def __str__(self):
        return(str(self.inorder()))
```


## 6.5.6: Searching for a Value
To find a value $v$ in the tree:
1.  Check the value at the current node.
2.  If $v$ is smaller than the current node, go left.
3.  If $v$ is larger than the current node, go right.

This is a natural generalization of binary search.

### Implementation Code
```python
class Tree:
    # Check if value v occurs in tree
    def find(self, v):
        if self.isempty():
            return (False)
        if self.value == v:
            return (True)
        if v < self.value:
            return(self.left.find(v))
        if v > self.value:
            return(self.right.find(v))
```


## 6.5.7: Minimum and Maximum
*   **Minimum**: The leftmost node in the tree.
*   **Maximum**: The rightmost node in the tree.

### Implementation Code
```python
class Tree:
    def minval(self):
        if self.left.isempty():
            return(self.value)
        else:
            return(self.left.minval())

    def maxval(self):
        if self.right.isempty():
            return(self.value)
        else:
            return(self.right.maxval())
```


## 6.5.8: Insert a Value
To insert a value $v$:
1.  Try to find $v$ in the tree.
2.  Insert at the position where the `find` operation fails.

### Implementation Code
```python
class Tree:
    def insert(self, v):
        if self.isempty():
            self.value = v
            self.left = Tree()
            self.right = Tree()
        
        if self.value == v:
            return

        if v < self.value:
            self.left.insert(v)
            return

        if v > self.value:
            self.right.insert(v)
            return
```


<img width="524" height="567" alt="image" src="https://github.com/user-attachments/assets/ea97702a-94d3-480f-85da-c32a80ae3955" />
<img width="537" height="437" alt="image" src="https://github.com/user-attachments/assets/05240c0c-dd5f-4b4f-9201-68b802ecfef5" />
<img width="534" height="559" alt="image" src="https://github.com/user-attachments/assets/69ba77fd-bf38-468e-a60b-3af186abb94e" />



## 6.5.9: Delete a Value
If $v$ is present in the tree, delete it based on the following cases:

1.  **Leaf Node**: No problem, simply remove it.
2.  **One Child**: Promote that subtree to the current position.
3.  **Two Children**: Replace $v$ with the maximum value of the left subtree (`self.left.maxval()`) and then delete that maximum value from the left subtree. 

> [!TIP]
> **Deletion Property**: The node `self.left.maxval()` is guaranteed to have no right child, simplifying its subsequent deletion.

### Implementation Code
```python
class Tree:
    def delete(self, v):
        if self.isempty():
            return
        if v < self.value:
            self.left.delete(v)
            return
        if v > self.value:
            self.right.delete(v)
            return
        if v == self.value:
            if self.isleaf():
                self.makeempty()
            elif self.left.isempty():
                self.copyright()
            elif self.right.isempty():
                self.copyleft()
            else:
                self.value = self.left.maxval()
                self.left.delete(self.left.maxval())
            return
```


### Helper Functions for Deletion
```python
    # Convert leaf node to empty node
    def makeempty(self):
        self.value = None
        self.left = None
        self.right = None
        return

    # Promote left child
    def copyleft(self):
        self.value = self.left.value
        self.right = self.left.right
        self.left = self.left.left
        return

    # Promote right child
    def copyright(self):
        self.value = self.right.value
        self.left = self.right.left
        self.right = self.right.right
        return
```


## 6.5.10: Complexity Analysis
*   **Operation Path**: `find()`, `insert()`, and `delete()` all walk down a single path in the tree.
*   **Worst-case Complexity**: The time taken is proportional to the **height of the tree**.

> [!WARNING]
> **Balance Concerns**: 
> *   An **unbalanced tree** with $n$ nodes may have a height of $O(n)$.
> *   **Balanced trees** have a height of $O(\log n)$.
> *   To ensure all operations remain $O(\log n)$, techniques must be used to keep the tree balanced.
