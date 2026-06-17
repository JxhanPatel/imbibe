# 9.1: Dynamic Programming

## Subtopic 9.1.1: Inductive Definitions
Recursive functions typically arise from inductive definitions.

**Factorial**:
*   $fact(0)=1$
*   $fact(n)=n\times fact(n-1)$

**Insertion sort**:
*   $isort([])=[]$
*   $isort([x_{0},x_{1},...,x_{n}])=insert(isort([x_{0},x_{1},...,x_{n-1}]),x_{n})$

## Subtopic 9.1.2: Recursive Programs
Inductive definitions are converted into code easily as recursive programs.

```python
def fact(n):
    if n <= 0:
        return(1)
    else:
        return(n * fact(n-1))

def isort(l):
    if l == []:
        return(1)
    else:
        return(insert(isort(l[:-1]), l[-1]))
```


## Subtopic 9.1.3: Optimal Substructure Property
The solution to the original problem can be derived by combining solutions to subproblems.

*   **Factorial**: $fact(n-1)$ is a subproblem of $fact(n)$. So are $fact(n-2), fact(n-3), ..., fact(0)$.
*   **Insertion sort**: $isort([x_{0},x_{1},...,x_{n-1}])$ is a subproblem of $isort([x_{0},x_{1},...,x_{n}])$. So is $isort([x_{i},...,x_{j}])$ for any $0 \le i < j \le n$.

## Subtopic 9.1.4: Context - Interval Scheduling
Recall the optimization task from greedy algorithms where IIT Madras has a special video classroom for online lectures.
*   Different teachers want to book the classroom.
*   Slot for instructor $i$ starts at $s(i)$ and finishes at $f(i)$.
*   Slots may overlap, so not all bookings can be honored.
*   **Goal**: Choose a subset of bookings to maximize the number of teachers who get to use the room.

### Subproblems in Interval Scheduling
Each subset of bookings is a subproblem. Given $N$ bookings, there are $2^{N}$ subproblems.
*   **Generic Greedy Strategy**:
    1.  Pick one request from those in contention.
    2.  Eliminate bookings in conflict with this choice.
    3.  Solve the resulting subproblem.
*   The greedy strategy looks at only a small fraction of subproblems, ruling out a large number of others with each choice. It needs a proof of optimality.

## Subtopic 9.1.5: Weighted Interval Scheduling
This is a variant where each request comes with a weight, for instance, the room rent for using the resource.
*   **Revised goal**: Maximize the total weight (revenue) of the bookings that are selected.
*   This is not the same as maximizing the number of bookings selected.

### Failure of Greedy Strategy
The greedy strategy for the unweighted case (selecting the request with the earliest finish time) is not a valid strategy anymore.

<img width="981" height="452" alt="image" src="https://github.com/user-attachments/assets/2326f84c-46f9-4dc7-aad4-c35b02c5f379" />

The earliest deadline is no longer valid because it may prefer two small weight items over one large weight item that overlaps them. We must look for an inductive solution that is "obviously" correct.

## Subtopic 9.1.6: Inductive Solution for Weighted Scheduling
1.  Order the bookings by starting time: $b_{1}, b_{2}, ..., b_{n}$.
2.  Begin with $b_{1}$.
3.  Either $b_{1}$ is part of the optimal solution, or it is not.
    *   **Include $b_{1}$**: Eliminate conflicting requests in $b_{2}, ..., b_{n}$ and solve the resulting subproblem.
    *   **Exclude $b_{1}$**: Solve subproblem $b_{2}, ..., b_{n}$.
4.  Evaluate both options and choose the maximum.

This inductive solution considers all options. For each $b_{j}$, the optimal solution either has $b_{j}$ or does not.
*   If $b_{2}$ is not in conflict with $b_{1}$, it will be considered in both subproblems.
*   If $b_{2}$ is in conflict with $b_{1}$, it will be considered in the subproblem where $b_{1}$ is excluded.

