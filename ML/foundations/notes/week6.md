# 6.1 Singular Value Decomposition

A great matrix factorization known as the **Singular Value Decomposition (SVD)** joins $LU$ from elimination and $QR$ from orthogonalization. This factorization, denoted as $A = U\Sigma V^T$, allows every matrix (even rectangular ones) to be factored into two orthogonal matrices and a diagonal matrix.,

## 6.1.1 The SVD Theorem

Any $m$ by $n$ matrix $A$ can be factored into:

$$
A = U\Sigma V^T = (\text{orthogonal})(\text{diagonal})(\text{orthogonal})
$$

1. **$U$**: An $m \times m$ orthogonal matrix. Its columns are the eigenvectors of $AA^T$.
2. **$V$**: An $n \times n$ orthogonal matrix. Its columns are the eigenvectors of $A^T A$.
3. **$\Sigma$**: An $m \times n$ diagonal (but rectangular) matrix. The $r$ singular values on its diagonal, $\sigma_1, \dots, \sigma_r$, are the square roots of the nonzero eigenvalues of both $AA^T$ and $A^T A$.,

## 6.1.2 Bases for the Four Fundamental Subspaces

The SVD chooses orthonormal bases for the four fundamental subspaces in an extremely special way.

* **First $r$ columns of $U$**: Provide an orthonormal basis for the column space $C(A)$.
* **Last $m - r$ columns of $U$**: Provide an orthonormal basis for the left nullspace $N(A^T)$.
* **First $r$ columns of $V$**: Provide an orthonormal basis for the row space $C(A^T)$.
* **Last $n - r$ columns of $V$**: Provide an orthonormal basis for the nullspace $N(A)$.

## 6.1.3 Derivation and Proof of the SVD

To prove that any matrix can be factored into $U\Sigma V^T$, we consider the symmetric matrices $A^T A$ and $AA^T$.

**Step 1: Identifying $V$ and $\Sigma**
Since $A = U\Sigma V^T$, we can write:

$$
A^T A =
(V\Sigma^T U^T)
(U\Sigma V^T)
$$

$$
= V\Sigma^T \Sigma V^T
$$

The matrix $A^T A$ is symmetric and its eigenvector matrix is $V$. The diagonal matrix $\Sigma^T \Sigma$ contains the eigenvalues of $A^T A$, which are $\sigma_1^2, \dots, \sigma_r^2$. This confirms that the singular values are the square roots of the eigenvalues of $A^T A$.

**Step 2: Identifying $U$**
Similarly, for $AA^T$:

$$
AA^T =
(U\Sigma V^T)
(V\Sigma^T U^T)
$$

$$
= U\Sigma \Sigma^T U^T
$$

$U$ is the eigenvector matrix for $AA^T$, and the diagonal matrix $\Sigma \Sigma^T$ contains the same $\sigma_1^2, \dots, \sigma_r^2$.

**Step 3: Connecting the Components**
Starting with $A^T A v_j = \sigma_j^2 v_j$, multiply by $A$:

$$
(AA^T)A v_j = \sigma_j^2 A v_j
$$

This demonstrates that $A v_j$ is an eigenvector of $AA^T$. The length of this eigenvector is $\sigma_j$ because:

$$
||A v_j||^2 = v_j^T A^T A v_j = \sigma_j^2 v_j^T v_j = \sigma_j^2
$$

The unit eigenvector is $u_j = A v_j / \sigma_j$, which confirms the relationship $AV = U\Sigma$.

> [!NOTE]
> For positive definite matrices, $\Sigma$ is identical to the eigenvalue matrix $\Lambda$, and $U\Sigma V^T$ is identical to $Q\Lambda Q^T$.

<img width="846" height="546" alt="image" src="https://github.com/user-attachments/assets/50c58c3a-07a6-4d63-9a48-363f86cbe1cb" />


## 6.1.4 Summary of SVD Properties

| Feature             | Description                                                      |
| :------------------ | :--------------------------------------------------------------- |
| **Rank**            | $rank(A) = rank(\Sigma) = r$ (number of nonzero singular values) |
| **Matrix $U$**      | Columns are "left singular vectors" (eigenvectors of $AA^T$)     |
| **Matrix $V$**      | Columns are "right singular vectors" (eigenvectors of $A^T A$)   |
| **Singular Values** | $\sigma_i > 0$ are the diagonal entries of $\Sigma$              |
| **Relation**        | $A v_i = \sigma_i u_i$ for $i \le r$; $A v_i = 0$ for $i > r$    |






# 6.2: Example of SVD

## 6.2.1: Example 1: Rank-1 Matrix

Consider a matrix $A$ with only one column:

$$
A =
\begin{bmatrix}
-1 \\
2 \\
2
\end{bmatrix}
$$

