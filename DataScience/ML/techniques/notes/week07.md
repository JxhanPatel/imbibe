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





---







# 7.2: Decision Tree Algorithms & Structure

## 1. Fundamental Structure of Decision Trees

A directed decision tree, or simply tree, consists of a first or root node displayed at the top, connected by successive directional links or branches to other nodes. These nodes are similarly connected until terminal or leaf nodes are reached, which have no further links.

The classification of a particular pattern begins at the root node, which asks for the value of a particular attribute of the pattern. The different links from the root node correspond to the different possible values of the attribute. Based on the answer, we follow the appropriate link to a subsequent or descendent node. In any valid decision tree, the links must be mutually distinct and exhaustive; that is, one and only one link will be followed. Each leaf node bears a category label, and the test pattern is assigned the category of the leaf node reached.

<img width="762" height="578" alt="image" src="https://github.com/user-attachments/assets/d5efe1bb-fc0b-40e1-bbce-1926be1d8619" />

### 1.1 Dimensionality and Splitting

* **Binary Trees:** Every decision, and hence every tree, can be represented using just binary decisions. Because of the universal expressive power of binary trees and the comparative simplicity in training, tree algorithms often concentrate on such trees.
* **Branching Factor:** The number of links descending from a node is called the node's branching factor or branching ratio, denoted $B$.
* **Hyperplane Decision Boundaries:** For numerical data, the test at each node has the form "is $X_j \le s$?". This partition of the feature space is made by lines that are parallel to the coordinate axes. This leads to hyperplane decision boundaries that are perpendicular to the coordinate axes, dividing the space into decision regions.
* **Monothetic vs. Polythetic:** Trees in which each test is based on a single property are called monothetic. If the query at any of the nodes involves two or more properties, the tree is called polythetic.

<img width="769" height="590" alt="image" src="https://github.com/user-attachments/assets/fdd59b9e-6396-4659-a5a9-fc37a49a477c" />

<img width="801" height="452" alt="image" src="https://github.com/user-attachments/assets/484c92b7-3f7c-4d61-b98f-864364f21626" />

> [!NOTE]
> A multi-way split can fragment the data too quickly, leaving insufficient data at the next level down. Since multiway splits can always be achieved by a series of binary splits, the latter are generally preferred.

# 7.2.1 Introduction to Decision Trees

### 1. Conceptual Framework

It is natural and intuitive to classify a pattern through a sequence of questions, in which the next question asked depends on the answer to the current question. This "20-questions" approach is particularly useful for nominal, non-metric data, since all of the questions can be asked in a "yes/no" or "true/false" or $\text{value(property)} \in \text{set of values}$ style that does not require any notion of metric.

In some cases, patterns should be represented as vectors of real-valued numbers, in others ordered lists of attributes, and in others descriptions of parts and their relations. We seek a representation in which the patterns that lead to the same action are somehow "close" to one another, yet "far" from those that demand a different action. The extent to which we create or learn a proper representation and how we quantify near and far apart determines the success of the pattern classifier.

### 2. Advantages of Decision Trees

* **Human Interpretability:** The partition is fully described by a single tree, making it readily interpretable by humans because it corresponds to a sequence of binary decisions applied to individual variables.
* **Logical Rules:** The information in a tree can be rendered as logical expressions. We can interpret the decision for any particular test pattern as the conjunction of decisions along the path to its corresponding leaf node.
* **Example:** The pattern $\mathbf{x} = \{\text{sweet, yellow, thin, medium}\}$ is classified as Banana because it is $(\text{color} = \text{yellow}) \text{ AND } (\text{shape} = \text{thin})$.
* **Example:** We can obtain clear interpretations of the categories themselves by creating logical descriptions using conjunctions and disjunctions, such as:

$$\text{Apple} = (\text{green AND medium}) \text{ OR } (\text{red AND medium})$$




* **Computational Efficiency:** Trees lead to rapid classification, employing a sequence of typically simple queries.
* **Prior Knowledge:** Trees provide a natural way to incorporate prior knowledge from human experts, which is of greatest use when the classification problem is fairly simple and the training set is small.
* **Mixed Data Types:** They naturally incorporate mixtures of numeric and categorical predictor variables and missing values.
* **Invariance:** They are invariant under strictly monotone transformations of the individual predictors; as a result, scaling or general transformations are not issues, and they are immune to the effects of predictor outliers.
* **Feature Selection:** They perform internal feature selection as an integral part of the procedure, rendering them resistant to the inclusion of many irrelevant predictor variables.

### 3. Structural Limitations

* **Instability (High Variance):** Often a small change in the data can result in a very different series of splits, making interpretation precarious. The major reason for this instability is the hierarchical nature of the process: the effect of an error in the top split is propagated down to all of the splits below it. The alteration of even a single training point can lead to radically different decisions overall due to the discrete and greedy nature of tree creation.
* **Lack of Smoothness:** The tree model produces piecewise-constant predictions with discontinuities at the split boundaries, which can degrade performance in the regression setting where we would normally expect the underlying function to be smooth.
* **Difficulty in Capturing Additive Structure:** If the underlying target is additive, e.g., $Y = c_1 I(X_1 < t_1) + c_2 I(X_2 < t_2) + \epsilon$, a binary tree must make its first split on $X_1$ near $t_1$, and then at the next level split both resulting nodes on $X_2$ at $t_2$. If there were ten rather than two additive effects, it would take many fortuitous splits to recreate the structure, and the data analyst would be hard pressed to recognize it in the estimated tree.

