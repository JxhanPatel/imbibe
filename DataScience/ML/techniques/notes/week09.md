# 9.1: Perceptron Fundamentals & Convergence

The perceptron, developed by Frank Rosenblatt in the late 1950s, occupies an important historical position in the field of pattern recognition. Initially simulated on an IBM 704 computer at Cornell in 1957, Rosenblatt subsequently constructed custom analog hardware that provided a direct, parallel implementation of perceptron learning. This historical hardware utilized a camera system with a primitive $20 \times 20$ array of cadmium sulphide photocells to produce a $400$-pixel input image, a patch board for wiring random feature configurations, and motor-driven rotary variable resistors (potentiometers) to automatically adjust the adaptive weight parameters.

<img width="1311" height="630" alt="image" src="https://github.com/user-attachments/assets/a24b823c-107a-4375-915a-7d8d29a865e3" />

## 1. Mathematical Model of the Perceptron

The perceptron is a two-category classifier that belongs to the class of linear discriminant models. The input vector $x$ is first transformed using a fixed set of nonlinear basis functions (also called feature extractors) to produce a feature vector $\phi(x)$. This feature vector is then used to construct a generalized linear model of the form:

$$y(x) = f(w^T \phi(x))$$


where the nonlinear activation function $f(a)$ is a discontinuous step function defined as:

$$f(a) = \begin{cases} +1, & a \ge 0 \\ -1, & a < 0 \end{cases}$$


In this formulation, the feature vector $\phi(x)$ typically includes a bias component $\phi_0(x) = 1$ to allow for an adjustable offset. The target values are coded as $t = +1$ for class $\mathcal{C}_1$ and $t = -1$ for class $\mathcal{C}_2$. This $t \in \{-1, +1\}$ target coding scheme matches the output range of the step activation function $f(a)$.

Alternatively, we can express the model in augmented, normalized coordinates. We group the weights into a single $(D+1)$-dimensional weight vector $a = (w_0, w^T)^T$ and the inputs into the augmented vector $y = (1, x^T)^T$. To unify the classification requirement, we "normalize" the training samples by multiplying the augmented vectors of class $\omega_2$ by $-1$.

<img width="1192" height="602" alt="image" src="https://github.com/user-attachments/assets/3bf96213-e5a9-4f73-a52b-004b318186fb" />

This normalization ensures that the decision boundary places all correctly classified patterns on the positive side of the decision hyperplanes:

$$a^T y_i > 0 \quad \forall i = 1, \dots, n$$

## 2. Structural Comparison: Perceptrons vs. Multilayer Perceptrons (MLPs)

In neural network literature, the term "multilayer perceptron" is widely used to describe feed-forward neural networks. However, this term is actually a misnomer.

* **The Rosenblatt Perceptron** utilizes discontinuous step-function nonlinearities. Because the step function has zero derivative almost everywhere, gradient-based learning cannot be used to optimize parameters based on the total number of misclassified patterns.
* **The Multilayer Perceptron (MLP)** utilizes continuous, differentiable sigmoidal nonlinearities (such as the logistic sigmoid or hyperbolic tangent) in its hidden units. This differentiability allows the network functions to be differentiable with respect to all of the parameters, enabling the analytical calculation of derivatives via error backpropagation.

## 3. Dual Representation and the Kernel Trick

A powerful property of the perceptron is that its linear model can be reformulated in terms of a dual representation where the input features enter only in the form of inner products. By utilizing the perceptron learning rule, the learned weight vector $w$ can be written as a linear combination of the training patterns multiplied by their targets:

$$w = \sum_{n=1}^N \alpha_n t_n \phi(x_n)$$

where $\alpha_n$ is a non-negative integer representing the number of times pattern $n$ was misclassified and corrected. Substituting this into the prediction function yields:

$$y(x) = f\left( \sum_{n=1}^N \alpha_n t_n k(x, x_n) \right)$$

where $k(x, x_n) = \phi(x)^T \phi(x_n)$ is the kernel function. This dual formulation avoids the explicit introduction of the feature vector $\phi(x)$, allowing the implicit use of high-dimensional or infinite-dimensional feature spaces.

### 9.1.1 Perceptron Learning Algorithm

#### 1. Inadequacy of the Total Misclassification Loss

To find the parameter vector $w$, we must define a criterion function to minimize. The most natural choice of error function would be the total number of misclassified patterns. However, this choice does not lead to a simple learning algorithm. The number of misclassified patterns is a piecewise constant function of $w$ with step discontinuities occurring whenever the decision boundary moves across a data point. Consequently, its gradient is zero almost everywhere, which prevents the application of any gradient descent optimization procedures.

