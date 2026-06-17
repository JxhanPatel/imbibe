# 8.1: Divide and Conquer - Counting Inversions

## 8.1.1: Divide and Conquer Strategy
Divide and Conquer involves breaking up a problem into disjoint subproblems that do not refer to each other. These subproblem solutions are solved separately and then combined efficiently. 

**Examples of Divide and Conquer:**
*   **Merge sort**: Split the list into left and right halves and sort each half separately, then merge the sorted halves.
*   **Quicksort**: Rearrange the list into lower and upper partitions, sort each partition separately, and place the pivot between the sorted lower and upper partitions.

## 8.1.2: Context - Recommender Systems
Online services recommend items by comparing a user's profile with other customers to identify people who share similar likes and dislikes. This involves comparing rankings, such as movie rankings, to measure similarity.

**Comparing Rankings Example**:
You and a friend rank 5 movies {A, B, C, D, E}.
*   Your ranking: D, B, C, A, E
*   Your friend's ranking: B, A, C, D, E

To measure similarity, compare preferences for each pair of movies. For instance, if you both rank B above C, you agree. If you rank D above B but your friend ranks B above D, there is a disagreement.

## 8.1.3: Compare based on Inversions
Dissimilarity can be measured based on **inversions**, which are pairs of items ranked in opposite order.
*   **No inversions**: The rankings are identical.
*   **Every pair inverted**: The rankings are maximally dissimilar.
*   **Range**: The number of inversions ranges from 0 to $n(n-1)/2$.

## 8.1.4: Permutations and Counting
To simplify, fix the order of one ranking as a sorted sequence $1, 2, \dots, n$. The other ranking becomes a permutation of $1, 2, \dots, n$.
*   **Inversion Definition**: An inversion is a pair $(i, j)$ where $i < j$ but $j$ appears before $i$ in the permutation.

**Example**:
Your ranking (Reference): $D=1, B=2, C=3, A=4, E=5$.
Your friend's ranking: B, A, C, D, E $\rightarrow$ 2, 4, 3, 1, 5.
Inversions in 2, 4, 3, 1, 5: (1, 2), (1, 3), (1, 4), (3, 4). There are 4 inversions.

## 8.1.5: Graphical Representation and Brute Force
Inversions can be visualized by writing two permutations as two rows of nodes and connecting every pair $(j, j)$ between the rows. Every crossing of these lines represents an inversion.

<img width="975" height="448" alt="image" src="https://github.com/user-attachments/assets/69014ed8-4e7d-439a-acc6-b2716dcd41a9" />

> [!WARNING]
> **Brute Force Complexity**: Checking every pair $(i, j)$ to determine if it is an inversion takes $O(n^{2})$ time.

## 8.1.6: Divide and Conquer for Counting Inversions
Let the friend's permutation be $i_{1}, i_{2}, \dots, i_{n}$.
1.  **Divide**: Split into two lists $L = [i_{1}, \dots, i_{n/2}]$ and $R = [i_{n/2+1}, \dots, i_{n}]$.
2.  **Recursion**: Recursively count inversions within $L$ and within $R$.
3.  **Combine**: Add inversions across the boundary between $L$ and $R$. An across-boundary inversion occurs when $i \in L, j \in R$, and $i > j$.

## 8.1.7: The "Merge and Count" Approach
To count boundary inversions efficiently, adapt the **Merge Sort** algorithm. While sorting $L$ and $R$ recursively, count inversions during the merge step.

**The Counting Rule during Merge**:
*   Standard merge compares the tops of sorted lists $L$ and $R$ and pulls the smaller element.
*   If an element $i_{m}$ is pulled from the right list $R$ while elements are still remaining in the left list $L$, $i_{m}$ has "overtaken" them.
*   This $i_{m}$ is smaller than all elements currently in $L$, meaning it is inverted with respect to all of them.
*   **Action**: Add the current size of $L$ to the inversion count.

## 8.1.8: Algorithm Implementation
The function `sortAndCount` is essentially Merge Sort using a specialized `mergeAndCount` procedure.

