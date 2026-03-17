# 5.1 Complex Matrices

It is no longer possible to work only with real vectors and real matrices. the basic problem was $Ax=b$, the solution was real when $A$ and $b$ were real. Complex numbers could have been permitted, but would have contributed nothing new. Now they cannot be avoided. A real matrix has real coefficients in $\det(A - \lambda I)$, but the eigenvalues (as in rotations) may be complex.

## 5.1.1 The Vector Space $\mathbb{C}^n$

The space $\mathbb{C}^n$ consists of vectors with $n$ complex components. Addition and matrix multiplication follow the same rules as before.

> [!IMPORTANT]
> Length is computed differently in $\mathbb{C}^n$. In the real case, the vector in $\mathbb{C}^2$ with components $(1, i)$ would have zero length since $1^2 + i^2 = 0$, which is not desirable. The correct length squared is $1^2 + |i|^2 = 2$.

This change to the definition of length forces a series of other changes in the inner product, the transpose, and the definitions of symmetric and orthogonal matrices.

## 5.1.2 Review of Complex Numbers and Conjugates

* **Definitions**: $i$ satisfies the equation $i^2 = -1$. A pure imaginary number is a multiple $ib$, where $b$ is real. A complex number is the sum $a + ib$, where $a$ is the real part and $ib$ is the imaginary part.
* **Addition**: $(a + ib) + (c + id) = (a + c) + i(b + d)$.
* **Multiplication**: $(a + ib)(c + id) = ac + ibc + iad + i^2bd = (ac - bd) + i(bc + ad)$.
* **Complex Conjugate**: The conjugate of $a + ib$ is the number $a - ib$, where the sign of the imaginary part is reversed. It is the mirror image across the real axis. The conjugate is denoted by a bar or a star: $(a + ib)^\star = \overline{a + ib} = a - ib$.
* **Properties of Conjugates**:

  1. The conjugate of a product equals the product of the conjugates: $\overline{(a + ib)(c + id)} = \overline{(a + ib)} \cdot \overline{(c + id)}$.
  2. The conjugate of a sum equals the sum of the conjugates: $\overline{(a + c) + i(b + d)} = \overline{(a + ib)} + \overline{(c + id)}$.
* **Absolute Value**: Multiplying any $a + ib$ by its conjugate $a - ib$ produces a real number $a^2 + b^2 = r^2$. This distance $r$ is the absolute value $|a + ib| = \sqrt{a^2 + b^2}$.
* **Polar Form**: Using trigonometry where $a = r \cos \theta$ and $b = r \sin \theta$, a complex number is written as $a + ib = r(\cos \theta + i \sin \theta) = re^{i\theta}$.
* **Unit Circle**: When $r = 1$, the number is $e^{i\theta} = \cos \theta + i \sin \theta$. As $\theta$ varies from $0$ to $2\pi$, this number circles around zero at a constant radial distance $|e^{i\theta}| = \sqrt{\cos^2 \theta + \sin^2 \theta} = 1$.

<img width="757" height="513" alt="image" src="https://github.com/user-attachments/assets/c80ae71c-253b-4d80-8d91-dce93a6c86a2" />

## 5.1.3  Inner Product and Length in $\mathbb{C}^n$

The complex vector space $\mathbb{C}^n$ contains all vectors $x$ with $n$ complex components $x_j = a_j + ib_j$. Scalar multiplication $cx$ is done with complex numbers $c$. $\mathbb{C}^n$ is a complex vector space of dimension $n$.

* **Length Squared**: Each $x_j^2$ is replaced by its modulus $|x_j|^2$:

$$
|x|^2 = |x_1|^2 + \cdot\cdot\cdot + |x_n|^2
$$

* **Inner Product**: To preserve the connection between length and inner product, the first vector in the inner product is conjugated. The inner product is:

$$
\overline{x}^T y = \overline{x}_1 y_1 + \cdot\cdot\cdot + \overline{x}_n y_n
$$

* **Order Property**: $\overline{y}^T x$ is different from $\overline{x}^T y$; the order of the vectors must be watched.

## 5.1.4 The Conjugate Transpose (Hermitian Transpose)

To condense the notation for conjugating and transposing, a superscript $H$ (or a star) is used to combine both operations. This matrix $\overline{A}^T = A^H = A^\star$ is called "**A Hermitian**".

 **Definition**: The entries of $A^H$ are 
 
 $$
 (A^H)*{ij} = \overline{A*{ji}}
 $$
 
* **Row Vectors**: $x^H$ is the row vector

$$
\begin{bmatrix}
\overline{x}_1 & \cdot\cdot\cdot & \overline{x}_n
\end{bmatrix}
$$

* **Example**:



$$
\begin{bmatrix}
2+i & 3i \\\
4-i & 5 \\\
0 & 0
\end{bmatrix}^H
$$

$$
\begin{bmatrix}
2-i & 4+i & 0 \\\
-3i & 5 & 0
\end{bmatrix}
$$

