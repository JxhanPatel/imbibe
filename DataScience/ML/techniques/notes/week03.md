# 3.1 Introduction to Clustering

## 1. Definition and Goals of Cluster Analysis

Unsupervised learning procedures are those that use unlabeled samples. In these cases, one has a set of $N$ observations $\{x_{1}, x_{2}, ..., x_{N}\}$ of a random $p$-vector $X$ having joint density $Pr(X)$. The goal is to directly infer the properties of this probability density without the help of a supervisor or teacher providing correct answers or a degree-of-error for each observation.

Cluster analysis, which is also referred to as data segmentation, groups or segments a collection of objects into subsets called clusters. The objective is to ensure that objects within each cluster are more closely related to one another than objects assigned to different clusters. An object can be described by a set of measurements or by its relation to other objects. In some cases, the goal is to arrange these clusters into a natural hierarchy by successively grouping them. Cluster analysis also serves as a descriptive statistic to ascertain whether data consists of distinct subgroups representing objects with substantially different properties.

<img width="806" height="551" alt="image" src="https://github.com/user-attachments/assets/0862557c-59d2-40a7-9c9a-c1ef4c975d0c" />

## 2. Motivations for Unsupervised Procedures

There are at least five basic reasons for interest in unsupervised procedures. First, collecting and labeling a large set of sample patterns can be surprisingly costly, such as accurately marking phonemes in recorded speech. Second, one may wish to train with large amounts of less expensive unlabeled data first and then use supervision to label the resulting groupings, which is appropriate for large data mining applications. Third, in many applications, the characteristics of patterns can change slowly with time, and improved performance can be achieved if these changes are tracked by a classifier running in an unsupervised mode. Fourth, unsupervised methods can be used to find features that are useful for subsequent categorization, representing a form of data-dependent smart preprocessing. Lastly, it may be valuable to gain insight into the structure of data in the early stages of an investigation, as discovering distinct subclasses may suggest altering the approach to designing a classifier.

## 3. Similarity and Dissimilarity Measures

The clustering problem requires a definition of what is meant by a natural grouping. Central to all goals of cluster analysis is the notion of the degree of similarity or dissimilarity between the individual objects being clustered. A clustering method attempts to group objects based on the definition of similarity supplied to it, which must come from subject matter considerations.

### 3.1 Distance-Based Similarity
The most obvious measure of similarity or dissimilarity between two samples is the distance between them. If distance is a good measure of dissimilarity, the distance between samples in the same cluster is expected to be significantly less than the distance between samples in different clusters. The choice of a distance threshold $d_{0}$ is critical; if $d_{0}$ is very large, all samples are assigned to one cluster, whereas if $d_{0}$ is very small, each sample forms an isolated singleton cluster.

<img width="976" height="452" alt="image" src="https://github.com/user-attachments/assets/dce55ad9-49c2-4fdb-8e44-b1c4de3db0d9" />

### 3.2 Impact of Coordinate Scaling
The results of clustering depend on the choice of distance as a measure of dissimilarity. Clusters defined by Euclidean distance are invariant to translations or rotations in feature space, which are rigid-body motions. However, they are not invariant to general linear transformations or simple scaling of coordinate axes. A simple rescaling can result in a completely different grouping of the data into clusters. 

One way to achieve invariance is to normalize the data prior to clustering, such as standardizing features to have zero mean and unit variance. However, if the data fall into well-separated clusters, normalization by a whitening transform for the full data may actually reduce the separation and be undesirable.

<img width="616" height="713" alt="image" src="https://github.com/user-attachments/assets/f379d6f6-9686-47da-9e02-e6b5c9169cd1" />

## 4. Types of Clustering Algorithms

Clustering algorithms are generally categorized into three distinct types:

*   **Combinatorial Algorithms:** These work directly on observed data without direct reference to an underlying probability model. They typically involve a mapping or encoder $C(i)$ that assigns the $i^{th}$ observation to the $k^{th}$ cluster to minimize a loss function such as within-cluster point scatter.
*   **Mixture Modeling:** This assumes that the data is an i.i.d. sample from a population characterized by a parameterized model taken to be a mixture of component density functions. Each component density describes one of the clusters.
*   **Mode Seeking:** Also known as bump hunters, these take a nonparametric perspective and attempt to directly estimate distinct modes of the probability density function. Individual clusters are defined by observations closest to each respective mode.

