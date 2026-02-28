# 4.1 Linear and Polynomial Regression


## 1. Introduction: What is Linear Regression?

Linear Regression is a fundamental machine learning method used to find the **best fit line (or hyperplane)** for a given dataset.

Given data:

$$
{(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)}
$$

* $x_i \in \mathbb{R}^d$ → feature vector
* $y_i \in \mathbb{R}$ → scalar output

The goal is to learn parameters $\theta$ such that predictions are as close as possible to actual values.

---

### Geometric Interpretation

Linear regression is equivalent to solving a **least squares problem**.

* If $A\theta = Y$ has an exact solution → perfect fit.
* If the system is inconsistent → we compute the **projection of $Y$ onto the column space of $A$**.
* This projection gives the **best approximation** in the least squares sense.

Thus, regression = **projection problem in linear algebra**.

---

## 2. Linear Model Formulation

### Single Feature (Straight Line)

Model:

<img width="787" height="247" alt="image" src="https://github.com/user-attachments/assets/5d9621a4-a27a-4d8f-b73c-8857b059f1e8" />

* $m$ → slope
* $c$ → intercept

Example (Housing price):

* 2 rooms → 200k
* 3 rooms → 300k
* 4 rooms → 400k

---

### Multiple Features (General Linear Model)

For $d$ features:

$$
y = \theta_0 + \theta_1 x_1 + \dots + \theta_d x_d
$$

Compact vector form:

$$
y = x^T \theta
$$

Where:

* $\theta = [\theta_0, \theta_1, \dots, \theta_d]^T$
* $x = [1, x_1, \dots, x_d]^T$

---

## 3. Error and Loss Function

Since exact fit is rare, we minimize error.

### Error for one data point

$$
e_i = (x_i^T \theta - y_i)
$$

### Squared Error

$$
(x_i^T \theta - y_i)^2
$$

### Total Loss (Least Squares Objective)

$$
L(\theta) = \frac{1}{2} \sum_{i=1}^{n} (x_i^T \theta - y_i)^2
$$

The $\frac{1}{2}$ simplifies derivatives.

---

## 4. Matrix Formulation

Define:

### Feature Matrix

$$
A =
\begin{bmatrix}
x_1^T \\
x_2^T \\
\vdots \\
x_n^T
\end{bmatrix}
$$

### Label Vector

$$
Y =
\begin{bmatrix}
y_1 \\
y_2 \\
\vdots \\
y_n
\end{bmatrix}
$$

Then loss becomes:

$$
L(\theta) = \frac{1}{2}(A\theta - Y)^T(A\theta - Y)
$$

---

## 5. Derivation of Least Squares Solution

To minimize:

$$
\nabla_\theta L(\theta) = 0
$$

This gives the **Normal Equation**:

$A^TA)θ\=A^TY$

If $A$ is full rank:

$$
\theta = (A^T A)^{-1} A^T Y
$$

This is the **Least Squares Solution**.

---

### Key Insight

The same equation arises from:

* Calculus (derivatives)
* Linear algebra (orthogonal projection)

Both approaches lead to identical normal equations.

---

## 6. Maximum Likelihood Interpretation

Assume data follows:

$$
y = \theta^T x + \epsilon
$$

Where noise:

* $\epsilon \sim \mathcal{N}(0, 1/\beta)$

Likelihood:

$$
\mathcal{L}(\theta) = \prod_{i=1}^{n}
\frac{\sqrt{\beta}}{\sqrt{2\pi}}
\exp\left(-\frac{\beta}{2}(y_i - \theta^T x_i)^2\right)
$$

Maximizing log-likelihood is equivalent to minimizing:

$$
\frac{1}{2} \sum (y_i - \theta^T x_i)^2
$$

> **Conclusion:**
> Least Squares = Maximum Likelihood Estimation under Gaussian noise.

---

## 7. Polynomial Regression

Linear regression fits a straight line.
Polynomial regression fits a curve by transforming features.

### General Polynomial (degree $m$)

$$
y = \theta_0 + \theta_1 x + \theta_2 x^2 + \dots + \theta_m x^m
$$

Define transformed features:

$$
\phi(x) = (1, x, x^2, \dots, x^m)
$$

Then:

$$
\hat{y}(x) = \theta^T \phi(x)
$$

