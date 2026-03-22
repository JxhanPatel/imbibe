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
