# 5.1: Complex Matrices

## 1. Introduction to Complex Vector Space ($\\mathbb{C}^n$)

The goal is to finally arrive at the most important theorem in linear algebra, the **Spectral Theorem**, in its more general form for complex vector spaces. The complex vector space $\\mathbb{C}^n$ is the complex counterpart of the usual $n$-dimensional Euclidean space $\\mathbb{R}^n$.

### Definitions
*   **Vector Components:** If $(x_1, \\dots, x_n) \\in \\mathbb{C}^n$, then each $x_i$ is a complex number for $i = 1 \\dots n$.
*   **Complex Addition:** $(a+ib) + (c+id) = (a+c) + i(b+d)$.
*   **Complex Multiplication:** $(a+ib)(c+id) = (ac-bd) + i(bc+ad)$.
*   **Complex Conjugate:** The complex conjugate of $(a+ib)$ is $(a-ib)$.
*   **Polar Form:** A complex number can be represented as $re^{i\\theta}$, where $r$ is the length and $\\theta$ is the angle with the real axis.

<img width="757" height="513" alt="image" src="https://github.com/user-attachments/assets/c80ae71c-253b-4d80-8d91-dce93a6c86a2" />

---

## 2. Inner Product and Length in $\\mathbb{C}^n$

In $\\mathbb{R}^n$, the length squared is defined by the inner product $\\|x\\|^2 = x^Tx$. We cannot use this same definition in $\\mathbb{C}^n$ because it leads to non-intuitive results.

### Why the Real Definition Fails
Consider the vector

$$
x = \\begin{bmatrix} 1 \\\\ i \\end{bmatrix}
$$

Using the real definition:
$$\\|x\\|^2 = 1^2 + i^2 = 1 - 1 = 0$$
This results in a non-zero vector having zero length, which is not good.

### New Definition of Inner Product
In $\\mathbb{C}^n$, the inner product between $x$ and $y$ is defined as:
$$x \\cdot y = \\bar{x}^T y = \\bar{x}_1 y_1 + \\dots + \\bar{x}_n y_n$$
This involves taking the **conjugate** of the first vector.

> [!NOTE]
> In the complex case, the inner product is not commutative: $\\bar{x}^T y \\neq \\bar{y}^T x$.
> Specifically: $x \\cdot y = \\overline{y \\cdot x}$.

### Definition of Length
For $x \\in \\mathbb{C}^n$, the length squared is:

$$
\\|x\\|^2 = x^★ x = \\bar{x}^T x
$$

Using this definition for 

$$
x = \\begin{bmatrix} 1 \\\\ i \\end{bmatrix}:
$$

$$
\\|x\\|^2 = \\begin{bmatrix} 1 & -i \\end{bmatrix} \\begin{bmatrix} 1 \\\\ i \\end{bmatrix} = 1 - i^2 = 1 + 1 = 2
$$

Thus, $\\|x\\| = \\sqrt{2} \\neq 0$.

---

## 3. The Conjugate Transpose ($A^★$)

The **conjugate transpose** (also called "A Hermitian") is denoted by $A^★$ (or $A^H$ in some texts). It is obtained by taking the transpose of the matrix and then taking the conjugate of every entry (the order of these operations is interchangeable).

### Mathematical Definition
$$A^★ = \\bar{A}^T$$
If $A$ is an $m \\times n$ matrix, then $A^★$ is $n \\times m$.

**Example:**

$$
A = \\begin{bmatrix} 1+i & 3-2i \\\\ 2-4i & i \\end{bmatrix} \\implies A^★ = \\begin{bmatrix} 1-i & 2+4i \\\\ 3+2i & -i \\end{bmatrix}
$$

### Properties of $A^★$
1.  $(A^★)^★ = A$.
2.  $(AB)^★ = B^★ A^★$.
3.  For a real matrix $A$, $A^★ = A^T$.

---

## 4. Special Classes of Complex Matrices

