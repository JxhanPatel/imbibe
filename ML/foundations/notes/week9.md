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