<img width="653" height="721" alt="image" src="https://github.com/user-attachments/assets/d69259d4-220e-4bca-b4c1-b96efebeaa27" />

#### 2. Derivation of the Perceptron Criterion

To resolve the zero-gradient limitation, we introduce a continuous, piecewise linear error function known as the Perceptron Criterion. We seek a weight vector $w$ such that all patterns satisfy $w^T \phi(x_n) t_n > 0$. The perceptron criterion associates zero error with any pattern that is correctly classified. For any misclassified pattern $x_n$, it minimizes the quantity $-w^T \phi(x_n) t_n$.

**Bishop Formulation (Explicit Target Sign)**

The total error function is given by the sum over the set of misclassified patterns $\mathcal{M}$:

$$E_P(w) = - \sum_{n \in \mathcal{M}} w^T \phi_n t_n$$


where $\phi_n = \phi(x_n)$. The contribution to this error from an individual misclassified pattern is a linear function of $w$ in regions where it is misclassified, and zero in regions where it is correctly classified, making the overall error piecewise linear.

**Duda Formulation (Normalized Coordinates)**

In normalized coordinates, where $a$ is the augmented weight vector and $y$ represents the normalized, augmented feature vectors, the Perceptron criterion function is defined as:

$$J_p(a) = \sum_{y \in \mathcal{Y}} (-a^T y)$$


where $\mathcal{Y}(a)$ is the set of samples misclassified by $a$ (for which $a^T y \le 0$).

#### 3. Batch Perceptron Algorithm

We apply gradient descent to the Perceptron criterion. The gradient of the Duda formulation $J_p(a)$ is given by:

$$\nabla J_p(a) = \sum_{y \in \mathcal{Y}} (-y)$$


The batch gradient descent update rule after each epoch (sweep through the entire training set) is:

$$a(k+1) = a(k) + \eta(k) \sum_{y \in \mathcal{Y}_k} y$$


where $\mathcal{Y}_k$ is the set of samples misclassified by the weight vector $a(k)$, and $\eta(k)$ is the learning rate.

**Algorithm 1: Batch Perceptron**

```text
begin
    initialize a, learning rate η(·), criterion threshold θ, step counter k = 0
    do
        k ← k + 1
        a ← a + η(k) * sum_{y ∈ Y_k}(y)
    until η(k) * sum_{y ∈ Y_k}(y) < θ
    return a
end

```

#### 4. Single-Sample Correction Perceptron Algorithm

Rather than evaluating the error over all patterns simultaneously, the single-sample correction algorithm processes the training patterns in a sequential or cyclic stream, updating the weight vector immediately upon encountering any misclassified pattern.

Let $y^1, y^2, \dots, y^k, \dots$ denote the sequence of misclassified samples encountered during sequential training. The fixed-increment single-sample correction rule is defined as:

$$a(k+1) = a(k) + y^k$$


where $a^T(k) y^k \le 0$ for all $k \ge 1$.

**Algorithm 2: Fixed-Increment Single-Sample Perceptron**

```text
begin
    initialize a, step counter k = 0
    do
        k ← (k + 1) mod n
        if y_k is misclassified by a then
            a ← a + y_k
        end if
    until all patterns are properly classified
    return a
end

```

### 9.1.2 Understanding Perceptron Update Rule

#### 1. Step-by-Step Interpretation of the Update Rule

The sequential learning rule cycles through the training patterns in turn. For each pattern $x_n$, we evaluate the perceptron function $y(x_n)$.

* If the pattern is correctly classified, the weight vector remains unchanged.
* If the pattern is incorrectly classified, we update the weight vector:
* For class $\mathcal{C}_1$ (where $t_n = +1$), we add the feature vector $\phi(x_n)$ to the current weight vector $w$.
* For class $\mathcal{C}_2$ (where $t_n = -1$), we subtract the feature vector $\phi(x_n)$ from $w$.



In normalized coordinates, if $a(k)$ misclassifies the normalized sample $y^k$, the inner product satisfies $a^T(k) y^k \le 0$. Adding $y^k$ to $a(k)$ moves the weight vector directly toward—and perhaps across—the decision hyperplane $a^T y^k = 0$. The new inner product is:

$$a^T(k+1)y^k = (a(k) + y^k)^T y^k = a^T(k)y^k + \vert{}y^k\vert{}^2 > a^T(k)y^k$$

Since $\vert{}y^k\vert{}^2 > 0$, this update is guaranteed to increase the inner product, moving the weight vector in a correct direction.

<img width="665" height="707" alt="image" src="https://github.com/user-attachments/assets/4bccb0fa-3149-4f71-a42f-a4ba6bfba7af" />

#### 2. Scale Invariance of the Learning Rate $\eta$