Important:

> Polynomial regression is still **linear in parameters**.

---

### Example (Quadratic Fit)

Given:

| x | y |
| - | - |
| 1 | 3 |
| 2 | 4 |
| 3 | 8 |

Matrix becomes:


$$
A =
\begin{bmatrix}
1 & 1 & 1 \\
1 & 2 & 4 \\
1 & 3 & 9
\end{bmatrix}
,
\quad
\theta =
\begin{bmatrix}
a \\
b \\
c
\end{bmatrix}
$$


We again solve:

$$
(A^T A)\theta = A^T Y
$$

Same framework — different features.

---

## 8. Regularized Linear Regression (Ridge Regression)

To prevent overfitting, add regularization.

### Regularized Loss

$$
\bar{L}(\theta) =
\frac{1}{2}\sum_{i=1}^{n}(x_i^T\theta - y_i)^2
+
\lambda |\theta|^2
$$

Taking derivative gives:

$$
(A^T A + \lambda I)\theta = A^T Y
$$

Solution:

$$
\theta = (A^T A + \lambda I)^{-1} A^T Y
$$

### Role of $\lambda$

* Small $\lambda$ → risk of overfitting
* Large $\lambda$ → underfitting
* Ensures matrix invertibility
* Controls model complexity

---

## 9. Summary Table

| Concept               | Meaning                                   |
| --------------------- | ----------------------------------------- |
| Parameters ($\theta$) | Weights learned by model                  |
| Features              | Input variables                           |
| Squared Error         | $(\hat{y}-y)^2$                           |
| Least Squares         | Minimizes total squared error             |
| Normal Equation       | $(A^T A)\theta = A^T Y$                   |
| Polynomial Regression | Linear regression on transformed features |
| Ridge Regression      | Adds $\lambda |\theta|^2$ penalty         |

---

## Final Big Picture

* Linear Regression = Solve least squares.
* Polynomial Regression = Feature transformation + least squares.
* Ridge Regression = Least squares + regularization.
* Least Squares = Maximum Likelihood under Gaussian noise.
* Geometrically = Projection onto column space of $A$.

Everything reduces to solving structured linear systems efficiently.



---


#  4.2: Eigenvalues and Eigenvectors



---


## 1. Introduction to Eigenvalues and Eigenvectors


Eigenvalues and eigenvectors are fundamental concepts used for simplifying mathematics for machines, specifically in areas like **Dimensionality Reduction** .


The concept of eigenvalues and eigenvectors begins the "second half" of linear algebra. While the first half was about $Ax = b$, the new problem $Ax = \lambda x$ is solved by simplifying a matrix—making it diagonal if possible. Eigenvalues and eigenvectors appear naturally and automatically when solving systems of differential equations such as:



$$
\frac{du}{dt} = Au,
$$


---


### Why do we care?


Eigenvalues help us understand the solutions to ordinary differential equations (ODEs).


---


### Example: A Coupled System


Consider the coupled pair of equations:



$$
\frac{dv}{dt} = 4v - 5w, \quad v=8 \text{ at } t=0
$$



$$
\frac{dw}{dt} = 2v - 3w, \quad w=5 \text{ at } t=0
$$



This can be rewritten in matrix form as:



$$
\frac{du}{dt} = Au,
$$


where



$$
A =
\begin{bmatrix}
4 & -5 \\
2 & -3
\end{bmatrix}
\quad \text{and} \quad
u(0) =
\begin{bmatrix}
8 \\
5
\end{bmatrix},
$$


To solve this, we look for "pure exponential solutions" of the form:



$$
u(t) = e^{\lambda t}x,
$$


Substituting this into $\frac{du}{dt} = Au$ yields:



$$
\lambda e^{\lambda t}x = Ae^{\lambda t}x
$$


Canceling $e^{\lambda t}$ produces the fundamental eigenvalue equation:



$$
Ax = \lambda x,
$$


>
> [!IMPORTANT]
>
> **Bottom Line:** We can solve $\frac{du}{dt}=Au$ using solutions of the form $u(t)=e^{\lambda t}x$ if the equation $Ax = \lambda x$ can be solved.
>
>
>


---


## 2. Definition


Given a **square matrix A** ($n \times n$ matrix):