## 5. Performance and Validity

Unlike supervised learning, there is no direct measure of success, such as expected loss over a joint distribution, to judge the adequacy of unsupervised learning in particular situations. It is difficult to ascertain the validity of inferences drawn from most unsupervised learning algorithms. Analysts must often resort to heuristic arguments for judgments as to the quality of the results. The effectiveness of a procedure is often a matter of opinion and cannot be verified directly, which has led to a heavy proliferation of proposed methods. The most important part of obtaining success with clustering is specifying an appropriate dissimilarity measure, which is often more critical than the choice of the clustering algorithm itself.





---






# 3.2 K-means Clustering (Lloyd's algorithm)

## 1. Introduction and Goal

K-means clustering is a nonprobabilistic technique for identifying groups, or clusters, of data points in a multidimensional space. Suppose we have a data set $\{x_{1},...,x_{N}\}$ consisting of N observations of a random D-dimensional Euclidean variable x. The goal is to partition the data set into some number K of clusters. Intuitively, a cluster comprises a group of data points whose inter-point distances are small compared with the distances to points outside of the cluster.

## 2. The Objective Function (Distortion Measure)

To formalize the clustering problem, we introduce a set of D-dimensional vectors $\mu_{k}$, where $k=1,...,K,$ in which $\mu_{k}$ is a prototype associated with the $k^{th}$ cluster. We can think of the $\mu_{k}$ as representing the centres of the clusters. 

### 2.1 Notation for Assignments
For each data point $x_{n}$, we introduce a corresponding set of binary indicator variables $r_{nk}\in\{0,1\}$, where $k=1,...,K,$ describing which of the K clusters the data point $x_{n}$ is assigned to. If data point $x_{n}$ is assigned to cluster k, then $r_{nk}=1$, and $r_{nj}=0$ for $j\ne k$. This is known as the 1-of-K coding scheme.

### 2.2 The Loss Function
The objective function, sometimes called a distortion measure, is defined as the sum of the squares of the distances of each data point to its closest vector $\mu_{k}$,:
$$J = \sum_{n=1}^{N} \sum_{k=1}^{K} r_{nk} ||x_{n} - \mu_{k}||^{2}$$
The goal is to find values for the $\{r_{nk}\}$ and the $\{\mu_{k}\}$ so as to minimize J.

<img width="673" height="720" alt="image" src="https://github.com/user-attachments/assets/ca7d0c16-0f71-48ab-b01e-b45f599ddf4c" />

## 3. The K-means Algorithm (Lloyd's Algorithm)

The optimization is performed through an iterative procedure where each iteration involves two successive steps. These two stages correspond to the E (expectation) and M (maximization) steps of the EM algorithm.

### 3.1 Step-by-Step Procedure
1.  **Initialization:** Choose some initial values for the cluster prototypes $\{\mu_{k}\}$.
2.  **E Step (Assignment):** Minimize J with respect to $r_{nk}$, keeping the $\mu_{k}$ fixed.
3.  **M Step (Update):** Minimize J with respect to the $\mu_{k}$, keeping $r_{nk}$ fixed.
4.  **Convergence:** Repeat steps 2 and 3 until there is no further change in the assignments or until a maximum number of iterations is exceeded.

### 3.2 Mathematical Derivation of the Updates

#### The E Step (Determination of $r_{nk}$)
Because J is a linear function of $r_{nk}$, the optimization can be performed independently for each n. We choose $r_{nk}$ to be 1 for whichever value of k gives the minimum value of $||x_{n} - \mu_{k}||^{2}$. More formally:
$$r_{nk} = \begin{cases} 1 & \text{if } k = arg \min_{j} ||x_{n} - \mu_{j}||^{2} \\ 0 & \text{otherwise} \end{cases}$$
In other words, we simply assign the $n^{th}$ data point to the closest cluster centre.

