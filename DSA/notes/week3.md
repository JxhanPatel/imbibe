# 3.1: Quicksort


## 1. Overview and Motivation

Quicksort is the final sorting algorithm addressed in this section of the course. It is designed to overcome specific shortcomings associated with **Merge Sort**.

### Shortcomings of Merge Sort:

- **Extra Space:** The `merge` function requires additional memory to store the merged list. This creates an extra copy of all the space used to store the original list 
- **Recursion Overhead:** Merge Sort has an inherent recursion. Programming language infrastructure for recursion requires suspending current computation, making a call, and restoring it (often called "paperwork") Recursive calls are computationally expensive.

### The Quicksort Premise:

Merge Sort requires merging because sorted halves cannot simply be stuck together; elements must be shifted across halves to assemble the full sorted list

**The Idea:** If we can divide the list such that everything on the left is smaller than everything on the right, we can sort each part and simply concatenate them without a merge step 

---

## 2. Theoretical Foundation: The Median Approach

If we could calculate the **median value** (the value that divides the list such that half are smaller and half are larger), we could partition the list by value rather than position 

### Recurrence for Median-Based Sorting:

$T(n) = 2T(n/2) + n$

- $n$: The cost to walk through the list and compare each value to the median to decide if it goes left or right 
- $2T(n/2)$: Sorting the two halves.
- **Result:** An $O(n \log n)$ algorithm 

**The Problem:** Finding the median is typically done by sorting the list first. Using sorting to find a median to use for sorting creates a circular argument and is inefficient 

---

## 3. The Quicksort Algorithm

To avoid the difficulty of finding the median, Quicksort (developed by **Tony Hoare**) picks an arbitrary value from the list called a **pivot element**

### High-Level Strategy:

1. **Choose a Pivot:** Usually the first element of the list 
2. **Partition:** Rearrange the list into two parts:

   - **Lower Part:** Everything smaller than or equal to the pivot.
   - **Upper Part:** Everything larger than the pivot.
3. **Place Pivot:** Move the pivot to its correct position between the Lower and Upper parts.
4. **Recurse:** Separately apply Quicksort to the Lower and Upper parts.


<img width="684" height="527" alt="image" src="https://github.com/user-attachments/assets/05074216-5eca-4ada-b8bc-d2b2830a9145" />



---

## 4. Partitioning Strategy

The heart of Quicksort is the partitioning process. One effective strategy involves scanning the list from left to right 

### Four Segments during Scanning:

1. **Pivot:** The element at the leftmost position (index 0).
2. **Lower:** Elements already seen and known to be $\le$ pivot.
3. **Upper:** Elements already seen and known to be $>$ pivot.
4. **Unclassified:** Elements not yet examined 

### The Partitioning Logic:

- If the **Unclassified** element is **larger** than the pivot: Simply move the boundary to include it in the Upper segment 
- If the **Unclassified** element is **smaller or equal** to the pivot: Exchange the unclassified element with the first element of the Upper segment. This "vacates" a spot at the end of the Lower segment and shifts the Upper segment forward


<img width="679" height="248" alt="image" src="https://github.com/user-attachments/assets/ad8fbefd-be8c-4e6d-91eb-74d04d5144d0" />


### Finalizing the Partition:

Once all elements are classified, the pivot (at the start) is swapped with the rightmost element of the Lower partition. This places the pivot in its final, correct sorted position 

---

## 5. Implementation (Python)

The following Python code represents the recursive implementation of Quicksort using the described partitioning strategy.

```python
def quicksort(L, l, r): # Sort L[l:r]
    if (r - l <= 1):
        return (L)
    
    # Partitioning step
    pivot = L[l]
    lower = l + 1
    upper = l + 1
    
    for i in range(l + 1, r):
        if L[i] > pivot:
            # Extend upper segment
            upper = upper + 1
        else:
            # Exchange L[i] with start of upper segment
            (L[i], L[lower]) = (L[lower], L[i])
            lower = lower + 1
            upper = upper + 1
            
    # Move pivot to its correct place
    (L[l], L[lower-1]) = (L[lower-1], L[l])
    lower = lower - 1
    
    # Recursive calls on left and right parts
    quicksort(L, l, lower)
    quicksort(L, lower + 1, upper)
    
    return(L)

```

