# 5.1: Complex Matrices

## 1. Introduction to Complex Vector Space ($\\mathbb{C}^n$)

The goal is to finally arrive at the most important theorem in linear algebra, the **Spectral Theorem**, in its more general form for complex vector spaces. The complex vector space $\\mathbb{C}^n$ is the complex counterpart of the usual $n$-dimensional Euclidean space $\\mathbb{R}^n$.

### Definitions
*   **Vector Components:** If $(x_1, \\dots, x_n) \\in \\mathbb{C}^n$, then each $x_i$ is a complex number for $i = 1 \\dots n$.
*   **Complex Addition:** $(a+ib) + (c+id) = (a+c) + i(b+d)$.
*   **Complex Multiplication:** $(a+ib)(c+id) = (ac-bd) + i(bc+ad)$.
*   **Complex Conjugate:** The complex conjugate of $(a+ib)$ is $(a-ib)$.
*   **Polar Form:** A complex number can be represented as $re^{i\\theta}$, where $r$ is the length and $\\theta$ is the angle with the real axis.

<img width="595" height="327" alt="image" src="https://github.com/user-attachments/assets/743b0de3-4c9a-4315-9438-2dcb94665ac8" />

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



