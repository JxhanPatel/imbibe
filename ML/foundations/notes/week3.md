# 3.1: Four Fundamental Subspaces

---

## 1. Introduction to Fundamental Subspaces

The goal of this lecture is to understand four fundamental subspaces associated with any matrix $A$. These spaces are critical for understanding the solvability of linear systems and various machine learning methods.

The four subspaces are:

1. **Column Space**: $C(A)$
2. **Null Space**: $N(A)$
3. **Row Space**: $C(A^T)$
4. **Left Null Space**: $N(A^T)$

---

## 2. Column Space C(A)

### Definition

If $A$ is a matrix with columns $u_1, u_2, \dots, u_n$, then the column space of $A$ is the span of these columns. It consists of all linear combinations of the vectors $u_1$ until $u_n$.

### Connection to Ax=b

The column space is directly related to solving the system of linear equations $Ax = b$:

- **Question:** For what $b$ does $Ax = b$ have a solution?
- **Answer:** The system is solvable if and only if $b$ belongs to the column space of $A$ ($b \in C(A)$).

### Example 1

Consider a matrix $A$ where $A$ has 4 rows and 3 columns ($4 \times 3$ matrix).

- The system $Ax = b$ represents 4 equations in 3 unknowns.
- If column 3 is just the sum of column 1 and column 2 ($C_3 = C_1 + C_2$), then $C(A) = \text{span}(C_1, C_2)$.
- In this case, $C(A)$ is a **two-dimensional subspace of R4**.
- Not every vector in $\mathbb{R}^4$ belongs to $C(A)$.

---

## 3. Null Space N(A)

### Definition

The null space of $A$ is the set of all $x$ such that $Ax = 0$.




### Proof: Why is N(A) a Subspace?

To verify $N(A)$ is a subspace, two conditions must be met:

1. **Additive Closure:** If $x_1, x_2 \in N(A)$, then $Ax_1 = 0$ and $Ax_2 = 0$.

   - $A(x_1 + x_2) = Ax_1 + Ax_2 = 0 + 0 = 0$.
   - Thus, $(x_1 + x_2) \in N(A)$.
2. **Scalar Multiplication Closure:** If $x \in N(A)$ and $\alpha$ is a scalar:

   - $A(\alpha x) = \alpha(Ax) = \alpha(0) = 0$.
   - Thus, $\alpha x \in N(A)$.

### Relationship to Solutions

- **If A is invertible:** $N(A)$ contains only the zero vector, and $Ax = b$ has a unique solution.
- **If A is not invertible:** $N(A)$ contains non-trivial vectors. The solutions to $Ax = b$ are of the form:



$$
x = x_p + x_n
$$


Where $Ax_p = b$ (particular solution) and $Ax_n = 0$ (null space solution).

---

## 4. Rank and Nullity

### Definitions

- **Rank (r):** The dimension of the column space $C(A)$. It is equal to the number of pivot columns.
- **Nullity:** The dimension of the null space $N(A)$. It is equal to the number of free variables.

### The Rank-Nullity Theorem

If a matrix $A$ has $n$ columns:



$$
\text{Rank}(A) + \text{Nullity}(A) = n
$$


$$
\text{dim}(C(A)) + \text{dim}(N(A)) = n
$$

---

## 5. Finding the Null Space via Gaussian Elimination

To find the basis for the null space, solve $Ax = 0$ by reducing $A$ to its row echelon form $U$.

### Example 2

Consider matrix $A$:



$$
A = \begin{bmatrix} 1 & 2 & 2 & 2 \\ 2 & 4 & 6 & 8 \\ 3 & 6 & 8 & 10 \end{bmatrix}
$$

1. **Gaussian Elimination** reduces this to:



U =
```math
\begin{bmatrix} 1 & 2 & 2 & 2 \\ 0 & 0 & 2 & 4 \\ 0 & 0 & 0 & 0 \end{bmatrix}
```


2. **Identify Pivots:** Columns 1 and 3 are pivot columns.
3. **Identify Free Variables:** Columns 2 and 4 are free variables.
4. **Solve $Ux=0$:**

   - **Case 1:** Set $x_2=1, x_4=0 \implies x_3=0, x_1=-2$. Vector $u = [-2, 1, 0, 0]^T$.
   - **Case 2:** Set $x_2=0, x_4=1 \implies x_3=-2, x_1=2$. Vector $v = [2, 0, -2, 1]^T$.
5. **Result:** $N(A) = \text{span}(u, v)$. $\text{Rank} = 2$, $\text{Nullity} = 2$. Total columns $n = 4$.

---

## 6. Row Space C($A^T$) and Left Null Space N($A^T$)

### Row Space C($A^T$)

- **Definition:** The column space of $A$ transpose. It is the span of the rows of $A$.
- **Important Fact:** $\text{Column Rank} = \text{Row Rank}$. The dimension of the row space is also $r$.

### Left Null Space N($A^T$)

- **Definition:** The set of all $y$ such that $A^T y = 0$, or equivalently $y^T A = 0$.
- **Interpretation:** It represents the linear combination of rows that results in the zero vector.
- **Dimension:** For an $m \times n$ matrix $A$:



$$
\text{dim}(N(A^T)) = m - r
$$

### Summary Table of Dimensions

| Subspace | Definition | Dimension | Subspace of |
| --- | --- | --- | --- |
| **Column Space** | C(A) | r | $R^m$ |
| **Null Space** | N(A) | n−r | $R^n$ |
| **Row Space** | C($A^T$) | r | $R^n$ |
| **Left Null Space** | N($A^T$) | m−r | $R^m$ |


## 7. Comprehensive Example

Matrix 
```math
A = \begin{bmatrix} 1 & 2 \\ 3 & 6 \end{bmatrix} 
```
($m=2, n=2$)

1. **Column Space:** $C_2 = 2 \times C_1$. $C(A)$ is the line through $[1, 3]^T$. $\text{Rank} (r) = 1$.
2. **Null Space:** Solve $Ax=0$. $-2(C_1) + 1(C_2) = 0$. $N(A)$ is the line through $[-2, 1]^T$. $\text{Nullity} = n - r = 1$.
3. **Row Space:** Span of rows $[1, 2]$. $C(A^T)$ is the line through $[1, 2]^T$. $\text{Dimension} = r = 1$.
4. **Left Null Space:** Solve $y^T A = 0$. $-3(\text{Row } 1) + 1(\text{Row } 2) = 0$. $N(A^T)$ is the line through $[-3, 1]^T$. $\text{Dimension} = m - r = 1$.

---