## Subtopic 9.1.7: The Challenge of Overlapping Subproblems
Inductive solutions can generate the same subproblem at different stages.

**Example Scenario**:
*   $b_{1}$ is in conflict with $b_{2}$.
*   Both $b_{1}$ and $b_{2}$ are compatible with $b_{3}, b_{4}, ..., b_{n}$.

<img width="1017" height="450" alt="image" src="https://github.com/user-attachments/assets/c1dc054d-60b5-4e71-b871-63e74b7088e1" />

*   Path 1: Choose $b_{1} \Rightarrow$ subproblem is $b_{3}, ..., b_{n}$.
*   Path 2: Exclude $b_{1} \Rightarrow$ subproblem is $b_{2}, ..., b_{n}$. Next stage for this path: Choose/exclude $b_{2}$. Both choices here result in subproblem $b_{3}, ..., b_{n}$ again.

A naive recursive implementation evaluates each instance of a subproblem from scratch, leading to wasteful recomputation.

**Solution**: Use **Memoization** and **Dynamic Programming** to avoid this.






---






# 9.2: Memoization

## 9.2.1: Inductive Definitions, Recursive Programs, Subproblems
Recursive functions typically arise from inductive definitions.

**Factorial**:
*   $fact(0)=1$
*   $fact(n)=n\times fact(n-1)$

**Insertion sort**:
*   $isort([])=[]$
*   $isort([x_{0},x_{1},...,x_{n}])=insert(isort([x_{0},x_{1},...,x_{n-1}]),x_{n})$

Inductive definitions are converted into code easily as recursive programs. The solution to the original problem can be derived by combining solutions to subproblems.
*   $fact(n-1)$ is a subproblem of $fact(n)$.
*   $isort([x_{0},x_{1},...,x_{n-1}])$ is a subproblem of $isort([x_{0},x_{1},...,x_{n}])$.

## 9.2.2: Evaluating Subproblems - Fibonacci Numbers
Consider the Fibonacci numbers:
*   $fib(0)=0$
*   $fib(1)=1$
*   $fib(n)=fib(n-1)+fib(n-2)$

**Recursive Implementation**:
```python
def fib(n):
    if n <= 1:
        value = n
    else:
        value = fib(n-1) + fib(n-2)
    return(value)
```


## 9.2.3: Wasteful Recomputation
Evaluating `fib(5)` generates a sequence of recursive calls:
1.  `fib(5)` requires `fib(4)` and `fib(3)`.
2.  `fib(4)` requires `fib(3)` and `fib(2)`.
3.  `fib(3)` requires `fib(2)` and `fib(1)`.
4.  `fib(2)` requires `fib(1)` and `fib(0)`.

<img width="978" height="462" alt="image" src="https://github.com/user-attachments/assets/22926142-f5af-4196-aebe-9119ef96d444" />

> [!CAUTION]
> **Performance Issue**: This recursive implementation leads to wasteful recomputation. The computation tree grows exponentially, making even relatively small inputs like `fib(50)` computationally infeasible.

## 9.2.4: Building a Memory Table (Memoization)
One way to avoid this wasteful recomputation is actually to remember. Build a table of values already computed, known as a **Memory table**.

**Memoization Process**:
*   Check if the value to be computed was already seen before.
*   Store each newly computed value in a table.
*   Look up the table before making a recursive call.

By doing this, the computation tree becomes **linear** instead of exponential.

<img width="988" height="456" alt="image" src="https://github.com/user-attachments/assets/c6651108-e820-4513-8083-f49c6e359850" />

## 9.2.5: Memoizing Recursive Implementations
To memoize the implementation, assume a dictionary called `fibtable` exists globally and is initially empty.

```python
def fib(n):
    if n in fibtable.keys():
        return(fibtable[n])
    if n <= 1:
        value = n
    else:
        value = fib(n-1) + fib(n-2)
    fibtable[n] = value
    return(value)
```


