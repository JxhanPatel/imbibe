# Introduction; Unsupervised Learning Representation Learning — PCA

## 1. Introduction to Pattern Recognition and Machine Learning

The problem of searching for patterns in data is a fundamental one with a long and successful history. The field of pattern recognition is concerned with the automatic discovery of regularities in data through the use of computer algorithms and with the use of these regularities to take actions such as classifying the data into different categories. Pattern recognition has its origins in engineering, whereas machine learning grew out of computer science. However, these activities can be viewed as two facets of the same field.

### 1.1 Training and Learning
In a typical machine learning approach, a large set of $N$ observations $\{x_{1},...,x_{N}\}$ called a training set is used to tune the parameters of an adaptive model. In supervised learning, the categories of the data in the training set are known in advance, typically by hand-labelling them. The result of the machine learning algorithm can be expressed as a function $y(x)$ which takes a new input $x$ and generates an output vector $y$. The ability to categorize correctly new examples that differ from those used for training is known as generalization.

### 1.2 Pre-processing and Feature Extraction
Original input variables are typically pre-processed to transform them into some new space of variables where the pattern recognition problem is easier to solve. This stage is also called feature extraction. Pre-processing may also be performed to speed up computation by evaluating useful features that are fast to compute while preserving discriminatory information. Because the number of such features is smaller than the number of pixels or original variables, this represents a form of dimensionality reduction. Care must be taken because often information is discarded, and if this information is important to the solution, the overall accuracy can suffer.

### 1.3 Types of Learning
Applications can be categorized into three broad types:
*   **Supervised Learning:** The training data comprises examples of input vectors along with their corresponding target vectors. If the aim is to assign inputs to discrete categories, it is a classification problem. If the desired output consists of continuous variables, it is a regression problem.
*   **Unsupervised Learning:** The training data consists of a set of input vectors $x$ without any corresponding target values. The goal may be to discover groups of similar examples (clustering), determine the distribution of data within the input space (density estimation), or project data from high-dimensional space to lower dimensions for visualization.
*   **Reinforcement Learning:** This is concerned with finding suitable actions to take in a given situation to maximize a reward through a process of trial and error.

<img width="666" height="722" alt="image" src="https://github.com/user-attachments/assets/7f5968aa-672c-46eb-bbad-2e3bab90bcbb" />

## 2. Representation Learning and Component Analysis

Component analysis is an unsupervised approach to finding the right features from the data. In these problems, there is no outcome measure, and the goal is to describe the associations and patterns among a set of input measures. In low-dimensional problems (say $p \le 3$), variety of effective nonparametric methods exist for directly estimating density, but these fail in high dimensions due to the curse of dimensionality.

One must instead settle for estimating crude global models or simple descriptive statistics that characterize regions where the joint density $Pr(X)$ is relatively large. Techniques such as principal components, multidimensional scaling, and self-organizing maps attempt to identify low-dimensional manifolds within the $X$-space that represent high data density. This provides information about the associations among variables and whether they can be considered as functions of a smaller set of latent variables.

## 3. Principal Component Analysis (PCA)

Principal component analysis, or PCA, is a technique widely used for applications such as dimensionality reduction, lossy data compression, feature extraction, and data visualization. It is also known as the Karhunen-Loève transform. PCA can be defined in two equivalent ways that give rise to the same algorithm:
1.  The orthogonal projection of data onto a lower-dimensional linear space (principal subspace) such that the variance of the projected data is maximized.
2.  The linear projection that minimizes the average projection cost, defined as the mean squared distance between data points and their projections.

<img width="634" height="483" alt="image" src="https://github.com/user-attachments/assets/49245dc3-89a9-4b5a-aad5-3b13d4d06302" />

### 3.1 Maximum Variance Formulation
Consider a data set of observations $\{x_{n}\}$ where $n=1,...,N$ and $x_{n}$ is a Euclidean variable with dimensionality $D$. The goal is to project the data onto a space of dimensionality $M < D$ while maximizing the variance.

For a projection onto a one-dimensional space ($M=1$), the direction is defined by a unit vector $u_{1}$ such that $u_{1}^{T}u_{1} = 1$. Each data point $x_{n}$ is projected onto a scalar value $u_{1}^{T}x_{n}$. The mean of the projected data is $u_{1}^{T}\overline{x}$ where:
$$\overline{x} = \frac{1}{N} \sum_{n=1}^{N} x_{n}$$
The variance of the projected data is given by:
$$\frac{1}{N} \sum_{n=1}^{N} \{u_{1}^{T}x_{n} - u_{1}^{T}\overline{x}\}^{2} = u_{1}^{T}Su_{1}$$
where $S$ is the data covariance matrix:
$$S = \frac{1}{N} \sum_{n=1}^{N} (x_{n} - \overline{x})(x_{n} - \overline{x})^{T}$$

