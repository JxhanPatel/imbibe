# 7.1 Principal Component Analysis

Principal Component Analysis (PCA) is an application of the Singular Value Decomposition (SVD), which is described as the "best basis" for representing a matrix. It provides a way to extract the "essential information" from large data sets, such as image processing or financial portfolios.

## 7.1.1 Central Ideas of Principal Component Analysis

The beauty of linear algebra is seen through the visualization of combinations of vectors and the four fundamental subspaces. PCA focuses on the following central concepts:

* **Principal Axes**: In mathematics, the formula $A = Q\Lambda Q^T$ is known as the spectral theorem. The orthogonal eigenvectors $q_i$ give the principal axes. In geometry or mechanics, these are the right choice of axes for an ellipse or ellipsoid.
* **The Best Basis**: The SVD chooses orthonormal bases for the four fundamental subspaces in an extremely special way. The real action of a matrix $A$ is between the row space and column space.
* **Dimension Reduction**: PCA aims to find the essential information inside a large matrix (e.g., 1000 by 1000 pixels) and send or store only that. Any matrix is the sum of $r$ matrices of rank 1:
  $$A = U\Sigma V^T = u_1\sigma_1v_1^T + u_2\sigma_2v_2^T + \cdot\cdot\cdot + u_r\sigma_r v_r^T$$.

<img width="1023" height="686" alt="image" src="https://github.com/user-attachments/assets/722b3ddc-ef5e-448a-b008-389bc76dc4a4" />
<img width="2743" height="2416" alt="image" src="https://github.com/user-attachments/assets/f297cd77-7076-4d1f-96ec-e08f61fd3d05" />

## 7.1.2 Derivation of the PCA Algorithm (via SVD)

The SVD factors any $m$ by $n$ matrix $A$ into $U\Sigma V^T$, where $U$ and $V$ are orthogonal matrices and $\Sigma$ is diagonal.

### 1. Identifying $V$ and $\Sigma$

With rectangular matrices, the key is to consider $A^T A$ and $AA^T$.
$$A^T A = (V\Sigma^T U^T)(U\Sigma V^T) = V\Sigma^T \Sigma V^T$$.
$V$ must be the eigenvector matrix for $A^T A$. The diagonal matrix $\Sigma^T \Sigma$ has the same eigenvalues $\sigma_1^2, \dots, \sigma_r^2$. The columns of $V$ are the "right singular vectors".

### 2. Identifying $U$

Similarly, for $AA^T$:
$$AA^T = (U\Sigma V^T)(V\Sigma^T U^T) = U\Sigma \Sigma^T U^T$$.
$U$ must be the eigenvector matrix for $AA^T$. The eigenvalue matrix in the middle is $\Sigma\Sigma^T$, which is $m$ by $m$ with $\sigma_1^2, \dots, \sigma_r^2$ on the diagonal. The columns of $U$ are the "left singular vectors".

### 3. Connecting the Components

Starting with $A^T Av_j = \sigma_j^2 v_j$, multiply by $A$:
$$(AA^T)Av_j = \sigma_j^2 Av_j$$.
This demonstrates that $Av_j$ is an eigenvector of $AA^T$. The length of this eigenvector is $\sigma_j$, because $|Av_j|^2 = v_j^T A^T A v_j = \sigma_j^2 v_j^T v_j = \sigma_j^2$. The unit eigenvector is $u_j = Av_j / \sigma_j$, confirming the relationship $AV = U\Sigma$.

## 7.1.3 Feature Selection and Effective Rank

In exact arithmetic, counting pivots determines rank. In real arithmetic, the SVD provides a more stable measure known as **Effective Rank**.

> [!NOTE]
> Based on the accuracy of the data, a tolerance (like $10^{-6}$) is chosen, and only singular values above it are counted. This determines the effective rank.

In image processing, if a 1000 by 1000 pixel image has singular values where only 20 are significant and 980 are extremely small, we keep the 20 significant columns of $U$ and $V$ and ignore the rest. This achieves a 25 to 1 compression.

## 7.1.4 Reconstruction Error

The total matrix is the sum of rank-1 matrices. If only $k$ terms are kept, the reconstruction $A_k$ is:
$$A_k = \sum_{i=1}^k \sigma_i u_i v_i^T$$
The columns ignored in the SVD are multiplied by the small $\sigma$'s that are deemed insignificant.

