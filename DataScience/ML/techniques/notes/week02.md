# 2.1 Issues with PCA

## 1. Limitations of Linear Representation and Linearity

Principal Component Analysis (PCA) is fundamentally a technique for linear dimensionality reduction. It seeks to find a space of lower dimensionality, known as the principal subspace, through orthogonal projection. Because the mapping is linear, it is unable to capture the structure of data that lies close to a nonlinear manifold.

*   **Degeneracy in Higher Dimensions:** When mapping points from a lower-dimensional space to a higher-dimensional space ($\hat{d} > d$), the induced density will be degenerate, meaning it is zero everywhere except on the specific curve or surface where the data resides. 
*   **Curse of Dimensionality:** A general series expansion to approximate nonlinearities can lead to unrealistic requirements for computation and data. The number of basis functions required often needs to grow exponentially with the dimensionality $D$ of the input space.
*   **Pre-image Problem:** In Kernel PCA, projecting points in feature space onto the linear PCA subspace typically does not result in a point that lies on the nonlinear $D$-dimensional manifold. Such a projection will not have a corresponding pre-image in the original data space.

<img width="875" height="503" alt="image" src="https://github.com/user-attachments/assets/526bd4f0-2595-47ed-a142-50963a92ef7c" />

## 2. Sensitivity to Scaling and Metric Choice

The results of PCA are highly dependent on the choice of the coordinate system and the units of measurement.

*   **Lack of Invariance to Scaling:** PCA is invariant to translations or rotations (rigid-body motions) but is not invariant to general linear transformations or simple scaling of coordinate axes. A simple rescaling of axes can result in a completely different grouping of data into clusters.
*   **Problem of Noncomparable Quantities:** Feature vectors often measure noncomparable quantities, such as meters and kilograms. PCA depends on the Euclidean metric, and the results will differ based on whether a length is measured in meters or inches.
*   **Normalization Dilemma:** Routine normalization, such as standardizing features to zero mean and unit variance (whitening), is often used to prevent features with large numerical values from dominating distance calculations. However, this can be inappropriate if the spread in values is due to the presence of actual subclasses rather than random variation, potentially obscuring groups and reducing separation.

<img width="3335" height="1255" alt="image" src="https://github.com/user-attachments/assets/8347c1f7-3fa7-4058-ac2e-5867ae9e946d" />

## 3. Representation vs. Discrimination (Unsupervised Nature)

A central issue with PCA in classification contexts is that it is an unsupervised method.

*   **Focus on Variance over Labels:** PCA depends only on the values $x_{n}$ and does not use class-label information. It chooses directions of maximum variance, which may lead to strong class overlap if the classes are separated along a direction of low variance.
*   **Inadequacy for Discrimination:** For classification, the goal is discrimination, not just faithful representation. The most interesting features are often those where the difference in class means is large relative to the standard deviations, rather than those with the greatest overall variability.
*   **Noise Alignment:** If the noise in the data is large compared to the signal separating the categories, PCA will find the directions of the noise rather than the signal. Standard PCA assumes that the response varies most in the directions of high input variance, but if this assumption is wrong, PCA performance will suffer.

<img width="796" height="328" alt="image" src="https://github.com/user-attachments/assets/8cfbf528-bd10-4dc1-b7de-b332cb874a13" />

## 4. Computational Complexity and High-Dimensional Issues

PCA faces significant hurdles when applied to very high-dimensional data, particularly when the number of features $p$ is much larger than the number of observations $N$ ($p \gg N$).

*   **Eigendecomposition Cost:** Typical algorithms for finding the eigenvectors of a $D \times D$ covariance matrix have a computational cost that scales as $O(D^{3})$. For images with millions of pixels, direct application of PCA is computationally infeasible.
*   **Overfitting in $p \gg N$ Settings:** High-dimensional features are often correlated, leading to coefficient estimates with high variance. In these cases, there is not enough information in a small number of samples to efficiently estimate the covariance matrix, and PCA may fail to exploit feature correlations effectively.
*   **Masking:** In situations with multiple classes, the rigid nature of linear models can cause classes to be masked by others. Linear regression of an indicator matrix (related to PCA directions) may miss intermediate classes completely.

## 5. Statistical Nonidentifiability (Probabilistic PCA)

In the Probabilistic PCA (PPCA) framework, there is a specific issue regarding the uniqueness of the parameters.