# 7.2.2 Decision Tree Algorithm

### 1. CART Recursive Binary Splitting (Regression)

Our training data consists of $p$ inputs and a response, for each of $N$ observations: $(x_i, y_i)$ for $i=1,2,\dots,N$ with $x_i=(x_{i1}, x_{i2}, \dots, x_{ip})$. The algorithm must automatically decide on the splitting variables and split points.

Suppose we have a partition into $M$ regions $R_1, R_2, \dots, R_M$ and we model the response as a constant $c_m$ in each region:

$$f(x) = \sum_{m=1}^{M} c_m I(x \in R_m)$$

If we minimize the sum of squares $\sum (y_i - f(x_i))^2$, the optimal $\hat{c}_m$ is the average of $y_i$ in region $R_m$:

$$\hat{c}_m = \text{ave}(y_i \mid x_i \in R_m)$$

#### 1.1 Greedy Split Search

We proceed with a greedy algorithm. Starting with all of the data, consider a splitting variable $j$ and split point $s$, and define the pair of half-planes:

$$R_1(j,s) = \{X \mid X_j \le s\} \quad \text{and} \quad R_2(j,s) = \{X \mid X_j > s\}$$

We seek the splitting variable $j$ and split point $s$ that solve:

$$\min_{j,s} \left[ \min_{c_1} \sum_{x_i \in R_1(j,s)} (y_i - c_1)^2 + \min_{c_2} \sum_{x_i \in R_2(j,s)} (y_i - c_2)^2 \right]$$

For any choice of $j$ and $s$, the inner minimization is solved by:

$$\hat{c}_1 = \text{ave}(y_i \mid x_i \in R_1(j,s)) \quad \text{and} \quad \hat{c}_2 = \text{ave}(y_i \mid x_i \in R_2(j,s))$$

Having found the best split, we partition the data into the two resulting regions and repeat the splitting process on each of the two regions, continuing recursively.

### 2. Classification Trees and Node Impurity Measures

For classification, the target is a outcome taking values $1, 2, \dots, K$. In a node $m$, representing a region $R_m$ with $N_m$ observations, the proportion of class $k$ observations in node $m$ is defined as:

$$\hat{p}_{mk} = \frac{1}{N_m} \sum_{x_i \in R_m} I(y_i = k)$$

We classify the observations in node $m$ to class $k(m) = \arg\max_k \hat{p}_{mk}$, which is the majority class in node $m$.

<img width="825" height="493" alt="image" src="https://github.com/user-attachments/assets/bb8529e7-07f4-47f0-8cfa-c94b1942d259" />

The node impurity measure $Q_m(T)$ can be computed in three common ways:

#### 2.1 Misclassification Error

$$1 - \hat{p}_{mk(m)}$$

#### 2.2 Gini Index

$$\sum_{k \ne k'} \hat{p}_{mk} \hat{p}_{mk'} = \sum_{k=1}^{K} \hat{p}_{mk} (1 - \hat{p}_{mk})$$

Alternatively, in regions $\mathcal{R}_{\tau}$, it is formulated as:

$$Q_{\tau}(T) = \sum_{k=1}^{K} p_{\tau k}(1 - p_{\tau k})$$

#### 2.3 Cross-Entropy or Deviance

$$-\sum_{k=1}^{K} \hat{p}_{mk} \ln \hat{p}_{mk}$$

Alternatively, in regions $\mathcal{R}_{\tau}$, it is formulated as:

$$Q_{\tau}(T) = \sum_{k=1}^{K} p_{\tau k} \ln p_{\tau k}$$

These both vanish for $p_{\tau k} = 0$ and $p_{\tau k} = 1$ and have a maximum at $p_{\tau k} = 0.5$.

> [!IMPORTANT]
> Cross-entropy and the Gini index are more sensitive to changes in the node probabilities than the misclassification rate. Both Gini and cross-entropy should be used when growing the tree, while misclassification rate is typically used to guide cost-complexity pruning.

### 3. Stopping Criteria

Growing the tree fully until each leaf node corresponds to the lowest impurity typically overfits the data. To prevent this, several stopping rules can be implemented:

* **Impurities Reduction Threshold:** Stop splitting if the best candidate split at a node reduces the impurity by less than some pre-set threshold $\beta$, i.e., if $\max \Delta i(s) \le \beta$.
* **Node Size Threshold:** Stop when a node represents fewer than some threshold number of points (e.g., 10), or some fixed percentage of the total training set (e.g., 5%).
* **Global Complexity Penalty:** Split until a minimum in a global criterion function is reached:

$$\alpha \cdot \text{size} + \sum_{\text{leaf nodes}} i(N)$$