## 9.2.6: Generic Memoization Pattern
There is nothing specific to the Fibonacci function; any recursive function can be memoized using a dictionary where the keys are the argument combinations (e.g., triples).

```python
def f(x,y,z):
    if (x,y,z) in ftable.keys():
        return(ftable[(x,y,z)])
    
    # recursively compute value from subproblems
    
    ftable[(x,y,z)] = value
    return(value)
```


## 9.2.7: Dynamic Programming
Dynamic programming is a related technique that anticipates the structure of subproblems.
*   Derive the structure from the inductive definition.
*   Dependencies form a **DAG** (Directed Acyclic Graph).
*   Solve subproblems in **topological order**.
*   This iterative evaluation means you never need to make a recursive call.

<img width="918" height="435" alt="image" src="https://github.com/user-attachments/assets/96ace054-4b58-4a64-b282-aaf538859f39" />

## 9.2.8: Summary
*   **Memoization**: Store subproblem values in a table; look up the table before making a recursive call. It is a **top-down** strategy.
*   **Dynamic Programming**: Solve subproblems in topological order of dependency; iterative evaluation without recursion. It is a **bottom-up** strategy.






---









# 9.3: Grid Paths

## 9.3.1: Defining Grid Paths
A rectangular grid of one-way roads is considered, often called a Manhattan grid. In this grid, movement is restricted: one can only go **up** and **right**, and cannot go left or down. The objective is to determine how many paths exist from the starting point $(0,0)$ at the bottom-left corner to a target intersection $(m,n)$ at the top-right.

<img width="988" height="493" alt="image" src="https://github.com/user-attachments/assets/ca023ce0-b58b-472f-a3a6-368fc15d19e0" />

## 9.3.2: Combinatorial Solution (No Holes)
Every path from $(0,0)$ to $(m,n)$ consists of exactly $m+n$ segments.
*   For a target $(5,10)$, every path has exactly 15 segments.
*   Out of these 15 segments, exactly 5 must be right moves and 10 must be up moves.
*   The path is uniquely determined by fixing the positions of the 5 right moves among the 15 total positions.

**Mathematical Formula**:
$$\binom{15}{5} = \frac{15!}{10! \cdot 5!} = 3003$$.

This is symmetric to fixing the 10 up moves: $\binom{15}{5} = \binom{15}{10}$.

## 9.3.3: Blocked Intersections (Holes)
If an intersection is blocked (e.g., due to road work), paths passing through that point must be discarded.

**Example with one hole at (2,4)**:
To find the number of valid paths avoiding $(2,4)$, subtract paths passing through $(2,4)$ from the total.
1.  **Paths to the hole**: From $(0,0)$ to $(2,4)$, there are $\binom{2+4}{2} = 15$ paths.
2.  **Paths from the hole to target**: From $(2,4)$ to $(5,10)$, there are $\binom{3+6}{3} = 84$ paths.
3.  **Total paths via (2,4)**: $15 \times 84 = 1260$ paths.
4.  **Valid paths**: $3003 - 1260 = 1743$ paths.

**Example with multiple holes**:
If two intersections (e.g., $(2,4)$ and $(4,4)$) are blocked, some paths passing through both holes are counted twice. The counting requires the **Inclusion-Exclusion principle**, which becomes messy as the number of holes increases.

## 9.3.4: Inductive Formulation
To reach intersection $(i,j)$, there are exactly two neighboring corners from which the last step could have been taken:
1.  Moving **up** from $(i, j-1)$.
2.  Moving **right** from $(i-1, j)$.

Each path to these neighbors extends to a unique path to $(i,j)$.

**Recurrence for $P(i,j)$**:
$$P(i,j) = P(i-1,j) + P(i,j-1)$$.

**Base Cases and Boundary Conditions**:
*   $P(0,0) = 1$ (Only one way to stay at the start: doing nothing).
*   $P(i,0) = P(i-1,0)$ (Bottom row: can only come from the left).
*   $P(0,j) = P(0,j-1)$ (Left column: can only come from below).
*   **Hole property**: $P(i,j) = 0$ if there is a hole at $(i,j)$.