*   **Rotational Invariance:** The predictive distribution is governed by the matrix $W$, but there is redundancy in this parameterization corresponding to rotations of the latent space coordinates. For any orthogonal matrix $R$, $W$ and $WR$ give rise to the same predictive distribution.
*   **Lack of Unique Sources:** This rotational invariance represents a form of statistical nonidentifiability. It is therefore impossible to identify any particular latent variables as unique underlying sources based on the second-order moments alone.

## 6. Sensitivity to Outliers

*   **Squared Error Influence:** PCA (and its probabilistic variants) often utilizes a sum-of-squares criterion. Squared-error loss is far less robust to grossly mis-measured values ("outliers") because it places much more emphasis on observations with large residuals.
*   **Global Nature:** Traditional PCA basis functions are global, meaning a change in one region of the input space can affect the representation across all other regions. This lack of locality can make the model sensitive to remote "wild shots".

<img width="1632" height="731" alt="image" src="https://github.com/user-attachments/assets/430b4b55-63d6-4f6f-96d1-e36bc74d00ff" />




---






# 2.2 Time-complexity Issue with PCA

## 1. Standard Eigendecomposition Complexity

The core of principal component analysis involves finding the $M$ eigenvectors of the data covariance matrix $S$ corresponding to the $M$ largest eigenvalues,. The computational cost of computing the full eigenvector decomposition for a matrix of size $D \times D$ is $O(D^3)$. If the intent is to project data onto only the first $M$ principal components, more efficient techniques, such as the power method, can be used that scale like $O(MD^2)$,.

In applications where the number of features $p$ is very high (such as images where each pixel is a dimension), direct application of these algorithms is often computationally infeasible. For example, a few hundred images in a space of several million dimensions (corresponding to color values for each pixel) would make the $O(D^3)$ scaling of typical eigenvector algorithms impossible to manage.

<img width="690" height="705" alt="image" src="https://github.com/user-attachments/assets/c3e7f86b-7a57-43d9-9d54-a94a27cb48ae" />

## 2. Covariance Matrix Evaluation

Evaluating the data covariance matrix $S = \frac{1}{N} \sum_{n=1}^{N} (x_{n} - \overline{x})(x_{n} - \overline{x})^{T}$ itself carries a significant computational burden,. This evaluation takes $O(ND^2)$ computations, where $N$ is the number of data points. Algorithms such as the snapshot method, which assume that the eigenvectors are linear combinations of the data vectors, avoid direct evaluation of the covariance matrix but are $O(N^3)$ and hence unsuited to large data sets.

## 3. High-Dimensional Computational Shortcut ($N < D$)

In cases where the number of observations $N$ is smaller than the dimensionality of the data space $D$, a linear subspace defined by these points has a dimensionality of at most $N - 1$. Consequently, at least $D - N + 1$ of the eigenvalues of the $D \times D$ covariance matrix will be zero, corresponding to directions with zero variance.

The computational problem for $N < D$ can be resolved by considering the $N \times N$ matrix $N^{-1}XX^{T}$, where $X$ is the $(N \times D)$ dimensional centered data matrix.
*   Let $S = N^{-1}X^{T}X$ be the $D \times D$ covariance matrix.
*   The eigenvector equation is $\frac{1}{N}X^{T}Xu_{i} = \lambda_{i}u_{i}$.
*   Pre-multiplying by $X$ gives $\frac{1}{N}XX^{T}(Xu_{i}) = \lambda_{i}(Xu_{i})$.
*   Defining $v_{i} = Xu_{i}$ yields $\frac{1}{N}XX^{T}v_{i} = \lambda_{i}v_{i}$,.

This allows the eigenvector problem to be solved in an $N$-dimensional space with a computational cost of $O(N^3)$ instead of $O(D^3)$. The eigenvectors in the original $D$-dimensional space are then recovered using:
$$u_{i} = \frac{1}{(N\lambda_{i})^{1/2}} X^{T} v_{i}$$
where $v_{i}$ has been normalized to unit length.

## 4. Efficiency of the EM Algorithm for PCA

For large-scale applications, the EM algorithm provides a computationally efficient alternative to the eigenvector decomposition of the sample covariance matrix,. This approach is particularly beneficial when only a few leading eigenvectors are required and avoids the need to evaluate the data covariance matrix as an intermediate step,,.