```python
def mergeAndCount(A, B):
    (m, n) = (len(A), len(B))
    (C, i, j, k, count) = ([], 0, 0, 0, 0)
    while k < m + n:
        if i == m: # Left list exhausted
            C.append(B[j])
            (j, k) = (j + 1, k + 1)
        elif j == n: # Right list exhausted
            C.append(A[i])
            (i, k) = (i + 1, k + 1)
        elif A[i] < B[j]: # No inversion detected
            C.append(A[i])
            (i, k) = (i + 1, k + 1)
        else: # Element from R is smaller: Inversion!
            C.append(B[j])
            # Remaining elements in L (A) are all inverted with B[j]
            (j, k, count) = (j + 1, k + 1, count + (m - i))
    return(C, count)

def sortAndCount(A):
    n = len(A)
    if n <= 1:
        return(A, 0)
    (L, countL) = sortAndCount(A[:n//2])
    (R, countR) = sortAndCount(A[n//2:])
    (B, countB) = mergeAndCount(L, R)
    return(B, countL + countR + countB)
```


## 8.1.9: Complexity Analysis
*   **Recurrence**: Similar to Merge Sort: $T(n) = 2T(n/2) + n$, with $T(0) = T(1) = 1$.
*   **Total Time**: $O(n \log n)$.

> [!IMPORTANT]
> **Efficient Counting**: Even though the total number of inversions can be $O(n^{2})$, this algorithm counts them in $O(n \log n)$ by processing "chunks" of inversions at once during the merge, rather than enumerating each pair individually.






---




# 8.2: Closest Pair of Points

## 8.2.1: Context - Video Game Objects
In a video game with several objects on the screen, a basic step is to find the closest pair of objects. With $n$ objects, a naive brute force algorithm takes $O(n^{2})$ time by computing the distance for every pair and reporting the minimum. There is a clever divide and conquer algorithm that reduces this time to $O(n \log n)$.

## 8.2.2: Problem Statement
Points $p$ in 2D are represented as $(x, y)$. The Euclidean distance between $p_{1}=(x_{1}, y_{1})$ and $p_{2}=(x_{2}, y_{2})$ is defined as:
$$d(p_{1}, p_{2}) = \sqrt{(y_{2}-y_{1})^{2} + (x_{2}-x_{1})^{2}}$$.
Given $n$ points $\{p_{1}, p_{2}, \dots, p_{n}\}$, the goal is to find the closest pair.

> [!NOTE]
> **Assumption**: Assume no two points have the same $x$ or $y$ coordinate. Points can always be rotated slightly to ensure this without changing their relative distances.

<img width="530" height="424" alt="image" src="https://github.com/user-attachments/assets/9d1297cd-be09-4d33-87bf-58a6b32984b8" />

## 8.2.3: Closest Pair in 1 Dimension
In 1D, points are just $x$-coordinates, and distance is $d(p_{i}, p_{j}) = |x_{j} - x_{i}|$.
1.  **Sort** the points: $O(n \log n)$.
2.  **Scan**: In sorted order, the nearest points to $p$ are its immediate neighbors.
3.  Perform an $O(n)$ scan to find the minimum separation between adjacent points.
**Total Time**: $O(n \log n)$.

## 8.2.4: Divide and Conquer in 2 Dimensions
The strategy involves splitting the points into two halves using a vertical line.
1.  **Divide**: Split points into equal size sets $Q$ (left) and $R$ (right).
2.  **Recursion**: Compute the closest pair in each half recursively.
3.  **Combine**: Compare the shortest distance found in each half to the shortest distance across the dividing line.

<img width="1006" height="473" alt="image" src="https://github.com/user-attachments/assets/50438ea6-24db-4597-a77b-2034f17e1c04" />

