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












# 10.2: Linear Programming: Production Planning

## 10.2.1: Production Planning Scenario - Handwoven Carpets
A company makes handwoven carpets. The production parameters are as follows:
*   **Workforce**: The company has 30 employees at the beginning of the year.
*   **Standard Production**: Each employee produces 20 carpets a month.
*   **Wages**: Each employee is paid a salary of Rs 20,000.
*   **Labor Cost**: The labor cost is Rs 1,000 per carpet ($20,000 / 20 = 1,000$).
*   **Seasonal Demand**: Monthly demand varies from January to December, represented as $d_{1}, d_{2}, \dots, d_{12}$.

## 10.2.2: Coping with Varying Demand
To meet demand that fluctuates above or below the standard capacity of 600 carpets ($30 \text{ workers} \times 20 \text{ carpets}$), the business has several options:

1.  **Overtime**:
    *   Workers are paid 80% extra for overtime carpets.
    *   The cost of an overtime carpet is Rs 1,800 (Rs 1,000 regular cost + Rs 800 bonus).
    *   Overtime limit is 30% per worker, meaning a worker can make at most 6 extra carpets per month (26 total).
2.  **Hiring and Firing**:
    *   **Hiring Cost**: Rs 3,200 per worker (due to training, searching, etc.).
    *   **Firing Cost**: Rs 4,000 per worker (due to bonus for hardship, etc.).
3.  **Surplus Storage**:
    *   Excess carpets made in one month can be stored to meet future demand.
    *   **Storage Cost**: Rs 80 per carpet per month.

## 10.2.3: Formulating the Linear Program
To find the minimum cost to meet monthly demand exactly for 12 months, the problem is modeled using 74 variables.

### Variables
For each month $i \in \{1, 2, \dots, 12\}$:
*   $w_{i}$: number of workers employed in month $i$.
*   $x_{i}$: total carpets made in month $i$.
*   $o_{i}$: carpets made in overtime during month $i$.
*   $h_{i}$: workers hired at the start of month $i$.
*   $f_{i}$: workers fired at the start of month $i$.
*   $s_{i}$: surplus carpets stored after month $i$.

**Boundary Conditions**:
*   $w_{0} = 30$ (initial workforce).
*   $s_{0} = 0$ (initial inventory).

### Constraints
For each month $i \in \{1, 2, \dots, 12\}$, the following linear constraints must be satisfied:

1.  **Non-negativity**: All variables must be greater than or equal to zero.
    *   $w_{i}, x_{i}, o_{i}, h_{i}, f_{i}, s_{i} \ge 0$.
2.  **Production Composition**: Total carpets made equals regular production plus overtime production.
    *   $x_{i} = 20w_{i} + o_{i}$.
3.  **Workforce Consistency**: The number of workers must match hiring and firing actions.
    *   $w_{i} = w_{i-1} + h_{i} - f_{i}$.
4.  **Inventory Consistency**: Stored carpets are connected to the earlier stock, current production, and current demand.
    *   $s_{i} = s_{i-1} + x_{i} - d_{i}$.
5.  **Overtime Limit**: Overtime production is at most 6 carpets per worker (30% of regular production).
    *   $o_{i} \le 6w_{i}$.

### Objective Function
The goal is to **minimize the total cost** over 12 months:
$$\text{Minimize: } 20000(w_{1} + \dots + w_{12}) + 3200(h_{1} + \dots + h_{12}) + 4000(f_{1} + \dots + f_{12}) + 80(s_{1} + \dots + s_{12}) + 1800(o_{1} + \dots + o_{12})$$.

> [!NOTE]
> The objective includes the full salary for all workers, hiring/firing fees, storage fees, and the total cost (Rs 1,800) for every carpet made during overtime.

## 10.2.4: Solving the Linear Program
The linear program can be solved using the **Simplex algorithm**.

<img width="950" height="440" alt="image" src="https://github.com/user-attachments/assets/64b18dbf-e0ef-4780-b9a6-a6d10ab207aa" />

### Fractional vs. Integer Solutions
A standard linear program solution may result in fractional values (e.g., hiring 10.6 workers in March).
*   **Rounding**: One approach is to round the fraction to the nearest integer (e.g., 10 or 10) and recompute the cost.
    *   If the values are "large," rounding does not affect the quality of the solution much.
    *   If the values are "small," rounding requires more care as it can significantly change the overall cost.
*   **Integer Linear Programming (ILP)**: Insisting that variables must be integers (since people and carpets are indivisible) makes the problem **computationally intractable**.
*   **Intractability**: Computationally intractable means there is no clever shortcut; an exhaustive search of integer points within the feasible region may be unavoidable in the worst case.








---












# 10.3: Linear Programming: Bandwidth Allocation

## 10.3.1: Network Bandwidth Scenario
A telecom network or an internet service provider (ISP) has three users (or locations of a company), **A**, **B**, and **C**, that need to be connected to each other. The hubs maintained by the ISP are represented as **a**, **b**, and **c**. Each company office is linked to its nearest hub, and these hubs are linked to each other.