The most computationally demanding steps in the EM algorithm involve sums over the data set that scale as $O(NDM)$. For large $D$ and $M \ll D$, this represents a significant saving compared to the $O(ND^2)$ cost of constructing the covariance matrix or the $O(D^3)$ cost of a full eigendecomposition. Furthermore, the EM algorithm can be implemented in an on-line form where each $D$-dimensional data point is processed and then discarded, which is advantageous if both $N$ and $D$ are large.

<img width="858" height="686" alt="image" src="https://github.com/user-attachments/assets/8f214f15-cfa9-434a-a771-d482cd222136" />

## 5. Summary of Related Methods

| Method | Computational Cost | Key Characteristic |
| :--- | :--- | :--- |
| **Standard PCA** | $O(D^3)$ | Full eigenvector decomposition of $D \times D$ matrix. |
| **Power Method** | $O(MD^2)$ | Finding only the first $M$ eigenvalues/vectors. |
| **Covariance Calculation** | $O(ND^2)$ | Cost of simply evaluating the matrix $S$. |
| **High-Dim PCA ($N < D$)** | $O(N^3)$ | Solving eigenvectors for the $N \times N$ matrix $XX^T$. |
| **EM Algorithm** | $O(NDM)$ | Sums over data; avoids explicit covariance matrix. |
| **SVD (QR/Cholesky)** | $O(Np^2)$ or $O(p^3)$ | Standard numerical fitting techniques. |
| **Kernel PCA** | $O(N^3)$ | Eigenvector expansion of the $N \times N$ matrix $K$,. |

Kernel PCA involves finding the eigenvectors of an $N \times N$ matrix $\tilde{K}$ rather than the $D \times D$ matrix $S$ of conventional linear PCA; thus, for large data sets, approximations are often necessary,. Additionally, any algorithm that converges to the global maximum of the lower bound $\mathcal{L}(q, \theta)$ in the EM framework will find a parameter value that is also a global maximum of the log likelihood.




---








# 2.3 Feature Transformation

## 1. Introduction to Pre-processing and Feature Extraction

For most practical applications, the original input variables are typically pre-processed to transform them into some new space of variables where, it is hoped, the pattern recognition problem will be easier to solve. This pre-processing stage is sometimes also called feature extraction. New test data must be pre-processed using the same steps as the training data. 

Pre-processing might also be performed in order to speed up computation. Instead of presenting huge numbers of pixels directly to a complex algorithm, the aim is to find useful features that are fast to compute, and yet that also preserve useful discriminatory information. Because the number of such features is smaller than the number of pixels, this kind of pre-processing represents a form of dimensionality reduction. Care must be taken during pre-processing because often information is discarded, and if this information is important to the solution of the problem then the overall accuracy of the system can suffer.

## 2. Standardization and Whitening

Another application of principal component analysis is to data pre-processing. In this case, the goal is not dimensionality reduction but rather the transformation of a data set in order to standardize certain of its properties. Typically, it is done when the original variables are measured in various different units or have significantly different variability.

### 2.1 Standardization
Subtracting the mean and dividing by the standard deviation is an appropriate normalization if this spread of values is due to normal random variation. Standardizing each variable to have mean zero and unit variance (whitening) is often used to prevent features with large numerical values from dominating distance calculations.

### 2.2 Whitening (Sphereing)
It is sometimes convenient to perform a coordinate transformation that converts an arbitrary multivariate normal distribution into a spherical one, i.e., one having a covariance matrix proportional to the identity matrix $I$. If we define $\Phi$ to be the matrix whose columns are the orthonormal eigenvectors of $\Sigma$, and $\Lambda$ the diagonal matrix of the corresponding eigenvalues, then the transformation $A_w = \Phi\Lambda^{-1/2}$ applied to the coordinates ensures that the transformed distribution has covariance matrix equal to the identity matrix. In signal processing, the transform $A_w$ is called a whitening transformation, since it makes the spectrum of eigenvectors of the transformed distribution uniform.

<img width="601" height="361" alt="image" src="https://github.com/user-attachments/assets/5d600cdb-13de-4464-a511-862532d1a751" />

For each data point $x_n$, a transformed value is given by:
$$y_n = L^{-1/2}U^T(x_n - \overline{x})$$
where $\overline{x}$ is the sample mean. The set $\{y_n\}$ has zero mean, and its covariance is given by the identity matrix because:
$$\frac{1}{N} \sum_{n=1}^{N} y_n y_n^T = L^{-1/2}U^T S U L^{-1/2} = L^{-1/2} L L^{-1/2} = I$$
This operation is known as whitening or sphereing the data.