#### The M Step (Optimization of $\mu_{k}$)
With $r_{nk}$ held fixed, J is a quadratic function of $\mu_{k}$. It is minimized by setting its derivative with respect to $\mu_{k}$ to zero:
$$2 \sum_{n=1}^{N} r_{nk} (x_{n} - \mu_{k}) = 0$$
Solving for $\mu_{k}$ gives:
$$\mu_{k} = \frac{\sum_{n} r_{nk} x_{n}}{\sum_{n} r_{nk}}$$
The denominator is equal to the number of points assigned to cluster k. The result is to set $\mu_{k}$ equal to the mean of all of the data points $x_{n}$ assigned to cluster k.

> [!IMPORTANT]
> Because each phase of the algorithm reduces the value of the objective function J, convergence of the algorithm is assured. However, it may converge to a local rather than global minimum of J.

## 4. Algorithmic Properties

### 4.1 Computational Complexity
The computational complexity of the K-means algorithm is $O(ndcT)$, where n is the number of d-dimensional patterns, c is the assumed number of clusters, and T is the number of iterations. In practice, the number of iterations is generally much less than the number of samples. 

### 4.2 Initialization Strategies
The results of K-means depend on the starting configuration. Deliberately poor initial values can lead to slow convergence. Better procedures include:
*   Choosing the cluster centres $\mu_{k}$ to be equal to a random subset of K data points.
*   Forward stepwise assignment: Choose a new centre $i_{k}$ to minimize the criterion given the centres already chosen at previous steps.

### 4.3 Choosing the Number of Clusters K
The within-cluster dissimilarity $W_{K}$ generally decreases as K increases. Methods to estimate the optimal $K★$ include:
*   **The "Kink" Method:** Identifying a sharp decrease in successive differences $W_{K} - W_{K+1}$.
*   **The Gap Statistic:** Comparing the curve $\log W_{K}$ to a curve obtained from data uniformly distributed over a rectangle containing the data. The optimal $K★$ is where the gap between the two curves is largest.

<img width="826" height="490" alt="image" src="https://github.com/user-attachments/assets/13ff8563-464c-4014-b55b-1c1be0b25391" />

## 5. Applications: Vector Quantization (VQ)

K-means clustering is a key tool for lossy data compression known as vector quantization. 

### 5.1 The Process
*   **Encoding Step:** The image or signal is broken into small blocks (e.g., $2 \times 2$ pixels). Each block is treated as a point in a D-dimensional space (e.g., $\mathbb{R}^{4}$). K-means is run in this space to find a set of centroids known as a codebook. Each block is then approximated by its nearest centroid, called a codeword.
*   **Storage:** For each block, only the identity k of the cluster to which it is assigned is stored. This requires $\log_{2} K$ bits per block.
*   **Decoding Step:** The approximate image is reconstructed by replacing the cluster indices with their corresponding codebook vectors.

<img width="795" height="437" alt="image" src="https://github.com/user-attachments/assets/1c402dc8-8f04-41f1-8b06-f466629cf93c" />

## 6. Generalizations and Variants

### 6.1 Online (Stochastic) K-means
A sequential update can be derived by applying the Robbins-Monro procedure to finding the roots of the regression function given by the derivatives of J. For each data point $x_{n}$ in turn, the nearest prototype $\mu_{k}$ is updated:
$$\mu_{k}^{new} = \mu_{k}^{old} + \eta_{n} (x_{n} - \mu_{k}^{old})$$
where $\eta_{n}$ is a learning rate parameter that typically decreases monotonically.

### 6.2 K-medoids
K-means is nonrobust to outliers because squared Euclidean distance places high influence on large distances. The K-medoids algorithm generalizes K-means for arbitrary dissimilarities $D(x_{i}, x_{i'})$. It restricts cluster centres to be one of the actual observations assigned to the cluster:
$$i_{k}^{★} = arg \min_{\{i: C(i)=k\}} \sum_{C(i')=k} D(x_{i}, x_{i'})$$
K-medoids is far more computationally intensive than K-means, as the update step requires $O(N_{k}^{2})$ calculations.

### 6.3 Relation to Gaussian Mixtures
K-means can be viewed as a particular nonprobabilistic limit of the EM algorithm applied to a mixture of Gaussians. Specifically, if we assume K mixture components each has a Gaussian density with a scalar covariance matrix $\sigma^{2}I$, then as $\sigma^{2} \rightarrow 0$, the "soft" assignments (responsibilities) of the EM algorithm become the "hard" assignments of K-means.