## 9.3.5: Dynamic Programming Approach
Naive recursion recomputes the same subproblems repeatedly. For instance, $P(5,10)$ needs $P(4,10)$ and $P(5,9)$, both of which require $P(4,9)$.

**DAG Structure**:
The dependencies form a Directed Acyclic Graph (DAG) where each $(i,j)$ depends on $(i-1,j)$ and $(i,j-1)$. $P(0,0)$ has no dependencies.

<img width="1001" height="498" alt="image" src="https://github.com/user-attachments/assets/6f24782b-3740-4fc4-8a66-c983cc12c03d" />

**Evaluation Orders**:
Any order respecting the topological sort of the DAG is valid:
*   **Row by row**: Fill the bottom row, then the next row from left to right.
*   **Column by column**: Fill the left column, then the next column from bottom to top.
*   **Diagonal by diagonal**: Fill all entries where $i+j$ is constant (e.g., $(0,3), (1,2), (2,1), (3,0)$).

## 9.3.6: Example with Holes
Using dynamic programming to calculate paths for a grid with holes at $(2,4)$ and $(4,4)$:
1.  Initialize $P(0,0) = 1$.
2.  Fill entries using the recurrence.
3.  At hole positions, override the sum and set the value to $0$.
4.  Final result for $(5,10)$ with these two holes is **1358** paths.

## 9.3.7: Memoization vs. Dynamic Programming
Consider a barrier of holes just inside the border.
*   **Memoization**: A top-down strategy that only explores reachable subproblems. It never explores the region behind the barrier, filling only $O(m+n)$ entries.
*   **Dynamic Programming**: A bottom-up strategy that blindly fills all $mn$ cells of the table, including the shaded region blocked by holes.
*   **Tradeoff**: While dynamic programming can be "wasteful" by filling irrelevant cells, iterative evaluation is generally better than recursion in practice.







---








# 9.4: Common Subwords and Subsequences

## 9.4.1: Longest Common Subword
Given two strings, the goal is to find the length of the longest common subword. A subword is a segment that occurs as a block in both strings.

**Examples**:
*   "secret", "secretary" $\rightarrow$ "secret", length 6
*   "bisect", "trisect" $\rightarrow$ "isect", length 5
*   "bisect", "secret" $\rightarrow$ "sec", length 3
*   "director", "secretary" $\rightarrow$ "ec", "re", length 2

**Formal Definition**:
Let $u=a_{0}a_{1}...a_{m-1}$ and $v=b_{0}b_{1}...b_{n-1}$. A common subword of length $k$ exists if for some positions $i$ and $j$:
$$a_{i}a_{i+1}...a_{i+k-1} = b_{j}b_{j+1}...b_{j+k-1}$$.
We want to find the largest such $k$.

## 9.4.2: Brute Force Approach
Try every pair of starting positions $i$ in $u$ and $j$ in $v$. Match $(a_{i}, b_{j}), (a_{i+1}, b_{j+1}), \dots$ as far as possible and keep track of the longest match.

**Complexity**:
Assuming $m > n$, there are $mn$ pairs of starting positions, and from each, the scan could be $O(n)$. Thus, the overall complexity is $O(mn^2)$.

## 9.4.3: Inductive Structure for LCW
Let $LCW(i, j)$ be the length of the longest common subword in the suffixes $a_{i}a_{i+1}...a_{m-1}$ and $b_{j}b_{j+1}...b_{n-1}$.

*   If $a_{i} \neq b_{j}$, then $LCW(i,j) = 0$, as there is a mismatch at the start.
*   If $a_{i} = b_{j}$, then $LCW(i,j) = 1 + LCW(i+1, j+1)$.

**Base Cases**:
*   $LCW(m, n) = 0$
*   $LCW(i, n) = 0$ for all $0 \le i \le m$
*   $LCW(m, j) = 0$ for all $0 \le j \le n$