<img width="617" height="590" alt="image" src="https://github.com/user-attachments/assets/7b472174-08d5-4506-bdd7-d8d31378254a" />

## 3. Linear Basis Expansions

The core idea is to augment or replace the vector of inputs $X$ with additional variables, which are transformations of $X$, and then use linear models in this new space of derived input features. Denote by $h_m(X): \mathbb{R}^p \rightarrow \mathbb{R}$ the $m^{th}$ transformation of $X$, $m=1, ..., M$. We then model:
$$f(X) = \sum_{m=1}^M \beta_m h_m(X)$$
which is a linear basis expansion in $X$. Once the basis functions $h_m$ have been determined, the models are linear in these new variables, and the fitting proceeds as before.

### 3.1 Examples of Basis Functions
*   **Original Linear Model:** $h_m(X) = X_m$, $m=1, ..., p$.
*   **Polynomial Terms:** $h_m(X) = X_j^2$ or $h_m(X) = X_j X_k$ allows the inclusion of higher-order Taylor expansions.
*   **Nonlinear Transformations:** $h_m(X) = \log(X_j), \sqrt{X_j}, \dots$.
*   **Functional Regions:** $h_m(X) = I(L_m \le X_k < U_m)$ is an indicator for a region of $X_k$.

## 4. Generalized Linear Discriminant Functions

The linear discriminant function $g(x)$ can be written as:
$$g(x) = a^T y$$
where $a$ is a $\hat{d}$-dimensional weight vector, and where the $\hat{d}$ functions $y_i(x)$ are arbitrary functions of $x$. These are sometimes called $\phi$ functions and might be computed by a feature detecting subsystem. The resulting discriminant function is not linear in $x$, but it is linear in $y$. The $\hat{d}$ functions $y_i(x)$ merely map points in $d$-dimensional $x$-space to points in $\hat{d}$-dimensional $y$-space. 

<img width="618" height="432" alt="image" src="https://github.com/user-attachments/assets/ddb29783-2d56-4f5b-a9eb-169d200f37f5" />

### 4.1 Augmented Feature Vector
The addition of a constant component to $x$ preserves all distance relationships among samples. The mapping from $d$-dimensional $x$-space to $(d+1)$-dimensional $y$-space is convenient. An augmented feature vector is written as:

$$
y = \begin{bmatrix} 1 \\\ x_1 \\\ \vdots \\\ x_d \end{bmatrix} = \begin{bmatrix} 1 \\\ x \end{bmatrix}
$$

The augmented weight vector is:

$$
a = \begin{bmatrix} w_0 \\\ w_1 \\\ \vdots \\\ w_d \end{bmatrix} = \begin{bmatrix} w_0 \\\ w \end{bmatrix}
$$

By using this mapping, the problem of finding a weight vector $w$ and a threshold weight $w_0$ is reduced to finding a single weight vector $a$.

<img width="659" height="436" alt="image" src="https://github.com/user-attachments/assets/fb3a5614-b851-477e-8c03-ee6320031a11" />

## 5. Mapping to High-Dimensional Space

If the class of node decisions does not match the form of the training data, a very complicated decision tree will result. If "proper" decision forms are used, such as linear combinations of features, the tree can be quite simple.

### 5.1 Complexity and the Curse of Dimensionality
A complete quadratic discriminant function involves $\hat{d} = (d+1)(d+2)/2$ terms. Inclusion of cubic and higher orders leads to $O(\hat{d}^3)$ terms. A general series expansion of $g(x)$ can easily lead to unrealistic requirements for computation and data. This drawback can be accommodated by imposing a constraint of large margins, or bands between training patterns. In this case, we rely on the assumption that the mapping to a high-dimensional space does not impose any spurious structure or relationships among the training points.

## 6. Invariances in Representation

In seeking to achieve an optimal representation for a particular pattern classification task, we confront the problem of invariances. A particular object should be assigned the same classification irrespective of its position within the image (translation invariance) or of its size (scale invariance).

### 6.1 Types of Invariances
*   **Translation Invariance:** The absolute position on a conveyor belt is irrelevant to the category.
*   **Rotation Invariance:** A classifier should be insensitive to two-dimensional rotation about the camera's line of sight or rotations about an arbitrary line in three dimensions.
*   **Scale Invariance:** A young, small salmon is still a salmon.
*   **Rate Invariance:** For patterns with inherent temporal variation, the recognizer may need to be insensitive to the rate at which the pattern evolves (e.g., speech).
*   **Deformation Invariance:** Radical variation in images, such as a hand grasping an object or snapping fingers, requires non-rigid deformation invariance.

