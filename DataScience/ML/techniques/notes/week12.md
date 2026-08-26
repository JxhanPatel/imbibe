# 12.1 Classification Loss Functions & Models

## 12.1.1 Classification Loss Functions

In supervised pattern classification, we seek a function $f(x)$ or discriminant $y(x)$ to predict the class label $t \in \{-1, +1\}$ or $t \in \{0, 1\}$. To evaluate the quality of our model's predictions, we define a loss function $L(y, f(x))$ or $L(t, y(x))$ that penalizes prediction errors. The most common loss functions used for two-class classification are defined below as functions of the "margin" $z = yf(x)$ or $z = ty(x)$ , where $y \in \{-1, +1\}$ is the true label and $f(x)$ is the continuous prediction.

### 1. Misclassification Error (0-1 Loss)
The misclassification loss assigns a unit penalty to all incorrect decisions and no penalty to correct decisions:
$$L_{01}(yf(x)) = I(yf(x) < 0)$$
where the indicator function $I(\cdot)$ is equal to 1 if the condition is met, and 0 otherwise.

### 2. Binomial Deviance (Logistic Loss / Cross-Entropy)
The binomial negative log-likelihood or deviance loss function, interpreting $f(x)$ as the logit transform of the class probabilities, is defined as:
$$L_{LR}(yf(x)) = \ln(1 + \exp(-2yf(x)))$$
Or, under target coding $t \in \{0, 1\}$ and network output $y \in [0, 1]$:
$$E(w) = -\sum_{n=1}^N \{t_n \ln y_n + (1 - t_n) \ln(1 - y_n)\}$$

### 3. Support Vector Hinge Loss
The hinge loss function, so-called because of its shape, is used in support vector machines and is defined as:

$$
L_{SV}(yf(x)) = [1 - yf(x)]_+ = \max(0, 1 - yf(x))
$$

where $[\cdot]_+$ denotes the positive part.

### 4. Exponential Loss (Boosting Loss)
The exponential error function minimized by the AdaBoost algorithm is defined as:
$$L_{Exp}(yf(x)) = \exp(-yf(x))$$

### 5. Squared Error Loss
The squared-error loss, sometimes used as a surrogate for misclassification error in classification, is given by:
$$L_{Sq}(yf(x)) = [y - f(x)]^2 = [1 - yf(x)]^2$$

### 6. "Huberised" Square Hinge Loss
A modified version of the quadratic loss that combines the properties of binomial deviance, quadratic loss, and the SVM hinge loss is defined as:

$$
L_{Hub}(yf(x)) = \begin{cases} [1 - yf(x)]_+^2 & \text{if } yf(x) \ge -1 \\ -4yf(x) & \text{if } yf(x) < -1 \end{cases}
$$

### Population Minimizers of Classification Loss Functions

At the population level, we seek the function $f★(x)$ that minimizes the expected loss $\mathbb{E}_{Y|x}[L(Y, f(x))]$. The population minimizers for these loss functions are summarized in the table below:

| Loss Function | Mathematical Form $L[y, f(x)]$ | Population Minimizer $f★(x)$ |
| :--- | :--- | :--- |
| **Binomial Deviance** | $\ln(1 + e^{-2yf(x)})$ | $f★(x) = \frac{1}{2} \ln \frac{Pr(Y=+1 \mid x)}{Pr(Y=-1 \mid x)}$ (Half the log-odds ratio) |
| **SVM Hinge Loss** | $[1 - yf(x)]_+$ | $f★(x) = \text{sign}\left[ Pr(Y=+1 \mid x) - \frac{1}{2} \right]$ (The Bayes decision classifier) |
| **Squared Error** | $[y - f(x)]^2 = [1 - yf(x)]^2$ | $f★(x) = 2Pr(Y=+1 \mid x) - 1$ (Linear transformation of probability) |
| **Huberised Square Hinge** | $[1-yf(x)]_+^2$ for $yf(x) \ge -1$; $-4yf(x)$ otherwise | $f★(x) = 2Pr(Y=+1 \mid x) - 1$ (Linear transformation of probability) |
| **Exponential Loss** | $\exp(-yf(x))$ | $f★(x) = \frac{1}{2} \ln \frac{Pr(Y=+1 \mid x)}{Pr(Y=-1 \mid x)}$ (Half the log-odds ratio) |