## 7.1.5 Variance of the Projected Data

The magnitude of the singular values $\sigma_i$ relates directly to the variance and the "amplifying power" of the matrix.

* **Maximum Variance**: The norm of $A$ measures the largest amount by which any vector is amplified: $|A| = \max \frac{|Ax|}{|x|}$. This equals the square root of the largest eigenvalue of $A^T A$ (the largest singular value $\sigma_1$).
* **Covariance Matrix**: In applications like portfolio management, the variance is given by $Var[R] = x^T Gx$, where $G$ is the symmetric positive semidefinite covariance matrix.
* **Principal Axis Alignment**: The change of variables $y = Q^T x$ (rotating the axes via the eigenvectors) simplifies the quadratic form to a sum of squares: $\lambda_1 y_1^2 + \cdot\cdot\cdot + \lambda_n y_n^2 = 1$. The axes of the resulting ellipsoid point along the eigenvectors, with lengths $1/\sqrt{\lambda_1}, \dots, 1/\sqrt{\lambda_n}$ from the center.

<img width="896" height="832" alt="image" src="https://github.com/user-attachments/assets/62133686-7378-4e9f-a53b-cbddc3df377b" />
<img width="376" height="584" alt="image" src="https://github.com/user-attachments/assets/370bc8f0-c49c-48d9-b169-161198340077" />

| Feature                       | Description                                                                                                       |
| :---------------------------- | :---------------------------------------------------------------------------------------------------------------- |
| **First Principal Component** | The eigenvector corresponding to $\sigma_{\max}$, representing the direction of maximum amplification (variance). |
| **Condition Number**          | The ratio $\sigma_{\max}/\sigma_{\min}$, measuring the sensitivity of the output to changes in input.             |
| **Principal Axis Theorem**    | Gives the right choice of axes for an ellipse; axes are perpendicular and point along eigenvectors.               |




---




# L7.2: Principal Component Analysis (Contd.)

Principal Component Analysis (PCA) continues the application of the Singular Value Decomposition (SVD), specifically focusing on identifying the directions of maximum variance and reducing dimensions while preserving essential information.

## 7.2.1 Principal Component Analysis Derivation (Continued)

The derivation of PCA is rooted in the factorization $A = U\Sigma V^T$. This decomposition identifies the "best basis" by choosing orthonormal bases for the four fundamental subspaces in a special way.

### 1. Matrix Approximation and Energy

Any $m$ by $n$ matrix is the sum of $r$ matrices of rank 1:

$$
A = U\Sigma V^T = u_1\sigma_1v_1^T + u_2\sigma_2v_2^T + \cdot\cdot\cdot + u_r\sigma_r v_r^T
$$

In data compression and image processing, if only $k$ terms are kept, the reconstruction $A_k$ is the sum of the first $k$ rank-1 matrices. The singular values $\sigma_i$ (in $\Sigma$) reveal exactly what information is large and what is small. Typically, some $\sigma$'s are significant and others are extremely small.

### 2. Identifying Effective Rank

In practical applications involving data with errors, counting pivots is not a stable way to determine rank.

* **Method**: Based on the accuracy of the data, a tolerance (such as $10^{-6}$) is chosen.
* **Condition**: Only the singular values above this tolerance are counted to determine the **effective rank**.
* **Result**: If an image has 1000 by 1000 pixels but only 20 significant singular values, keeping only the corresponding 20 columns of $U$ and $V$ achieves a 25 to 1 compression.

<img width="1147" height="860" alt="image" src="https://github.com/user-attachments/assets/17dd7aad-e15d-43bf-b2f7-73f77771bd16" />
<img width="1024" height="927" alt="image" src="https://github.com/user-attachments/assets/48aec621-8756-4677-8f66-e4925d16dff9" />




### 3. Directions of Maximum Variance (Rayleigh Quotient)

PCA seeks the direction in which the data is amplified most. This is equivalent to finding the vector $x$ that maximizes the Rayleigh quotient for the symmetric matrix $A^T A$:

$$
R(x) = \frac{||Ax||^2}{||x||^2} = \frac{x^T A^T Ax}{x^T x}
$$