### 6.2 Incorporation of Knowledge
The symmetries described above are continuous (translated, rotated, sped up). Other discrete symmetries are relevant, such as flips left-to-right or top-to-bottom. A central technique, when there is insufficient training data, is to incorporate knowledge of the problem domain, such as how the patterns themselves were produced. One method is analysis by synthesis, where one has a model of how each pattern is generated. If this underlying model of production can be determined from the sound or image, we can classify the pattern by how it was produced.

<img width="867" height="576" alt="image" src="https://github.com/user-attachments/assets/da5809ac-ca29-49d1-8cac-e522326fe67f" />






---










# 2.4 Kernel Functions and Kernel PCA

## 1. Introduction to Kernel Methods and the Kernel Trick

A class of pattern recognition techniques exists where the training data points, or a subset of them, are kept and used during the prediction phase. Many linear parametric models can be re-cast into an equivalent dual representation in which the predictions are based on linear combinations of a kernel function evaluated at the training data points. For models based on a fixed nonlinear feature space mapping $\phi(x)$, the kernel function is given by the relation $k(x,x') = \phi(x)^T\phi(x')$.

The concept of a kernel formulated as an inner product in a feature space allows for the construction of extensions to well-known algorithms through the kernel trick, also known as kernel substitution. The general idea is that if an algorithm is formulated such that the input vector $x$ enters only in the form of scalar products, that scalar product can be replaced with another choice of kernel. This technique can be applied to principal component analysis to develop a nonlinear variant known as kernel PCA.

## 2. Construction of Valid Kernels