A vector $v$ is an **eigenvector** of $A$ if multiplying $v$ by $A$ results in a scaled version of $v$. In other words, the direction of $v$ remains unchanged 


### Mathematical Expression



$$
A \cdot v = λ \cdot v
$$


Where:


- **A**: An $n \times n$ square matrix.
- **v**: The **Eigenvector**.
- **λ**: The **Eigenvalue** (a scalar)  


The scalar λ is the eigenvalue corresponding to the eigenvector $v$. It represents the factor by which the vector is scaled or expanded during the transformation  
<img width="1143" height="509" alt="image" src="https://github.com/user-attachments/assets/7241c774-c1b3-4cf4-90e1-a527fd348d3f" />


---



## 3. Special Case: λ=0


If zero is an eigenvalue, then:



$$
Ax = 0x,
$$


which means



$$
Ax = 0,
$$


In this case, the eigenvector $x$ is in the **nullspace** of $A$ ($x \in N(A)$). A zero eigenvalue signals that the matrix $A$ is singular (not invertible).


---


## 4. Calculation of Eigenvalues and Eigenvectors


The equation $Ax = λ x$ is nonlinear because λ multiplies $x$. To solve it, we rewrite it as:



$$
(A - λ I)x = 0,
$$


---


### Step 1: Finding Eigenvalues


For a non-zero eigenvector $x$ to exist, the matrix $(A - \lambda I)$ must be **singular**. This is true if and only if its determinant is zero:



$$
\det(A - \lambda I) = 0,
$$


This equation is known as the **characteristic equation**  


For an $n \times n$ matrix, this determinant is a polynomial of degree $n$, called the **characteristic polynomial**. The $n$ roots of this polynomial are the eigenvalues of $A$ 


---


### Step 2: Finding Eigenvectors


For each eigenvalue $\lambda$, we solve the linear system:



$$
(A - \lambda I)x = 0,
$$


Since the determinant is zero, there are non-trivial solutions (vectors in the nullspace). These are found using Gaussian elimination 

---


## 5. Example 1: Finding λ and v


Matrix $A$:




$$
A =
\begin{bmatrix}
2 & 2 \\
1 & 3
\end{bmatrix}
$$



## 1. Find Eigenvalues



$$
\det
\begin{bmatrix}
2-\lambda & 2 \\
1 & 3-\lambda
\end{bmatrix}
= 0
$$



$$
(2-\lambda)(3-\lambda) - (2)(1) = 0
$$



$$
\lambda^2 - 5\lambda + 6 - 2 = 0
$$



$$
\lambda^2 - 5\lambda + 4 = 0
$$


**Eigenvalues:**



$$
\lambda_1 = 1, \quad \lambda_2 = 4
$$


 

---


### 2. Find Eigenvectors for λ1 = 1 



$$
(A - 1I)v = 0
$$




$$
\begin{bmatrix}
1 & 2 \\
1 & 2
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2
\end{bmatrix} =
\begin{bmatrix}
0 \\
0
\end{bmatrix}
$$ 



$$
x_1 + 2x_2 = 0
$$



$$
x_1 = -2x_2
$$


Vector $v_1$:




$$
\text{Span}
\begin{bmatrix}
-2 \\
1
\end{bmatrix}
$$


 

---


## 6. Example 2: Calculation for a 2×2  Matrix


Let:



$$
A =
\begin{bmatrix}
3 & 1 \\
1 & 3
\end{bmatrix}
$$


1. **Characteristic Equation**



$$
\det(A - \lambda I) = \lambda^2 - 6\lambda + 8 = 0,
$$


1. **Eigenvalues**



$$
\lambda_1 = 4, \quad \lambda_2 = 2,
$$


Check:


Trace:



$$
3+3 = 4+2 = 6
$$


Determinant:



$$
(3)(3)-(1)(1) = (4)(2) = 8
$$


1. **Eigenvectors for λ1 = 4**




$$
(A - 4I)x =
\begin{bmatrix}
-1 & 1 \\
1 & -1
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2
\end{bmatrix}
= 0
$$




$$
x_1 =
\begin{bmatrix}
1 \\
1
\end{bmatrix}
$$


1. **Eigenvectors for λ2 = 2**