where $\text{size}$ represents the number of nodes or links, and $\alpha$ is a positive complexity penalty.
* **Statistical Significance:** Stop splitting if a candidate split does not reduce the impurity significantly according to a statistical test, such as a chi-squared test comparing the split to the null hypothesis of a random split.

### 4. Cost-Complexity Pruning

The preferred strategy is to grow a large tree $T_0$, stopping the splitting process only when some minimum node size (such as 5) is reached, and then prune $T_0$ back using cost-complexity pruning.

We define a subtree $T \subset T_0$ to be any tree obtained by collapsing any number of non-terminal nodes of $T_0$. We index terminal nodes by $m$, representing region $R_m$. Let $\vert{}T\vert{}$ denote the number of terminal nodes in $T$. The cost-complexity criterion is:

$$C_{\alpha}(T) = \sum_{m=1}^{\vert{}T\vert{}} N_m Q_m(T) + \alpha \vert{}T\vert{}$$

Here, $N_m = \{x_i \in R_m\}$.
$Q_m(T)$ is the node impurity.
The tuning parameter $\alpha \ge 0$ governs the tradeoff between tree size and its goodness of fit to the data.

#### 4.1 Weakest Link Pruning

To find $T_{\alpha}$, we use weakest link pruning: we successively collapse the internal node that produces the smallest per-node increase in $\sum_{m} N_m Q_m(T)$, and continue until we produce the single-node (root) tree. This yields a finite sequence of subtrees that must contain the optimal $T_{\alpha}$. We choose the optimal parameter value $\hat{\alpha}$ via five- or tenfold cross-validation to minimize the cross-validated sum of squares. Our final tree is $T_{\hat{\alpha}}$.

### 5. Alternative Tree-Building Algorithms

#### 5.1 ID3

ID3 is intended for use with nominal (unordered) inputs only. If the problem involves real-valued variables, they are binned into intervals, each interval being treated as an unordered nominal attribute. Every split has a branching factor $B_j$, where $B_j$ is the number of discrete attribute bins of the chosen variable $j$. The algorithm continues until all nodes are pure or there are no more variables to split on.

#### 5.2 C4.5

C4.5 is the successor and refinement of ID3. Real-valued variables are treated the same as in CART, while multi-way ($B > 2$) splits are used with nominal data. It uses pruning heuristics based on the statistical significance of splits.

##### Rule Pruning (C4.5Rules)

C4.5 has a provision for pruning based on rules derived from the learned tree. Each leaf node has an associated rule consisting of the conjunction of the decisions leading from the root node to that leaf. A technique called C4.5Rules deletes redundant antecedents in these rules, which can simplify the description and prune information corresponding to nodes near the root.

### 6. Algorithmic Complexity

Suppose we have $n$ training patterns in $d$ dimensions in a two-category problem, and we construct a binary tree based on splits parallel to the feature axes.

#### 6.1 Training Phase Complexity

* **Sorting:** At the root node (level 0), we must first sort the training data, taking $\mathcal{O}(n \ln n)$ operations for each of the $d$ features.
* **Impurity Calculations:** The impurity calculation takes $\mathcal{O}(n) + (n-1)\mathcal{O}(d)$ since we examine $n-1$ possible splitting points.
* **Root Node Total:** The root node split has a time complexity of $\mathcal{O}(d n \ln n)$.
* **Subsequent Levels:** Assuming on average that half the training points are sent to each of the two branches, splitting the two nodes in level 1 takes $\mathcal{O}\left(d \cdot \frac{n}{2} \ln\left(\frac{n}{2}\right)\right) \times 2 = \mathcal{O}\left(d n \ln\left(\frac{n}{2}\right)\right)$.
* **Total Average Complexity:** For $\mathcal{O}(\ln n)$ levels, the total average time complexity of training is:

$$\mathcal{O}(d n (\ln n)^2)$$



#### 6.2 Recall Phase Complexity

The time complexity for recall is simply the depth of the tree, which is:

$$\mathcal{O}(\ln n)$$

The space complexity, representing the number of nodes (assuming a single training point per leaf node), is:

$$1 + 2 + 4 + \dots + \frac{n}{2} \approx n \implies \mathcal{O}(n)$$

#### 6.3 Summary Table of Tree Algorithms

| Feature | CART | ID3 | C4.5 |
| --- | --- | --- | --- |
| **Splitting Type** | Binary splits ($B = 2$) | Multi-way splits based on bins ($B_j$) | Multi-way splits ($B > 2$) for nominal data; binary for real |
| **Data Types** | Mixed: continuous and categorical | Nominal (unordered) inputs only | Mixed: continuous and categorical |
| **Impurity Criteria** | Gini index, cross-entropy, deviance, MSE | Gain ratio impurity | Gain ratio impurity |
| **Pruning Method** | Cost-complexity weakest link pruning | None in standard implementation | Pruning based on statistical significance and rule-based simplification |
| **Missing Values** | Surrogate splits | Omitted/Not standard | Weights sub-models by probability of decisions |

> [!CAUTION]
> The greedy partitioning algorithm tends to favor categorical predictors with many levels $q$, because the number of partitions grows exponentially in $q$. This can lead to severe overfitting if $q$ is large, and such variables should be avoided.
