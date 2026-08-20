# 7.1: Foundations of Classification & Distance-Based Learning

The field of pattern recognition is concerned with the automatic discovery of regularities in data through the use of computer algorithms and with the use of these regularities to take actions such as classifying the data into different categories. Applications in which the training data comprises examples of the input vectors along with their corresponding target vectors are known as supervised learning problems. Cases in which the aim is to assign each input vector to one of a finite number of discrete categories, are called classification problems. If the desired output consists of one or more continuous variables, then the task is called regression. The goal of regression is to predict the value of one or more continuous target variables $t$ given the value of a $D$-dimensional vector $x$ of input variables.

There is a class of pattern recognition techniques in which the training data points, or a subset of them, are kept and used also during the prediction phase. These are examples of memory-based methods that involve storing the entire training set in order to make predictions for future data points. They typically require a metric to be defined that measures the similarity of any two vectors in input space, and are generally fast to train but slow at making predictions for test data points.

<img width="991" height="270" alt="image" src="https://github.com/user-attachments/assets/0af0ca92-1109-494e-9668-3d1dd5ecdeda" />

A central aspect in virtually every pattern recognition problem is that of achieving a good representation, one in which the structural relationships among the components is simply and naturally revealed, and one in which the true (unknown) model of the patterns can be expressed. We seek a representation in which the patterns that lead to the same action are somehow close to one another, yet far from those that demand a different action.

> [!NOTE]
> If we divide a region of a space into regular cells, then the number of such cells grows exponentially with the dimensionality of the space. The problem with an exponentially large number of cells is that we would need an exponentially large quantity of training data in order to ensure that the cells are not empty. We have no hope of applying such a technique in a space of more than a few variables, and so we need to find a more sophisticated approach. The number of basis functions needs to grow rapidly, often exponentially, with the dimensionality $D$ of the input space.
>
>

# 7.1.1 Introduction to Binary Classification

For regression problems, the target variable $t$ was simply the vector of real numbers whose values we wish to predict. In the case of classification, there are various ways of using target values to represent class labels. For probabilistic models, the most convenient, in the case of two-class problems, is the binary representation in which there is a single target variable $t \in \{0,1\}$ such that $t=1$ represents class $\mathcal{C}_1$ and $t=0$ represents class $\mathcal{C}_2$. We can interpret the value of $t$ as the probability that the class is $\mathcal{C}_1$, with the values of probability taking only the extreme values of 0 and 1. Alternatively, the training data set can comprise $N$ input vectors $x_1, \dots, x_N$ with corresponding target values $t_1, \dots, t_N$ where $t_n \in \{-1, 1\}$.

As each pattern emerges, nature is in one or the other of the two possible states. We let $w$ denote the state of nature, with $\omega=\omega_1$ for one class and $\omega=\omega_2$ for the other. The distribution of a continuous random variable $x$ depends on the state of nature, which is expressed as $p(x\vert{}\omega_1)$ or $p(x\vert{}\omega_2)$. These are the class-conditional probability density functions, which are also sometimes called state-conditional probability densities. The prior probability that nature is in state $\omega_j$ is described by $P(\omega_j)$. Then the posterior probability $P(\omega_j\vert{}x)$ can be computed from $p(x\vert{}\omega_j)$ by Bayes' formula:

$$
P(\omega_j\vert{}x) = \frac{p(x\vert{}\omega_j)P(\omega_j)}{\sum_{i=1}^{c}p(x\vert{}\omega_i)P(\omega_i)}
$$

One of the most useful ways to represent pattern classifiers is in terms of a set of discriminant functions $g_i(x)$, $i=1, \dots, c$. The classifier is said to assign a feature vector $x$ to class $\omega_i$ if:

$$
g_i(x) \gt g_j(x)
$$

for all $j \ne i$.

The effect of any decision rule is to divide the feature space into $c$ decision regions, $\mathcal{R}_1, \dots, \mathcal{R}_c$. The regions are separated by decision boundaries, which are surfaces in feature space where ties occur among the largest discriminant functions.

