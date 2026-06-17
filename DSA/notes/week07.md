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




---





# 7.2: Greedy Algorithms - Interval Scheduling

## 7.2.1: Defining Greedy Algorithms
A greedy algorithm needs to make a sequence of choices to achieve a global optimum. At each stage, it makes the next choice based on some local criterion. The algorithm never goes back and revises an earlier decision. This approach drastically reduces the space to search for solutions, making the algorithms very efficient. However, one must prove that these local choices actually achieve the global optimum.

### Examples of Greedy Algorithms
*   **Dijkstra's Algorithm**: Locally freezes the distance to the nearest unvisited vertex to achieve the global optimum of shortest distances from a source.
*   **Prim's Algorithm**: Locally adds the nearest non-tree vertex to the spanning tree to achieve a global minimum cost spanning tree.
*   **Kruskal's Algorithm**: Locally adds the smallest edge that does not form a cycle to achieve a global minimum cost spanning tree.

## 7.2.2: The Interval Scheduling Problem
IIT Madras has a special video classroom for delivering online lectures, and different teachers want to book the classroom. A slot for instructor $i$ starts at $s(i)$ and finishes at $f(i)$. Because slots may overlap, not all bookings can be honored. The goal is to choose a subset of bookings to maximize the number of teachers who get to use the room. 

> [!NOTE]
> The global optimum here is to choose a subset of compatible bookings of the largest size. Compatible means no two bookings in the set overlap in time.

## 7.2.3: Natural Greedy Strategies and Counterexamples
There are many "natural" greedy strategies for interval scheduling, but most of them are wrong.

### Strategy 1: Earliest Starting Time
Choose the booking whose starting time is earliest.
*   **Counterexample**: A single very long booking might start earliest but overlap with multiple shorter, non-overlapping bookings that could have been scheduled instead.

<img width="518" height="197" alt="image" src="https://github.com/user-attachments/assets/e9446d49-941c-4926-8ae0-10f2ff9d391a" />

### Strategy 2: Shortest Interval
Choose the booking spanning the shortest interval.
*   **Counterexample**: A very short interval might overlap with two non-overlapping long intervals. Choosing the shortest interval would allow only one booking, whereas choosing the two long ones would allow two.

<img width="549" height="184" alt="image" src="https://github.com/user-attachments/assets/e22f0baf-133e-4d68-9290-c0af5e421872" />

### Strategy 3: Minimum Overlaps
Choose the booking that overlaps with the minimum number of other bookings.
*   **Counterexample**: A specific configuration where a booking with the fewest overlaps rules out multiple other bookings that could have been part of a larger compatible set.

<img width="557" height="239" alt="image" src="https://github.com/user-attachments/assets/eff4fa98-838a-40e6-ba74-79e73e0920b7" />

## 7.2.4: The Correct Greedy Strategy
**Strategy 4**: Choose the booking whose finish time is the earliest.

### The Greedy Algorithm for Interval Scheduling
Let $B$ be the set of bookings and $A$ be the set of accepted bookings.
1.  Initially, $A$ is empty.
2.  While $B$ is not empty:
    *   Pick $b \in B$ with the earliest finishing time.
    *   Add $b$ to $A$.
    *   Remove from $B$ all bookings that overlap with $b$.

<img width="537" height="359" alt="image" src="https://github.com/user-attachments/assets/1a6eb878-b00f-40f9-9d3f-c007f9ab8599" />
<img width="554" height="356" alt="image" src="https://github.com/user-attachments/assets/2d87aae3-770a-43da-9203-96d92bb93a36" />


## 7.2.5: Proof of Correctness
To prove optimality, we compare the greedy solution $A$ to an arbitrary optimal set of accepted bookings $O$. $A$ and $O$ need not be identical, as multiple allocations of the same size may exist. We must show that $|A| = |O|$.