To maximize $u_{1}^{T}Su_{1}$ subject to $u_{1}^{T}u_{1} = 1$, we introduce a Lagrange multiplier $\lambda_{1}$:
$$u_{1}^{T}Su_{1} + \lambda_{1}(1 - u_{1}^{T}u_{1})$$
Setting the derivative with respect to $u_{1}$ to zero shows that $u_{1}$ must be an eigenvector of $S$:
$$Su_{1} = \lambda_{1}u_{1}$$
Left-multiplying by $u_{1}^{T}$ and using $u_{1}^{T}u_{1} = 1$ shows the variance is $u_{1}^{T}Su_{1} = \lambda_{1}$. The variance is maximized when $u_{1}$ is the eigenvector corresponding to the largest eigenvalue $\lambda_{1}$, known as the first principal component.

For an $M$-dimensional projection space, the optimal linear projection is defined by the $M$ eigenvectors $u_{1},...,u_{M}$ of the covariance matrix $S$ corresponding to the $M$ largest eigenvalues $\lambda_{1},...,\lambda_{M}$.

### 3.2 Minimum Error Formulation
PCA can also be formulated based on minimizing the distortion introduced by dimensionality reduction. We approximate each data point $x_{n}$ by a representation in an $M$-dimensional subspace:

$$
\tilde{x}_{n} = \sum_{i=1}^{M} z_{ni}u_{i} + \sum_{i=M+1}^{D} b_{i}u_{i}
$$

where $\{u_{i}\}$ is a complete orthonormal set of basis vectors. To minimize the distortion measure 

$J = \frac{1}{N} \sum_{n=1}^{N} ||x_{n} - \tilde{x}-{n}||^{2}$ with respect to $\{z_{ni}\}$ and $\{b_{i}\}$

we find:

$$
z_{nj} = x_{n}^{T}u_{j} \text{ for } j=1,...,M
$$

$$
b_{j} = \overline{x}^{T}u_{j} \text{ for } j=M+1,...,D
$$

The displacement vector $x_{n} - \tilde{x}_{n}$ lies in the space orthogonal to the principal subspace. The distortion measure reduces to:

$$
J = \sum_{i=M+1}^{D} u_{i}^{T}Su_{i}
$$

The minimum value of $J$ is obtained by selecting the eigenvectors corresponding to the $D - M$ smallest eigenvalues. Thus, the principal subspace is defined by the $M$ eigenvectors with the largest eigenvalues.

<img width="665" height="602" alt="image" src="https://github.com/user-attachments/assets/4f942a3a-6abf-4f69-b394-cdf4b4739ca2" />

## 4. Practical Aspects and Applications

### 4.1 Data Pre-processing: Whitening
PCA can be used to standardize a data set to have zero mean and unit covariance, an operation known as whitening or sphereing. We define a transformed value for each data point:
$$y_{n} = L^{-1/2}U^{T}(x_{n} - \overline{x})$$
where $L$ is a diagonal matrix of eigenvalues and $U$ is an orthogonal matrix of eigenvectors. The resulting set $\{y_{n}\}$ has unit covariance $I$.

### 4.2 Data Visualization
High-dimensional data can be projected onto a two-dimensional ($M=2$) principal subspace. A data point $x_{n}$ is plotted at coordinates $(x_{n}^{T}u_{1}, x_{n}^{T}u_{2})$ where $u_{1}$ and $u_{2}$ are the eigenvectors for the two largest eigenvalues.

### 4.3 PCA for High-Dimensional Data ($N < D$)
In applications like image processing, the number of data points $N$ may be smaller than the dimensionality $D$. Direct application of PCA involves an $O(D^{3})$ computation for eigenvectors, which is infeasible. However, the $D \times D$ covariance matrix $S = N^{-1}X^{T}X$ and the $N \times N$ matrix $N^{-1}XX^{T}$ share the same $N - 1$ non-zero eigenvalues. We can solve the eigenvector problem for the smaller $N \times N$ matrix and then compute the eigenvectors in the original space:
$$u_{i} = \frac{1}{(N\lambda_{i})^{1/2}} X^{T} v_{i}$$

## 5. Probabilistic PCA (PPCA)