### Key Implementation Details:

- **Base Case:** If the slice has at most one element (`r - l <= 1`), return 
- **Indices:** The algorithm uses indices to sort slices of the list `L[l:r]` in place 
- **Pivot Exclusion:** The recursive calls exclude the pivot's new position (`lower`) because the pivot is already in the correct place 

---

## 6. Summary and Observations

- **In-Place Sorting:** Unlike Merge Sort, Quicksort can be implemented to update the list in place, avoiding extra space for merging
- **Alternative Strategies:** Other partitioning strategies exist, such as building Lower and Upper segments from opposite ends of the list and meeting in the middle
- **Efficiency:** While Quicksort avoids the merge step, its performance depends on how well the pivot divides the list, which requires complexity analysis .

> [!IMPORTANT]
> The partitioning algorithm is where implementation mistakes usually occur. The recursive calls are straightforward, but the logic to correctly rearrange elements around the pivot must be handled with care 








---

# 3.2: Analysis of Quicksort

### Overview of Quicksort Mechanism

Quicksort operates through the following steps:

1. **Choose a pivot**: Select an element from the list.
2. **Partition**: Divide the array into two segments a lower and a right segment based on the pivot.
3. **Recursive Sort**: Recursively sort the two partitions.

The efficiency of the algorithm depends on how well the partitioning divides the array into two smaller parts.


### Best-Case Analysis (The Median Pivot)

If the chosen pivot is the median, the two halves are roughly equal in size (each containing approximately half the elements).

- **Partitioning Cost**: $O(n)$ (takes linear time in one scan).
- **Recurrence Relation**:



$$
T(n) = 2 \cdot T\left(\frac{n}{2}\right) + n
$$
- **Result**: $O(n \log n)$.

In this scenario, Quicksort behaves like Merge Sort but functions as an in-place algorithm without the extra space overhead of recursion or additional arrays.

### Worst-Case Analysis

In the worst case, there is no control over the pivot (e.g., picking the first element without analyzing values). The pivot may be the smallest or largest value in the list.

- **Partition Behavior**: One partition will have $0$ elements, while the other will have $n-1$ elements.
- **Recurrence Relation**:



$$
T(n) = T(n-1) + n
$$
- **Result**: $O(n^2)$.

**Example of the Worst Case**: Paradoxically, an already sorted array is a worst-case input if the first element is always chosen as the pivot.

- If sorting in ascending order and the first element is picked, it is the smallest.
- This produces an upper partition of $n-1$ sorted elements.
- The next step picks the next smallest element, producing $n-2$ sorted elements, and so on.



### Average-Case Analysis

The average-case complexity for Quicksort is $O(n \log n)$.

- **Relative Order**: Since sorting depends on "compare and swap," the actual values do not matter as much as their relative order. An input of size $n$ can be viewed as one of the $n!$ (n factorial) permutations of elements.
- **Assumption**: In a general sorting scenario, every permutation is assumed to be equally likely.
- **Expected Running Time**: Averaged over all $n!$ permutations, the time is $O(n \log n)$, making $O(n^2)$ a rare case.

### Randomized Quicksort

To prevent an adversary from providing a worst-case input, a fixed strategy for choosing a pivot (like always picking the first, last, or middle element) should be avoided.

- **Randomized Strategy**: Pick a random value between $0$ and $n-1$ uniformly (with probability $1/n$).
- **Benefit**: By picking the pivot at random in each iteration, it is impossible for an input to consistently trigger the worst-case behavior.
- **Result**: This achieves an expected running time of $O(n \log n)$.

### Implementation Characteristics

