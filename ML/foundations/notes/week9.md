# 9.1 Constrained Optimization

In constrained optimization, we minimize an objective function subject to constraints on the variables. A general formulation for these problems is:

$$
\min_{x \in \mathbb{R}^n} f(x)
$$

subject to

$$
\begin{cases} c_i(x) = 0, & i \in \mathcal{E}, \\ c_i(x) \ge 0, & i \in \mathcal{I}, \end{cases}
$$

where $f$ and the functions $c_i$ are all smooth, real-valued functions on a subset of $\mathbb{R}^n$, and $\mathcal{I}$ and $\mathcal{E}$ are finite index sets of inequality and equality constraints, respectively. The objective function is $f$, $c_i, i \in \mathcal{E}$ are the equality constraints, and $c_i, i \in \mathcal{I}$ are the inequality constraints. The feasible set $\Omega$ is the set of points $x$ that satisfy the constraints:
$$\Omega = \{x | c_i(x) = 0, i \in \mathcal{E}; c_i(x) \ge 0, i \in \mathcal{I} \}$$.

## 9.1.1 Local and Global Solutions
A vector $x^\star$ is a local solution of the problem if $x^\star \in \Omega$ and there is a neighborhood $\mathcal{N}$ of $x^\star$ such that $f(x) \ge f(x^\star)$ for $x \in \mathcal{N} \cap \Omega$.
*   **Strict Local Solution**: A vector $x^\star$ is a strict local solution (also called a strong local solution) if $x^\star \in \Omega$ and there is a neighborhood $\mathcal{N}$ of $x^\star$ such that $f(x) > f(x^\star)$ for all $x \in \mathcal{N} \cap \Omega$ with $x \ne x^\star$.
*   **Isolated Local Solution**: A point $x^\star$ is an isolated local solution if $x^\star \in \Omega$ and there is a neighborhood $\mathcal{N}$ of $x^\star$ such that $x^\star$ is the only local solution in $\mathcal{N} \cap \Omega$.

## 9.1.2 The Active Set
The active set $\mathcal{A}(x)$ at any feasible $x$ consists of the equality constraint indices from $\mathcal{E}$ together with the indices of the inequality constraints $i$ for which $c_i(x) = 0$:
$$\mathcal{A}(x) = \mathcal{E} \cup \{i \in \mathcal{I} | c_i(x) = 0\}$$.
At a feasible point $x$, the inequality constraint $i \in \mathcal{I}$ is said to be active if $c_i(x) = 0$ and inactive if the strict inequality $c_i(x) > 0$ is satisfied.

## 9.1.3 Equality Constraints and Descent Directions
To understand the conditions for optimality, consider a two-variable problem with a single equality constraint:
$$\min x_1 + x_2 \text{ s.t. } x_1^2 + x_2^2 - 2 = 0$$.
The feasible set is the boundary of the circle of radius $\sqrt{2}$ centered at the origin.

### First-Order Feasibility and Descent
To retain feasibility with respect to the function $c_1(x) = 0$, we require any small step $s$ to satisfy $c_1(x+s) = 0$. To first order, this requirement is:
$$\nabla c_1(x)^T s = 0$$.
For the step $s$ to produce a decrease in $f$, we require, to first order:
$$\nabla f(x)^T s < 0$$.
Existence of a small step $s$ that satisfies both conditions suggests existence of a direction $d$ with the same properties:
$$\nabla c_1(x)^T d = 0 \text{ and } \nabla f(x)^T d < 0$$.
If no such direction $d$ exists, then $x^\star$ appears to be a local minimizer. This occurs when $\nabla f(x)$ and $\nabla c_1(x)$ are parallel, such that $\nabla f(x^\star) = \lambda_1^\star \nabla c_1(x^\star)$ for some scalar $\lambda_1^\star$ called a Lagrange multiplier.

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/80f0e28a-8200-48e7-b78f-3bcfebbb96bf" />

