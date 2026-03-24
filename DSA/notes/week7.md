# 7.1: Balanced Search Trees

## 7.1.1: Context - The Need for Balance
In a standard binary search tree, the operations `find()`, `insert()`, and `delete()` all function by walking down a single path from the root. Consequently, the performance of these operations is proportional to the **height of the tree**.
*   **Unbalanced Tree**: A tree with $n$ nodes may become unbalanced (e.g., a linear chain), resulting in a height of $O(n)$.
*   **Balanced Tree**: A balanced tree maintains a height of $O(\log n)$, ensuring efficient searching, inserting, and deleting.
*   **Requirement**: The tree must be adjusted incrementally as it grows and shrinks to maintain this balance inductively.

<img width="1079" height="572" alt="image" src="https://github.com/user-attachments/assets/62964e77-c835-4e58-a539-2e5b1d4584d9" />

## 7.1.2: Defining Balance
Balance generally means that the left and right subtrees of every node should be "equal". Two quantitative measures are considered:

### Size Balance
*   This requires `self.left.size()` and `self.right.size()` to be equal.
*   This property is only achievable for **complete binary trees**, which have exactly $2^{k}-1$ nodes.
*   A relaxed version where sizes differ by at most 1 is plausible but very difficult to maintain incrementally.

### Height Balance (AVL Trees)
*   **Height Definition**: The number of nodes on the longest path from the root to a leaf. 
    *   Height is 0 for an empty tree and 1 for a single root node.
    *   General formula: $1 + \max(\text{height of left subtree, height of right subtree})$.
*   **Height Balance Property**: A tree is height-balanced if, at every node, the heights of the left and right subtrees differ by **at most 1**.
*   These trees are known as **AVL trees**, named after Adelson-Velskii and Landis.

## 7.1.3: Guaranteeing Logarithmic Height
To justify using height-balanced trees, it must be proved that their height remains $O(\log n)$. This is done by finding the **minimum size height-balanced tree** $S(h)$ for a given height $h$.

**Construction Strategy for $S(h)$**:
*   To achieve height $h$ with the fewest nodes, place the smallest height-balanced tree of height $h-1$ as the left subtree and the smallest of height $h-2$ as the right subtree.
*   **Recurrence**: $S(h) = 1 + S(h-1) + S(h-2)$, with $S(0) = 0$ and $S(1) = 1$.
*   **Comparison to Fibonacci**: The Fibonacci sequence follows $F(n) = F(n-1) + F(n-2)$. Since $S(h)$ adds 1 at each step, $S(n) \ge F(n)$.
*   Because the Fibonacci sequence grows exponentially, $S(h)$ also grows exponentially with $h$.
*   **Result**: For a tree of size $n$, the height $h$ is guaranteed to be **$O(\log n)$**.

<img width="1142" height="527" alt="image" src="https://github.com/user-attachments/assets/7c766cb0-2532-4c55-90cf-eead0a51c2d4" />

## 7.1.4: Correcting Imbalance - Rotations
The **Slope** of a node is defined as: `self.left.height() - self.right.height()`.
*   A balanced node has a slope in $\{-1, 0, 1\}$.
*   `insert()` and `delete()` operations can alter the slope to **+2** or **-2**.

Imbalances are corrected using **rotations**, which move nodes between subtrees to adjust height while strictly preserving the binary search tree property.

### Left Rotation

<img width="1077" height="534" alt="image" src="https://github.com/user-attachments/assets/492a6a68-c814-4b0b-81b9-17219e0bda0b" />


Used when the right subtree is too tall (slope is -2).
*   **Implementation**:
    ```python
    def leftrotate(self):
        v = self.value
        vr = self.right.value
        tl = self.left
        trl = self.right.left
        trr = self.right.right
        newleft = Tree(v)
        newleft.left = tl
        newleft.right = trl
        self.value = vr
        self.right = trr
        self.left = newleft
        return
    ```
   

### Right Rotation

<img width="1076" height="544" alt="image" src="https://github.com/user-attachments/assets/108d75a9-2c90-4bdd-8c8c-a2b4a8204805" />

Used when the left subtree is too tall (slope is +2).
*   **Implementation**:
    ```python
    def rightrotate(self):
        v = self.value
        vl = self.left.value
        tll = self.left.left
        tlr = self.left.right
        tr = self.right
        newright = Tree(v)
        newright.left = tlr
        newright.right = tr
        self.value = vl
        self.left = tll
        self.right = newright
        return
    ```
   

> [!NOTE]
> Rotations only involve a constant number of pointer and value reassignments at the top level of the tree, taking $O(1)$ time regardless of tree size.

## 7.1.5: Rebalancing Strategy
Rebalancing is applied bottom-up during recursive modifications. For a root with slope **+2** (left-heavy):
*   **Case 1**: Left child's slope is $0$ or $1$. A single **Right rotation** at the root restores balance.
<img width="1043" height="290" alt="image" src="https://github.com/user-attachments/assets/7da5b09f-5d37-4f6e-b962-dc8439bd8335" />
<img width="1054" height="282" alt="image" src="https://github.com/user-attachments/assets/19f9b7bd-f9c3-4c3b-93d4-48a51e287258" />


*   **Case 2**: Left child's slope is $-1$. 
    1.  Perform a **Left rotation** on the left child.
    2.  Perform a **Right rotation** on the root.
<img width="1111" height="343" alt="image" src="https://github.com/user-attachments/assets/6df306f1-794b-4246-985b-881c5032e216" />
<img width="1094" height="426" alt="image" src="https://github.com/user-attachments/assets/850db3e0-9b52-481b-a96d-1afb8e0ccc4c" />
<img width="1080" height="517" alt="image" src="https://github.com/user-attachments/assets/c5e4ab9b-15c2-4377-8de4-2d4331083036" />

*   Rebalancing a slope of **-2** is **symmetric** using Left rotations.



## 7.1.6: Updating Insert and Delete
Standard search tree operations must be updated to include a call to `rebalance()` after the recursive modification.

```python
def insert(self, v):
    if self.isempty():
        # (Standard leaf creation)
        ...
    if v < self.value:
        self.left.insert(v)
        self.left.rebalance() # Fixes balance bottom-up
        return
    if v > self.value:
        self.right.insert(v)
        self.right.rebalance()
        return
```


## 7.1.7: Efficient Height Tracking
Computing height recursively at each step is $O(n)$, which would make rebalancing $O(n^2)$ overall. To avoid this, a dedicated **`height` field** is maintained in each node.
*   After every modification or rebalance, the field is updated: `self.height = 1 + max(self.left.height, self.right.height)`.
*   This ensures height lookup is $O(1)$.

## 7.1.8: Summary
*   **Height balanced trees** (AVL trees) maintain a height of **$O(\log n)$** through local rotations.
*   The `find()`, `insert()`, and `delete()` operations all walk a single path and thus take **$O(\log n)$** time.
*   Over a sequence of $n$ operations, the total work is **$O(n \log n)$**.
