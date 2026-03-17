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