A two-category classifier can implement the following decision boundary rule: decide $\omega_1$ if $g(x) \gt 0$ and $\omega_2$ if $g(x) \lt 0$. Alternatively, for the two-category case, we can write the single discriminant function as:

$$
g(x) = P(\omega_1\vert{}x) - P(\omega_2\vert{}x)
$$

$$
g(x) = \ln\frac{p(x\vert{}\omega_1)}{p(x\vert{}\omega_2)} + \ln\frac{P(\omega_1)}{P(\omega_2)}
$$

We decide $\omega_1$ if $g(x) \gt 0$ and $\omega_2$ if $g(x) \le 0$. An input vector $x$ is assigned to class $\mathcal{C}_1$ if $y(x) \ge 0$ and to class $\mathcal{C}_2$ otherwise, where the linear discriminant function is $y(x) = w^T x + w_0$. The likelihood ratio is given by $p(x\vert{}\omega_1)/p(x\vert{}\omega_2)$, and this ratio can range between zero and infinity. If the decision regions of a two-class classification problem are chosen to minimize the probability of misclassification, this probability will satisfy:

$$
p(\text{mistake})\le\int{p(x,\mathcal{C}_1)p(x,\mathcal{C}_2)}^{1/2}dx
$$

There are two ways in which a classification error can occur; either an observation $x$ falls in $\mathcal{R}_2$ and the true state of nature is $\omega_1$, or $x$ falls in $\mathcal{R}_1$ and the true state of nature is $\omega_2$. Since these events are mutually exclusive and exhaustive, the probability of error is:

$$
P(e) = P(x \in \mathcal{R}_2, \omega_1) + P(x \in \mathcal{R}_1, \omega_2) = \int_{\mathcal{R}_2} p(x\vert{}\omega_1)P(\omega_1)dx + \int_{\mathcal{R}_1} p(x\vert{}\omega_2)P(\omega_2)dx
$$

The threshold value $l^\star$ is chosen using design or training samples. No single threshold value $l^\star$ or decision boundary will serve to unambiguously discriminate between the two categories. The value $l^\star$ marked will lead to the smallest number of errors, on average. Similarly, a lightness threshold value $x^\star$ can be marked to divide the categories with the smallest number of errors on average.

# 7.1.2 K-Nearest Neighbours

To extend the K-nearest-neighbour technique to classification, we apply the K-nearest-neighbour density estimation technique to each class separately and then make use of Bayes' theorem. Suppose that we have a data set comprising $N_k$ points in class $\mathcal{C}_k$ with $N$ points in total, so that $\sum_k N_k = N$. If we wish to classify a new point $x$, we draw a sphere centred on $x$ containing precisely $K$ points irrespective of their class. Suppose this sphere has volume $V$ and contains $K_k$ points from class $\mathcal{C}_k$. Then, the estimate of the density associated with each class is:

$$
p(x\vert{}\mathcal{C}_k) = \frac{K_k}{N_k V}
$$

Similarly, the unconditional density is given by:

$$
p(x) = \frac{K}{N V}
$$

The prior probabilities for each class are given by:

$$
p(\mathcal{C}_k) = \frac{N_k}{N}
$$

Combining these density estimates and class priors using Bayes' theorem, we obtain the posterior probability of class membership:

$$
p(\mathcal{C}_k\vert{}x) = \frac{p(x\vert{}\mathcal{C}_k)p(\mathcal{C}_k)}{p(x)} = \frac{K_k}{K}
$$

<img width="1102" height="394" alt="image" src="https://github.com/user-attachments/assets/38976052-a0c3-48c9-8a98-6dc32debc943" />