### Setup
Let $A = \{i_{1}, i_{2}, \dots, i_{k}\}$ and $O = \{j_{1}, j_{2}, \dots, j_{m}\}$. 
Assume both are sorted by finish time:
*   For $A$: $f(i_{1}) \le s(i_{2}), f(i_{2}) \le s(i_{3}), \dots$.
*   For $O$: $f(j_{1}) \le s(j_{2}), f(j_{2}) \le s(j_{3}), \dots$.
Our goal is to show $k = m$.

### Claim: Greedy Algorithm "Stays Ahead"
For each $l \le k$, the finish time of the $l$-th greedy job is no later than the finish time of the $l$-th optimal job: $f(i_{l}) \le f(j_{l})$.

**Proof by Induction on $l$**:
*   **Base Case ($l=1$)**: By the greedy strategy, $i_{1}$ has the earliest overall finish time in the global set, so it cannot finish later than $j_{1}$.
*   **Induction Step**: Assume $f(i_{l-1}) \le f(j_{l-1})$. 
    *   Since the optimal set is compatible, $f(j_{l-1}) \le s(j_{l})$.
    *   By the inductive hypothesis, $f(i_{l-1}) \le f(j_{l-1}) \le s(j_{l})$.
    *   This shows that $j_{l}$ is compatible with the greedy set chosen so far ($i_{1}, \dots, i_{l-1}$).
    *   If $f(j_{l}) < f(i_{l})$, the greedy strategy would have picked $j_{l}$ because it finishes earlier.
    *   Therefore, we must have $f(i_{l}) \le f(j_{l})$.

### Proving $|A| = |O|$ (Optimality)
Suppose $m > k$.
1.  From the "stays ahead" claim, we know $f(i_{k}) \le f(j_{k})$.
2.  Consider the request $j_{k+1}$ from the optimal set.
3.  Since $f(i_{k}) \le f(j_{k}) \le s(j_{k+1})$, this request $j_{k+1}$ is compatible with the greedy set $A$.
4.  Therefore, $j_{k+1}$ must still be in $B$ when the greedy algorithm stops.
5.  But the greedy strategy stops only when $B$ is empty.
6.  This is a contradiction; hence, $m$ cannot be greater than $k$.

## 7.2.6: Implementation and Complexity
1.  **Sort** $n$ bookings by finish time: $O(n \log n)$.
2.  **Renumber** bookings to reflect this sorted order.
3.  **Record** start times $S[i]$ and finish times $F[i]$ in an array or dictionary.
4.  **Selection**: Add the first booking to $A$. In general, after adding booking $j$, scan the renumbered list to find the smallest index $r$ such that $S[r] \ge F[j]$.

> [!TIP]
> This selection process is a single scan, taking $O(n)$ time overall. The overall complexity is dominated by the sort: **$O(n \log n)$**.




---






# 7.3: Greedy Algorithms - Minimizing Lateness

## 7.3.1: Defining Greedy Algorithms
A greedy algorithm makes a sequence of local choices to achieve a global optimum. It never goes back and revises an earlier decision. This drastically reduces the space to search for solutions, but it is necessary to prove that these local choices actually achieve the global optimum.

There are two common strategies to prove a greedy algorithm's correctness:
*   **Strategy 1**: Incrementally show that the greedy solution is at least as good as an optimal one. The greedy algorithm "stays ahead" of the optimal.
*   **Strategy 2**: Transform an arbitrary optimal solution to match the greedy one step-by-step, preserving optimality. This is often called an **exchange argument**.

## 7.3.2: The Minimizing Lateness Problem
IIT Madras has a single 3D printer that a number of users need to access.
*   Each user $i$ has an item that takes time $T(i)$ to print.
*   Each user $i$ has a deadline $D(i)$ by which they want their item ready.
*   Each user will get access to the printer, but they may not finish before their deadline.
*   User $i$ starts at time $S(i)$ and finishes at time $F(i) = S(i) + T(i)$.
*   If $F(i) > D(i)$, user $i$ is late by $L(i) = F(i) - D(i)$.
*   If the user finishes before the deadline, lateness $L(i)$ is considered 0.
*   **Maximum Lateness**: $\max_{i} L(i)$.