$$
(A - 2I)x =
\begin{bmatrix}
1 & 1 \\
1 & 1
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2
\end{bmatrix}
= 0
$$



$$
x_2 =
\begin{bmatrix}
1 \\
-1
\end{bmatrix}
$$



---


## 7. Example 3: Projection Matrix P


Consider a matrix $P$ that projects vectors onto a plane.


1. If $x$ is in the plane:



$$
Px = x
$$


Thus, **λ=1** is an eigenvalue, and every vector in the plane is an eigenvector.


1. If $x$ is perpendicular to the plane:



$$
Px = 0
$$


Thus, **λ=0** is an eigenvalue, and every vector orthogonal to the plane is an eigenvector.


*The eigenvalues of a projection matrix are always 1 or 0.*




---


## 8. Example 4: Permutation Matrix B


Let:




$$
B =
\begin{bmatrix}
0 & 1 \\
1 & 0
\end{bmatrix}
$$


1. If:



$$
x =
\begin{bmatrix}
1 \\
1
\end{bmatrix}
$$



$$
Bx =
\begin{bmatrix}
1 \\
1
\end{bmatrix}
$$


So **λ=1**.


1. If:



$$
x =
\begin{bmatrix}
1 \\
-1
\end{bmatrix}
$$



$$
Bx =
\begin{bmatrix}
-1 \\
1
\end{bmatrix} =
-1
\begin{bmatrix}
1 \\
-1
\end{bmatrix}
$$


So **λ = −1**.



---


## 9. Example 5: Triangular Matrix


If $A$ is triangular:



$$
\det(A - \lambda I) =
\begin{vmatrix}
1-\lambda & 4 & 5 \\
0 & 3/4-\lambda & 6 \\
0 & 0 & 1/2-\lambda
\end{vmatrix} =
(1-\lambda)(3/4-\lambda)(1/2-\lambda)
$$


The eigenvalues are exactly the entries on the **main diagonal**:



$$
1, \quad \frac{3}{4}, \quad \frac{1}{2}
$$


---


## 10. Example 6: Defective Matrix


Let:



$$
A =
\begin{bmatrix}
3 & 1 \\
0 & 3
\end{bmatrix}
$$


The eigenvalues are:



$$
\lambda_1 = \lambda_2 = 3
$$


Solving:



$$
(A - 3I)x =
\begin{bmatrix}
0 & 1 \\
0 & 0
\end{bmatrix}
\begin{bmatrix}
x_1 \\
x_2
\end{bmatrix}
= 0
$$



$$
x =
\begin{bmatrix}
1 \\
0
\end{bmatrix}
$$


There is **no other eigenvector** that is linearly independent of $x$.


---


## 11. Fundamental Properties


## Trace Property


The sum of the $n$ eigenvalues equals the sum of the $n$ diagonal entries:



$$
\lambda_1 + \dots + \lambda_n = a_{11} + \dots + a_{nn}
$$


### Determinant Property


The product of the $n$ eigenvalues equals the determinant:



$$
\lambda_1 \cdot \lambda_2 \cdot \dots \cdot \lambda_n = \det(A)
$$


### Linear Independence Property


Eigenvectors corresponding to different eigenvalues of a matrix are linearly independent.


### Powers Property


If $\lambda$ is an eigenvalue of $A$, then $\lambda^k$ is an eigenvalue of $A^k$.


Derivation:



$$
A^2x = A(Ax) = A(\lambda x) = \lambda(Ax) = \lambda(\lambda x) = \lambda^2 x
$$

Example:
<img width="1310" height="795" alt="image" src="https://github.com/user-attachments/assets/e1423bce-db7d-4dda-ad7f-cae1fd1e16f9" />

 

---


## 12. Real and Complex Eigenvalues


- **Symmetric Matrices:** A real symmetric matrix always has **real eigenvalues**.
- **Complex Eigenvalues:** If a matrix is not symmetric (e.g., rotation matrix



$$
Q =
\begin{bmatrix}
0 & -1 \\
1 & 0
\end{bmatrix}
$$


), the eigenvalues and eigenvectors may be **complex**.


---


## 13. Diagonalization


A matrix $A$ is **diagonalizable** if there exists an invertible matrix $S$ such that:



$$
S^{-1}AS = λ (or D) 
$$