## 9.4.4: Subproblem Dependency and Dynamic Programming
Subproblems are $LCW(i, j)$ for $0 \le i \le m, 0 \le j \le n$. $LCW(i, j)$ depends only on $LCW(i+1, j+1)$.

<img width="985" height="482" alt="image" src="https://github.com/user-attachments/assets/0ac21102-dcce-4bde-8f82-c6cebcf9e5b2" />

**Filling the Table**:
*   Start at the bottom-right and fill row by row or column by column.
*   **Reading off the solution**: Find the entry $(i, j)$ with the largest $LCW$ value. The actual subword can be read off diagonally.

## 9.4.5: LCW Implementation and Complexity
```python
def LCW(u,v):
    import numpy as np
    (m,n) = (len(u), len(v))
    lcw = np.zeros((m+1, n+1))
    maxlcw = 0
    for c in range(n-1, -1, -1):
        for r in range(m-1, -1, -1):
            if u[r] == v[c]:
                lcw[r,c] = 1 + lcw[r+1, c+1]
            else:
                lcw[r,c] = 0
            if lcw[r,c] > maxlcw:
                maxlcw = lcw[r,c]
    return(maxlcw)
```
.

**Complexity**:
The inductive solution is $O(mn)$ because we fill a table of size $O(mn)$ and each entry takes constant time to compute.

## 9.4.6: Longest Common Subsequence (LCS)
In a subsequence, one can drop some letters in between.

**Examples**:
*   "secret", "secretary" $\rightarrow$ 6
*   "bisect", "secret" $\rightarrow$ "sect", length 4
*   "director", "secretary" $\rightarrow$ "ectr", "retr", length 4

**Applications**:
*   **Analyzing genes**: DNA is a long string over {A, T, G, C}. Two species are similar if their DNA has long common subsequences.
*   **Unix `diff` command**: Compares text files by finding the longest matching subsequence of lines.

## 9.4.7: Inductive Structure for LCS
Let $LCS(i, j)$ be the length of the longest common subsequence in $a_{i}a_{i+1}...a_{m-1}$ and $b_{j}b_{j+1}...b_{n-1}$.

*   If $a_{i} = b_{j}$, we can assume $(a_{i}, b_{j})$ is part of the LCS: $LCS(i, j) = 1 + LCS(i+1, j+1)$.
*   If $a_{i} \neq b_{j}$, $a_{i}$ and $b_{j}$ cannot both be part of the LCS. We solve $LCS(i, j+1)$ and $LCS(i+1, j)$ and take the maximum.

**Base Cases**:
*   $LCS(i, n) = 0$ for all $0 \le i \le m$.
*   $LCS(m, j) = 0$ for all $0 \le j \le n$.

## 9.4.8: Subproblem Dependency for LCS
$LCS(i, j)$ depends on three values: $LCS(i+1, j+1)$, $LCS(i, j+1)$, and $LCS(i+1, j)$.

<img width="980" height="490" alt="image" src="https://github.com/user-attachments/assets/913a791b-fc2c-47af-b2f5-206177ea2216" />

**Reading off the solution**:
Trace back the path by which each entry was filled. Each diagonal step (where $a_i = b_j$) corresponds to an element of the LCS.

## 9.4.9: LCS Implementation and Complexity
```python
def LCS(u,v):
    import numpy as np
    (m,n) = (len(u), len(v))
    lcs = np.zeros((m+1, n+1))
    for c in range(n-1, -1, -1):
        for r in range(m-1, -1, -1):
            if u[r] == v[c]:
                lcs[r,c] = 1 + lcs[r+1, c+1]
            else:
                lcs[r,c] = max(lcs[r+1, c], lcs[r, c+1])
    return(lcs)
```
**Complexity**:
$O(mn)$ using dynamic programming or memoization. We fill a table of size $O(mn)$ where each entry takes constant time to compute.







