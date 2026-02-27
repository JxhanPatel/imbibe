# 4.1: Linear and Polynomial Regression

## 1. Introduction to Linear Regression

The main idea illustrated in this lecture is that a linear regression problem, which is like solving a least squares problem, can be solved either directly by taking derivatives or more simply by solving a system of equations using projections.

### Key Concepts:
*   **Geometric Connection:** The system of equations obtained via calculus coincides with the system of equations obtained by doing projections onto the column space of a matrix $A$.
*   **Inconsistent Systems:** Even if $Ax = b$ is not solvable, a system of equations can be solved to find the "best" fit through the data points.
*   **Polynomial Regression:** This is a generalization of curve fitting where, instead of fitting a line, a higher-dimensional object like a parabola or a cubic polynomial is fit using a transformed set of features.

---

## 2. Linear Regression Derivation

### Problem Setup
Given $n$ data points $\{(x_1, y_1), \dots, (x_n, y_n)\}$, where:
*   $x_i \in \mathbb{R}^d$ (d-dimensional input features)
*   $y_i \in \mathbb{R}$ (real-valued labels)

### Loss Function
We fit a linear function through this data by minimizing the sum of square loss:
$$L(\theta) = \frac{1}{2} \sum_{i=1}^{n} (x_i^T \theta - y_i)^2$$

### Matrix Formulation
To minimize $L$, we define the following:
*   **Feature Matrix ($A$):** All features stacked as rows.


$$
A = \begin{bmatrix} x_1^T \\\ x_2^T \\\ \vdots \\\ x_n^T \end{bmatrix}
$$
    
    
*   **Label Vector ($Y$):**

$$
Y = \begin{bmatrix} y_1 \\\ y_2 \\\ \vdots \\\ y_n \end{bmatrix}
$$


This leads to the expression:
$$L(\theta) = \frac{1}{2} (A\theta - Y)^T (A\theta - Y)$$

### Minimization via Calculus
Setting the gradient of the loss function with respect to $\theta$ to zero:
$$\nabla_{\theta} L(\theta) = 0$$
$$\nabla_{\theta} ((A\theta - Y)^T (A\theta - Y)) = 0$$
$$\implies A^T (A\theta - Y) = 0$$
$$\implies (A^T A) \theta = A^T Y$$

> [!IMPORTANT]
> **Least Squares Solution:** The equation $(A^T A) \theta = A^T Y$ is the fundamental equation for linear regression. If $A$ is full rank, $A^T A$ is invertible, and the solution is:
> $$\theta = (A^T A)^{-1} A^T Y$$

---

## 3. Maximum Likelihood and Least Squares

Suppose the data is generated according to a linear model with Gaussian noise:
$$y = \theta^T x + \epsilon$$
Where $\epsilon$ is zero-mean noise, $\epsilon \sim$ Gaussian with mean 0 and variance $1/\beta$.

### The Likelihood Function
Assuming the dataset $D = \{(x_i, y_i), i=1 \dots n\}$ is generated in an i.i.d. (independent and identically distributed) fashion, the likelihood approach seeks a $\theta$ that maximizes:
$$\mathcal{L}(\theta) = \prod_{i=1}^{n} \frac{\sqrt{\beta}}{\sqrt{2\pi}} \exp \left( -\frac{\beta}{2} (y_i - \theta^T x_i)^2 \right)$$

### Log-Likelihood
Maximizing $\mathcal{L}(\theta)$ is equivalent to maximizing the log-likelihood:
$$\log \mathcal{L}(\theta) = \frac{n}{2} \log \beta - \frac{n}{2} \log 2\pi - \beta \left[ \frac{1}{2} \sum (y_i - \theta^T x_i)^2 \right]$$

> [!NOTE]
> **Conclusion:** Maximizing the log-likelihood is the same as minimizing the least squares objective. Least squares regression solves a maximum likelihood estimation problem under a linear model.

---

## 4. Polynomial Regression

Polynomial regression generalizes curve fitting to any polynomial of degree $m$.

### Transformed Features
For one-dimensional data $D = \{(x_i, y_i), i=1 \dots n\}$, the predicted value is:
$$\hat{y}(x) = \theta_0 + \theta_1 x + \theta_2 x^2 + \dots + \theta_m x^m$$
This can be written as $\hat{y}(x) = \sum_{j=0}^{m} \theta_j \phi_j(x)$, where $\phi_j(x) = x^j$.

For a given $x$, the **transformed feature vector** is:
$$\phi(x) = (1, x, x^2, \dots, x^m)$$

### Solution
Using these transformed features, the problem reduces to linear regression:
$$\hat{y}(x) = \theta^T \phi(x)$$
Where $\theta = (\theta_0, \dots, \theta_m)$. The system to solve is:
$$(A^T A) \theta = A^T Y$$
In this case, the matrix $A$ contains the transformed features:

$$
A = \begin{bmatrix} \phi(x_1)^T \\\ \vdots \\\ \phi(x_n)^T \end{bmatrix}
$$

---

## 5. Regularized Linear Regression (Ridge Regression)

Instead of ordinary linear regression, we can solve a regularized version to control overfitting.

### Regularized Objective
Minimize the function $\bar{L}(\theta)$, which includes a **regularization term**:
$$\bar{L}(\theta) = \frac{1}{2} \sum_{i=1}^{n} (x_i^T \theta - y_i)^2 + \lambda \|\theta\|^2$$

### Regularized Solution
Repeating the calculus derivation leads to:
$$(A^T A + \lambda I) \theta_{reg} = A^T Y$$
$$\theta_{reg} = (A^T A + \lambda I)^{-1} A^T Y$$

### Significance of $\lambda$:
1.  **Invertibility:** $(A^T A + \lambda I)$ is invertible even if $A$ is not full rank.
2.  **Overfitting Control:**
    *   Too small $\lambda \to$ Overfitting.
    *   Too large $\lambda \to$ Underfitting.