Where $D$ is a **Diagonal Matrix** consisting of eigenvalues  


### Construction of X and D


1. **Matrix X (or S)**: A matrix where columns are the eigenvectors of $A$.



$$
X = [x_1, x_2, \dots, x_n]
$$


1. **Matrix D**: A diagonal matrix where the diagonal elements are the corresponding eigenvalues.



$$
D =
\begin{bmatrix}
\lambda_1 & 0 & \dots \\
0 & \lambda_2 & \dots \\
\vdots & \vdots & \ddots
\end{bmatrix}
$$


**Note:** The order of eigenvectors in $X$ must strictly match the order of eigenvalues in $D$  

<img width="1411" height="769" alt="image" src="https://github.com/user-attachments/assets/6416c345-09b4-476a-8ce0-bb8346a758fc" />


Example:
<img width="990" height="538" alt="image" src="https://github.com/user-attachments/assets/d2d38726-f551-40c1-bb48-d07b480a9f15" />
<img width="777" height="500" alt="image" src="https://github.com/user-attachments/assets/8860ae37-65fd-4621-804e-74ce7f9c1f25" />
<img width="944" height="299" alt="image" src="https://github.com/user-attachments/assets/b5fed3d0-1d72-4824-a558-1425a01427f6" />
<img width="872" height="474" alt="image" src="https://github.com/user-attachments/assets/6cde9a40-e3f9-4b47-a79a-65bfa9ef064b" />

---


### Conditions for Diagonalization


- An $n \times n$ matrix is diagonalizable if it has $n$ **linearly independent eigenvectors** 
- If a matrix has $n$ **distinct eigenvalues**, it is guaranteed to be diagonalizable 

<img width="1164" height="516" alt="image" src="https://github.com/user-attachments/assets/2ed72821-6b45-404c-95e7-3fb37cc2d183" />

---


## 14. Orthogonal Diagonalization


A matrix $A$ is **orthogonally diagonalizable** if there exists an orthogonal matrix $P$ such that:



$$
P^T AP = D
$$
or 
$$
A = PDP^{-1}
$$


where $P^T = P^{-1}$  


## Key Requirements


- **Symmetry:** A matrix $A$ is orthogonally diagonalizable **if and only if** it is a **Symmetric Matrix** ($A = A^T$) 
- **Orthonormality:** Each column of the matrix $P$ must be **orthonormal** (orthogonal to each other and having a length/norm of 1)  


---


### Gram–Schmidt Process


If eigenvectors are orthogonal but not orthonormal, or if you have a basis that needs to be orthogonalized, use the **Gram–Schmidt Process** 


### Formula for v2  ​



$$
v_2 = x_2 - \text{proj}_{v_1}(x_2) =
x_2 - \frac{x_2 \cdot v_1}{\|v_1\|^2}v_1
$$


 

---


### Final Normalization


To ensure the matrix $P$ is orthogonal, divide each column by its norm:



$$
\text{New Column} =
\frac{\text{Column}}{\|\text{Column}\|}
$$





---


## Conclusion


Diagonalization simplifies complex calculations, such as computing high powers of matrices (e.g., $A^{200}$), by allowing us to work with diagonal forms



# Practise Questions:

<img width="692" height="229" alt="image" src="https://github.com/user-attachments/assets/b6ffe9bb-24bd-49c2-86cb-2884599505b5" />
<img width="343" height="170" alt="image" src="https://github.com/user-attachments/assets/4d691fd6-3053-43b6-a66f-1fbd12e5629a" />
<img width="540" height="210" alt="image" src="https://github.com/user-attachments/assets/7d599a6b-ae8f-4a0d-9b9e-981b1a465cf0" />
<img width="878" height="170" alt="image" src="https://github.com/user-attachments/assets/49142138-8e90-40dc-8cf1-5285f88a6231" />
<img width="346" height="205" alt="image" src="https://github.com/user-attachments/assets/0efe164c-1867-4807-88a1-e18b6d9283e5" />
<img width="1147" height="114" alt="image" src="https://github.com/user-attachments/assets/5eb90f45-0d6c-4b8f-ba25-2d76f9f47af1" />
<img width="295" height="200" alt="image" src="https://github.com/user-attachments/assets/5ba5a66f-0d81-4753-b64d-8e0d0f431231" />
