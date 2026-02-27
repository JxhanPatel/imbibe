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