If we wish to minimize the probability of misclassification, this is done by assigning the test point $x$ to the class having the largest posterior probability, corresponding to the largest value of $K_k/K$. Thus, to classify a new point, we identify the $K$ nearest points from the training data set and then assign the new point to the class having the largest number of representatives amongst this set. Ties can be broken at random. The particular case of $K=1$ is called the nearest-neighbour rule, because a test point is simply assigned to the same class as the nearest point from the training set.

An interesting property of the nearest-neighbour ($K=1$) classifier is that, in the limit $N \to \infty$, the error rate is never more than twice the minimum achievable error rate of an optimal classifier, i.e., one that uses the true class distributions, which is the Bayes rate. Both the K-nearest-neighbour method and the kernel density estimator require the entire training data set to be stored, leading to expensive computation if the data set is large. This effect can be offset, at the expense of some additional one-off computation, by constructing tree-based search structures to allow approximate near neighbours to be found efficiently without doing an exhaustive search of the data set.

<img width="1123" height="538" alt="image" src="https://github.com/user-attachments/assets/7b0693a9-7d87-43f4-b0b8-62b38d68cc59" />

To estimate $p(x)$ from $n$ training samples or prototypes, we can center a cell about $x$ and let it grow until it captures $k_n$ samples, where $k_n$ is some specified function of $n$. These samples are the $k_n$ nearest-neighbors of $x$. If the density is high near $x$, the cell will be relatively small, which leads to good resolution, whereas if the density is low, the cell will grow large.

**Voronoi Tessellations**

The nearest-neighbor algorithm partitions the feature space into cells consisting of all points closer to a given training point $x'$ than to any other training points. All points in such a cell are thus labelled by the category of the training point, creating a Voronoi tessellation of the space. In two dimensions, the nearest-neighbor algorithm leads to a partitioning of the input space into Voronoi cells. In three dimensions, the cells are three-dimensional, and the decision boundary resembles the surface of a crystal.

<img width="1063" height="567" alt="image" src="https://github.com/user-attachments/assets/4524d8da-d2f6-4561-bb2e-570a7fd0f610" />

The Voronoi cells induced by the single-nearest neighbor algorithm must always be convex. That is, for any two points $x_1$ and $x_2$ in a cell, all points on the line linking $x_1$ and $x_2$ must also lie in the cell.

**Error Rates & Bounds**

The nearest-neighbor rule is a sub-optimal procedure; its use will usually lead to an error rate greater than the minimum possible, the Bayes rate. When the maximum posterior probability $P(\omega_m\vert{}x)$ is close to unity, the nearest-neighbor selection is almost always the same as the Bayes selection. When $P(\omega_m\vert{}x)$ is close to $1/c$, so that all classes are essentially equally likely, the selections made by the nearest-neighbor rule and the Bayes decision rule are rarely the same, but the probability of error is approximately $1-1/c$ for both.

The k-nearest-neighbor rule classifies $x$ by assigning it the label most frequently represented among the $k$ nearest samples, taking a vote. If $k$ is fixed and the number $n$ of samples is allowed to approach infinity, then all of the $k$ nearest neighbors will converge to $x$.

<img width="944" height="535" alt="image" src="https://github.com/user-attachments/assets/431c37f5-4008-46af-8428-e0bf50f34e64" />

The $k$-nearest-neighbor rule (for $k$ odd in a two-class case) selects the dominant class $\omega_m$ with a probability of:

$$
\sum_{i=(k+1)/2}^{k}\binom{k}{i}P(\omega_m\vert{}x)^i [1-P(\omega_m\vert{}x)]^{k-i}
$$

If $k$ is allowed to increase with $n$ but is restricted by $k \lt a\sqrt{n}$, then the error rate $P_n(e) \to 0$ as $n \to \infty$.

**Computational Complexity**

In the most naive approach to finding the closest point to a test point $x$ for $k=1$, we inspect each stored point in turn, calculate its Euclidean distance to $x$, retaining the identity only of the current closest one. Each distance calculation is $O(d)$, and thus this search is $O(dn^2)$. Alternatively, with $N$ observations and $p$ predictors, nearest-neighbor classification requires $Np$ operations to find the neighbors per query point. A parallel nearest-neighbor circuit can perform search in constant, i.e., $O(1)$ time.

