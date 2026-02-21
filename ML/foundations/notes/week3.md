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

If $A$ is a matrix with columns $u_1, u_2, \\dots, u_n$, then the column space of $A$ is the span of these columns. It consists of all linear combinations of the vectors $u_1$ until $u_n$.

### Connection to Ax=b

The column space is directly related to solving the system of linear equations $Ax = b$:

- **Question:** For what $b$ does $Ax = b$ have a solution?
- **Answer:** The system is solvable if and only if $b$ belongs to the column space of $A$ ($b \\in C(A)$).

### Example 1

Consider a matrix $A$ where $A$ has 4 rows and 3 columns ($4 \\times 3$ matrix).

- The system $Ax = b$ represents 4 equations in 3 unknowns.
- If column 3 is just the sum of column 1 and column 2 ($C_3 = C_1 + C_2$), then $C(A) = \\text{span}(C_1, C_2)$.
- In this case, $C(A)$ is a **two-dimensional subspace of R4**.
- Not every vector in $\\mathbb{R}^4$ belongs to $C(A)$.

---

## 3. Null Space N(A)

### Definition

The null space of $A$ is the set of all $x$ such that $Ax = 0$.




### Proof: Why is N(A) a Subspace?

To verify $N(A)$ is a subspace, two conditions must be met:

1. **Additive Closure:** If $x_1, x_2 \\in N(A)$, then $Ax_1 = 0$ and $Ax_2 = 0$.

   - $A(x_1 + x_2) = Ax_1 + Ax_2 = 0 + 0 = 0$.
   - Thus, $(x_1 + x_2) \\in N(A)$.
2. **Scalar Multiplication Closure:** If $x \\in N(A)$ and $\\alpha$ is a scalar:

   - $A(\\alpha x) = \\alpha(Ax) = \\alpha(0) = 0$.
   - Thus, $\\alpha x \\in N(A)$.

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
\\text{Rank}(A) + \\text{Nullity}(A) = n
$$


$$
\\text{dim}(C(A)) + \\text{dim}(N(A)) = n
$$

---

## 5. Finding the Null Space via Gaussian Elimination

To find the basis for the null space, solve $Ax = 0$ by reducing $A$ to its row echelon form $U$.

### Example 2

Consider matrix $A$:



$$
A = \\begin{bmatrix} 1 & 2 & 2 & 2 \\\\ 2 & 4 & 6 & 8 \\\\ 3 & 6 & 8 & 10 \\end{bmatrix}
$$

1. **Gaussian Elimination** reduces this to:



U =
```math
\\begin{bmatrix} 1 & 2 & 2 & 2 \\\\ 0 & 0 & 2 & 4 \\\\ 0 & 0 & 0 & 0 \\end{bmatrix}
```


2. **Identify Pivots:** Columns 1 and 3 are pivot columns.
3. **Identify Free Variables:** Columns 2 and 4 are free variables.
4. **Solve $Ux=0$:**

   - **Case 1:** Set $x_2=1, x_4=0 \\implies x_3=0, x_1=-2$. Vector $u = [-2, 1, 0, 0]^T$.
   - **Case 2:** Set $x_2=0, x_4=1 \\implies x_3=-2, x_1=2$. Vector $v = [2, 0, -2, 1]^T$.
5. **Result:** $N(A) = \\text{span}(u, v)$. $\\text{Rank} = 2$, $\\text{Nullity} = 2$. Total columns $n = 4$.

---

## 6. Row Space C($A^T$) and Left Null Space N($A^T$)

### Row Space C($A^T$)

- **Definition:** The column space of $A$ transpose. It is the span of the rows of $A$.
- **Important Fact:** $\\text{Column Rank} = \\text{Row Rank}$. The dimension of the row space is also $r$.

### Left Null Space N($A^T$)

- **Definition:** The set of all $y$ such that $A^T y = 0$, or equivalently $y^T A = 0$.
- **Interpretation:** It represents the linear combination of rows that results in the zero vector.
- **Dimension:** For an $m \\times n$ matrix $A$:



$$
\\text{dim}(N(A^T)) = m - r
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
A = \\begin{bmatrix} 1 & 2 \\\\ 3 & 6 \\end{bmatrix} 
```
($m=2, n=2$)

1. **Column Space:** $C_2 = 2 \\times C_1$. $C(A)$ is the line through $[1, 3]^T$. $\\text{Rank} (r) = 1$.
2. **Null Space:** Solve $Ax=0$. $-2(C_1) + 1(C_2) = 0$. $N(A)$ is the line through $[-2, 1]^T$. $\\text{Nullity} = n - r = 1$.
3. **Row Space:** Span of rows $[1, 2]$. $C(A^T)$ is the line through $[1, 2]^T$. $\\text{Dimension} = r = 1$.
4. **Left Null Space:** Solve $y^T A = 0$. $-3(\\text{Row } 1) + 1(\\text{Row } 2) = 0$. $N(A^T)$ is the line through $[-3, 1]^T$. $\\text{Dimension} = m - r = 1$.

---


# 3.2: Orthogonal Vectors and Subspaces 

---

### 1. Introduction to Orthogonality

A basis is a set of independent vectors that span a space. Geometrically, it is a set of coordinate axes. A vector space is defined without those axes, but every time we actually compute, we are using them.

The goal in this section is to move from a "random" basis to an **orthogonal basis**. The coordinate axes are perpendicular, which makes the mathematics much simpler.

---

### 2. Orthogonal Vectors

#### 2.1 Definition: Perpendicular Vectors

Two vectors are orthogonal (perpendicular) if their **inner product** is zero.

For vectors $x$ and $y$ in $\\mathbb{R}^n$:



$$
x^T y = x_1 y_1 + x_2 y_2 + \\dots + x_n y_n = 0
$$

#### 2.2 The Pythagorean Theorem

In a right-angled triangle, the square of the hypotenuse is the sum of the squares of the legs. In vector terms, if $x \\perp y$, then:



$$
\\|x + y\\|^2 = \\|x\\|^2 + \\|y\\|^2
$$

**Derivation:**



$$
\\|x + y\\|^2 = (x + y)^T (x + y) = x^T x + x^T y + y^T x + y^T y
$$


If $x$ and $y$ are orthogonal, then $x^T y = 0$ and $y^T x = 0$, leaving:



$$
\\|x + y\\|^2 = \\|x\\|^2 + \\|y\\|^2
$$

---

### 3. Orthogonal Subspaces

#### 3.1 Definition: Orthogonal Subspaces

Two subspaces $V$ and $W$ of a vector space are **orthogonal** if every vector $v$ in $V$ is perpendicular to every vector $w$ in $W$.


$$
v^T w = 0 \\quad \\text{for all } v \\in V \\text{ and all } w \\in W
$$

<img width="250" height="173" alt="image" src="https://github.com/user-attachments/assets/1727d7d6-c96b-4de7-9aec-b6dbb99baec8" />



#### 3.2 Important Distinction

If two subspaces meet at any vector other than the zero vector, they **cannot** be orthogonal. The zero vector is the only vector in the intersection of orthogonal subspaces.

---

### 4. Fundamental Subspaces and Orthogonality

The "Big Picture" of linear algebra relates the four fundamental subspaces through orthogonality.

#### 4.1 The Fundamental Theorem of Orthogonality

1. **Row Space and Nullspace:** The nullspace $N(A)$ and the row space $C(A^T)$ are orthogonal subspaces in $\\mathbb{R}^n$.
2. **Column Space and Left Nullspace:** The left nullspace $N(A^T)$ and the column space $C(A)$ are orthogonal subspaces in $\\mathbb{R}^m$.

#### 4.2 Why the Row Space is Orthogonal to the Nullspace

The equation $Ax = 0$ means that the vector $x$ is perpendicular to every row of $A$.



$$
Ax = \\begin{bmatrix} \\text{row } 1 \\\\ \\vdots \\\\ \\text{row } m \\end{bmatrix} \\begin{bmatrix} x \\end{bmatrix} = \\begin{bmatrix} 0 \\\\ \\vdots \\\\ 0 \\end{bmatrix}
$$


Since $x$ is perpendicular to each row, it is perpendicular to any linear combination of the rows (the row space).

---

### 5. Orthogonal Complements

#### 5.1 Definition

The **orthogonal complement** of a subspace $V$ contains *every* vector that is perpendicular to $V$. It is denoted as $V^\\perp$ ("V-perp").

#### 5.2 Dimensions and the Direct Sum

If $V$ is a subspace of $\\mathbb{R}^n$, then the dimensions of $V$ and its orthogonal complement $V^\\perp$ must add up to $n$:



$$
\\dim(V) + \\dim(V^\\perp) = n
$$


Every vector $x$ in $\\mathbb{R}^n$ can be uniquely split into a piece $v$ in $V$ and a piece $w$ in $V^\\perp$.



$$
x = v + w
$$

---

### 6. Summary Table: The Four Subspaces

| Subspace | Notation | Location | Dimension | Orthogonal Complement |
| --- | --- | --- | --- | --- |
| **Row Space** | C($A^T$) | $R^n$ | r | Nullspace N(A) |
| **Nullspace** | N(A) | $R^n$ | n−r | Row Space C($A^T$) |
| **Column Space** | C(A) | $R^m$ | r | Left Nullspace N($A^T$) |
| **Left Nullspace** | N($A^T$) | $R^m$ | m−r | Column Space C(A) |

### 7. Example: Finding Perpendicular Vectors

**Problem:** Find all vectors perpendicular to $(1, 3, 1)$ and $(2, 7, 2)$.

**Solution:**
Create a matrix $A$ where these vectors are the rows:



$$
A = \\begin{bmatrix} 1 & 3 & 1 \\\\\\ 2 & 7 & 2 \\end{bmatrix}
$$


Solving $Ax = 0$ finds the nullspace, which is the orthogonal complement to the row space.

1. Elimination:

$$
R_2 - 2R_1 \\to \\begin{bmatrix} 1 & 3 & 1 \\\\\\ 0 & 1 & 0 \\end{bmatrix}
$$

3. Back-substitution: $x_2 = 0$ and $x_1 + x_3 = 0$.
4. The nullspace is all multiples of $(-1, 0, 1)$.

## Practise Questions

<img width="508" height="298" alt="image" src="https://github.com/user-attachments/assets/dfb1455f-1112-48d7-a6ba-28f105c3b17c" />

<img width="626" height="141" alt="image" src="https://github.com/user-attachments/assets/52a615c1-5ef5-4a6c-94a8-21e2d829bf9d" />

<img width="383" height="262" alt="image" src="https://github.com/user-attachments/assets/69f9683c-699e-4618-ae5f-20fbb7e4caa4" />

<img width="383" height="253" alt="image" src="https://github.com/user-attachments/assets/c47dce3a-9b4b-474f-b968-89eb2e20e421" />


<img width="404" height="107" alt="image" src="https://github.com/user-attachments/assets/344e7f65-c13f-4254-bc19-09f26f515645" />




---


# 3.3: Projections

## Introduction to Projections and Least Squares

The concept of projections is very, very important because they have a strong connection to least squares problems. The need for projections is motivated using solutions to linear system of equations in the case where they are inconsistent.

### Why Project?
1. **Geometric Goal:** We want to project a vector $b$ onto the line through $x$, or, more generally, onto the column space of a matrix $A$.
2. **Computational Goal:** Given a basis for a subspace $S$ (spanned by the columns of $A$), we seek an easy way to calculate the projection $p$ of $b$ onto $S$.

<img width="354" height="171" alt="image" src="https://github.com/user-attachments/assets/7f731877-835a-4a65-a15c-cfcaa34320e2" />

### Motivation: Inconsistent Systems
Suppose we are given data points $(x_1, b_1), \\dots, (x_n, b_n)$. 
For example:
$$
\\begin{matrix} 2x = b_1 \\\\ 3x = b_2 \\\\ 4x = b_3 \\end{matrix} \\quad \\text{or} \\quad \\begin{matrix} x + 2y = 4 \\\\ x + 3y = 5 \\\\ 2x + 4y = 6 \\end{matrix}
$$
These systems are "inconsistent," meaning there is no solution that satisfies the system of equations.

**Matrix View:**
$Ax = b$ is inconsistent if $b \\notin C(A)$ (meaning $b$ is not in the column space of $A$). 

> [!IMPORTANT]
> In such situations, it makes sense to project $b$ onto $C(A)$.

---

## Projection onto a Line

Consider a vector $b$ and a line through vector $a$.

<img width="217" height="149" alt="image" src="https://github.com/user-attachments/assets/542c6016-b00c-4060-a3cf-8be61c28ba21" />



### Derivation of the Projection
Let $p$ be the projection of $b$ onto the line through $a$. Since $p$ lies on the line, it must be some scalar multiple of $a$:
$$p = \\hat{x}a$$
The error vector $e$ is defined as the difference between the original vector $b$ and its projection $p$:
$$e = b - p = b - \\hat{x}a$$
The fundamental geometric property of a projection is that the error vector $e$ must be perpendicular to the line (vector $a$):
$$e \\perp a$$
This implies that the dot product is zero:
$$(b - \\hat{x}a) \\perp a \\implies a^T(b - \\hat{x}a) = 0$$
Solving for the scalar $\\hat{x}$:
$$a^T b - \\hat{x} a^T a = 0 \\implies \\hat{x} = \\frac{a^T b}{a^T a}$$
The projection $p$ is then:
$$p = \\hat{x}a = \\left( \\frac{a^T b}{a^T a} \\right) a$$

---

## Cauchy-Schwarz Inequality Derivation
The length of the error vector $e$ squared must be greater than or equal to zero:
$$\\|e\\|^2 = \\|b - p\\|^2 \\ge 0$$
Substituting the expression for $p$:
$$\\left\\| b - \\frac{a^T b}{a^T a} a \\right\\|^2 = b^T b - 2\\frac{(a^T b)^2}{a^T a} + \\left( \\frac{a^T b}{a^T a} \\right)^2 a^T a \\ge 0$$
Simplifying the expression:
$$\\frac{(b^T b)(a^T a) - (a^T b)^2}{a^T a} \\ge 0$$
Since $a^T a$ is positive (assuming $a \\neq 0$), this leads to:
$$(b^T b)(a^T a) \\ge (a^T b)^2$$
Taking the square root gives the **Cauchy-Schwarz Inequality**:
$$|a^T b| \\le \\|a\\| \\|b\\|$$

---

## The Projection Matrix ($P$)

The projection $p$ can be rewritten to isolate the vector $b$:
$$p = \\left( \\frac{a^T b}{a^T a} \\right) a = \\left( \\frac{a a^T}{a^T a} \\right) b$$
We define the **Projection Matrix** $P$ as:
$$P = \\frac{a a^T}{a^T a}$$
To project any vector $b$ onto the line through $a$, simply left-multiply $b$ by the projection matrix $P$: $p = Pb$.

### Example: $a = \\begin{bmatrix} 1 \\\\ 1 \\\\ 1 \\end{bmatrix}$
The projection matrix is:
$$P = \\frac{a a^T}{a^T a} = \\frac{1}{3} \\begin{bmatrix} 1 \\\\ 1 \\\\ 1 \\end{bmatrix} \\begin{bmatrix} 1 & 1 & 1 \\end{bmatrix} = \\begin{bmatrix} 1/3 & 1/3 & 1/3 \\\\ 1/3 & 1/3 & 1/3 \\\\ 1/3 & 1/3 & 1/3 \\end{bmatrix}$$

### Properties of the Projection Matrix $P$
1.  **Symmetry:** $P$ is symmetric ($P^T = P$).
2.  **Idempotency:** $P^2 = P$.
    *   *Idea:* Left multiplication by $P$ brings a vector onto the line through $a$. Once a vector $Pb$ is already on that line, another round of projection doesn't change it ($P(Pb) = Pb$).
3.  **Subspaces:**
    *   **Column Space $C(P)$:** The line through $a = \\begin{bmatrix} 1 \\\\ 1 \\\\ 1 \\end{bmatrix}$.
    *   **Null Space $N(P)$:** The plane orthogonal to $a = \\begin{bmatrix} 1 \\\\ 1 \\\\ 1 \\end{bmatrix}$.
4.  **Rank:** $r(P) = 1$.

> [!NOTE]


Scaling the vector $a$ does not change the projection matrix. For example, if

$$

a = \\begin{bmatrix} 2 \\\\ 2 \\\\ 2 \\end{bmatrix}

$$


, the resulting $P$ is identical to the one derived from $a = \\begin{bmatrix} 1 \\\\ 1 \\\\ 1 \\end{bmatrix}$.

---

## Tutorial Examples

### Example 1: Orthogonality in a Set
**Problem:** Let $S = \\{ (1, 2, 4, 0)^T, (-2, 3, -1, 0)^T, (0, 2, 6, -1)^T \\}$. Which pair(s) are orthogonal?
Let $u = (1, 2, 4, 0)^T, v = (-2, 3, -1, 0)^T, w = (0, 2, 6, -1)^T$.

**Solution:**
*   $u \\cdot v = (1)(-2) + (2)(3) + (4)(-1) + (0)(0) = -2 + 6 - 4 = 0$ (Orthogonal)
*   $v \\cdot w = (-2)(0) + (3)(2) + (-1)(6) + (0)(-1) = 0 + 6 - 6 = 0$ (Orthogonal)
*   $u \\cdot w = (1)(0) + (2)(2) + (4)(6) + (0)(-1) = 4 + 24 = 28$ (Not Orthogonal)
**Answer:** $(u, v)$ and $(v, w)$ are orthogonal pairs.

### Example 2: Projection Matrix and Error Calculation
**Problem:** i. Find the projection matrix for
$$

a = \\begin{bmatrix} 2 \\\\ -1 \\\\ 2 \\\\ 3 \\end{bmatrix}

$$

ii. Obtain the projection of $$
b = \\begin{bmatrix} 1 \\\\ 3 \\\\ -2 \\\\ 5 \\end{bmatrix}
$$

onto $a$ and compute the error.

**Solution:**
i. **Projection Matrix ($P$):**
$$
a a^T = \\begin{bmatrix} 2 \\\\ -1 \\\\ 2 \\\\ 3 \\end{bmatrix} \\begin{bmatrix} 2 & -1 & 2 & 3 \\end{bmatrix} = \\begin{bmatrix} 4 & -2 & 4 & 6 \\\\ -2 & 1 & -2 & -3 \\\\ 4 & -2 & 4 & 6 \\\\ 6 & -3 & 6 & 9 \\end{bmatrix}
$$

$$
a^T a = 2^2 + (-1)^2 + 2^2 + 3^2 = 4 + 1 + 4 + 9 = 18
$$

$$
P = \\frac{1}{18} \\begin{bmatrix} 4 & -2 & 4 & 6 \\\\ -2 & 1 & -2 & -3 \\\\ 4 & -2 & 4 & 6 \\\\ 6 & -3 & 6 & 9 \\end{bmatrix} = \\begin{bmatrix} 2/9 & -1/9 & 2/9 & 1/3 \\\\ -1/9 & 1/18 & -1/9 & -1/6 \\\\ 2/9 & -1/9 & 2/9 & 1/3 \\\\ 1/3 & -1/6 & 1/3 & 1/2 \\end{bmatrix}
$$


ii. **Projection ($p$) and Error ($e$):**

$$
p = Pb = \\begin{bmatrix} 10/9 \\\\ -5/18 \\\\ 10/9 \\\\ 5/3 \\end{bmatrix}
$$

$$
e = b - p = \\begin{bmatrix} 1 \\\\ 3 \\\\ -2 \\\\ 5 \\end{bmatrix} - \\begin{bmatrix} 10/9 \\\\ -5/18 \\\\ 10/9 \\\\ 5/3 \\end{bmatrix} = \\frac{1}{18} \\begin{bmatrix} -2 \\\\ 59 \\\\ -56 \\\\ 60 \\end{bmatrix}
$$