## 8.2.5: Efficiently Dividing and Sorting
To avoid repeated sorting, the algorithm maintains two lists: $P_{x}$ (points sorted by $x$) and $P_{y}$ (points sorted by $y$).
*   **Splitting $P_{x}$**: $Q_{x}$ is the first half of $P_{x}$, and $R_{x}$ is the second half.
*   **Splitting $P_{y}$**: Let $x_{R}$ be the smallest $x$-coordinate in $R$. Scan $P_{y}$ once; if a point's $x$-coordinate is $< x_{R}$, move it to $Q_{y}$, otherwise to $R_{y}$.
*   This ensures $Q_{y}$ and $R_{y}$ are constructed in sorted $y$-order in $O(n)$ time.

## 8.2.6: Combining Solutions - The Band Strategy
Let $d_{Q}$ and $d_{R}$ be the closest distances in $Q$ and $R$ respectively.
1.  Set $\delta = \min(d_{Q}, d_{R})$.
2.  Only consider points within a vertical band of distance $\delta$ on either side of the separator.
3.  No pair of points outside this $2\delta$-wide band can be closer than $\delta$.

<img width="981" height="468" alt="image" src="https://github.com/user-attachments/assets/e594eec9-b5c5-4963-8887-dd003f6a9a27" />

## 8.2.7: The Box Argument for Linear Scan
Divide the $\delta$-band into boxes with sides of $\delta/2$.
*   **Property**: Each box can contain at most one point because the diagonal is $\delta/\sqrt{2} < \delta$, and any two points in the same box (on the same side of the line) would have to be at least $\delta$ apart.
*   **Neighborhood**: Any point within distance $\delta$ of a point $p$ must lie within a $4 \times 4$ neighborhood of boxes.
*   **Linear Scan**: Extract $S_{y}$ (points in the band sorted by $y$) from $Q_{y}$ and $R_{y}$. Scan $S_{y}$ from bottom to top, comparing each point $p$ with only the next **15 points** in the list.

<img width="962" height="488" alt="image" src="https://github.com/user-attachments/assets/f7cdc0e2-f303-4429-b149-767d3110b7f3" />

## 8.2.8: Algorithm Pseudocode
```python
def ClosestPair(Px, Py):
    if len(Px) <= 3:
        compute pairwise distances
        return closest pair and distance
    
    # Divide
    Construct (Qx, Qy), (Rx, Ry)
    
    # Conquer
    (q1, q2, dQ) = ClosestPair(Qx, Qy)
    (r1, r2, dR) = ClosestPair(Rx, Ry)
    
    # Combine
    delta = min(dQ, dR)
    Construct Sy from Qy, Ry
    Scan Sy, find cross-boundary closest pair (s1, s2, dS)
    
    return the pair with the overall minimum distance (dQ, dR, or dS)
```

## 8.2.9: Complexity Analysis
*   **Initial Sort**: $O(n \log n)$ to get $P_{x}$ and $P_{y}$.
*   **Recursive Work**:
    *   Constructing $(Q_{x}, Q_{y}), (R_{x}, R_{y})$: $O(n)$.
    *   Constructing $S_{y}$: $O(n)$.
    *   Scanning $S_{y}$: $O(n)$.
*   **Recurrence**: $T(n) = 2T(n/2) + O(n)$, which is identical to Merge Sort.
*   **Overall Time**: $O(n \log n)$.









---









# 8.3: Integer Multiplication

## 8.3.1: Standard Multiplication (Naive Approach)
The problem is how to multiply two integers $x$ and $y$. The method learned in school involves forming partial products by multiplying each digit of $y$ separately by $x$. 

**Process**:
1. Form partial products by taking each digit of $y$ and multiplying it by all of $x$.
2. Add up all the partial products.
3. This works the same in any base, such as binary.

**Complexity Analysis**:
To multiply two $n$-bit numbers:
*   There are $n$ partial products.
*   Adding each partial product to the cumulative sum is $O(n)$.
*   **Overall time**: $O(n^{2})$.

> [!NOTE]
> It appears that every row (partial product) is necessary, especially if every bit in the multiplier is 1, suggesting $n^{2}$ operations are unavoidable in this approach.