---









# 9.5: Edit Distance

## 9.5.1: Document Similarity
The concept of similarity between documents can be illustrated by comparing two sentences:
1. "The students were able to appreciate the concept optimal substructure property and its use in designing algorithms"
2. "The lecture taught the students to appreciate how the concept of optimal substructures can be used in designing algorithms"

Clearly these are similar sentences and one could imagine that the second sentence was perhaps created by editing the first one. Our goal is to try and find out how much editing was done or what is the minimum amount of editing needed to make the first document look like the second document.

## 9.5.2: Edit Operations
To transform one document to another, three types of character-level operations are used:
*   **Insert a character**: Adding a character to the document.
*   **Delete a character**: Removing a character from the document.
*   **Substitute one character by another**: Replacing one character with another.

Every operation applies to one character at a time, one symbol at a time.

<img width="980" height="483" alt="image" src="https://github.com/user-attachments/assets/1a0d95e0-415f-41ff-956a-0d6f67050bc8" />

## 9.5.3: Defining Edit Distance
The **Edit distance** (also called **Levenshtein distance**, named after Vladimir Levenshtein, 1965) is the minimum number of edit operations needed to transform one document to the other. 

In the provided example, the transformation involves:
*   24 characters inserted.
*   18 characters deleted.
*   2 characters substituted.
*   **Total cost (Edit distance)**: at most 44.

> [!NOTE]
> We are assuming that each of these has the same cost whether you delete a character or you insert a character or you substitute a character, you are paying the same cost in terms of the editing cost.

### Applications of Edit Distance:
*   Suggestions for spelling correction: Looking for the nearest words in terms of edit operations to a misspelled word.
*   Genetic similarity of species: Measuring how many genes must be altered to go from one species to another.

## 9.5.4: Edit Distance and Longest Common Subsequence (LCS)
Longest common subsequence is equivalent to edit distance without substitution.
*   If $LCS(u, v) = k$, then the minimum number of deletes needed to make $u$ and $v$ equal is $(m-k) + (n-k)$.
*   Deleting a letter from $u$ is equivalent to inserting it in $v$.

**Example: bisect vs. secret**
*   LCS is "sect".
*   Transformation involves deleting "b", "i" in "bisect" and "r", "e" in "secret".
*   Equivalently, delete "b", "i" and then insert "r", "e" in "bisect".

## 9.5.5: Inductive Structure
Let $u = a_{0}a_{1}...a_{m-1}$ and $v = b_{0}b_{1}...b_{n-1}$. 
Let $ED(i, j)$ be the edit distance for the suffixes $a_{i}a_{i+1}...a_{m-1}$ and $b_{j}b_{j+1}...b_{n-1}$.

If $a_{i} = b_{j}$, there is nothing to be done for this position:
$$ED(i, j) = ED(i+1, j+1)$$

If $a_{i} \ne b_{j}$, we choose the best among three options:
1.  **Substitute** $a_{i}$ by $b_{j}$: $1 + ED(i+1, j+1)$
2.  **Delete** $a_{i}$: $1 + ED(i+1, j)$
3.  **Insert** $b_{j}$ before $a_{i}$: $1 + ED(i, j+1)$

**Recurrence Relation**:
$$ED(i, j) = 1 + \min[ED(i+1, j+1), ED(i+1, j), ED(i, j+1)]$$

### Base Cases:
*   **Both strings empty**: $ED(m, n) = 0$
*   **Second string empty**: $ED(i, n) = m - i$. This corresponds to deleting the entire remaining suffix $a_{i}a_{i+1}...a_{m-1}$ from $u$.
*   **First string empty**: $ED(m, j) = n - j$. This corresponds to inserting the suffix $b_{j}b_{j+1}...b_{n-1}$ into $u$.