In order to exploit kernel substitution, it is necessary to construct valid kernel functions. One approach is to choose a feature space mapping $\phi(x)$ and use it to find the corresponding kernel $k(x,x') = \phi(x)^T\phi(x') = \sum_{i=1}^M \phi_i(x)\phi_i(x')$. An alternative is to construct kernel functions directly, ensuring they correspond to a scalar product in some feature space.

### 2.1 Necessary Conditions
A necessary and sufficient condition for a function $k(x,x')$ to be a valid kernel is that the Gram matrix $K$, with elements $K_{nm} = k(x_n, x_m)$, must be positive semidefinite for all possible choices of the set $\{x_n\}$. A positive semidefinite matrix is one where $w^T Aw \ge 0$ holds for all values of $w$, which is equivalent to all eigenvalues $\lambda_i \ge 0$.

### 2.2 Techniques for Constructing New Kernels
Given valid kernels $k_1(x, x')$ and $k_2(x, x')$, the following are also valid:
*   $k(x, x') = c k_1(x, x')$ for $c > 0$.
*   $k(x, x') = f(x) k_1(x, x') f(x')$ for any function $f(\cdot)$.
*   $k(x, x') = q(k_1(x, x'))$ where $q(\cdot)$ is a polynomial with nonnegative coefficients.
*   $k(x, x') = \exp(k_1(x, x'))$.
*   $k(x, x') = k_1(x, x') + k_2(x, x')$.
*   $k(x, x') = k_1(x, x') k_2(x, x')$.

### 2.3 Common Kernel Examples
*   **Linear Kernel:** $\phi(x) = x$, resulting in $k(x, x') = x^T x'$.
*   **Polynomial Kernel:** $k(x, x') = (x^T x' + c)^M$ with $c > 0$. For example, if $p=2$ and $d=2$, the kernel $K(x,y) = (\langle x,y \rangle + 1)^2$ has $M=6$ eigenfunctions spanning the space of polynomials of total degree 2.
*   **Gaussian (Radial Basis Function) Kernel:** $k(x, x') = \exp(-\|x - x'\|^2 / 2\sigma^2)$. It is not interpreted as a probability density in this context, and its corresponding feature vector has infinite dimensionality.
*   **Sigmoidal Kernel:** $k(x, x') = \tanh(ax^T x' + b)$, whose Gram matrix is not generally positive semidefinite but has been used in practice.
*   **Stationary Kernels:** Functions of the difference between arguments, $k(x, x') = k(x - x')$, which are invariant to translations in input space.

<img width="876" height="539" alt="image" src="https://github.com/user-attachments/assets/a76a8e0e-6e9e-4ff1-896e-ffabb18c205b" />

## 3. Kernel Principal Component Analysis (KPCA)

Kernel PCA is a non-linear version of linear principal components that mimics the result of expanding features by non-linear transformations and applying PCA in the transformed space. 

### 3.1 Mathematical Derivation
Consider a data set $\{x_n\}$ where the sample mean has been subtracted so that $\sum_n x_n = 0$. If we apply a nonlinear transformation $\phi(x)$ into an $M$-dimensional feature space, we can perform standard PCA there. Assuming the projected data also has zero mean ($\sum_n \phi(x_n) = 0$), the $M \times M$ sample covariance matrix in feature space is:
$$C = \frac{1}{N} \sum_{n=1}^N \phi(x_n)\phi(x_n)^T$$
The eigenvector expansion is defined by $C v_i = \lambda_i v_i$ for $i=1,...,M$. Substituting the definition of $C$ shows that $v_i$ is a linear combination of the $\phi(x_n)$:
$$v_i = \sum_{n=1}^N a_{in} \phi(x_n)$$
By substituting this expansion back into the eigenvector equation and expressing it in terms of the kernel function $k(x_n, x_m) = \phi(x_n)^T \phi(x_m)$, we obtain the dual eigenvalue problem:
$$K a_i = \lambda_i N a_i$$
where $a_i$ is an $N$-dimensional column vector with elements $a_{ni}$.

### 3.2 Normalization and Projection
The normalization condition for the coefficients $a_i$ requires the eigenvectors in feature space to be normalized, such that:
$$1 = v_i^T v_i = \lambda_i N a_i^T a_i$$
The principal component projection of a point $x$ onto eigenvector $i$ is then expressed in terms of the kernel function:
$$y_i(x) = \phi(x)^T v_i = \sum_{n=1}^N a_{in} k(x, x_n)$$

### 3.3 Centering in Feature Space
In general, the projected data set $\phi(x_n)$ will not have zero mean. To avoid working directly in feature space, the Gram matrix must be centered using only the kernel function. The elements of the centered Gram matrix $\tilde{K}$ are given by:

$$
\tilde{K}_{nm} = k(x_n, x_m) - \frac{1}{N} \sum_{l=1}^N k(x_l, x_m) - \frac{1}{N} \sum_{l=1}^N k(x_n, x_l) + \frac{1}{N^2} \sum_{j=1}^N \sum_{l=1}^N k(x_j, x_l)
$$

In matrix notation, this is $\tilde{K} = K - 1_N K - K 1_N + 1_N K 1_N$, where $1_N$ is an $N \times N$ matrix with elements $1/N$.

<img width="872" height="508" alt="image" src="https://github.com/user-attachments/assets/586e2b78-6f26-4e3d-8703-1a1b2bce5295" />

## 4. Properties and Limitations of Kernel PCA

### 4.1 Dimensionality and Eigenvalues
In the original $D$-dimensional space, there are at most $D$ linear principal components. In the feature space, the dimensionality $M$ can be much larger or even infinite, allowing for a number of nonlinear principal components that exceeds $D$. However, the number of nonzero eigenvalues cannot exceed the number of data points $N$.

### 4.2 The Pre-image Problem
In standard linear PCA, a data vector $x_n$ can be approximated by its projection $\tilde{x}_n$ onto a lower-dimensional principal subspace. In kernel PCA, the mapping $\phi(x)$ maps the $D$-dimensional $x$ space to a $D$-dimensional manifold in the $M$-dimensional feature space. The projection of points in feature space onto the linear PCA subspace typically does not lie on this nonlinear manifold. Consequently, such a projection will generally not have a corresponding pre-image in the original data space.

### 4.3 Computational Complexity
Kernel PCA involves finding the eigenvectors of an $N \times N$ matrix $\tilde{K}$ rather than the $D \times D$ matrix $S$ used in conventional linear PCA. For large data sets, approximations are often necessary due to this computational burden.

### 4.4 Comparison with Spectral Clustering
There are close connections between kernel PCA and spectral clustering. Spectral clustering uses a Laplacian constructed from a similarity matrix, which is often a localized version of a kernel matrix (e.g., setting similarities to zero for non-neighbors). While kernel PCA finds the eigenvectors corresponding to the largest eigenvalues of a kernel matrix $K$, spectral clustering finds the eigenvectors corresponding to the smallest eigenvalues of the Laplacian.

<img width="887" height="577" alt="image" src="https://github.com/user-attachments/assets/4581f6af-fb04-4182-ae90-b613a6bb268e" />