<img width="335" height="292" alt="image" src="https://github.com/user-attachments/assets/043f0095-fd35-4ee6-88df-39c364b44b93" />

## 8.3.2: Naive Divide and Conquer
The logical way to apply divide and conquer is to split the $n$ bits of each number into two groups of $n/2$.

*   Let $x = x_{1} \cdot 2^{n/2} + x_{0}$, where $x_{1}$ is the upper half and $x_{0}$ is the lower half of the bits.
*   Similarly, $y = y_{1} \cdot 2^{n/2} + y_{0}$.

**Rewriting the product $xy$**:
$xy = (x_{1} \cdot 2^{n/2} + x_{0})(y_{1} \cdot 2^{n/2} + y_{0})$
**Regrouping the terms**:
$xy = x_{1}y_{1} \cdot 2^{n} + (x_{1}y_{0} + x_{0}y_{1}) \cdot 2^{n/2} + x_{0}y_{0}$

This decomposition requires **four** $n/2$-bit multiplications:
1. $x_{1}y_{1}$
2. $x_{1}y_{0}$
3. $x_{0}y_{1}$
4. $x_{0}y_{0}$

**Recurrence and Result**:
*   $T(1) = 1$
*   $T(n) = 4T(n/2) + n$
*   Combining the products requires adding $O(n)$ bit numbers.
*   Solving this recurrence results in $T(n) = O(n^{2})$.
*   **Conclusion**: This naive divide and conquer approach does not help as it yields the same complexity as the standard method.

## 8.3.3: Karatsuba's Algorithm
Anatolii Karatsuba discovered that four multiplications can be replaced by three by cleverly rearranging terms.

**The Observation**:
Consider the product: $(x_{1} - x_{0})(y_{1} - y_{0}) = x_{1}y_{1} - (x_{1}y_{0} + x_{0}y_{1}) + x_{0}y_{0}$.
The middle term we need, $(x_{1}y_{0} + x_{0}y_{1})$, can be extracted using three products:
1. $p = x_{1}y_{1}$
2. $q = x_{0}y_{0}$
3. $r = (x_{1} - x_{0})(y_{1} - y_{0})$

**Formula**:
$(x_{1}y_{0} + x_{0}y_{1}) = p + q - r$.

### The Algorithm Pseudocode
```python
Fast-Multiply(x, y, n):
    if n == 1:
        return x * y
    else:
        m = n // 2
        # Extract halves using bit shifting (division and remainder)
        (x1, x0) = (x // 2**m, x % 2**m)
        (y1, y0) = (y // 2**m, y % 2**m)
        
        (a, b) = (x1 - x0, y1 - y0)
        
        p = Fast-Multiply(x1, y1, m)
        q = Fast-Multiply(x0, y0, m)
        r = Fast-Multiply(a, b, m)
        
        return p * 2**n + (p + q - r) * 2**(n//2) + q
```


## 8.3.4: Complexity Analysis of Karatsuba
The recurrence for Karatsuba's algorithm is:
$T(1) = 1$
$T(n) = 3T(n/2) + n$

**Expansion**:
$T(n) = 3T(n/2) + n$
$T(n) = 3^{2}T(n/2^{2}) + (3/2 + 1)n$
$T(n) = 3^{3}T(n/2^{3}) + ((3/2)^{2} + (3/2)^{1} + 1)n$

After $\log n$ steps:
$T(n) = 3^{\log n}T(1) + \text{Geometric Series Contribution}$

**Key Term**:
$3^{\log n} = n^{\log 3}$
Since $\log_2 3 \approx 1.59$:
**Overall Complexity**: $O(n^{1.59})$

> [!IMPORTANT]
> Karatsuba's algorithm reduces the complexity of integer multiplication from $O(n^{2})$ to a subquadratic $O(n^{1.59})$.