**Goal**: Minimize the maximum lateness.

## 7.3.3: Natural Greedy Strategies and Counterexamples
Several heuristics might seem plausible but can be shown to be incorrect using counterexamples.

### Strategy 1: Shortest Processing Time
Schedule requests in increasing order of length $T(i)$.
*   **Counterexample**: 
    *   Job 1: $T(1)=1, D(1)=100$
    *   Job 2: $T(2)=10, D(2)=10$
    *   If Job 1 goes first, it finishes at time 1. Job 2 then finishes at time 11. Job 2 is late by 1 ($11 - 10$).
    *   If Job 2 goes first, it finishes at time 10 (on time). Job 1 finishes at time 11 (on time). The maximum lateness is 0.

### Strategy 2: Smallest Slack Time
Give priority to requests with smaller slack time $D(i) - T(i)$.
*   **Counterexample**:
    *   Job 1: $T(1)=1, D(1)=2$ (Slack = 1)
    *   Job 2: $T(2)=10, D(2)=10$ (Slack = 0)
    *   Smaller slack first (Job 2): Job 2 finishes at 10 (on time). Job 1 finishes at 11. Job 1 is late by 9 ($11 - 2$).
    *   Doing Job 1 first: Job 1 finishes at 1 (on time). Job 2 finishes at 11. Job 2 is late by only 1 ($11 - 10$). Maximum lateness is reduced to 1.

## 7.3.4: The Correct Greedy Strategy
**Strategy 3**: Schedule requests in increasing order of deadlines $D(i)$.

### The Greedy Algorithm for Minimizing Lateness
1.  **Sort** the $n$ requests by deadline $D(i)$.
2.  **Renumber** the jobs such that $D(1) \le D(2) \le \dots \le D(n)$.
3.  **Schedule** the jobs continuously in this order:
    *   Job 1 starts at $S(1) = 0$, ends at $F(1) = T(1)$.
    *   Job 2 starts at $S(2) = F(1)$, ends at $F(2) = S(2) + T(2)$.
    *   In general, Job $i$ starts at $S(i) = F(i-1)$ and ends at $F(i) = S(i) + T(i)$.

## 7.3.5: Correctness - The Exchange Argument
To prove optimality, we compare the greedy schedule to an arbitrary optimal schedule $O$.

### Step 1: No Idle Time
Our greedy schedule has no gaps; the printer is in continuous use from $S(1)=0$ to $F(n)$. There is always an optimal schedule with no idle time, because any idle time can be eliminated by shifting jobs earlier, which can only reduce lateness.

### Step 2: Inversions
A schedule $O$ has an **inversion** if job $i$ appears before job $j$ but $D(j) < D(i)$.
*   The greedy schedule has no inversions by construction.
*   **Claim**: Any two schedules with no inversions and no idle time have the same maximum lateness. 
    *   If they have no inversions and no idle time, the only difference can be the order of requests with the same deadline. 
    *   Reordering jobs with the same deadline produces the same lateness.

<img width="533" height="175" alt="image" src="https://github.com/user-attachments/assets/a20a8549-10d5-4555-a3fc-dbff474066aa" />

### Step 3: Removing Inversions
**Claim**: There is an optimal schedule with no inversions and no idle time.
1.  Let $O$ be an optimal schedule with no idle time.
2.  If $O$ has an inversion, there must be at least one pair of adjacent jobs $i, j$ such that $j$ is scheduled immediately after $i$ and $D(j) < D(i)$.
3.  **Swap** $i$ and $j$ to get a schedule with one less inversion.
4.  **Optimality Preservation**: Swapping $i$ and $j$ does not increase the maximum lateness of $O$.
    *   Jobs before $i$ and after $j$ are unaffected.
    *   Job $j$ finishes earlier, so its lateness decreases.
    *   Job $i$ finishes later, but its new finish time is the old finish time of $j$. Since $D(j) < D(i)$, the lateness of $i$ in the new schedule is less than the lateness of $j$ in the original schedule.

