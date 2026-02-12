
# Week 2: Complexity & Sorting

## 🎯 Core Objectives

* [ ] **Master Asymptotic Analysis:** Understand $O(\cdot)$, $\Omega(\cdot)$, and $\Theta(\cdot)$ notations.
* [ ] **Analyze Algorithm Complexity:** Compute time complexity for loops and solve recurrence relations.
* [ ] **Implement & Analyze Search:** Compare Linear vs Binary Search.
* [ ] **Implement & Analyze Sorting:** Study Selection, Insertion, and Merge Sort using Divide & Conquer.

---

# 1. Asymptotic Analysis (Orders of Magnitude)

Efficiency is measured as a function of input size $n$.

We focus on **growth rate**, ignoring:

* Constant factors
* Lower-order terms

Because for large $n$, the highest-order term dominates.

Example:

$$
3n^2 + 5n + 7 \in O(n^2)
$$

---

## 1.1 The Asymptotic Notations

| Notation      | Meaning                  | Definition                                                                                 |
| ------------- | ------------------------ | ------------------------------------------------------------------------------------------ |
| **Big O**     | Upper Bound (Worst Case) | $f(n) \in O(g(n))$ if $\exists c,n_0$ such that $f(n) \le c g(n)$ for all $n \ge n_0$      |
| **Big Omega** | Lower Bound (Best Case)  | $f(n) \in \Omega(g(n))$ if $\exists c,n_0$ such that $f(n) \ge c g(n)$ for all $n \ge n_0$ |
| **Big Theta** | Tight Bound              | $f(n) \in \Theta(g(n))$ if $f(n)$ is both $O(g(n))$ and $\Omega(g(n))$                     |

---

### 🔑 Dominance Rule

If:

$$
f(n) = n^2 + 100n
$$

Then:

$$
f(n) \in \Theta(n^2)
$$

Because:

$$
n^2 \gg n
$$

Always drop:

* Constants
* Lower-order terms

---

## 1.2 Common Growth Orders

From fastest to slowest:

$$
O(1) < O(\log n) < O(n) < O(n \log n) < O(n^2) < O(2^n) < O(n!)
$$

---

## 1.3 Combining Phases

If an algorithm has two phases:

* Phase 1: $O(n)$
* Phase 2: $O(n^2)$

Total complexity:

$$
O(n) + O(n^2) = O(n^2)
$$

Dominant term wins.

---

# 2. Calculating Complexity

---

## 2.1 Iterative Algorithms

### 🔹 Simple Loop

```python
for i in range(n):
    ...
```

Runs $n$ times:

$$
T(n) = O(n)
$$

---

### 🔹 Nested Loops

```python
for i in range(n):
    for j in range(n):
        ...
```

Runs $n \times n$:

$$
T(n) = O(n^2)
$$

---

### 🔹 Triangular Loops

```python
for i in range(n):
    for j in range(i):
        ...
```

Total operations:

$$
1 + 2 + \dots + (n-1) = \frac{n(n-1)}{2}
$$

Thus:

$$
T(n) = O(n^2)
$$

---

## 2.2 Recursive Algorithms

We analyze recursion using **recurrence relations**.

---

### Example: Towers of Hanoi
<img width="641" height="156" alt="image" src="https://github.com/user-attachments/assets/b73cd8ce-36ad-459d-b326-6c50b80e02ca" />

To move $n$ disks:

1. Move $n-1$ disks → $T(n-1)$
2. Move largest disk → $O(1)$
3. Move $n-1$ disks → $T(n-1)$

Recurrence:

$$
T(n) = 2T(n-1) + 1
$$

Unwinding:

$$
T(n) = 2^n - 1
$$

Therefore:

$$
T(n) \in O(2^n)
$$

⚠ Exponential time becomes infeasible quickly.

---

# 3. Searching Algorithms

---

## 3.1 Linear Search

Works on **unsorted data**.

```python
for x in arr:
    if x == target:
        return True
```

### ⏱ Complexity

* Best Case: $O(1)$
* Worst Case: $O(n)$
* Average Case: $O(n)$
* Space: $O(1)$

---

## 3.2 Binary Search

Requires **sorted input**.

Repeatedly halves the search interval.

