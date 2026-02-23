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