<img width="512" height="272" alt="image" src="https://github.com/user-attachments/assets/aff50cf7-a8a4-4f54-9a0c-35976cde2332" />

### Conclusion of Proof
There are at most $n(n-1)/2$ inversions in $O$ to start with. We can repeatedly remove adjacent inversions without increasing lateness until no inversions remain. The resulting schedule is identical to the greedy schedule (up to reordering jobs with the same deadline) and remains optimal.

## 7.3.6: Implementation and Complexity
1.  **Sort** the requests by $D(i)$: $O(n \log n)$.
2.  **Read off** the schedule in sorted order: $O(n)$.
**Overall complexity**: **$O(n \log n)$**.

## 7.3.7: Summary
The simple greedy algorithm sorts requests by deadline and processes them in that order to minimize maximum lateness. Correctness is established through an exchange argument, transforming any optimal solution into the greedy solution without sacrificing optimality.



---




# 7.4: Greedy Algorithms - Huffman Coding

## 7.4.1: Efficient Communication
Digital communication uses $\{0, 1\}$. To send messages in a language like English, we must encode the alphabet $\{a, b, \dots, z\}$ using $\{0, 1\}$.

*   **Fixed length encoding**: Since there are 26 letters and $2^4 < 26 \le 2^5$, binary strings of length 5 are required.
*   **Variable length encoding**: Shorter strings are used for more frequent letters to optimize data transfer.

### Morse Code Example
Morse code encodes letters using dots (0) and dashes (1). For example:
*   $e$ is 0
*   $t$ is 1
*   $a$ is 01

**The Decoding Problem**: A sequence like `0101` is ambiguous. It could be decoded as `aa`, `etet`, `aet`, or `eta`. Morse code solves this by using pauses between letters, effectively adding a third symbol for encoding.

## 7.4.2: Prefix Codes
To avoid ambiguity without a third symbol, we use **prefix codes**.
*   **Definition**: The encoding of $x$, $E(x)$, is not a prefix of $E(y)$ for any $x, y$.
*   In Morse code, $E(e) = 0$ is a prefix of $E(a) = 01$, so it is not a prefix code.

| $X$ | $a$ | $b$ | $C$ | $d$ | $e$ |
| :--- | :--- | :--- | :--- | :--- | :--- |
| $E(x)$ | 11 | 01 | 001 | 10 | 000 |

**Unambiguous Decoding**: Using the prefix code above, the sequence `0010000011101` is decoded by identifying the first valid encoding boundary: `001` (C), `000` (e), `001` (C), `11` (a), `01` (b).

## 7.4.3: Optimal Prefix Codes
The goal is to produce the most efficient prefix code possible for a given set of letters $A$ and frequencies $f(x)$.

*   **Frequency $f(x)$**: The fraction of occurrences of $x$ over a large text corpus. The sum of frequencies $\sum f(x_i) = 1$.
*   **Average Bits per Letter (ABL)**: If a message has $n$ symbols, the total length is $\sum_{x \in A} n \cdot f(x) \cdot |E(x)|$. Dividing by $n$ gives the average number of bits per letter:
$$\sum_{x \in A} f(x) \cdot |E(x)|$$

### Comparison Example
Using frequencies: $a: 0.32, b: 0.25, C: 0.20, d: 0.18, e: 0.05$.
*   **Standard Prefix Code**: ABL = $(0.32 \cdot 2) + (0.25 \cdot 2) + (0.20 \cdot 3) + (0.18 \cdot 2) + (0.05 \cdot 3) = 2.25$.
*   **Fixed Length Encoding**: Requires 3 bits per letter. Variable length saves 25%.
*   **A Better Encoding**: $a: 11, b: 10, C: 01, d: 001, e: 000$. ABL = 2.23.

## 7.4.4: Codes as Trees
Encoding can be viewed as a binary tree where:
*   Letters are at the **leaves**.
*   The path to the leaf describes the encoding ($0$ is left, $1$ is right).
*   In a prefix code, no internal nodes encode letters.