## Summary of Properties in the Complex Case

1. The inner product of $x$ and $y$ is $x^H y$.
2. Orthogonal vectors have $x^H y = 0$.
3. The squared length of $x$ is $|x|^2 = x^H x = |x_1|^2 + \cdot\cdot\cdot + |x_n|^2$.
4. The conjugate transpose of a product follows the reverse order rule: $(AB)^H = B^H A^H$.

##### Comparison Table: Real vs. Complex
| Property           | Real ($\mathbb{R}^n$)                          | Complex ($\mathbb{C}^n$)                                             |
| :----------------- | :-------------------------------------------- | :------------------------------------------------------------------- |
| **Components**     | $n$ real numbers                              | $n$ complex numbers                                                  |
| **Length Squared** | $x^2 = x_1^2 + \cdots + x_n^2$              | $x^2 = x_1^2 + \cdots + x_n^2$                                 |
| **Transpose**      | $A_{ij}^T = A_{ji}$                           | $A_{ij}^H = \overline{A_{ji}}$                                       |
| **Product Rule**   | $(AB)^T = B^T A^T$                            | $(AB)^H = B^H A^H$                                                   |
| **Inner Product**  | $x^T y = x_1 y_1 + \cdots + x_n y_n$          | $x^H y = \overline{x}_1 y_1 + \cdots + \overline{x}_n y_n$           |
| **Orthogonality**  | $x^T y = 0$                                   | $x^H y = 0$                                                          |
| **Adjoint Rule**   | $(Ax)^T y = x^T (A^T y)$                      | $(Ax)^H y = x^H (A^H y)$                                             |



---
# Random Questions
<img width="711" height="149" alt="image" src="https://github.com/user-attachments/assets/cb0afa3c-18db-40fc-8027-1545def1190f" />
<img width="950" height="245" alt="image" src="https://github.com/user-attachments/assets/1bdf1fe4-d47c-4cd4-b5b3-ed6fb09271de" />
<img width="445" height="232" alt="image" src="https://github.com/user-attachments/assets/65bc3af4-5423-41d0-a223-0d997566a280" />
<img width="295" height="246" alt="image" src="https://github.com/user-attachments/assets/9759e3c1-6584-49a9-9dbc-084c2fc1e569" />
<img width="712" height="376" alt="image" src="https://github.com/user-attachments/assets/32d48a6f-fb13-46c7-806b-6245ddf07013" />
<img width="451" height="358" alt="image" src="https://github.com/user-attachments/assets/8c4015df-4599-48b3-a32a-172d969179d7" />


---



# 5.2 Hermitian Matrices

Symmetric matrices $A = A^T$ are the most important class of real matrices. With complex entries, this idea of symmetry has to be extended. The right generalization is not to matrices that equal their transpose, but to matrices that equal their conjugate transpose. These are the **Hermitian matrices**.

## 5.2.1 Definition and Basic Structure

A complex matrix is Hermitian if $A = A^H$ (also denoted by $A^\star$ ★).

* **Entry-wise Definition**: The diagonal entries must be real; they are unchanged by conjugation. Each off-diagonal entry is matched with its mirror image across the main diagonal, and $a_{ij} = \overline{a_{ji}}$.
* **Example**:

$$
A =
\begin{bmatrix}
2 & 3-3i \\\
3+3i & 5
\end{bmatrix}
= A^H
$$

$3-3i$ is the conjugate of $3+3i$. The diagonal entries $2$ and $5$ are real.

> [!NOTE]
> A real symmetric matrix is certainly Hermitian. For real matrices there is no difference between $A^T$ and $A^H$.

## 5.2.2 Property 1: The Quadratic Form $x^H Ax$

**If $A = A^H$, then for all complex vectors $x$, the number $x^H Ax$ is real**.

**Derivation in the 2 by 2 case with $x = (u, v)$**:

$$
x^H A x =
\begin{bmatrix}
\overline{u} & \overline{v}
\end{bmatrix}
\begin{bmatrix}
2 & 3-3i \\\
3+3i & 5
\end{bmatrix}
\begin{bmatrix}
u \\\
v
\end{bmatrix}
$$

$$
= 2\overline{u}u + 5\overline{v}v + (3-3i)\overline{u}v + (3+3i)u\overline{v}
$$

$$
= \text{real} + \text{real} + (\text{sum of complex conjugates})
$$

**General Proof**:
$(x^H Ax)^H$ is the conjugate of the 1 by 1 matrix $x^H Ax$. Since $(x^H Ax)^H = x^H A^H x^{HH} = x^H Ax$, that number must be real.

## 5.2.3 Property 2: Real Eigenvalues

**If $A = A^H$, every eigenvalue is real**.

**Proof**:
Suppose $Ax = \lambda x$. Multiply by $x^H$:

$$
x^H A x = \lambda x^H x
$$

The left-hand side is real by Property 1. The right-hand side $x^H x = |x|^2$ is real and positive because $x \ne 0$. Therefore, $\lambda = x^H Ax / x^H x$ must be real.