### Implementation

```python
def binary_search(arr: list[int], target: int) -> bool:
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return True
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
            
    return False
```

---

### Mathematical Analysis

Each step halves the input:

$$
T(n) = T(n/2) + O(1)
$$

After $k$ steps:

$$
\frac{n}{2^k} = 1
$$

So:

$$
k = \log_2 n
$$

Therefore:

$$
T(n) \in O(\log n)
$$

---

### ⏱ Complexity

* Best Case: $O(1)$
* Average/Worst: $O(\log n)$
* Space:

  * Iterative: $O(1)$
  * Recursive: $O(\log n)$ (call stack)

---

# 4. Quadratic Sorting Algorithms — $O(n^2)$

---

## 4.1 Selection Sort

Strategy:

* Repeatedly find minimum
* Swap into correct position

### Complexity

Inner loop always runs fully.

Total comparisons:

$$
(n-1) + (n-2) + \dots + 1 = \frac{n(n-1)}{2}
$$

Thus:

$$
T(n) \in \Theta(n^2)
$$

* Best: $O(n^2)$
* Worst: $O(n^2)$
* Space: $O(1)$
* Stable: ❌ No

Advantage: Minimal swaps.

---

## 4.2 Insertion Sort

Builds sorted prefix incrementally.

---

### ⏱ Complexity

* Best Case (already sorted):

$$
T(n) \in O(n)
$$

* Worst Case (reverse sorted):

$$
T(n) \in O(n^2)
$$

* Space: $O(1)$
* Stable: ✅ Yes

---

### 🔥 Key Insight

Insertion sort is **adaptive**.

For nearly sorted arrays:

$$
T(n) \approx O(n)
$$

Used in hybrid algorithms (e.g., Timsort).

---

# 5. Efficient Sorting — Merge Sort (O(n log n))

## Strategy: Divide and Conquer

1. Divide into halves
2. Recursively sort
3. Merge sorted halves

---

## 5.1 Merge Operation

Merging two sorted lists of total size $n$:

$$
T(n) = O(n)
$$

---

## 5.2 Recurrence Relation

Each level:

* Split into 2 subproblems of size $n/2$
* Merge cost: $O(n)$

Recurrence:

$$
T(n) = 2T(n/2) + O(n)
$$

---

### Recursion Tree Analysis

* Tree height: $\log n$
* Work per level: $O(n)$

Total work:

$$
O(n \log n)
$$

---

## ⏱ Complexity

* Best: $O(n \log n)$
* Average: $O(n \log n)$
* Worst: $O(n \log n)$
* Auxiliary Space: $O(n)$
* Recursion Depth: $O(\log n)$
* Stable: ✅ Yes

---

# 6. Comparative Summary

---

## 🔎 Searching Comparison

| Feature          | Linear | Binary      |
| ---------------- | ------ | ----------- |
| Sorted Required? | No     | Yes         |
| Worst Case       | $O(n)$ | $O(\log n)$ |
| Best Case        | $O(1)$ | $O(1)$      |
| Space            | $O(1)$ | $O(1)$      |

---

## 🔎 Sorting Comparison

| Algorithm | Time          | Stable | In-Place         | Notes                  |
| --------- | ------------- | ------ | ---------------- | ---------------------- |
| Selection | $O(n^2)$      | ❌      | ✅                | Few swaps              |
| Insertion | $O(n^2)$      | ✅      | ✅                | Adaptive               |
| Merge     | $O(n \log n)$ | ✅      | ❌ ($O(n)$ space) | Guaranteed performance |

---

# 📚 Glossary

| Term                      | Definition                                                                                  |
| ------------------------- | ------------------------------------------------------------------------------------------- |
| **Asymptotic Complexity** | Growth rate as $n \to \infty$                                                               |
| **Amortized Analysis**    | Average cost per operation over worst-case sequence (e.g., list append is amortized $O(1)$) |
| **Divide & Conquer**      | Split → Solve → Combine                                                                     |
| **Stable Sort**           | Preserves order of equal elements                                                           |
| **In-Place**              | Uses $O(1)$ auxiliary space                                                                 |
| **Recurrence Relation**   | Defines $T(n)$ in terms of smaller inputs                                                   |