## 8.3.5: Historical Notes
*   **Andrei Kolmogorov**: In the 1950s, he publicly conjectured that multiplication could not be done in subquadratic time ($O(n^{2})$).
*   **Seminar (1960)**: Kolmogorov mentioned this conjecture at Moscow University.
*   **The Breakthrough**: Anatolii Karatsuba, a 23-year-old student, returned two weeks later with this divide and conquer algorithm.
*   **Refinements**: Karatsuba's original proposal used $(x_{1} + x_{0})(y_{1} + y_{0})$, which led to $n+1$ bits and a messier analysis. Donald Knuth introduced the simplification using $(x_{1} - x_{0})(y_{1} - y_{0})$ to make the analysis go more smoothly.





---




# 8.4: Divide and Conquer - Recursion Trees

## 8.4.1: Solving Recurrences
Divide and conquer involves breaking up a problem into disjoint subproblems and combining the solutions efficiently. In this strategy, subproblems do not refer to each other and are solved separately. The complexity $T(n)$ of such an algorithm is expressed as a recurrence because it depends on the complexity of smaller subproblems plus the extra cost of splitting and combining. 

Simple recurrences can be solved by repeated substitution:
*   **Binary Search**: $T(n) = T(n/2) + 1$, resulting in $O(\log n)$.
*   **Merge Sort**: $T(n) = 2T(n/2) + n$, resulting in $O(n \log n)$.

More complicated recurrences arise in integer multiplication:
*   **Naive Divide and Conquer**: $T(n) = 4T(n/2) + n$, resulting in $O(n^{2})$.
*   **Karatsuba's Algorithm**: $T(n) = 3T(n/2) + n$, resulting in $O(n^{\log 3})$.

There is a uniform way to compute the asymptotic expression for $T(n)$ using recursion trees.

## 8.4.2: Recursion Tree Definition
A recursion tree is a rooted tree where each node represents one recursive subproblem. 
*   The **value** of each node is the time spent on that specific subproblem, excluding the recursive calls made from that node. 
*   Recursive calls are added up in the nodes below as children of the node that created them.

### Parameters of a Recursion Tree
On an input of size $n$:
*   $f(n)$ represents the time spent on **non-recursive work** (splitting the problem and combining answers).
*   $r$ is the **number of recursive calls**.
*   Each recursive call works on a subproblem of size $n/c$, where $c$ is a constant fraction of the input.

> [!IMPORTANT]
> The parameters $r$ (number of subproblems) and $c$ (size reduction factor) are independent. For example, in Karatsuba's algorithm, $r=3$ and $c=2$ because three subproblems of half the size are solved.

The resulting general recurrence is $T(n) = rT(n/c) + f(n)$.

<img width="975" height="478" alt="image" src="https://github.com/user-attachments/assets/54526351-5815-48a5-9f27-4f6e12c98348" />

## 8.4.3: Analyzing the Tree Structure
1.  **Root**: The root of the recursion tree for $T(n)$ has the value $f(n)$.
2.  **Children**: The root has $r$ children, each representing the root of a tree for $T(n/c)$.
3.  **Levels**: Each node at level $d$ has the value $f(n/c^{d})$. 
4.  **Level Population**: Level $i$ contains $r^{i}$ nodes.
5.  **Tree Depth**: The tree has $L$ levels, where $L = \log_{c} n$ (assuming $n$ is a power of $c$).
6.  **Base Case**: Leaves correspond to the base case $T(1)$, which is assumed to be $1$ for asymptotic analysis.
7.  **Leaf Level Cost**: There are $r^{L}$ leaves. The cost of the last level is $r^{\log_{c} n} \cdot f(1) = n^{\log_{c} r}$ (utilizing the property $a^{\log_{b} c} = c^{\log_{b} a}$).

The total cost $T(n)$ is the sum of costs level by level: $T(n) = \sum_{i=0}^{L} r^{i} \cdot f(n/c^{i})$.

## 8.4.4: Three Common Complexity Cases
The total cost can be interpreted as a series where $T(n) = \text{cost of level } 0 + \text{level } 1 + \dots + \text{level } L$.