> [!CAUTION]
> If $A$ is real but not symmetric, the eigenvalues might not be real. If $A = A^T$, we can be sure $\lambda$ and $x$ stay real.

#### 5.2.4 Property 3: Orthogonality of Eigenvectors

**Two eigenvectors of a real symmetric matrix or a Hermitian matrix, if they come from different eigenvalues, are orthogonal to one another**.

**Proof**:
Start with $Ax = \lambda_1 x$, $Ay = \lambda_2 y$, and $A = A^H$:

$$
(\lambda_1 x)^H y = (Ax)^H y = x^H A^H y = x^H Ay = x^H (\lambda_2 y)
$$

The outside numbers are $\lambda_1 x^H y = \lambda_2 x^H y$ (since $\lambda$ are real). If $\lambda_1 \ne \lambda_2$, this forces the conclusion that $x^H y = 0$.

## 5.2.5 Example: Finding Complex Eigenvectors

For the Hermitian matrix

$$
A = \begin{bmatrix} 2 & 3-3i \\\ 3+3i & 5 \end{bmatrix}
$$

, the eigenvalues are $\lambda_1 = 8$ and $\lambda_2 = -1$.

1. **For $\lambda_1 = 8$**:



$$
(A-8I)x =
\begin{bmatrix}
-6 & 3-3i \\\
3+3i & -3
\end{bmatrix}
\begin{bmatrix}
x_1 \\\
x_2
\end{bmatrix} =
\begin{bmatrix}
0 \\\
0
\end{bmatrix}
$$



$$
\implies
x =
\begin{bmatrix}
1 \\\
1+i
\end{bmatrix}
$$


2. **For $\lambda_2 = -1$**:

$$
(A+I)y =
\begin{bmatrix}
3 & 3-3i \\\
3+3i & 6
\end{bmatrix}
\begin{bmatrix}
y_1 \\\
y_2
\end{bmatrix} =
\begin{bmatrix}
0 \\\
0
\end{bmatrix}
$$

$$
\implies
y =
\begin{bmatrix}
1-i \\\
-1
\end{bmatrix}
$$

**Verification of Orthogonality**:

$$
x^H y =
\begin{bmatrix}
1 \\\ 1-i
\end{bmatrix}
\begin{bmatrix}
1-i \\\
-1
\end{bmatrix}
= (1-i) - (1-i) = 0
$$

## 5.2.6 The Spectral Theorem for Hermitian Matrices

**Every Hermitian matrix can be diagonalized by a unitary $U$**:

$$
U^{-1}AU = \Lambda \quad \text{or} \quad A = U \Lambda U^H
$$

* The columns of $U$ contain orthonormal eigenvectors of $A$.
* **Spectral Decomposition**: If we multiply columns by rows, $A$ becomes a combination of one-dimensional projections:

$$
A = \lambda_1 x_1 x_1^H + \lambda_2 x_2 x_2^H + \cdot\cdot\cdot + \lambda_n x_n x_n^H
$$

<img width="1687" height="1265" alt="image" src="https://github.com/user-attachments/assets/043a909c-becd-4bd0-b1eb-cd33501efd81" />

> A diagram illustrating the four fundamental subspaces for a singular matrix where the row space and nullspace are perpendicular

#### Comparison Table: Real Symmetric vs. Complex Hermitian

| Property               | Real Symmetric      | Complex Hermitian   |
| :--------------------- | :------------------ | :------------------ |
| **Symmetry Condition** | $A^T = A$           | $A^H = A$           |
| **Eigenvalues**        | Real                | Real                |
| **Eigenvectors**       | Orthonormal $Q$     | Orthonormal $U$     |
| **Diagonalization**    | $A = Q \Lambda Q^T$ | $A = U \Lambda U^H$ |
| **Inner Product**      | $x^T y = 0$         | $x^H y = 0$         |
| **Length Squared**     | $x^2 = x^T x$     | $x^2 = x^H x$     |

---
# Random Questions
<img width="544" height="247" alt="image" src="https://github.com/user-attachments/assets/bfb7cec3-93a7-4936-90ed-d7c050dd80dc" />
<img width="682" height="139" alt="image" src="https://github.com/user-attachments/assets/6ce2a866-8c59-4217-91bb-495deedc1bd5" />
<img width="577" height="205" alt="image" src="https://github.com/user-attachments/assets/ca95fbe2-d91d-4025-88de-10304b4f5b96" />
<img width="353" height="232" alt="image" src="https://github.com/user-attachments/assets/fc2a156d-b971-4d27-8b5e-78578451e869" />
<img width="727" height="131" alt="image" src="https://github.com/user-attachments/assets/c334cb7d-634a-43b5-ac1d-fa943e76e778" />
<img width="356" height="127" alt="image" src="https://github.com/user-attachments/assets/00db7a21-c4e0-4f9b-b900-444b46d6a11a" />

---
