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