## 9.1.4 Inequality Constraints and Feasible Directions
Consider an inequality-constrained problem:
$$\min x_1 + x_2 \text{ s.t. } 2 - x_1^2 - x_2^2 \ge 0$$.
The feasible region consists of the circle and its interior. To first order, feasibility is retained if:
$$c_1(x) + \nabla c_1(x)^T s \ge 0$$.

### Case Analysis for Inequality Optimality
*   **Case I (Inactive Constraint)**: If $c_1(x) > 0$, any step $s$ satisfies the feasibility condition if its length is small. A step $s = -\alpha \nabla f(x)$ decreases $f$ whenever $\nabla f(x) \ne 0$.
*   **Case II (Active Constraint)**: If $c_1(x) = 0$, the conditions become $\nabla f(x)^T s < 0$ and $\nabla c_1(x)^T s \ge 0$. No such step $s$ exists only when $\nabla f(x)$ and $\nabla c_1(x)$ point in the same direction:
    $$\nabla f(x) = \lambda_1 \nabla c_1(x), \text{ for some } \lambda_1 \ge 0$$.

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/7438bc87-710f-47b2-a3ea-b3e60ce4a870" />

## 9.1.5 The Lagrangian Function and Complementarity
The Lagrangian function combines the objective and the constraints using Lagrange multipliers:
$$\mathcal{L}(x, \lambda) = f(x) - \sum_{i \in \mathcal{E} \cup \mathcal{I}} \lambda_i c_i(x)$$.
When no first-order feasible descent direction exists at $x^\star$, we have:
$$\nabla_x \mathcal{L}(x^\star, \lambda^\star) = 0 \text{ for some } \lambda_i^\star \ge 0, i \in \mathcal{I}$$.
This is subject to the complementarity condition:
$$\lambda_i^\star c_i(x^\star) = 0 \text{ for all } i \in \mathcal{E} \cup \mathcal{I}$$.
This condition implies that the Lagrange multiplier $\lambda_i$ can be strictly positive only when the corresponding constraint $c_i$ is active.

## 9.1.6 Tangent Cone and Linearized Feasible Directions
*   **Tangent Vector**: A vector $d$ is a tangent to $\Omega$ at $x$ if there are a feasible sequence $\{z_k\}$ approaching $x$ and a sequence of positive scalars $\{t_k\} \to 0$ such that $\lim_{k \to \infty} (z_k - x)/t_k = d$. The set of all tangents is the tangent cone $T_{\Omega}(x)$.
*   **Linearized Feasible Directions**: Given a feasible point $x$, the set $\mathcal{F}(x)$ is:
    $$\mathcal{F}(x) = \{d | d^T \nabla c_i(x) = 0, \forall i \in \mathcal{E}; d^T \nabla c_i(x) \ge 0, \forall i \in \mathcal{A}(x) \cap \mathcal{I} \}$$.
*   **Constraint Qualifications**: These are assumptions, such as the Linear Independence Constraint Qualification (LICQ), which ensure that the linearized approximation $\mathcal{F}(x)$ captures the essential geometric features of the feasible set by making $\mathcal{F}(x) = T_{\Omega}(x)$.







---









# 9.2 Method of Lagrange Multiplier, Projected Gradient Descent

## 9.2.1 The Method of Lagrange Multipliers

The method of Lagrange multipliers is a technique for finding the stationary points of a function subject to one or more constraints. It escapes from linear equations by determining an unknown vector $x$ through a minimum principle.

### The Lagrangian Function

To build constraints into the objective function, we introduce new unknowns $y_1, \dots, y_l$ (or $\lambda_1, \dots, \lambda_m$) called **Lagrange multipliers**. For the general constrained optimization problem:
$$\min_{x \in \mathbb{R}^n} f(x) \quad \text{subject to } c_i(x) = 0, i \in \mathcal{E}; \quad c_i(x) \ge 0, i \in \mathcal{I}$$
The **Lagrangian function** is defined as:
$$\mathcal{L}(x, \lambda) = f(x) - \sum_{i \in \mathcal{E} \cup \mathcal{I}} \lambda_i c_i(x)$$.