| Case | Description | Resulting Complexity |
| :--- | :--- | :--- |
| **Decreasing** | Each term is a constant factor smaller than the previous term. | **Root dominates**: $T(n) = O(f(n))$. |
| **Equal** | All terms in the series across all levels are equal. | **Balanced**: $T(n) = O(f(n) \cdot L) = O(f(n) \log n)$. |
| **Increasing** | Series grows exponentially; each term is a constant factor larger than the previous. | **Leaves dominate**: $T(n) = O(n^{\log_{c} r})$. |

## 8.4.5: Examples of Recursion Tree Analysis
### Merge Sort
*   **Recurrence**: $T(n) = 2T(n/2) + n$.
*   **Analysis**: At level $0$, cost is $n$; at level $1$, cost is $2(n/2) = n$; at level $2$, cost is $4(n/4) = n$.
*   **Classification**: Equal series.
*   **Result**: $T(n) = O(n \log n)$.

### Quicksort (Clever Pivot)
*   Assume a pivot always stays in the middle third $[n/3, 2n/3]$, resulting in unequal partitions.
*   **Recurrence**: $T(n) = T(n/3) + T(2n/3) + n$.
*   **Analysis**: The tree will have "holes" because the left side shrinks faster than the right, but the longest path is $L = \log_{3/2} n = O(\log n)$. 
*   **Classification**: Every level still adds up to $n$, making it an equal series.
*   **Result**: $T(n) = O(n \log n)$.

### Integer Multiplication
*   **Naive Approach**: $T(n) = 4T(n/2) + n$.
    *   **Classification**: Increasing (exponential); leaves dominate.
    *   **Result**: $T(n) = n^{\log_{2} 4} = O(n^{2})$.
*   **Karatsuba's Algorithm**: $T(n) = 3T(n/2) + n$.
    *   **Classification**: Increasing (exponential); leaves dominate.
    *   **Result**: $T(n) = n^{\log_{2} 3} \approx O(n^{1.59})$.

## 8.4.6: Summary and Master Theorem
These recursion tree calculations are sometimes referred to as the **Master Theorem** for recursion. By determining if a recurrence is increasing, decreasing, or stable, one can directly read off the asymptotic complexity without manual expansion. This provides a uniform way to analyze divide and conquer algorithms.




---




# 8.5: Divide and Conquer - Quick Select

## 8.5.1: Selection
Selection is the problem of finding a specific position in a list. In particular, we want to find the $k^{th}$ largest value in a sequence of length $n$.

*   **Sort and Look Strategy**: Sort the list in descending order and look at position $k$. Complexity: $O(n \log n)$.
*   **Special Cases**:
    *   $k = 1$: Maximum value, $O(n)$.
    *   $k = n$: Minimum value, $O(n)$.
    *   **Fixed $k$**: $k$ passes, $O(kn)$.
    *   **Median**: $k = n/2$. Finding the median in $O(n)$ makes quicksort $O(n \log n)$.

## 8.5.2: Divide and Conquer Strategy
Recall the partitioning strategy for quicksort. A pivot partitions a sequence into a **lower** segment (values $\le$ pivot) and an **upper** segment (values $>$ pivot).

Let $m = len(lower)$. There are 3 cases for finding the $k^{th}$ largest element:
1.  **$k \le m$**: The answer lies in the **lower** segment.
2.  **$k == m + 1$**: The answer is the **pivot**.
3.  **$k > m + 1$**: The answer lies in the **upper** segment.

### Recursive Strategy
*   **Case 1**: `select(lower, k)`.
*   **Case 2**: `return(pivot)`.
*   **Case 3**: `select(upper, k - (m + 1))`.

<img width="1206" height="421" alt="image" src="https://github.com/user-attachments/assets/c01647a8-befd-495c-9bad-8ea3e0fe83bf" />