## 12.1.2 SVM and Logistic Loss

The comparison between support vector machines (SVM) and regularized logistic regression highlights fundamental differences in optimization behavior and model sparsity.

### 1. The Mathematical Source of Sparsity
Both the hinge loss and binomial deviance are monotone decreasing functions of the margin $yf(x)$, acting as continuous approximations to the discontinuous misclassification loss.
*   **SVM Hinge Loss:** The hinge loss is exactly zero for observations that lie on the correct side of the margin boundary, satisfying $yf(x) \ge 1$. This flat region in the loss function means that these points have zero gradient during optimization. Consequently, their associated Lagrange multipliers are zero ($a_n = 0$), and they are ignored when making predictions. This leads to sparse solutions where only a subset of data points (the support vectors, for which $yf(x) \le 1$ and $a_n > 0$) determine the decision boundary.
*   **Logistic Loss (Binomial Deviance):** The binomial deviance has a non-zero gradient everywhere, although it asymptotically approaches zero as the margin $yf(x) \to \infty$. Because there is no flat region, every single training observation in the dataset continues to exert some influence on the final decision boundary. Consequently, logistic regression does not yield sparse solutions in terms of data points.

### 2. Separable Case Convergence
For linearly separable datasets, the maximum likelihood solution for logistic regression is undefined because the log-likelihood can be driven to zero by letting the coefficient vector magnitude $\|w\| \to \infty$. However, it can be shown that as the regularization parameter $\lambda \to 0$, the regularized logistic regression coefficient vector (renormalized) $\hat{\beta}_{\lambda} / \|\hat{\beta}_{\lambda}\|$ converges exactly to the optimal separating direction of the maximum margin classifier (the SVM solution).


## 12.1.3 Perceptron and Boosting Loss

### 1. Rosenblatt's Perceptron Loss
The perceptron learning algorithm seeks a separating hyperplane by minimizing a piecewise linear criterion:
$$D(w, w_0) = -\sum_{i \in \mathcal{M}} y_i (x_i^T w + w_0)$$
where $\mathcal{M}$ indexes the set of misclassified points, and $y_i \in \{-1, +1\}$.
*   **Geometric interpretation:** The quantity is proportional to the distance of the misclassified points to the decision boundary defined by $w^T x + w_0 = 0$.
*   **Zero-Gradient Pitfall of Misclassification Count:** A natural choice of error function would be the total number of misclassified patterns. However, this is a piecewise constant function of $w$ with step discontinuities, meaning its gradient is zero almost everywhere, which prevents gradient descent. The Perceptron criterion resolves this by being continuous and piecewise linear.
*   **Update Rule:** Using stochastic gradient descent, the parameters are updated after visiting each misclassified observation:
    $$w^{(\tau+1)} = w^{(\tau)} + \eta x_i y_i$$
    $$w_0^{(\tau+1)} = w_0^{(\tau)} + \eta y_i$$

### 2. Boosting Loss (AdaBoost.M1)
The AdaBoost algorithm is equivalent to forward stagewise additive modeling optimizing the exponential loss function:
$$L_{Exp}(y, f(x)) = \exp(-y f(x))$$