This matrix has rank $r = 1$. The matrix $A^T A$ is the $1 \times 1$ matrix $$, which has the eigenvalue 9. Its square root is the singular value $\sigma_1 = 3$.

The SVD is:

$$
A =
\begin{bmatrix}
-1/3 & 2/3 & 2/3 \\
2/3 & -1/3 & 2/3 \\
2/3 & 2/3 & -1/3
\end{bmatrix}
\begin{bmatrix}
3 \\
0 \\
0
\end{bmatrix}
$$

The columns of $U$ are the unit eigenvectors of $AA^T$.

## 6.2.2 Example 2: Rank-2 Matrix

Consider the matrix:

$$
A =
\begin{bmatrix}
-1 & 1 & 0 \\
0 & -1 & 1
\end{bmatrix}
$$

$$
AA^T =
\begin{bmatrix}
2 & -1 \\
-1 & 2
\end{bmatrix}
$$

with eigenvalues $\lambda_1 = 3$ and $\lambda_2 = 1$. The singular values are $\sqrt{3}$ and $\sqrt{1}$.

The SVD factorization is:

$$
A =
\frac{1}{\sqrt{2}}
\begin{bmatrix}
-1 & 1 \\
1 & 1
\end{bmatrix}
\begin{bmatrix}
\sqrt{3} & 0 & 0 \\
0 & 1 & 0
\end{bmatrix}
\frac{1}{\sqrt{2}}
\begin{bmatrix}
1 & -2 & 1 \\
-1 & 0 & 1 \\
1 & 1 & 1
\end{bmatrix}^T
$$




## 6.2.3 Example: Orthonormal Basis Construction

Starting from 

$$
A = \begin{bmatrix} 1 & 1 & 0 \\\ 0 & 1 & 1 \end{bmatrix}
$$

1. **Find $U$**:

$$
AA^T =
\begin{bmatrix}
2 & 1 \\
1 & 2
\end{bmatrix}
$$

has $\sigma_1^2 = 3$ and $\sigma_2^2 = 1$.

Unit eigenvectors:

$$
u_1 =
\begin{bmatrix}
1/\sqrt{2} \\
1/\sqrt{2}
\end{bmatrix}
$$

$$
u_2 =
\begin{bmatrix}
1/\sqrt{2} \\
-1/\sqrt{2}
\end{bmatrix}
$$

2. **Find $V$**:

$$
A^T A =
\begin{bmatrix}
1 & 1 & 0 \\
1 & 2 & 1 \\
0 & 1 & 1
\end{bmatrix}
$$

has $\sigma_1^2 = 3$ with

$$
v_1 =
\begin{bmatrix}
1/\sqrt{6} \\
2/\sqrt{6} \\
1/\sqrt{6}
\end{bmatrix}
$$

$\sigma_2^2 = 1$ with

$$
v_2 =
\begin{bmatrix}
1/\sqrt{2} \\
0 \\
-1/\sqrt{2}
\end{bmatrix}
$$

and nullvector

$$
v_3 =
\begin{bmatrix}
1/\sqrt{3} \\
-1/\sqrt{3} \\
1/\sqrt{3}
\end{bmatrix}
$$

3. **Bases for Fundamental Subspaces**:

   * $C(A)$: Orthonormal basis is $u_1, u_2$.
   * $N(A)$: Orthonormal basis is $v_3$.
   * $C(A^T)$: Orthonormal basis is $v_1, v_2$.
   * $N(A^T)$: Orthonormal basis is empty (since $AA^T$ is invertible).

> [!NOTE]
> The SVD chooses orthonormal bases for all four fundamental subspaces. The first $r$ columns of $U$ span the column space $C(A)$, the last $m-r$ span the left nullspace $N(A^T)$, the first $r$ columns of $V$ span the row space $C(A^T)$, and the last $n-r$ span the nullspace $N(A)$.




---



# 6.3 Positive Definiteness

Positive definiteness brings the whole course together, linking pivots, determinants, and eigenvalues to the study of minima, maxima, and saddle points.

## 6.3.1 Stationary Points, Minima, Maxima, and Saddle Points

The mathematical problem is to move the second derivative test $F'' > 0$ into $n$ dimensions.

### Stationary Points

A stationary point occurs where the first derivatives of a function vanish. For a function $F(x,y)$, the linear terms in a Taylor series give a necessary condition: to have any chance of a minimum, the first derivatives must vanish at the point.

* **Condition**: $\frac{\partial F}{\partial x} = 0$ and $\frac{\partial F}{\partial y} = 0$.
* **Geometry**: At a stationary point, the surface $z = F(x,y)$ is tangent to a horizontal plane.

### Minima, Maxima, and Saddle Points