## 8.5.3: Quickselect Implementation
```python
def quickselect(L, l, r, k): # k-th largest in L[l:r]
    if (k < 1) or (k > r - l):
        return (None)
    
    (pivot, lower, upper) = (L[l], l + 1, l + 1)
    
    for i in range(l + 1, r):
        if L[i] > pivot: # Extend upper segment
            upper = upper + 1
        else: # Exchange L[i] with start of upper segment
            (L[i], L[lower]) = (L[lower], L[i])
            (lower, upper) = (lower + 1, upper + 1)
            
    (L[l], L[lower - 1]) = (L[lower - 1], L[l]) # Move pivot
    lower = lower - 1
    
    # Recursive calls
    lowerlen = lower - l
    if k <= lowerlen:
        return(quickselect(L, l, lower, k))
    elif k == (lowerlen + 1):
        return (L[lower])
    else:
        return(quickselect(L, lower + 1, r, k - (lowerlen + 1)))
```


## 8.5.4: Analysis of Quickselect
The recurrence is similar to quicksort:
*   $T(1) = 1$
*   $T(n) = \max(T(m), T(n - (m + 1))) + n$, where $m = len(lower)$.

> [!WARNING]
> **Worst Case Performance**:
> If the pivot is always an extreme value (e.g., $m$ is always 0 or $n-1$), the recurrence becomes $T(n) = T(n - 1) + n$. In this case, $T(n)$ is **$O(n^{2})$**.

## 8.5.5: Median of Medians (MoM)
To ensure $O(n)$ performance, we must pick a "good" pivot that knocks off a fixed fraction of the list. The **Median of Medians** algorithm achieves this.

### The MoM Algorithm
1.  **Divide** $L$ into blocks of 5.
2.  **Find** the median of each block (using brute force/sorting).
3.  **Collect** block medians into a list $M$.
4.  **Recursively** apply the process to $M$ to find the median of medians.

```python
def MoM(L): # Median of medians
    if len(L) <= 5:
        L.sort()
        return(L[len(L) // 2])
    
    # Construct list of block medians
    M = []
    for i in range(0, len(L), 5):
        X = L[i : i + 5]
        X.sort()
        M.append(X[len(X) // 2])
        
    return(MoM(M))
```


## 8.5.6: Visualization and Pivot Quality
If each block of 5 is arranged in ascending order top to bottom, and the blocks are ordered by their medians, we can visualize the distribution.

*   The median of medians (MoM) is guaranteed to lie between $3n/10$ and $7n/10$.
*   At least $3/10$ of the elements are smaller than the MoM, and at least $3/10$ are larger.

<img width="920" height="466" alt="image" src="https://github.com/user-attachments/assets/a3657837-21b6-47b0-8a9f-68816337eb82" />
<img width="879" height="476" alt="image" src="https://github.com/user-attachments/assets/72740016-d8df-4808-b239-7b2f3cf14de4" />


## 8.5.7: Fastselect Algorithm
`fastselect` is Quickselect using the Median of Medians to locate the pivot.

```python
def fastselect(L, l, r, k): # k-th largest in L[l:r]
    if (k < 1) or (k > r - l):
        return (None)
    
    # Find MoM pivot and move to L[l]
    pivot = MoM(L[l:r])
    pivotpos = min([i for i in range(l, r) if L[i] == pivot])
    (L[l], L[pivotpos]) = (L[pivotpos], L[l])
    
    # Partition and Recursive calls (Same as quickselect)
    ...
```


### Complexity of Fastselect
*   `MoM` is $O(n)$.
*   Recurrence for `fastselect`: $T(n) = \max(T(3n/10), T(7n/10)) + n$, which is **$O(n)$**.

## 8.5.8: Summary and Historical Notes
*   **Pivot Selection**: Median of block medians helps find a good pivot in $O(n)$.
*   **Performance**: Selection becomes $O(n)$; Quicksort can be made $O(n \log n)$ in the worst case using this strategy.
*   **Exact Median**: `fastselect` with $k = len(L)/2$ finds the real median in $O(n)$.

> [!NOTE]
> **History**:
> *   C.A.R. Hoare described Quickselect in 1962, the same paper introducing Quicksort.
> *   The Median of Medians algorithm was introduced in 1973 by Manuel Blum, Robert Floyd, Vaughn Pratt, Ron Rivest, and Robert Tarjan.