In the stochastic gradient update rule $w^{(\tau+1)} = w^{(\tau)} + \eta \phi_n t_n$, the parameter $\eta > 0$ acts as the learning rate. However, because the step activation function $f(a) = \text{Sgn}(a)$ is scale-invariant, the output of the perceptron $y(x, w)$ is completely unchanged if the weight vector $w$ is multiplied by an arbitrary positive constant. Therefore, we can set the learning rate parameter $\eta = 1$ without loss of generality.

#### 3. Individual Pattern Error Reduction vs. Total Error Fluctuations

For any single update, the error contribution from the misclassified pattern $n$ is reduced. Setting $\eta = 1$, the updated error contribution satisfies:

$$-w^{(\tau+1)T} \phi_n t_n = -(w^{(\tau)} + \phi_n t_n)^T \phi_n t_n = -w^{(\tau)T} \phi_n t_n - \vert{}\phi_n t_n\vert{}^2$$


Since $\vert{}\phi_n t_n\vert{}^2 > 0$, we have:

$$-w^{(\tau+1)T} \phi_n t_n < -w^{(\tau)T} \phi_n t_n$$


This reduction does not imply that the error contribution from other, non-active misclassified patterns is reduced. Furthermore, the change in the weight vector may cause previously correctly classified patterns to become misclassified. Thus, the perceptron learning rule is not guaranteed to reduce the total error function at each stage.

### 9.1.3 Proof of Convergence of Perceptron Algorithm

#### 1. The Perceptron Convergence Theorem

**Theorem 5.1 (Perceptron Convergence):** If there exists an exact solution (in other words, if the training dataset is linearly separable), then the fixed-increment single-sample correction perceptron learning algorithm (Algorithm 2) is guaranteed to find an exact solution in a finite number of steps.

#### 2. Step-by-Step Mathematical Proof of Convergence

Let the training dataset be linearly separable, meaning there exists a solution vector $\hat{a}$ such that:

$$\hat{a}^T y_i > 0 \quad \forall i = 1, \dots, n$$

Because any positive scaled version of $\hat{a}$ is also a solution, we can scale $\hat{a}$ by an arbitrary positive scaling factor $\alpha$. We evaluate the squared Euclidean distance between the weight vector $a(k+1)$ and the scaled solution vector $\alpha \hat{a}$:

$$\vert{}a(k+1) - \alpha \hat{a}\vert{}^2 = \vert{}(a(k) + y^k) - \alpha \hat{a}\vert{}^2$$

$$\vert{}a(k+1) - \alpha \hat{a}\vert{}^2 = \vert{}(a(k) - \alpha \hat{a}) + y^k\vert{}^2$$

$$\vert{}a(k+1) - \alpha \hat{a}\vert{}^2 = \vert{}a(k) - \alpha \hat{a}\vert{}^2 + 2(a(k) - \alpha \hat{a})^T y^k + \vert{}y^k\vert{}^2$$

$$\vert{}a(k+1) - \alpha \hat{a}\vert{}^2 = \vert{}a(k) - \alpha \hat{a}\vert{}^2 + 2 a^T(k) y^k - 2 \alpha \hat{a}^T y^k + \vert{}y^k\vert{}^2$$

Because $y^k$ is misclassified by $a(k)$, we must have:

$$a^T(k) y^k \le 0$$

Applying this inequality simplifies our expression:

$$\vert{}a(k+1) - \alpha \hat{a}\vert{}^2 \le \vert{}a(k) - \alpha \hat{a}\vert{}^2 - 2 \alpha \hat{a}^T y^k + \vert{}y^k\vert{}^2$$

Now, we define the following positive constants over our finite training set:

$$\beta^2 = \max_{i} \vert{}y_i\vert{}^2$$

$$\gamma = \min_{i} \hat{a}^T y_i > 0$$

Because $y^k$ is a member of our training set, we have $\vert{}y^k\vert{}^2 \le \beta^2$ and $\hat{a}^T y^k \ge \gamma$. This allows us to bound the terms:

$$-2 \alpha \hat{a}^T y^k + \vert{}y^k\vert{}^2 \le -2 \alpha \gamma + \beta^2$$

Following the classical proof, we choose the scale factor $\alpha$ to be:

$$\alpha = \frac{\beta^2}{\gamma}$$

Substituting this value of $\alpha$ into the bound yields:

$$-2 \alpha \gamma + \beta^2 = -2 \left( \frac{\beta^2}{\gamma} \right) \gamma + \beta^2 = -2 \beta^2 + \beta^2 = -\beta^2$$

Substituting this result back into our inequality, we obtain the monotonic distance contraction:

$$\vert{}a(k+1) - \alpha \hat{a}\vert{}^2 \le \vert{}a(k) - \alpha \hat{a}\vert{}^2 - \beta^2$$


By applying this contraction recursively for $k$ successive corrections, we get:

$$\vert{}a(k+1) - \alpha \hat{a}\vert{}^2 \le \vert{}a(1) - \alpha \hat{a}\vert{}^2 - k \beta^2$$


Because the squared Euclidean norm of a vector cannot be negative, we have:

$$0 \le \vert{}a(k+1) - \alpha \hat{a}\vert{}^2 \le \vert{}a(1) - \alpha \hat{a}\vert{}^2 - k \beta^2$$

This inequality implies that the sequence of corrections must terminate after a maximum number of steps, denoted $k_0$, where:

$$k_0 = \frac{\vert{}a(1) - \alpha \hat{a}\vert{}^2}{\beta^2}$$


If we initialize the weight vector at the origin, $a(1) = 0$, this bound simplifies to:

$$k_0 = \frac{\alpha^2 \vert{}\hat{a}\vert{}^2}{\beta^2}$$

By substituting our choice of $\alpha = \beta^2 / \gamma$, we obtain the final expression for the convergence limit:

$$k_0 = \frac{\beta^2 \vert{}\hat{a}\vert{}^2}{\gamma^2} = \frac{\max_i \vert{}y_i\vert{}^2 \vert{}\hat{a}\vert{}^2}{\min_i [\hat{a}^T y_i]^2}$$


Thus, the fixed-increment single-sample correction algorithm is guaranteed to converge to a separating hyperplane in a finite number of steps.

#### 3. Geometrical Interpretation of the Convergence Bound

The convergence bound $k_0$ has an intuitive geometric meaning:

* **The Numerator ($\max_i \vert{}y_i\vert{}^2$):** Measures the maximum squared length of the training vectors, representing the scale of the feature space.
* **The Denominator ($\min_i [\hat{a}^T y_i]^2$):** Shows that the difficulty of the problem is essentially determined by the samples most nearly orthogonal to the solution vector.
* **Coplanar Constraints:** Linearly separable problems can be made arbitrarily difficult to solve by making the samples almost coplanar, which pushes $\gamma \to 0$ and causes the bound $k_0 \to \infty$.

### 9.1.4 Generalizations, Nonseparable Behavior, and Limitations

#### 1. Generalization to Variable Increment with Margin

The perceptron algorithm can be generalized to introduce a variable increment $\eta(k)$ and a target margin $b$. The update is executed whenever $a^T(k) y^k \le b$:

$$a(k+1) = a(k) + \eta(k) y^k$$


It can be shown that if the samples are linearly separable, $a(k)$ will converge to a solution satisfying $a^T y_i > b$ for all $i$, provided the sequence of learning rates satisfies:

$$\eta(k) \ge 0$$


$$\lim_{m \to \infty} \sum_{k=1}^m \eta(k) = \infty$$


$$\lim_{m \to \infty} \frac{\sum_{k=1}^m \eta^2(k)}{\left(\sum_{k=1}^m \eta(k)\right)^2} = 0$$


These conditions are satisfied if $\eta(k)$ is a positive constant, or if it decreases like $1/k$.

Furthermore, if we generalize the scale factor $\alpha$ to any value greater than $\beta^2 / (2 \gamma)$, the bound on the maximum number of corrections is given by:

$$k_0 = \frac{\vert{}a_1 - \alpha \hat{a}\vert{}^2}{2 \alpha \gamma - \beta^2}$$

#### 2. Nonseparable Behavior

If the training dataset is not linearly separable, the perceptron learning algorithm will never converge, resulting in an infinite sequence of weight vector updates.

* **Weight Vector Length Bounding:** Although the algorithm does not terminate, the length of the weight vectors produced by the fixed-increment rule remains bounded.
* **Weight Vector Averaging:** By averaging the weight vectors produced by the correction rule over time, we can reduce the risk of obtaining a bad solution due to an unfortunate choice of termination time.
* **Alternative Algorithms:** Algorithms like the Ho-Kashyap procedure or linear programming can be used to provide explicit, automated evidence of nonseparability.

#### 3. Fundamental Limitations of the Perceptron

Despite its historical significance, the single-layer perceptron has severe limitations:

* **Linear Constraints:** Because it is based on linear combinations of fixed basis functions, it is ultimately constrained by the curse of dimensionality. It cannot solve non-linearly separable problems like the exclusive-OR (XOR) or $d$-bit parity problems.
* **Deterministic Outputs:** It does not provide probabilistic outputs.
* **Multiclass Scaling:** It does not generalize easily to multiclass classification ($K > 2$).