### Hermitian Matrices
A matrix $A$ is **Hermitian** if it equals its own conjugate transpose:
$$A^★ = A$$
Hermitian matrices are the complex equivalent of symmetric matrices.

> [!IMPORTANT]
> **Observation:** The diagonal entries of a Hermitian matrix must be **real numbers** because they must equal their own conjugates.

### Unitary Matrices
A square matrix $U$ is **Unitary** if it has orthonormal columns. This is the complex counterpart of an orthogonal matrix.
$$U^★ U = I \\iff U^{-1} = U^★$$

---

## 5. Tutorial Examples and Applications

### Example 1: Visualizing Complex Vectors
Visualization of a vector from $\\mathbb{C}^2$ (e.g., $x = [1+i, 2-i]^T$) would require **4 dimensions** (2 for the real parts, 2 for the imaginary parts).

### Example 2: Verifying a Unitary Matrix

Show that 


$$
U = \\begin{bmatrix} \\cos t & -\\sin t \\\\ \\sin t & \\cos t \\end{bmatrix} 
$$
is unitary.
$$
U^★ = U^T = \\begin{bmatrix} \\cos t & \\sin t \\\\ -\\sin t & \\cos t \\end{bmatrix}
$$



$$
U^★ U = \\begin{bmatrix} \\cos^2 t + \\sin^2 t & \\cos t \\sin t - \\sin t \\cos t \\\\ \\sin t \\cos t - \\cos t \\sin t & \\sin^2 t + \\cos^2 t \\end{bmatrix} = \\begin{bmatrix} 1 & 0 \\\\ 0 & 1 \\end{bmatrix} = I
$$


### Example 3: Real World Application
Complex matrices are fundamental to the **Discrete Fourier Transform (DFT)**, which has countless applications in signal processing, digital communication, and speech processing.

<img width="809" height="367" alt="image" src="https://github.com/user-attachments/assets/8324b2b3-bbe6-4815-aeae-2a38b7e7357b" />



---





# 5.2: Hermitian Matrices

## 1. Definition of a Hermitian Matrix

The concept of symmetry in real matrices ($A = A^T$) is extended to complex matrices through the notion of a **Hermitian matrix**.

### The Conjugate Transpose ($A^★$)

Before defining the matrix, we recall the **conjugate transpose** (also denoted as $A^H$ or $A^★$):

* **Definition:** $A^★ = \bar{A}^T$.
* It is obtained by taking the transpose of $A$ and then the complex conjugate of every entry.
* **Example:**

$$
A =
\begin{bmatrix}
2+i & 3i \\\
4-i & 5 \\\
0 & 0
\end{bmatrix}
$$

$$
A^★ =
\begin{bmatrix}
2-i & 4+i & 0 \\\
-3i & 5 & 0
\end{bmatrix}
$$


### Formal Definition

A matrix $A$ is **Hermitian** if it equals its own conjugate transpose:

$$
A^★ = A
$$

> [!NOTE]
> **Key Observation:** The diagonal entries of a Hermitian matrix must be **real numbers** because they are unchanged by conjugation ($a_{ii} = \bar{a}_{ii}$).

**Example of a Hermitian Matrix:**

$$
A =
\begin{bmatrix}
2 & 3-3i \\\
3+3i & 5
\end{bmatrix}
$$

In this case, $a_{12} = 3-3i$ is the conjugate of $a_{21} = 3+3i$, and the diagonal entries (2 and 5) are real.

---

## 2. Fundamental Properties of Hermitian Matrices

There are three basic properties of Hermitian matrices that also apply to real symmetric matrices.

### Property 1: Real Quadratic Form

If $A = A^★$, then for all complex vectors $x$, the number $x^H Ax$ is real.

* **Proof:** $(x^H Ax)^H = x^H A^H x^{HH} = x^H Ax$. Since the number equals its own conjugate, it must be real.

### Property 2: Real Eigenvalues

**Theorem:** If $A$ is Hermitian, every eigenvalue $\lambda$ is a real number.

