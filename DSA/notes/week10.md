# 10.1: Linear Programming

## 10.1.1: Optimization Problems
Many computational tasks involve optimization, such as finding the shortest path, minimum cost spanning tree, or longest common subsequence. These values must be found within some constraints. 
*   **Shortest path**: Follows edges in the graph.
*   **Spanning tree**: Is a subset of the given edges.
*   **Subsequence**: Letters are from the given words.

## 10.1.2: Definition of Linear Programming
Linear programming is a general class of problems where constraints and the objective to be optimized are linear functions. A linear function is something where you have a variable multiplied by a constant, but you do not have variables multiplied by each other (e.g., no $x^{2}$ or $x \times y$).

*   **Constraints**: $a_{1}x_{1} + a_{2}x_{2} + \dots + a_{m}x_{m} \le K$, $b_{1}x_{1} + b_{2}x_{2} + \dots + b_{m}x_{m} \ge L, \dots$.
*   **Objective**: $c_{1}x_{1} + c_{2}x_{2} + \dots + c_{m}x_{m}$.

The goal is to find the optimum values for $x_{1}$ to $x_{m}$, subject to these constraints, which optimizes (maximizes or minimizes) that objective.

## 10.1.3: Example - Maximize Profits (Grandiose Sweets)
Grandiose Sweets sells cashew barfis and dry fruit halwa.
*   Profit for each box of barfis is Rs 100.
*   Profit for each box of halwa is Rs 600.
*   Daily demand for barfis is at most 200 boxes.
*   Daily demand for halwa is at most 300 boxes.
*   Staff can produce 400 boxes a day, altogether.
*   **Goal**: What is the most profitable mix of barfis and halwa to produce?.

### Linear Programming Model
*   $b$: boxes of barfi to produce per day.
*   $h$: boxes of halwa to produce per day.
*   **Objective**: Maximize $100b + 600h$.
*   **Demand Constraints**: $b \le 200$, $h \le 300$.
*   **Production Constraint**: $b + h \le 400$.
*   **Implicit Constraints**: $b \ge 0, h \ge 0$.

<img width="967" height="489" alt="image" src="https://github.com/user-attachments/assets/e6329cbf-c3ab-45d8-84fe-79d650ac40cb" />

## 10.1.4: Solving Linear Programs - Pictorial Approach
The constraints define a **feasible region**. Any combination of $b$ and $h$ in this region satisfies all constraints.
*   At $(0, 100)$: Profit $c = 100(0) + 600(100) = 60,000$.
*   At $(0, 200)$: Profit $c = 120,000$.
*   At $(0, 300)$: Profit $c = 180,000$.
*   At $(100, 300)$: Profit $c = 100(100) + 600(300) = 190,000$.

> [!IMPORTANT]
> **Vertex Property**: The optimal value always lies at a vertex of the feasible region.

## 10.1.5: The Simplex Algorithm
Simplex is a classical algorithm that exploits the vertex property.
1.  Start at any vertex and evaluate the objective.
2.  If an adjacent vertex has a better value, move there.
3.  If the current vertex is better than all neighbors, stop.

**Complexity**: Can be exponential in the worst case, but is efficient in practice. Theoretically efficient (polynomial time) algorithms also exist.

## 10.1.6: Existence of Solutions
*   **Feasible region is convex**: A shape is convex if any two points inside it can be connected by a line that stays entirely within the shape.
*   **Empty region**: Constraints may be unsatisfiable, resulting in no solution.
*   **Unbounded**: There may be no upper/lower limit on the objective.

## 10.1.7: Extended Example - Almond Rasmalai
Grandiose Sweets adds almond rasmalai ($r$).
*   **Profit per box**: barfis - Rs 100, halwa - Rs 600, rasmalai - Rs 1300.
*   **Daily demand**: barfis - 200, halwa - 300, rasmalai - unlimited.
*   **Production capacity**: 400 boxes a day, altogether.
*   **Milk supply**: Limited to 600 boxes of halwa OR 200 boxes of rasmalai. Rasmalai needs 3 times as much milk.

### New Linear Program
*   **Objective**: Maximize $100b + 600h + 1300r$.
*   **Constraints**:
    *   $b \le 200$
    *   $h \le 300$
    *   $b + h + r \le 400$ (Production capacity)
    *   $h + 3r \le 600$ (Milk supply).
    *   $b, h, r \ge 0$.

**Optimum Solution**: $(0, 300, 100)$ with profit $c = 310,000$.

<img width="991" height="475" alt="image" src="https://github.com/user-attachments/assets/979bc06e-ff58-4ec6-9b56-a2bb77ea5c65" />

## 10.1.8: LP Duality
We can derive an upper bound on the objective through a linear combination of constraints.
Consider constraints:
(A) $h \le 300$
(B) $b + h + r \le 400$
(C) $h + 3r \le 600$.

Combine them as $100 \cdot (A) + 100 \cdot (B) + 400 \cdot (C)$:
$100(h) + 100(b + h + r) + 400(h + 3r) \le 100(300) + 100(400) + 400(600)$
$100b + 600h + 1300r \le 310,000$.

The LHS is the profit, and the value at $(0, 300, 100)$ matches this upper bound.


### Dual LP Problem
It is always possible to construct a linear combination of constraints that tightly captures the upper bound.
*   **Dual Goal**: Minimize the linear combination of constraints.
*   **Variables**: Multipliers for the linear combination.
*   **Implicit constraint**: Multipliers are non-negative.
*   **Theorem**: The optimum solution solves both the original (primal) and the dual LP.






---