> [!NOTE]
> In physical applications, $\frac{1}{2}x^T Ax$ often represents internal energy and $-x^T b$ represents external work. The system automatically goes to a point where the total energy is a minimum.

### First-Order Necessary Conditions (KKT)

If $x★$ is a local solution and the linear independence constraint qualification (LICQ) holds, there exists a Lagrange multiplier vector $\lambda★$ such that the following **Karush-Kuhn-Tucker (KKT)** conditions are satisfied:

1. **Stationarity**: $\nabla_x \mathcal{L}(x★, \lambda★) = \nabla f(x★) - \sum_{i \in \mathcal{A}(x★)} \lambda_i★ \nabla c_i(x★) = 0$.
2. **Primal Feasibility**: $c_i(x★) = 0, i \in \mathcal{E}$ and $c_i(x★) \ge 0, i \in \mathcal{I}$.
3. **Dual Feasibility**: $\lambda_i★ \ge 0, i \in \mathcal{I}$.
4. **Complementary Slackness**: $\lambda_i★ c_i(x★) = 0, i \in \mathcal{E} \cup \mathcal{I}$.

### The Idea Behind Lagrange Multipliers: Sensitivity

Lagrange multipliers represent the **sensitivity** of the optimal objective value $f(x★)$ to the presence of a constraint.

* **Dual Unknowns**: The dual variables $y$ tell how much the constrained minimum $P_{C/min}$ exceeds the unconstrained minimum $P_{min}$:
  $$P_{C/min} = P_{min} + \frac{1}{2}y^T(CA^{-1}b - d) \ge P_{min}$$.
* **Shadow Prices**: In economics, the components of $\lambda★$ are known as shadow prices. They give the rate of change of the minimum cost (the derivative) with respect to changes in the constraint vector $b$.
* **Marginal Cost**: If an inequality constraint is active, a perturbation by $\epsilon$ results in a change to the optimal value:


$$
\frac{df(x★(\epsilon))}{d\epsilon} = -\lambda_i★ ||\nabla c_i(x★)||
$$

<img width="829" height="394" alt="image" src="https://github.com/user-attachments/assets/b37eda5f-9660-42db-9f1c-6a6c578712f9" />

## 9.2.2 Projected Gradient Descent

The gradient projection method is an approach that allows the active set to change rapidly from iteration to iteration, which is particularly efficient for problems with simple bounds on the variables.

### Definition and Framework

For a bound-constrained problem $\min q(x) = \frac{1}{2}x^T Gx + x^T c$ subject to $l \le x \le u$, each iteration consists of two stages:

1. **Stage 1: Cauchy Point Computation**: We search along the steepest descent direction $-g = -(Gx + c)$. Whenever a bound is encountered, the direction is "bent" to stay feasible.
2. **Stage 2: Subspace Minimization**: We explore the face of the feasible box on which the Cauchy point lies by approximately solving a subproblem where the components active at the Cauchy point are fixed.

### The Projected Path

The projection of an arbitrary point $x$ onto the feasible "box" is defined component-wise as:

$$
P(x, l, u)_i = \begin{cases} l_i & \text{if } x_i < l_i \ x_i & \text{if } x_i \in [l_i, u_i] \ u_i & \text{if } x_i > u_i \end{cases}
$$

The piecewise-linear path $x(t)$ starting at $x$ is given by:

$$
x(t) = P(x - tg, l, u)
$$

<img width="534" height="473" alt="image" src="https://github.com/user-attachments/assets/66463cba-e79d-43d1-a0d6-3e81e7ec2965" />

### Identifying the Cauchy Point

The **Cauchy point** $x^c$ is defined as the first local minimizer of the univariate, piecewise-quadratic function $q(x(t))$ for $t \ge 0$.

* **Breakpoints**: We identify values $t_i$ where each component reaches its bound:

$$
\bar{t}_i = \begin{cases} (x_i - u_i)/g_i & \text{if } g_i < 0 \text{ and } u_i < +\infty \ (x_i - l_i)/g_i & \text{if } g_i > 0 \text{ and } l_i > -\infty \ \infty & \text{otherwise} \end{cases}
$$
  
* **Search**: Intervals between sorted breakpoints $[t_{j-1}, t_j]$ are examined. In each interval, the quadratic is $q(x(t)) = f_{j-1} + f'{j-1}\Delta t + \frac{1}{2}f''*{j-1}(\Delta t)^2$. We stop at the first local minimizer found.

### Subspace Minimization and Step Selection

Once $x^c$ is found, we define the active set $\mathcal{A}(x^c)$ and solve:

$$
\min_{x} q(x) \quad \text{subject to } x_i = x_i^c, i \in \mathcal{A}(x^c); \quad l_i \le x_i \le u_i, i \notin \mathcal{A}(x^c)
$$

In large-scale cases, the conjugate gradient method is often used to solve this subspace problem.

### Convergence and the Maratos Effect

For non-linear functions, a line search is performed along $p_k = \hat{x} - x_k$ to satisfy the sufficient decrease condition:

$$
f(x_k + \alpha_k p_k) \le f(x_k) + \eta \alpha_k \nabla f_k^T p_k
$$

> [!IMPORTANT]
> The search direction $p_k$ is a descent direction if $B_k$ is positive definite, because the algorithm ensures $q_k(x_k) > q_k(x^c) \ge q_k(\hat{x})$.

### Summary Table: Lagrange Multipliers vs. Gradient Projection

| Feature           | Lagrange Multipliers                   | Gradient Projection                          |
| :---------------- | :------------------------------------- | :------------------------------------------- |
| **Primary Use**   | Theoretical optimality and sensitivity | Practical algorithm for bound constraints    |
| **Key Mechanism** | Introduces dual variables to objective | Projects descent direction onto feasible set |
| **Stationarity**  | $\nabla_x \mathcal{L} = 0$             | $x - P(x - \nabla f(x), l, u) = 0$           |
| **Movement**      | Shifts between working sets            | Moves along piecewise-linear "bent" paths    |






---





# 9.3 Introduction to Convexity and Properties of Convex Sets

The concept of convexity is fundamental in optimization. Many practical problems possess this property, which generally makes them easier to solve both in theory and practice.

## 9.3.1 Introduction to Convexity and Convex Sets

A set $S \in \mathbb{R}^n$ is a **convex set** if the straight line segment connecting any two points in $S$ lies entirely inside $S$.

### Formal Definition

Formally, for any two points $x \in S$ and $y \in S$ we have:
$$\alpha x + (1 - \alpha)y \in S \text{ for all } \alpha \in$$.

Within Euclidean space $\mathbb{R}^n$, if $x$ and $y$ are any two elements of a subspace $S$, then $\alpha x + \beta y \in S$ for all $\alpha, \beta \in \mathbb{R}$. Subspaces are specific types of sets; for example, $S$ is a subspace of $\mathbb{R}^2$ if it consists of:

1. The whole space $\mathbb{R}^n$.
2. Any line passing through the origin.
3. The origin alone.
4. The empty set.

## 9.3.2 Hyperplanes and Half-spaces

Algebra is about equations, and analysis is often about inequalities. Linear programming is about inequalities but is unquestionably a part of linear algebra.

### Hyperplanes

* **Equations**: Equations give lines and planes.
* **Definition**: Solutions to $a^T x = 0$ give the plane (dimension $n - 1$) perpendicular to $a \ne 0$.
* **n-dimensions**: In $n$ dimensions, the "plane" has dimension $n - 1$.

### Half-spaces