<img width="933" height="580" alt="image" src="https://github.com/user-attachments/assets/cf542c96-e850-4037-ad15-b228aaf6ef75" />

There are three general algorithmic techniques for reducing the computational burden in nearest-neighbor searches: computing partial distances, prestructuring, and editing the stored prototypes.

- **Partial Distance:** We calculate the distance using some subset $r$ of the full $d$ dimensions, and if this partial distance is too great we do not compute further.
- **Prestructuring:** We create some form of search tree in which prototypes are selectively linked. During classification, we compute the distance of the test point to one or a few stored entry or root prototypes and then consider only the prototypes linked to it.
- **Editing (Pruning/Condensing):** We eliminate useless prototypes during training. A simple method to reduce the $O(n)$ space complexity is to eliminate prototypes that are surrounded by training points of the same category label. This leaves the decision boundaries and hence the error unchanged, while reducing recall times.

The algorithm for nearest-neighbor editing is formalised as follows:

```plaintext
begin initialize j=0, D = data set, n = #prototypes
construct the full Voronoi diagram of D
do j <- j+1 for each prototype x_j'
Find the Voronoi neighbors of x_j'
if any neighbor is not from the same class as x_j' then mark x_j'
until j=n
Discard all points that are not marked
Construct the Voronoi diagram of the remaining (marked) prototypes
end

```

The complexity of this editing algorithm is $O(d^3 n^{\lfloor d/2 \rfloor} \ln n)$. One drawback of such pruned nearest neighbor systems is that one generally cannot add training data later, since the pruning step requires knowledge of all the training data ahead of time.

**Distance Metrics**

The nearest-neighbor classifier relies on a metric or distance function between patterns. A metric $D(\cdot,\cdot)$ is merely a function that gives a generalized scalar distance between two argument patterns, which must satisfy four properties for all vectors $a$, $b$, and $c$:

- **Non-negativity:** $D(a, b) \ge 0$
- **Reflexivity:** $D(a, b) = 0$ if and only if $a = b$
- **Symmetry:** $D(a, b) = D(b, a)$
- **Triangle inequality:** $D(a, c) \le D(a, b) + D(b, c)$

The Tanimoto metric is used in taxonomy, where the distance between two sets is defined as:

$$
D_{\text{Tanimoto}}(\mathcal{S}_1, \mathcal{S}_2) = \frac{n_1 + n_2 - 2n_{12}}{n_1 + n_2 - n_{12}}
$$

**Tangent Distance**

A related technique, called tangent distance, can be used to build invariance properties into distance-based methods such as nearest-neighbour classifiers. The uncritical use of the Euclidean metric cannot address the problem of translation invariance. The general approach in tangent distance classifiers is to use a measure of distance and a linear approximation to the arbitrary transforms.

During construction of the classifier we take each stored prototype $x'$ and perform each of the transformations $\mathcal{F}_i(x';\alpha_i)$ on it. We then construct a tangent vector $TV_i$ for each transformation:

$$
TV_i = \mathcal{F}_i(x';\alpha_i) - x'
$$

In this way we construct for each prototype $x'$ an $r \times d$ matrix $T$, consisting of the tangent vectors at $x'$. Each point in the subspace spanned by the $r$ tangent vectors passing through $x'$ represents the linearized approximation to the full combination of transforms.

<img width="654" height="699" alt="image" src="https://github.com/user-attachments/assets/01be2eac-a2ec-432e-b408-1ee77aad0113" />

Formally, the two-sided tangent distance allows both the stored prototype $x'$ and the test point $x$ to be transformed, and is defined as:

$$
D_{\text{2tan}}(x', x) = \min_{a,b}\left[ \Vert{} (x' + Ta) - (x + Sb) \Vert{} \right]
$$