* **Local Minimum**: A point $x^\star$ is a local minimizer if there is a neighborhood $N$ of $x^\star$ such that $f(x^\star) \le f(x)$ for all $x$ in $N$.
* **Local Maximum**: If $-f$ has a local minimum, then $f$ has a local maximum.
* **Saddle Point**: A stationary point that is neither a maximum nor a minimum. In two dimensions, this occurs when one direction gives a minimum while another direction gives a maximum (e.g., $f = x^2 - y^2$).


<img width="778" height="531" alt="image" src="https://github.com/user-attachments/assets/7a0132e1-0cb5-41ba-ba66-306f36a3243e" />



## 6.3.2 First and Second Derivative Tests for Quadratic Functions

Every quadratic form $f = ax^2 + 2bxy + cy^2$ has a stationary point at the origin. The second derivatives at the stationary point are decisive.

### The Second Derivative Matrix

The second derivatives fit into a symmetric 2 by 2 matrix $A$. The terms $ax^2$ and $cy^2$ appear on the diagonal, while the cross derivative $2bxy$ is split between the off-diagonal entries.

$$
f(x,y) =
\begin{bmatrix}
x & y
\end{bmatrix}
\begin{bmatrix}
a & b \\
b & c
\end{bmatrix}
\begin{bmatrix}
x \\
y
\end{bmatrix}
$$

### The 2 by 2 Test for a Minimum

For a function of two variables, the quadratic $ax^2 + 2bxy + cy^2$ is positive definite (a minimum) if and only if:

1. $a > 0$
2. $ac > b^2$

> [!IMPORTANT]
> These conditions guarantee $c > 0$. The sign of $b$ is of no importance to the existence of a minimum.

### Summary of 2 by 2 Quadratic Form Behaviors

| Type                      | Conditions                 | Geometry                        |
| :------------------------ | :------------------------- | :------------------------------ |
| **Positive Definite**     | $a > 0$ and $ac - b^2 > 0$ | Bowl opening upward (Minimum)   |
| **Negative Definite**     | $a < 0$ and $ac - b^2 > 0$ | Bowl opening downward (Maximum) |
| **Indefinite**            | $ac - b^2 < 0$             | Saddle point                    |
| **Positive Semidefinite** | $a > 0$ and $ac - b^2 = 0$ | Valley                          |

## 6.3.3 Positive Definiteness and Linear Algebra Connection

A symmetric matrix $A$ is positive definite if $x^T Ax > 0$ for all nonzero real vectors $x$. This definition generalizes to $n$ dimensions.

### The Four Tests for Positive Definiteness

Each of the following is a necessary and sufficient condition for a real symmetric matrix $A$ to be positive definite:

1. **Eigenvalue Test**: All eigenvalues $\lambda_i > 0$.
2. **Determinant Test**: All the upper left submatrices $A_k$ have positive determinants ($det A_k > 0$).
3. **Pivot Test**: All the pivots (without row exchanges) are positive ($d_k > 0$).
4. **Quadratic Form Test**: $x^T Ax > 0$ for all nonzero vectors $x$.

### Connection to Pivots (Completing the Square)

Elimination and completing the square are the same. The pivots are the coefficients outside the squares.

$$
ax^2 + 2bxy + cy^2 =
a\left(x + \frac{b}{a}y\right)^2 + \frac{ac - b^2}{a}y^2
$$

In $n$ dimensions, if $A = LDL^T$, then:

$$
x^T Ax =
(L^T x)^T D (L^T x) =
\sum d_i ( \text{squared terms} )
$$

### The Law of Inertia

For any symmetric matrix $A$, the signs of the pivots agree with the signs of the eigenvalues. The eigenvalue matrix $\Lambda$ and the pivot matrix $D$ have the same number of positive, negative, and zero entries.

### Geometric Connection: Ellipsoids

The equation $x^T Ax = 1$ defines an ellipsoid in $n$ dimensions if $A$ is positive definite.

* **Rotation**: Rotating axes via $y = Q^T x$ (where $Q$ is the matrix of orthonormal eigenvectors) simplifies the equation to:

$$
\lambda_1 y_1^2 + \lambda_2 y_2^2 + \dots + \lambda_n y_n^2 = 1
$$

* **Axis Lengths**: The axes of the ellipsoid have lengths $1/\sqrt{\lambda_1}, \dots, 1/\sqrt{\lambda_n}$ from the center.
* **Direction**: The axes point along the eigenvectors of $A$.

<img width="1600" height="1400" alt="image" src="https://github.com/user-attachments/assets/a098a56e-b255-4bcf-9422-10b5a623cce1" />
<img width="1400" height="1645" alt="image" src="https://github.com/user-attachments/assets/69cecbf5-8daf-48fe-b4ee-72e3ff3d4318" />