* **Definition**: An inequality divides $n$-dimensional space into a halfspace in which the inequality is satisfied, and a halfspace in which it is not.
* **Example**: For $x + 2y \ge 4$, the boundary between the two halfspaces is the line $x + 2y = 4$ where the inequality is "tight".
* **Separating Hyperplane**: A plane in $\mathbb{R}^n$ that separates a vector $g$ from a cone $K$. If a vector $b$ lies outside a cone, it is separated by a hyperplane perpendicular to a fixed vector $y$. The angle between $y$ and $b$ is greater than $90^\circ$, so $yb < 0$, while the angle between $y$ and every column of $A$ is less than $90^\circ$, so $yA \ge 0$.

<img width="601" height="412" alt="image" src="https://github.com/user-attachments/assets/b086a20b-b1b0-4ebf-a504-b478faf48c2a" />

## 9.3.3 Intersection of Convex Sets

A feasible set is composed of the solutions to a family of linear inequalities like $Ax \ge b$. This is the intersection of $m$ halfspaces.

* **Property**: The intersection of finitely many closed sets is closed.
* **Linear Algebra Context**: Given any set of vectors $a_i \in \mathbb{R}^n, i = 1, 2, \dots, m$, the set $\mathcal{S} = {w \in \mathbb{R}^n \mid a_i^T w = 0, i = 1, 2, \dots, m}$ is a subspace.
* **Non-subspace Intersection**: The set ${w \in \mathbb{R}^n \mid a_i^T w \ge 0, i = 1, 2, \dots, m}$ is not in general a subspace.

## 9.3.4 Convex Combinations and Convex Hulls

### Convex Combination

A convex combination of a finite set of vectors ${x_1, x_2, \dots, x_m}$ in $\mathbb{R}^m$ is any vector $x$ of the form:
$$x = \sum_{i=1}^m \alpha_i x_i$$
where
$$\sum_{i=1}^m \alpha_i = 1, \text{ and } \alpha_i \ge 0 \text{ for all } i = 1, 2, \dots, m$$.

### Convex Hull

The convex hull of ${x_1, x_2, \dots, x_m}$ is the set of all convex combinations of these vectors.

## 9.3.5 Examples of Convex Sets

| Set Type              | Description / Definition                                                                                | Source |
| :-------------------- | :------------------------------------------------------------------------------------------------------ | :----- |
| **Euclidean Ball**    | The unit ball ${y \in \mathbb{R}^n \mid |y|_2 \le 1}$                                                   |        |
| **Polyhedron**        | A set defined by linear equalities and inequalities: ${x \in \mathbb{R}^n \mid Ax=b, Cx \le d}$         |        |
| **Cone**              | A set $\mathcal{F}$ where $x \in \mathcal{F} \Rightarrow \alpha x \in \mathcal{F}$ for all $\alpha > 0$ |        |
| **Ice-cream Cone**    | $\Omega = {x \in \mathbb{R}^3 \mid x_3 \ge 2\sqrt{x_1^2 + x_2^2}}$                                      |        |
| **Feasible Set (LP)** | Intersection of $m+n$ halfspaces ($Ax \ge b$ and $x \ge 0$); it has flat sides                          |        |

### Specific Properties of Cones

* Cones are not necessarily convex. For example, the set ${(x_1, x_2)^T \mid x_1 \ge 0 \text{ or } x_2 \ge 0}$, which encompasses three quarters of the two-dimensional plane, is a cone.
* The cone generated by ${x_1, x_2, \dots, x_m}$ is the set of all vectors $x$ of the form $x = \sum_{i=1}^m \alpha_i x_i$ where $\alpha_i \ge 0$. All cones of this form are convex.

<img width="1296" height="1213" alt="image" src="https://github.com/user-attachments/assets/1c177655-7a3b-47f3-8802-b02b330b7fee" />

### Topological Definitions

* **Affine Hull**: The smallest affine set containing $\mathcal{F}$ (denoted by aff $\mathcal{F}$).
* **Relative Interior**: The interior of a set relative to its affine hull, denoted by ri $\mathcal{F}$. For the ice-cream cone, $ri\mathcal{F} = {x \in \mathbb{R}^3 \mid x_3 > 2\sqrt{x_1^2 + x_2^2}}$.






---