## 9.5.6: Subproblem Dependency
Subproblems are $ED(i, j)$ for $0 \le i \le m$ and $0 \le j \le n$.
These can be represented in a table of $(m+1) \cdot (n+1)$ values.
$ED(i, j)$ depends on three neighbors: $ED(i+1, j+1)$, $ED(i, j+1)$, and $ED(i+1, j)$.

<img width="987" height="484" alt="image" src="https://github.com/user-attachments/assets/724ed438-1135-4606-ae4d-b8f04f7adc6b" />

## 9.5.7: Reading off the Solution
The final edit distance is the value at $ED(0, 0)$. To recover the actual sequence of operations, trace back the path by which each entry was filled:
*   **Vertical move (Down)**: Corresponds to a deletion.
*   **Horizontal move (Right)**: Corresponds to an insertion.
*   **Diagonal move**: Corresponds to a match (if $a_{i} = b_{j}$) or a substitution (if $a_{i} \ne b_{j}$).

**Example: bisect to secret**
1.  Delete "b"
2.  Delete "i"
3.  Insert "r"
4.  Insert "e"

## 9.5.8: Implementation
```python
def ED(u,v):
    import numpy as np
    (m,n) = (len(u),len(v))
    ed = np.zeros((m+1,n+1))

    # Initialize boundaries
    for i in range(m-1,-1,-1):
        ed[i,n] = m-i
    for j in range(n-1,-1,-1):
        ed[m,j] = n-j

    # Fill table inductively
    for j in range(n-1,-1,-1):
        for i in range(m-1,-1,-1):
            if u[i] == v[j]:
                ed[i,j] = ed[i+1,j+1]
            else:
                ed[i,j] = 1 + min(ed[i+1,j+1], 
                                  ed[i,j+1], 
                                  ed[i+1,j])
    return(ed)
```

## 9.5.9: Complexity Analysis
*   **Space Complexity**: The algorithm fills a table of size $O(mn)$.
*   **Time Complexity**: Each table entry takes constant time to compute by looking at its neighbors.
*   **Overall Complexity**: $O(mn)$ using dynamic programming or memoization.







---








# 9.6: Matrix Multiplication

## 9.6.1: Multiplying Matrices
To multiply matrices $A$ and $B$, the dimensions must be compatible. If $A$ is an $m \times n$ matrix and $B$ is an $n \times p$ matrix, the resulting matrix $AB$ is $m \times p$. The entry in the $i^{th}$ row and the $j^{th}$ column of $AB$ is defined as:
$$AB[i,j] = \sum_{k=0}^{n-1} A[i,k]B[k,j]$$.

### Complexity of Single Multiplication
*   Computing each entry in $AB$ requires running through the summation, which takes $O(n)$ time.
*   There are $m \times p$ entries to compute.
*   Overall, the cost of computing $AB$ is $O(mnp)$.

<img width="969" height="492" alt="image" src="https://github.com/user-attachments/assets/86808cc0-e572-4121-802f-69d045770258" />

## 9.6.2: Matrix Chain Associativity
Matrix multiplication is associative, meaning for matrices $A, B, C$:
$$ABC = (AB)C = A(BC)$$.
Bracketing the expression does not change the final answer, but it can drastically affect the complexity of the computation.

### Example of Cost Difference
Let $A: 1 \times 100$, $B: 100 \times 1$, and $C: 1 \times 100$.

1.  **Computing $A(BC)$**:
    *   $BC$ results in a $100 \times 100$ matrix. Steps: $100 \cdot 1 \cdot 100 = 10,000$.
    *   $A(BC)$ results in a $1 \times 100$ matrix. Steps: $1 \cdot 100 \cdot 100 = 10,000$.
    *   **Total Cost**: $20,000$ steps.

2.  **Computing $(AB)C$**:
    *   $AB$ results in a $1 \times 1$ matrix. Steps: $1 \cdot 100 \cdot 1 = 100$.
    *   $(AB)C$ results in a $1 \times 100$ matrix. Steps: $1 \cdot 1 \cdot 100 = 100$.
    *   **Total Cost**: $200$ steps.