Probabilistic PCA expresses PCA as the maximum likelihood solution of a probabilistic latent variable model. This approach brings several advantages, including the use of an EM algorithm for parameter estimation, principled handling of missing values, and a basis for a Bayesian treatment.

### 5.1 The Generative Model
We introduce an explicit latent variable $z$ corresponding to the principal subspace. The model defines a Gaussian prior over the latent variable and a Gaussian conditional distribution for the observed variable $x$:
$$p(z) = \mathcal{N}(z|0, I)$$
$$p(x|z) = \mathcal{N}(x|Wz + \mu, \sigma^{2}I)$$
The mean of $x$ is a general linear function of $z$ governed by a $D \times M$ matrix $W$ and a $D$-dimensional vector $\mu$. The D-dimensional observed variable is defined by:
$$x = Wz + \mu + \epsilon$$
where $\epsilon$ is a zero-mean Gaussian noise variable with covariance $\sigma^{2}I$.

<img width="845" height="402" alt="image" src="https://github.com/user-attachments/assets/de789c1f-5cc9-4374-b799-15ed04a1911c" />

### 5.2 Marginal and Posterior Distributions
The marginal distribution $p(x)$ is obtained by integrating out the latent variable:
$$p(x) = \int p(x|z)p(z) dz = \mathcal{N}(x|\mu, C)$$
where the covariance matrix $C$ is defined by:
$$C = WW^{T} + \sigma^{2}I$$
Using Bayes' theorem, the posterior distribution $p(z|x)$ is also Gaussian:
$$p(z|x) = \mathcal{N}(z|M^{-1}W^{T}(x - \mu), \sigma^{-2}M)$$
where $M = W^{T}W + \sigma^{2}I$.

### 5.3 Maximum Likelihood Solution
The maximum likelihood solution for $\mu$ is the sample mean $\overline{x}$. All stationary points of the log-likelihood for $W$ can be written as:
$$W_{ML} = U_{M}(L_{M} - \sigma^{2}I)^{1/2}R$$
where $U_{M}$ is a $D \times M$ matrix whose columns are eigenvectors of the data covariance matrix $S$, $L_{M}$ is a diagonal matrix of eigenvalues, and $R$ is an arbitrary orthogonal matrix. The maximum is reached when the $M$ eigenvectors correspond to the largest eigenvalues. The maximum likelihood solution for $\sigma^{2}$ is:
$$\sigma_{ML}^{2} = \frac{1}{D-M} \sum_{i=M+1}^{D} \lambda_{i}$$
which is the average variance associated with the discarded dimensions.

### 5.4 The EM Algorithm for PCA
For large-scale applications where $M \ll D$, the EM algorithm can be computationally more efficient than conventional PCA.
*   **E-step:** Compute the sufficient statistics of the latent space posterior:
    $$\mathbb{E}[z_{n}] = M^{-1}W^{T}(x_{n} - \overline{x})$$
    $$\mathbb{E}[z_{n}z_{n}^{T}] = \sigma^{2}M^{-1} + \mathbb{E}[z_{n}]\mathbb{E}[z_{n}]^{T}$$
*   **M-step:** Revise parameter values:
    $$W_{new} = [\sum_{n=1}^{N}(x_{n} - \overline{x})\mathbb{E}[z_{n}]^{T}][\sum_{n=1}^{N}\mathbb{E}[z_{n}z_{n}^{T}]]^{-1}$$
    $$\sigma_{new}^{2} = \frac{1}{ND} \sum_{n=1}^{N} \{||x_{n} - \overline{x}||^{2} - 2\mathbb{E}[z_{n}]^{T}W_{new}^{T}(x_{n} - \overline{x}) + Tr(\mathbb{E}[z_{n}z_{n}^{T}]W_{new}^{T}W_{new})\}$$

<img width="830" height="679" alt="image" src="https://github.com/user-attachments/assets/244b23ec-c792-4993-b28b-12b57a205016" />

### 5.5 Bayesian PCA
A Bayesian approach allows the dimensionality $M$ of the principal subspace to be determined automatically from the data. By using an automatic relevance determination (ARD) prior over $W$, surplus dimensions can be pruned out of the model:
$$p(W|\alpha) = \prod_{i=1}^{M} (\frac{\alpha_{i}}{2\pi})^{D/2} exp \{-\frac{1}{2}\alpha_{i}w_{i}^{T}w_{i}\}$$
where each $w_{i}$ is a column of $W$ and $\alpha_{i}$ is a precision hyperparameter. Maximizing the marginal likelihood causes some $\alpha_{i}$ to be driven to infinity, effectively suppressing the corresponding vectors $w_{i}$.