* **Principal Axes**: The maximum value of $R(x)$ is $\lambda_{max}(A^T A) = \sigma_1^2$.
* **Eigenvectors**: The vector that $A$ amplifies the most is the first column of $V$ (the eigenvector of $A^T A$ corresponding to the largest eigenvalue).
* **Geometric Meaning**: The axes point toward the eigenvectors of $A^T A$. The major axis of the ellipsoid $x^T (A^T A)x = 1$ corresponds to the smallest eigenvalue, while the direction of maximum amplification corresponds to the largest singular value $\sigma_1$.

## 7.2.2 Example Problem: Projecting Data in One Dimension

The simplest case of dimension reduction is projecting a vector (data point) onto a single line (the first principal component).

### Problem Statement

Project the data vector

$$
b =
\begin{bmatrix}
1 \\\
2 \\\
3
\end{bmatrix}
$$

onto the line in the direction of

$$
a =
\begin{bmatrix}
1 \\\
1 \\\
1
\end{bmatrix}
$$

### Step 1: Calculate the Coefficient $\hat{x}$

The best estimate $\hat{x}$ minimizes the distance from $b$ to the line through $a$. The formula is:

$$
\hat{x} = \frac{a^T b}{a^T a}
$$

Calculation:

$$
a^T b = (1)(1) + (1)(2) + (1)(3) = 6
$$

$$
a^T a = (1)^2 + (1)^2 + (1)^2 = 3
$$

$$
\hat{x} = \frac{6}{3} = 2
$$

### Step 2: Find the Projection $p$

The projection $p$ is the multiple of $a$ that is closest to $b$:

$$
p =
2
\begin{bmatrix}
1 \\\
1 \\\
1
\end{bmatrix} =
$$

$$
\begin{bmatrix}
2 \\\
2 \\\
2
\end{bmatrix}
$$

### Step 3: Verify the Error $e$

The error vector $e = b - p$ must be perpendicular to the direction $a$:

$$
e =
\begin{bmatrix}
1 \\\
2 \\\
3
\end{bmatrix} -
$$

$$
\begin{bmatrix}
2 \\\
2 \\\
2
\end{bmatrix} =
$$

$$
\begin{bmatrix}
-1 \\\
1 \\\
1
\end{bmatrix}
$$

Check for orthogonality ($a^T e = 0$):

$$
(1)(-1) + (1)(1) + (1)(1) = -1 + 1 + 1 = 1
$$

(Note: In the specific example $b=(1,2,2)$ onto $a=(1,1,1)$, $a^T b=5$, $\hat{x}=5/3$, $p=(5/3,5/3,5/3)$, and $e^T a = 0$.)

> [!IMPORTANT]
> The projection matrix for this one-dimensional projection is $P = \frac{aa^T}{a^T a}$. For $a =^T$, the matrix is:

$$
P =
\frac{1}{3}
\begin{bmatrix}
1 & 1 & 1 \\\
1 & 1 & 1 \\\
1 & 1 & 1
\end{bmatrix}
$$

<img width="859" height="455" alt="image" src="https://github.com/user-attachments/assets/c90fb4e4-6022-4a4d-9454-4a1cf89e3ca1" />


### 4. Summary of One-Dimensional Projection Properties

1. **Symmetry**: The projection matrix $P$ is symmetric ($P^T = P$).
2. **Idempotency**: The square of the projection matrix is itself ($P^2 = P$). Projecting twice gives the same result as projecting once.
3. **Trace**: The trace of a rank-1 projection matrix $aa^T/a^T a$ always equals 1.
4. **Eigenvalues**: The eigenvalues of a projection matrix are 1 or 0.



---




# 7.3: PCA as Maximizing Variance

Principal Component Analysis (PCA) is a technique used to find the "best basis" for representing a matrix and extracting essential information from large data sets. It provides a way to identify the directions in which data varies the most.

## 7.3.1 Using PCA for Maximizing Variance of the Projected Data

The magnitude of the singular values $\sigma_i$ relates directly to the variance and the "amplifying power" of the matrix. To find the direction of maximum variance, we look for the vector that the matrix $A$ amplifies the most.

### 1. The Rayleigh Quotient
The mathematical tool used to find the direction of maximum variance is the Rayleigh quotient. For a symmetric matrix $A^T A$, the Rayleigh quotient is defined as:
$$R(x) = \frac{||Ax||^2}{||x||^2} = \frac{x^T A^T Ax}{x^T x}$$ 