> [!IMPORTANT]
> By choosing a better order, an enormous saving in time can be achieved (20,000 steps vs. 200 steps).

## 9.6.3: The Matrix Chain Problem
Given $n$ matrices $M_{0}: r_{0} \times c_{0}, M_{1}: r_{1} \times c_{1}, \dots, M_{n-1}: r_{n-1} \times c_{n-1}$.
*   **Compatibility**: Dimensions must match such that $r_{j} = c_{j-1}$ for $0 < j < n$.
*   **Goal**: Find an optimal order to compute the product $M_{0} \cdot M_{1} \dots M_{n-1}$ by bracketing the expression to minimize total cost.

## 9.6.4: Inductive Structure
Any final step in the computation combines two subproducts:
$$(M_{0} \cdot M_{1} \dots M_{k-1}) \cdot (M_{k} \cdot M_{k+1} \dots M_{n-1})$$ for some $0 < k < n$.

*   The first factor has dimensions $r_{0} \times c_{k-1}$.
*   The second factor has dimensions $r_{k} \times c_{n-1}$.
*   Note: $r_{k} = c_{k-1}$.

### Recurrence for Cost $C(j, k)$
Let $C(j, k)$ be the cost of multiplying the chain from $M_{j}$ to $M_{k}$.
*   **Generic Case**: $C(j, k) = \min_{j < l \le k} [C(j, l-1) + C(l, k) + r_{j} r_{l} c_{k}]$.
*   **Base Case**: $C(j, j) = 0$ for $0 \le j < n$, as a single matrix requires no multiplication.

To find the minimum cost for the entire chain, we try all possible values of $k$ and choose the minimum.

## 9.6.5: Subproblem Dependency
We must compute $C(i, j)$ for $0 \le i, j < n$.
*   **Constraint**: $j \ge i$ because multiplication must proceed from left to right; we only compute entries on and above the main diagonal.
*   **Dependencies**: $C(i, j)$ depends on all pairs $(C(i, k-1), C(k, j))$ for every $i < k \le j$.
*   Unlike LCS or Edit Distance, each entry has $O(n)$ dependencies, requiring information from its left and below.

<img width="985" height="485" alt="image" src="https://github.com/user-attachments/assets/a38cb442-980c-4282-a376-f1bced3a8666" />
<img width="984" height="483" alt="image" src="https://github.com/user-attachments/assets/00902d94-95d6-4861-9431-76b0e84378b0" />

### Evaluation Order
Because of these dependencies, the matrix cannot be filled row-by-row or column-by-column. It must be filled **diagonal by diagonal**, starting from the main diagonal and moving towards the top-right corner $C(0, n-1)$.

## 9.6.6: Implementation
The implementation assumes an input matrix `dim` containing pairs $(r_i, c_i)$ for each matrix.

```python
def C(dim):
    import numpy as np
    n = dim.shape
    C = np.zeros((n,n))
    
    # Base case: main diagonal
    for i in range(n):
        C[i,i] = 0
    
    # Fill diagonal by diagonal
    for diff in range(1, n):
        for i in range(0, n - diff):
            j = i + diff
            # Initialize with the first possible split (k = i+1)
            C[i,j] = C[i,i] + C[i+1,j] + dim[i]*dim[i+1]*dim[j]
            
            # Find minimum over all other possible splits
            for k in range(i+2, j+1):
                C[i,j] = min(C[i,j], C[i,k-1] + C[k,j] + dim[i]*dim[k]*dim[j])
                
    return(C[0,n-1])
```
.

## 9.6.7: Complexity Analysis
*   **Space**: We fill a table of size $O(n^2)$.
*   **Time per entry**: Filling each entry requires a `for` loop to check $O(n)$ possible split points.
*   **Overall Complexity**: $O(n^3)$.

> [!NOTE]
> This problem is distinct from LCW/LCS/ED because the number of subproblems considered for each entry is not constant but proportional to $n$.
