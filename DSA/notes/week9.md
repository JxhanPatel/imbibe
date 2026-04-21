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






