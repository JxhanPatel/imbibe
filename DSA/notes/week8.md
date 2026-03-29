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