<img width="454" height="330" alt="image" src="https://github.com/user-attachments/assets/72c253f2-5b27-45d5-a19e-18293e7f6fca" />

### Optimality Claims
*   **Claim 1**: Any optimal prefix code produces a **full tree** (each node has 0 or 2 children). If a node has one child, you can "promote" it to get a shorter tree.
*   **Claim 2**: In an optimal tree, if leaf $x$ is at a smaller depth (higher) than leaf $y$, then $f(x) \ge f(y)$. Otherwise, exchanging labels would improve the tree.
*   **Claim 3**: In an optimal tree, for any leaf at maximum depth, its sibling is also a leaf.

## 7.4.5: A Recursive Algorithm (Huffman's Algorithm)
Based on the claims, leaves at maximum depth occur in pairs and have the lowest frequencies.

1.  Pick $x, y$ with smallest frequencies $f(x), f(y)$.
2.  "Combine" $x, y$ into a new letter $xy$ with $f(xy) = f(x) + f(y)$.
3.  Update the alphabet: $A' = (A \setminus \{x, y\}) \cup \{xy\}$.
4.  Recursively find an optimal tree $T'$ for $A'$.
5.  $T'$ will have a leaf labeled $xy$. Replace this leaf with an internal node with two children labeled $x, y$.


<img width="1131" height="481" alt="image" src="https://github.com/user-attachments/assets/e09e93fa-ea39-4b87-b3c9-e239b02983b0" />
<img width="1152" height="525" alt="image" src="https://github.com/user-attachments/assets/6881398e-6b01-406a-b7b9-99c091f26975" />
<img width="1169" height="486" alt="image" src="https://github.com/user-attachments/assets/02249197-5f51-4203-ba9d-9e2f66f94d70" />



## 7.4.6: Optimality of Huffman's Algorithm
Proof by induction on the size of alphabet $A$.
*   **Base case**: $|A| = 2$. A single letter code $\{0, 1\}$ is optimal.
*   **Inductive Step**: For $|A| = k$, combine the lowest frequency $x, y$ into $xy$. $ABL(T')$ is optimal for $A'$ by induction.

**Claim**: $ABL(T) - ABL(T') = f(xy)$.
The only change to ABL from $T'$ to $T$ is due to replacing $xy$ with $x$ and $y$.
*   Subtract $depth(xy)f(xy)$.
*   Add $depth(x)f(x) + depth(y)f(y)$.
*   Since $f(xy) = f(x) + f(y)$ and $depth(x) = depth(y) = 1 + depth(xy)$:
*   Net increase = $(1 + depth(xy))(f(x) + f(y)) - depth(xy)(f(x) + f(y)) = f(x) + f(y) = f(xy)$.

This transformation preserves optimality. Any optimal tree $S$ for $A$ can be shuffled so $x, y$ are siblings and contracted to $S'$ over $A'$. Since $ABL(T') \le ABL(S')$, it follows that $ABL(T) \le ABL(S)$.

## 7.4.7: Implementation and Complexity
*   **Naive implementation**: Store frequencies in an array and use a linear scan to find minimum values. Number of recursive calls is $k-1$. Complexity: $O(k^2)$.
*   **Heap-based implementation**: Maintain frequencies in a **min-heap**. Extracting two minimums and adding the compound letter takes $O(\log k)$.
*   **Overall complexity**: **$O(k \log k)$**.

## 7.4.8: Historical Notes
*   **Greedy Nature**: Huffman coding is greedy because it recursively combines letters with lowest frequencies based on a locally optimal choice and never reconsiders other pairings.
*   **Claude Shannon**: Invented information theory and established mathematical lower bounds on optimal encodings but did not describe how to construct them.
*   **Shannon-Fano codes**: A recursive solution by Shannon and Robert Fano that was not always optimal.
*   **David Huffman**: Discovered his algorithm as a graduate student in Fano's class at MIT while working on a term paper.