*   **Derivation of the Population Minimizer:**
    To find the population minimizer $f★(x)$, we minimize the conditional expectation of the exponential loss:
    $$\mathbb{E}_{Y|x}[e^{-Y f(x)}] = Pr(Y=1 \mid x) e^{-f(x)} + Pr(Y=-1 \mid x) e^{f(x)}$$
    Taking the derivative with respect to $f(x)$ and setting it to zero:
    $$-Pr(Y=1 \mid x) e^{-f(x)} + Pr(Y=-1 \mid x) e^{f(x)} = 0$$
    $$e^{2f(x)} = \frac{Pr(Y=1 \mid x)}{Pr(Y=-1 \mid x)} = \frac{Pr(Y=1 \mid x)}{1 - Pr(Y=1 \mid x)}$$
    $$f★(x) = \frac{1}{2} \ln \frac{Pr(Y=1 \mid x)}{Pr(Y=-1 \mid x)}$$
    Thus, the additive expansion produced by AdaBoost is estimating one-half the log-odds of the posterior class probabilities. This justifies using its sign as the final classification decision rule.

*   **Robustness and Sensitivity to Outliers:**
    *   For large negative values of the margin $yf(x)$, the binomial deviance increases only linearly with $yf(x)$, whereas the exponential loss function increases exponentially.
    *   This means that the exponential loss function places an extremely heavy penalty on observations with large negative margins. In noisy settings where the Bayes error rate is not close to zero, or where class labels in the training set are mislabeled (outliers), these noisy observations will dominate the optimization.
    *   Consequently, the performance of AdaBoost dramatically degrades in the presence of outliers. Binomial deviance is far more robust because it spreads the influence more evenly among all data points.
*   **Normalization Limitations:**
    *   The exponential loss function $e^{-yf}$ is not a proper log-likelihood because it does not correspond to the logarithm of any well-defined probability mass function for a binary random variable $Y \in \{-1, +1\}$ (the corresponding conditional distribution $p(t|x)$ cannot be correctly normalized).
    *   Furthermore, unlike cross-entropy (binomial deviance), the exponential loss does not generalize easily to multiclass classification problems having $K > 2$ classes.


---

# 12.2 Neural Networks & Parameter Calculation

## 12.2.1 Neural Networks

Feed-forward neural networks, also known as multilayer perceptrons (MLPs), are parametric nonlinear models used for regression and classification.

### 1. Functional Form and Transformations
The standard two-layer network (often called a single-hidden-layer network because it contains one layer of hidden units) performs a series of functional transformations. First, we construct $M$ linear combinations of the input variables $x_1, \dots, x_D$:
$$a_j = \sum_{i=1}^D w_{ji}^{(1)} x_i + w_{j0}^{(1)} = \sum_{i=0}^D w_{ji}^{(1)} x_i$$
where $j = 1, \dots, M$, and the superscript (1) indicates the first layer of weights. The parameters $w_{ji}^{(1)}$ are weights, $w_{j0}^{(1)}$ are biases, and $a_j$ are activations. We define an additional dummy input $x_0 = 1$ to absorb the biases into the summation.

Each activation $a_j$ is transformed using a differentiable, nonlinear activation function $h(\cdot)$ (typically a sigmoidal function such as the logistic sigmoid or the hyperbolic tangent 'tanh') to give the hidden unit outputs:
$$z_j = h(a_j)$$

These hidden unit values are then linearly combined to give the output unit activations:
$$a_k = \sum_{j=1}^M w_{kj}^{(2)} z_j + w_{k0}^{(2)} = \sum_{j=0}^M w_{kj}^{(2)} z_j$$
where $k = 1, \dots, K$, and $K$ is the total number of outputs.

Finally, the output activations are transformed using an appropriate activation function to give the network outputs $y_k = g_k(a_k)$:
*   **Regression:** Identity activation function $g_k(a_k) = a_k$.
*   **Multiple Independent Binary Classification:** Logistic sigmoid activation function $g_k(a_k) = \sigma(a_k)$.
*   **Multiclass Classification:** Softmax activation function:
    $$g_k(a_k) = \frac{\exp(a_k)}{\sum_j \exp(a_j)}$$

Combining these stages, the overall network function takes the form:
$$y_k(x, w) = g_k\left( \sum_{j=0}^M w_{kj}^{(2)} h\left( \sum_{i=0}^D w_{ji}^{(1)} x_i \right) \right)$$