*   **Capacity Constraints**: Each link has a bandwidth capacity measured in Mbps.
    *   Example: No more than 10 Mbps can flow between hub **b** and office **B**.
*   **Service Requirements**: Each connection between pairs (A-B, B-C, A-C) must have a minimum of **2 Mbps** of bandwidth.
*   **Connection Types**: Bandwidth is not affected by the number of hops. Both direct (short) and indirect (long) connections are allowed.
    *   **A-B Short Route**: A-a-b-B.
    *   **A-B Long Route**: A-a-c-b-B.
*   **Revenue**: The ISP earns differential revenue per Mbps for each route.
    *   **A-B**: Rs 300/Mbps.
    *   **B-C**: Rs 200/Mbps.
    *   **A-C**: Rs 400/Mbps.

**Goal**: Allocate bandwidth across the network to **maximize revenue** while satisfying all capacity and service constraints.


<img width="987" height="496" alt="image" src="https://github.com/user-attachments/assets/28456bc7-d7b7-4f8f-a300-f7e9c971f531" />
<img width="987" height="496" alt="image" src="https://github.com/user-attachments/assets/6a9a26aa-6624-4422-908a-11712e76fdd0" />

## 10.3.2: Formulating the Linear Program
To model this, variables are created for each possible route between the offices.

### Variables
Let $x$ denote a short route and $y$ denote a long route.
*   $x_{AB}, y_{AB}$: Bandwidth for A-B via short and long connections.
*   $x_{AC}, y_{AC}$: Bandwidth for A-C via short and long connections.
*   $x_{BC}, y_{BC}$: Bandwidth for B-C via short and long connections.

### Constraints
The constraints must reconcile the link capacities with the traffic flowing across them.

1.  **Link Capacity Constraints**: The sum of all traffic on a specific edge cannot exceed its Mbps capacity.
    *   **Edge b-B (Capacity 10)**: $x_{AB} + y_{AB} + x_{BC} + y_{BC} \le 10$.
    *   **Edge a-A (Capacity 12)**: $x_{AB} + y_{AB} + x_{AC} + y_{AC} \le 12$.
    *   **Edge c-C (Capacity 8)**: $x_{AC} + y_{AC} + x_{BC} + y_{BC} \le 8$.
    *   **Internal Edge a-b (Capacity 6)**: $x_{AB} + y_{AC} + y_{BC} \le 6$ (Each internal link supports one short and two long edges).
    *   **Internal Edge a-c (Capacity 13)**: $y_{AB} + x_{BC} + y_{AC} \le 13$.
    *   **Internal Edge b-c (Capacity 10)**: $y_{AB} + y_{BC} + x_{AC} \le 10$.

2.  **Service Quality Constraints**: Minimum pairwise bandwidth must be at least 2 Mbps.
    *   $x_{AB} + y_{AB} \ge 2$.
    *   $x_{BC} + y_{BC} \ge 2$.
    *   $x_{AC} + y_{AC} \ge 2$.

3.  **Implicit Constraints**: Traffic on all routes must be non-negative.
    *   $x_{AB}, y_{AB}, x_{AC}, y_{AC}, x_{BC}, y_{BC} \ge 0$.

### Objective Function
Maximize the total revenue earned from allocated bandwidth:
$$\text{Maximize: } 300(x_{AB} + y_{AB}) + 200(x_{BC} + y_{BC}) + 400(x_{AC} + y_{AC})$$

## 10.3.3: Solution and Analysis
Using the **Simplex algorithm**, the following optimal values are obtained:
*   $x_{AB} = 0, y_{AB} = 7$ (Total A-B bandwidth = 7).
*   $x_{BC} = 1.5, y_{BC} = 1.5$ (Total B-C bandwidth = 3).
*   $x_{AC} = 0.5, y_{AC} = 4.5$ (Total A-C bandwidth = 5).

> [!NOTE]
> **Observations on Solution**:
> *   **Fractional Values**: Unlike production planning (people/carpets), bandwidth is infinitely divisible, so fractional solutions are perfectly acceptable.
> *   **Saturation**: In this solution, all edges are at full capacity except for edge **a-c**.
> *   **Revenue Logic**: The least allocation is given to the B-C route (3 Mbps), which provides the lowest revenue (Rs 200/Mbps).

## 10.3.4: Limitations of the LP Modeling Strategy
The current strategy of using one variable per path **does not scale well**.

*   **Exponential Growth**: In general, the number of possible paths between any two nodes in a graph is **exponential**.
*   **Large Encodings**: If one variable is constructed for every possible route, the resulting linear program becomes excessively large relative to the original graph.
*   **Alternative Approach**: Such problems are more effectively solved using a better approach to analyze **network flows** directly on the graph rather than converting them to a standard linear program with path-based variables.