**Derivation/Proof:**

1. Suppose $Ax = \lambda x$ with $x \neq 0$.
2. Multiply both sides by $x^H$: $x^H Ax = \lambda x^H x$.
3. The left side $x^H Ax$ is real by Property 1.
4. The right side $x^H x = |x|^2$ is real and positive because $x \neq 0$.
5. Therefore, $\lambda = \frac{x^H Ax}{x^H x}$ must be real.

**Example Calculation:**

$$
A =
\begin{bmatrix}
2 & 3-3i \\\
3+3i & 5
\end{bmatrix}
$$

$$
\det(A - \lambda I) =
\begin{vmatrix}
2-\lambda & 3-3i \\\
3+3i & 5-\lambda
\end{vmatrix}
$$

$$
= \lambda^2 - 7\lambda + 10 - |3-3i|^2
$$

$$
= \lambda^2 - 7\lambda - 8 = (\lambda - 8)(\lambda + 1)
$$

The eigenvalues are $\lambda_1 = 8$ and $\lambda_2 = -1$, which are real.


### Property 3: Orthogonal Eigenvectors

**Theorem:** Two eigenvectors of a Hermitian matrix, if they come from different eigenvalues, are orthogonal to one another ($x^H y = 0$).

**Derivation/Proof:**

1. Let $Ax = \lambda_1 x$ and $Ay = \lambda_2 y$ with $\lambda_1 \neq \lambda_2$.
2. Consider the inner product $(\lambda_1 x)^H y = (Ax)^H y = x^H A^H y = x^H Ay = x^H (\lambda_2 y)$.
3. This gives $\bar{\lambda}_1 x^H y = \lambda_2 x^H y$.
4. Since $\lambda_1$ is real, $\bar{\lambda}_1 = \lambda_1$. Thus, $\lambda_1(x^H y) = \lambda_2(x^H y)$.
5. Since $\lambda_1 \neq \lambda_2$, it forces $x^H y = 0$.

**Example continued:**

$$
x =
\begin{bmatrix}
1 \\
1+i
\end{bmatrix}
\quad
y =
\begin{bmatrix}
1-i \\\
-1
\end{bmatrix}
$$

**Check orthogonality:**

$$
x^H y =
\begin{bmatrix}
1 \\1-i
\end{bmatrix}
\begin{bmatrix}
1-i \\\
-1
\end{bmatrix}
$$

$$
= (1-i) - (1-i) = 0
$$

---

## 3. Diagonalization and the Spectral Theorem

### Unitary Diagonalizability

A matrix $A$ is **unitarily diagonalizable** if there exists a unitary matrix $U$ (where $U^H U = I$) and a diagonal matrix $\Lambda$ such that:

$$
A = U\Lambda U^★
$$

> [!IMPORTANT]
> **The Spectral Theorem:** Every Hermitian matrix is unitarily diagonalizable. Its orthonormal eigenvectors are the columns of $U$ and its real eigenvalues are on the diagonal of $\Lambda$.

**Example of Diagonalized Form:**
For the matrix $A$ used above:

$$
A =
\begin{bmatrix}
1 & 1-i \\\
1+i & -1
\end{bmatrix}
\begin{bmatrix}
8 & 0 \\\
0 & -1
\end{bmatrix}
\begin{bmatrix}
1 & 1-i \\\
1+i & -1
\end{bmatrix}^{-1}
$$

*(Note: Vectors in the matrix above must be normalized to unit length to make the matrix unitary)*

### Converse Remark

* **Hermitian $\implies$ Unitarily Diagonalizable** is true.
* **Unitarily Diagonalizable $\implies$ Hermitian** is **not** always true.
* **Example:**

$$
A =
\begin{bmatrix}
0 & -1 \\\
1 & 0
\end{bmatrix}
$$

is unitarily diagonalizable (it has distinct eigenvalues $i$ and $-i$) but it is not Hermitian ($A^★ \neq A$).