<img width="1009" height="477" alt="image" src="https://github.com/user-attachments/assets/8330c7e4-f6f5-40a3-ad83-562834a97727"/>

> [!NOTE]
> The term "multilayer perceptron" is really a misnomer. The network comprises layers of logistic regression models with continuous, differentiable nonlinearities (sigmoids), rather than multiple Rosenblatt perceptrons with discontinuous step nonlinearities. This differentiability is what allows gradient-based learning via backpropagation.

### 2. Universal Approximation Theorem
The approximation properties of feed-forward networks are very general, rendering them universal approximators. A two-layer network with linear output units can uniformly approximate any continuous function on a compact input domain to arbitrary accuracy, provided the network has a sufficiently large number of hidden units $M$ and the activation function $h(\cdot)$ is non-polynomial.

### 3. Weight-Space Symmetries
A distinctive property of feed-forward networks is that multiple distinct choices for the weight vector $w$ can give rise to the same input-output mapping function.
*   **Sign-Flip Symmetry:** For hidden units utilizing the hyperbolic tangent 'tanh' activation function (which is an odd function satisfying $\tanh(-a) = -\tanh(a)$ changing the sign of all weights and the bias leading into a particular hidden unit can be exactly compensated by changing the sign of all weights leading out of that hidden unit. For $M$ hidden units, there are $2^M$ such equivalent weight configurations.
*   **Interchange Symmetry:** Interchanging the values of all weights and biases leading into and out of a particular hidden unit with those of another hidden unit does not alter the input-output mapping. For $M$ hidden units, there are $M!$ equivalent orderings.
*   **Total Symmetry Factor:** The overall weight-space symmetry factor for a two-layer network is $M! 2^M$. For networks with $L$ hidden layers, the total symmetry is the product of such factors:
    $$\prod_{l=1}^L M_l! 2^{M_l}$$


## 12.2.2 Computing Parameters of a Neural Network

Computing the total number of parameters (weights and biases) is essential to analyze a network's complexity and expressive power.

### 1. Mathematical Formulation for a Standard Two-Layer Network
Consider a standard fully connected two-layer network with $D$ input units (excluding bias), $M$ hidden units, and $K$ output units.
*   **Layer 1 (Input-to-Hidden):**
    Each of the $M$ hidden units is connected to all $D$ inputs and has an associated bias.
    $$\text{Number of Weights} = M \times D$$
    $$\text{Number of Biases} = M \times 1 = M$$
    $$\text{Layer 1 Parameters} = M(D + 1)$$
*   **Layer 2 (Hidden-to-Output):**
    Each of the $K$ output units is connected to all $M$ hidden units and has an associated bias.
    $$\text{Number of Weights} = K \times M$$
    $$\text{Number of Biases} = K \times 1 = K$$
    $$\text{Layer 2 Parameters} = K(M + 1)$$
*   **Total Parameters ($W$):**
    $$W = M(D + 1) + K(M + 1)$$


### 2. Case Study: ZIP Code Character Recognition
To illustrate how parameter counts and connections scale under different architectural constraints, we examine five different networks trained on the benchmark ZIP code database of handwritten digits ($16 \times 16$ grayscale images, $256$ input pixels, $10$ output classes):

<img width="822" height="773" alt="image" src="https://github.com/user-attachments/assets/9c5080d5-bd98-4ef4-85e8-58ae4597bd6f" />

#### Net-1: Single-Layer Network (Multinomial Logistic Regression)
*   **Architecture:** No hidden layer; fully connected from inputs to output units.
*   **Inputs ($D$):** $256$ pixels (plus 1 bias).
*   **Outputs ($K$):** $10$ units.
*   **Parameter Calculation:**
    $$W = 10 \times (256 + 1) = 2570 \text{ weights}$$
*   **Links (Connections):** $2570$.
*   **Independent Parameters:** $2570$.

#### Net-2: Two-Layer Network (Vanilla MLP)
*   **Architecture:** Fully connected with $1$ hidden layer containing $12$ hidden units.
*   **Inputs ($D$):** $256$ pixels.
*   **Hidden Units ($M$):** $12$ units.
*   **Outputs ($K$):** $10$ units.
*   **Parameter Calculation:**
    - Input-to-Hidden: $12 \times (256 + 1) = 3084$ parameters.
    - Hidden-to-Output: $10 \times (12 + 1) = 130$ parameters.
    $$W_{\text{total}} = 3084 + 130 = 3214 \text{ parameters}$$
*   **Links (Connections):** $3214$.
*   **Independent Parameters:** $3214$.

#### Net-3: Locally Connected Network
*   **Architecture:** Two hidden layers with local connectivity (no weight sharing). Each unit in a layer connects only to a local patch in the layer below.
*   **Hidden Layer 1:** $8 \times 8 = 64$ units. Each unit takes input from a local $3 \times 3$ patch (9 pixels) of the input layer (+ 1 bias).
    - First layer parameters: $64 \times (9 + 1) = 640$ parameters.
*   **Hidden Layer 2:** $4 \times 4$ array, locally connected to $5 \times 5$ patches.
*   **Links (Connections):** $1226$.
*   **Independent Parameters:** $1226$. (By replacing global connections with local patches, the parameter count drops from 3214 to 1226 while maintaining local feature extraction).

#### Net-4: Locally Connected with Weight Sharing (Convolutional Network)
*   **Architecture:** Two hidden layers with local connectivity and weight sharing.
*   **Hidden Layer 1:** Two $8 \times 8$ feature maps. Each unit takes input from a $3 \times 3$ patch.
*   **Weight Sharing Constraint:** All 64 units in a single feature map share the exact same set of $3 \times 3 = 9$ weights, though they each maintain their own unique bias parameter.
*   **Parameter Calculation:**
    - First layer links: $2 \times 64 \times (9 + 1) = 1280$ links.
    - First layer independent parameters: $2 \times 9$ shared weights $+ 2 \times 64$ biases $= 146$ parameters.
*   **Links (Connections):** $2266$.
*   **Independent Parameters:** $1132$. (Weight sharing reduces the number of independent parameters to about half the number of links).

#### Net-5: Two-Layer Locally Connected with Two Levels of Weight Sharing
*   **Architecture:** Complex hierarchical local connectivity with weight sharing in both hidden layers.
*   **Hidden Layer 1:** Two $8 \times 8$ feature maps (local $3 \times 3$ patches, shared weights).
*   **Hidden Layer 2:** Four $4 \times 4$ feature maps. Each unit connects to a local $5 \times 5$ patch in the layer below, with shared weights.
*   **Links (Connections):** $5194$.
*   **Independent Parameters:** $1060$.

#### Summary of ZIP Code Network Performance and Sizing

| Network Architecture | Total Links | Independent Weights | % Correct on Test Data |
| :--- | :--- | :--- | :--- |
| **Net-1**: Single-layer network | $2570$ | $2570$ | $80.0\%$ |
| **Net-2**: Two-layer network (MLP) | $3214$ | $3214$ | $87.0\%$ |
| **Net-3**: Locally connected (no sharing) | $1226$ | $1226$ | $88.5\%$ |
| **Net-4**: Constrained network 1 | $2266$ | $1132$ | $94.0\%$ |
| **Net-5**: Constrained network 2 | $5194$ | $1060$ | $98.4\%$ |

> [!IMPORTANT]
> Net-5 achieves the highest performance (only $1.6\%$ error) despite having a massive number of links ($5194$), because weight sharing restricts the number of independent parameters to be very small ($1060$). This design forces the network to extract spatially invariant features (like handwriting styles) regardless of their position in the image, illustrating that incorporating domain-specific knowledge into network topology is far superior to using a standard fully connected network.