### 2. Rayleigh's Principle
Rayleigh's Principle states that the minimum value of the Rayleigh quotient is the smallest eigenvalue $\lambda_1$. Its maximum value is the largest eigenvalue $\lambda_n$. 
*   **Minimum Value**: $R(x)$ reaches its minimum at the first eigenvector $x_1$ of $A$.
*   **Maximum Value**: $R(x)$ reaches its maximum at the last eigenvector $x_n$ of $A$.

### 3. Connection to Matrix Norms
The norm of $A$ measures the largest amount by which any vector is amplified. $||A||^2$ is the square root of the largest eigenvalue of $A^T A$:
$$||A||^2 = \max \frac{x^T A^T Ax}{x^T x} = \lambda_{\max}(A^T A)$$.
This maximum value equals the square of the largest singular value, $\sigma_1^2$ 

<img width="456" height="447" alt="image" src="https://github.com/user-attachments/assets/49b37080-9e0d-4d2c-97ca-fec4857e91ff" />

## 7.3.2 Principal Directions and Principal Components

PCA uses the spectral theorem and singular value decomposition to identify the principal axes of a data set 

### 1. Principal Directions
The principal directions are the orthogonal eigenvectors of the matrix
*   In mathematics, the formula $A = Q\Lambda Q^T$ is known as the spectral theorem.
*   The orthogonal eigenvectors $q_i$ give the principal axes
*   In geometry or mechanics, these are the right choice of axes for an ellipse or ellipsoid.

### 2. Principal Components
The first principal component is the eigenvector corresponding to $\sigma_{\max}$ [7.1.5]. This vector represents the direction of maximum amplification or maximum variance

> [!NOTE]
> The axes of an ellipsoid $x^T Ax = 1$ point along the eigenvectors of $A$. The major axis corresponds to the smallest eigenvalue, while the direction of greatest amplification corresponds to the largest singular value

## 7.3.3 Example Problem: Principal Axes of an Ellipse

Consider the symmetric matrix 

$$
A = \begin{bmatrix} 5 & 4 \\\ 4 & 5 \end{bmatrix}
$$

 and the quadratic form $x^T Ax = 5u^2 + 8uv + 5v^2 = 1$. Find the principal directions (axes) and the lengths of those axes.

### Step 1: Find the Eigenvalues
The characteristic equation is solved to find $\lambda_1 = 1$ and $\lambda_2 = 9$.

### Step 2: Find the Unit Eigenvectors (Principal Directions)
The unit eigenvectors are:
*   $x_1 = (1, -1) / \sqrt{2}$.
*   $x_2 = (1, 1) / \sqrt{2}$.
These eigenvectors are at $45^\circ$ angles with the $u$-$v$ axes and are lined up with the axes of the ellipse.

### Step 3: Determine Axis Lengths
The equation $x^T Ax = 1$ can be rewritten using a change of variables $y = Q^T x$ to produce a sum of squares:
$$\lambda_1 y_1^2 + \lambda_2 y_2^2 = 1 \cdot y_1^2 + 9 \cdot y_2^2 = 1$$.
*   The axes have lengths $1/\sqrt{\lambda_1}$ and $1/\sqrt{\lambda_2}$.
*   **Major Axis Length**: $1/\sqrt{1} = 1$.
*   **Minor Axis Length**: $1/\sqrt{9} = 1/3$.

<img width="376" height="584" alt="image" src="https://github.com/user-attachments/assets/f0829dcb-60a8-43bf-b640-6733f0312438" />

### Summary Table: Variance and Principal Components

| Concept | Linear Algebra Equivalent |
| :--- | :--- |
| **Total Matrix** | Sum of $r$ rank-1 matrices: $A = \sum \sigma_i u_i v_i^T$. |
| **Maximum Variance** | Largest singular value $\sigma_1$ or $\lambda_{\max}(A^T A)$ |
| **Principal Axis** | Eigenvector $q_i$ or $v_i$ |
| **Axis Length** | $1/\sqrt{\lambda_i}$ from the center of the ellipsoid. |
| **Dimension Reduction** | Keeping only $k$ significant singular values $\sigma_i$ |