- **Iterative Quicksort**: Quicksort can be implemented iteratively. Because calls happen on disjoint parts of the array (bounded intervals from left to right), working on one segment does not influence others.
- **In-place**: Unlike Merge Sort, Quicksort works in place and does not require the construction of new arrays.
- **Efficiency**: Quicksort is very fast in practice and is often the algorithm used for built-in sorting functions in programming languages (e.g., Python's `sort` or `sorted`).

### Summary Table: Quicksort Complexity

| Case | Complexity | Condition |
| --- | --- | --- |
| **Best Case** | O(nlogn) | Pivot is always the median. |
| **Worst Case** | O($n^2$) | Pivot is always the smallest or largest element. |
| **Average Case** | O(nlogn) | Based on the distribution of all permutations. |
| **Expected Case** | O(nlogn) | Using a randomized pivot selection strategy. |

 [!IMPORTANT]
> Quicksort illustrates that using an upper bound ($O(n^2)$) as a prediction for overall behavior is often very pessimistic, as its practical performance is highly efficient.




---

## Concluding Remarks on Sorting Algorithms

### 1. Stability in Sorting

Stability is a crucial property when sorting **compound values** (e.g., rows in a table or spreadsheet). A list of rows typically contains several sub-parts or **attributes** (columns) like roll number, name, and marks.

- **Definition of Stability**: A sorting algorithm is stable if it preserves the relative order of elements with equal keys. If you sort a list that was previously sorted by one column (e.g., Roll Number) by a new column (e.g., Name), stability ensures that students with the same name remain in their original roll number order.

#### Algorithm-Specific Stability:

- **Quicksort**: Generally **not stable**. The partitioning process involves exchanges (swaps) that can shuffle elements with equal values in the sorting key but different values in other attributes.
- **Merge Sort**: Can be made **stable** quite easily. To guarantee stability during the merge process, if the same value is encountered in both the left and right sub-lists, the element from the **left list** should be moved first. This prevents the value on the right from "overtaking" the value on the left.


### 2. Data Movement

Beyond the number of comparisons and swaps, **data movement** is an important, orthogonal criterion for evaluating sorting algorithms.

- In physical scenarios (e.g., moving heavy boxes) or specific digital contexts, the cost of physically moving data from one location to another can be high.
- In such cases, one might prefer an algorithm that performs more comparisons if it results in fewer overall data movements.

### 3. Choosing the "Best" Sorting Algorithm

There is no single "best" sorting algorithm; the choice depends on the specific context and constraints:

- **General Purpose/Built-in Functions**: Quicksort is often the algorithm of choice for built-in functions in programming languages despite its worst-case complexity, due to its practical speed.
- **External Sorting**: When data is too large to fit into memory (e.g., in databases), **Merge Sort** is typically used because it handles parts of data being moved in and out of memory effectively.
- **Other O(nlogn) Algorithms**: Other algorithms like **Heap Sort** also provide $O(n \log n)$ performance.
- **Hybrid Strategies**: Practical implementations often combine algorithms. For example, a system might use a divide-and-conquer strategy (like Merge Sort or Quicksort) for large $n$, but switch to **Insertion Sort** for small lists (e.g., $n \le 16$ or $32$).

### Summary of Considerations

| Criterion | Description |
| --- | --- |
| **Stability** | Does it preserve the relative order of equal elements? |
| **Data Movement** | How much physical or memory-level moving of data is required? |
| **Memory Constraints** | Can the data fit in memory (internal) or does it require disk access (external)? |
| **Hybrid Use** | Using different algorithms for different scales of data. |

> [!TIP]
> While most users will rely on built-in sorting routines, understanding these properties is essential for situations where you must implement or choose a specific sorting strategy for a database or high-performance application.




---


# 3.5: Difference between Lists and Arrays

## 1. Introduction to Sequences

In the context of programming, we are interested in maintaining **sequences of values**.

- We have a first value, a second value, and so on.
- We want to talk about the $i^{th}$ value and the $j^{th}$ value, look them up, or exchange them.
- Lists and arrays are two different ways of maintaining such a sequence.

---

## 2. Conceptual Definitions

### 2.1 Lists (Linked Lists)

A list, in the conventional understanding, is a **flexible length sequence**.

- **Dynamic Nature:** It is a sequence that can grow and shrink; you can modify its structure.
- **Memory Allocation:** It is typically scattered or dispersed in memory. You cannot point to a single segment of memory and say the entire list is sitting there; it might be sitting in a lot of places.
- **Python Example:** A built-in Python list allows you to take a slice and replace it with a longer or shorter slice, append to the beginning, or delete a value in the middle.

### 2.2 Arrays

An array is typically designed **not to grow or shrink**.

- **Fixed Size:** In the conventional understanding, it is a fixed-size sequence.
- **Memory Allocation:** Because the size is fixed and known in advance, space can be blocked/allocated in a fixed space in memory.
- **Use Case Example:** Processing graphs often requires an adjacency matrix or a list of vertices. If the number of vertices $n$ is known, the length is fixed and will not grow or shrink.

---

## 3. Structural Representation

### 3.1 The Linked List Structure

A list is a sequence of **nodes**.

- **Node Composition:** Each node has a **value** and a pointer/link to the **next node** in the sequence.
- **Navigation:** It is like following directions; you ask the first node where the next one is and follow the links.
- **Metadata:** Typically, we know where the list begins (**head**). We may also keep information about where the last node is to help extend the list without traversing the whole thing.
- **Analogy:** Like a train consisting of independent wagons (bogies) connected to each other. If the links were elastic, the parts could be all over the place, but they remain logically connected.

<img width="999" height="248" alt="image" src="https://github.com/user-attachments/assets/ed541861-3fb4-4b28-b592-75693d1cb14a" />


### 3.2 The Array Structure

- **Homogeneity:** Usually, all elements in an array are of the same type (and thus the same size).
- **Memory Calculation:** If each value takes a fixed size and we need $n$ of them, we allocate $n \times \text{size}$ in one contiguous position.
- **Random Access:** Because it sits in one place, we can use arithmetic to calculate the exact position of any element.

  - **Formula:** To get to the $i^{th}$ entry, skip past $i \times \text{space for an element}$ from the starting point.
  - **Analogy:** Like walking on tiles where you can close your eyes and walk exactly 20 steps to reach your destination without checking every tile in between.

---

## 4. Operation Analysis and Trade-offs

### 4.1 Accessing Elements

- **Lists:** Accessing the $i^{th}$ element takes time **proportional to i** (linear time) because you must walk down from the head. There is no direct way to reach it.
- **Arrays:** Supports **Random Access**. Accessing the beginning, middle, or end takes the same amount of time, independent of whether the list has 100 or 1 million elements (constant time).

### 4.2 Insertion and Deletion

- **Lists:**

  - **The "Plumbing" Operation:** Inserting a value $x$ between $v_{i-1}$ and $v_{i}$ involves changing the link of $v_{i-1}$ to point to $x$, and $x$ to point to $v_{i}$.
  - **Cost:** It is a local operation with constant overhead **provided you are already at the position**.
  - **Deletion:** Bypassing a node so that the previous node points directly to the next one.
- **Arrays:**

  - **The "Crowded Bookshelf" Analogy:** Inserting a value requires shifting everything below it by one position to make space.
  - **Cost:** In the worst case (inserting at the beginning), it takes $O(n)$ work to move everything to the right. Deleting requires compressing the hole, taking $O(n)$ to push everything from right to left.

<img width="631" height="244" alt="image" src="https://github.com/user-attachments/assets/fa950829-a50f-4673-8009-22dc05dc090f" />


### 4.3 Summary Table of Operations

| Operation | Array | List (Linked List) |
| --- | --- | --- |
| **Accessing $i^th$ element** | O(1) (Constant) | O(n) (Linear) |
| **Exchange A\[i\],A\[j\]** | O(1) | O(n) (must walk to i and j) |
| **Insert/Delete (at known position)** | O(n) (due to shifting) | O(1) (local rearrangement) |


---

## 5. Algorithmic Impact: Insertion Sort Example

When inserting a value $v$ into a sorted sequence of length $n$:

1. **Finding the Position:**

   - **Array:** Can use **Binary Search** to find the position in $O(\log n)$ time because of random access to midpoints.
   - **List:** Must walk halfway or more, taking $O(n)$ to find the position.
2. **The Insert Step:**

   - **Array:** Once found, inserting requires shifting, taking $O(n)$ time.
   - **List:** Once found, the "plumbing" is $O(1)$.

**Conclusion:** In both cases, the total operation is pushed to $O(n)$, but for different reasons (shifting for arrays, finding the position for lists).

---

## 6. Key Takeaways

> [!IMPORTANT]
>
> - **Lists** are flexible and dynamic but accessing elements takes linear time.
> - **Arrays** support random access (constant time) but expansion and contraction take linear time.
> - Algorithm analysis depends heavily on the underlying representation. Swapping $A[i]$ and $A[j]$ is a "basic operation" only if the sequence is an array.
> - When choosing a representation, consider if the sequence is **dynamic** (frequently growing/shrinking) or **fixed**.
>
