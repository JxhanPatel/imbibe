## 11.1 Soft-Margin Support Vector Machines (SVM)

In practical pattern classification problems, the class-conditional distributions frequently overlap in the input feature space. In such scenarios, enforcing a hard margin where the training data must be perfectly separated is highly undesirable because it can lead to severe overfitting or, in many cases, make the optimization problem completely infeasible since no separating hyperplane exists.

To address overlapping class distributions, the hard-margin support vector machine is generalized to a **Soft-Margin Support Vector Machine**. This relaxation is achieved by introducing non-negative **slack variables** $\xi_n \ge 0$ for each training observation $n = 1, \dots, N$.

<img width="1208" height="462" alt="image" src="https://github.com/user-attachments/assets/fdedb9ac-f952-4cf9-99be-6554c593aa64" />

### Slack Variables and Point Locations

The slack variable $\xi_n$ measures the proportional deviation of the training point $x_n$ from its ideal margin boundary. The strict classification constraints $t_n (w^T \phi(x_n) + b) \ge 1$ are replaced by the relaxed constraints:
$$t_n (w^T \phi(x_n) + b) \ge 1 - \xi_n, \quad \xi_n \ge 0, \quad n = 1, \dots, N$$

The physical and geometric locations of the data points are characterized by the values of their corresponding slack variables:
*   $\xi_n = 0$: The data point lies exactly on the margin boundary or on the correct side of the margin boundary (correctly classified).
*   $0 < \xi_n \le 1$: The data point lies inside the margin boundary but on the correct side of the decision boundary (correctly classified, but violates the margin).
*   $\xi_n > 1$: The data point lies on the incorrect side of the decision boundary, which means it is misclassified.

### Primal Optimization Problem
The soft-margin support vector machine seeks to maximize the margin while softly penalizing points that violate the margin or are misclassified. This is formulated as the following convex **Primal Optimization Problem**:
$$\min_{w, b, \xi} \frac{1}{2} \|w\|^2 + C \sum_{n=1}^N \xi_n$$
$$\text{subject to } t_n (w^T \phi(x_n) + b) \ge 1 - \xi_n \quad \text{and} \quad \xi_n \ge 0, \quad n = 1, \dots, N$$

The parameter $C > 0$ is a regularization parameter that controls the trade-off between the slack variable penalty and the margin. Because any misclassified point must have $\xi_n > 1$, the sum $\sum_{n} \xi_n$ represents an upper bound on the total number of training misclassifications. The parameter $C$ is therefore analogous to the inverse of a regularization coefficient. 
*   **Large $C$**: Penalizes margin violations heavily, forcing the margin to be narrow to accommodate points, which decreases bias but increases variance (prone to overfitting).
*   **Small $C$**: Tolerates more margin violations in exchange for a wider margin, which increases bias but decreases variance (smoother decision boundary).


### 11.1.1 Dual Formulation for Soft-Margin SVM

To solve this constrained optimization problem, we introduce two sets of non-negative Lagrange multipliers: $a_n \ge 0$ for the margin constraints and $\mu_n \ge 0$ for the non-negativity of the slack variables. The Primal Lagrangian is defined as:
$$L(w, b, \xi, a, \mu) = \frac{1}{2} \|w\|^2 + C \sum_{n=1}^N \xi_n - \sum_{n=1}^N a_n \left\{ t_n (w^T \phi(x_n) + b) - 1 + \xi_n \right\} - \sum_{n=1}^N \mu_n \xi_n$$

where $a = (a_1, \dots, a_N)^T$ and $\mu = (\mu_1, \dots, \mu_N)^T$ are the vectors of Lagrange multipliers.

#### Stationary Conditions
To find the saddle point, we take the partial derivatives of the Primal Lagrangian with respect to the primal variables $w, b$, and $\xi_n$ and set them to zero:
$$\frac{\partial L}{\partial w} = 0 \implies w = \sum_{n=1}^N a_n t_n \phi(x_n)$$
$$\frac{\partial L}{\partial b} = 0 \implies \sum_{n=1}^N a_n t_n = 0$$
$$\frac{\partial L}{\partial \xi_n} = 0 \implies C - a_n - \mu_n = 0 \implies a_n + \mu_n = C$$

#### Deriving the Dual objective Function
We use these stationary conditions to eliminate the primal variables from the Lagrangian.
*   First, we substitute the relation $w = \sum_{n=1}^N a_n t_n \phi(x_n)$ into the term $\frac{1}{2} \|w\|^2$:
    $$\frac{1}{2} \|w\|^2 = \frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N a_n a_m t_n t_m \phi(x_n)^T \phi(x_m)$$
*   Next, the terms involving the bias parameter $b$ vanish because of the constraint $\sum_{n=1}^N a_n t_n = 0$.
*   Finally, the terms involving the slack variables $\xi_n$ are grouped:
    $$\sum_{n=1}^N C \xi_n - \sum_{n=1}^N a_n \xi_n - \sum_{n=1}^N \mu_n \xi_n = \sum_{n=1}^N (C - a_n - \mu_n) \xi_n$$
    Since the stationary condition requires $C - a_n - \mu_n = 0$, this entire term sums to zero.

Substituting these simplifications back into the Primal Lagrangian yields the **Wolfe Dual Lagrangian** objective function:
$$\tilde{L}(a) = \sum_{n=1}^N a_n - \frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N a_n a_m t_n t_m k(x_n, x_m)$$

where the kernel function is defined as $k(x_n, x_m) = \phi(x_n)^T \phi(x_m)$. 

This dual objective function is identical in functional form to that of the separable (hard-margin) support vector machine. However, the constraints are modified. The Lagrange multipliers are constrained by the relation $a_n + \mu_n = C$. Since $\mu_n \ge 0$, this relation requires that $a_n \le C$. 

The resulting dual optimization problem requires maximizing this quadratic function under **box constraints**:
$$\max_{a} \tilde{L}(a)$$
$$\text{subject to } 0 \le a_n \le C, \quad n = 1, \dots, N$$
$$\text{and } \sum_{n=1}^N a_n t_n = 0$$

This is a convex quadratic programming problem.


### 11.1.2 Complementary Slackness Conditions for Soft-Margin SVM

Because the optimization problem is convex, any optimal solution must satisfy the **Karush-Kuhn-Tucker (KKT)** conditions, which comprise the following properties:
1.  $a_n \ge 0$
2.  $t_n y(x_n) - 1 + \xi_n \ge 0$
3.  $a_n \left\{ t_n y(x_n) - 1 + \xi_n \right\} = 0$
4.  $\mu_n \ge 0$
5.  $\xi_n \ge 0$
6.  $\mu_n \xi_n = 0 \implies (C - a_n) \xi_n = 0$

where the third condition is the complementary slackness condition for the margin constraints, and the sixth is the complementary slackness condition for the non-negativity of the slack variables.

#### Physical Analysis of the Data Points
Using these joint conditions, we can fully classify the state of any training point $x_n$ based on the value of its optimal Lagrange multiplier $a_n^★$:

##### 1. Non-Support Vectors ($a_n^★ = 0$)
If $a_n^★ = 0$, then the margin constraint is inactive, meaning $t_n y(x_n) - 1 + \xi_n > 0$. Because $a_n^★ = 0$, the relation $a_n^★ + \mu_n★ = C$ implies that $\mu_n★ = C > 0$. Under the condition $\mu_n★ \xi_n = 0$, this forces the slack variable $\xi_n = 0$. 
Thus, the point satisfies:
$$t_n y(x_n) > 1$$
These points lie strictly outside the margin boundary on the correct side of the decision boundary. Because $a_n^★ = 0$, they do not contribute to the weight vector expansion and play no role in making predictions for new test points.

##### 2. Support Vectors ($a_n^★ > 0$)
If $a_n^★ > 0$, the point must satisfy the active margin constraint:
$$t_n y(x_n) = 1 - \xi_n$$
These critical points are called **Support Vectors**. Based on the upper bound $C$, they fall into two distinct sub-categories:

*   **Margin-Bound Support Vectors ($0 < a_n^★ < C$):**
    Since $a_n^★ < C$, the relation $a_n^★ + \mu_n★ = C$ implies that $\mu_n★ = C - a_n^★ > 0$. The complementary slackness condition $\mu_n★ \xi_n = 0$ then requires that the slack variable $\xi_n = 0$.
    Thus, these points satisfy:
    $$t_n y(x_n) = 1$$
    These support vectors lie exactly on the correct margin slab boundary.
*   **Non-Bound Support Vectors ($a_n^★ = C$):**
    Since $a_n^★ = C$, we have $\mu_n★ = C - a_n^★ = 0$. In this case, the slack variable is not constrained to be zero, and can take any positive value $\xi_n \ge 0$.
    *   If $0 < \xi_n \le 1$: The point lies inside the margin boundary but on the correct side of the decision boundary.
    *   If $\xi_n > 1$: The point lies on the incorrect side of the decision boundary and is misclassified.

<img width="969" height="678" alt="image" src="https://github.com/user-attachments/assets/3c3373ff-4e82-4e30-a849-4ae061da2b9b" />

#### Analytical Solution for the Bias Parameter $b^★$
The optimal bias parameter $b^★$ can be computed from any support vector $x_n$ for which the constraint is strictly intermediate ($0 < a_n^★ < C$), since its slack variable $\xi_n = 0$. 
For such a point, the relation $t_n y(x_n) = 1$ yields:
$$t_n \left( \sum_{m \in \mathcal{S}} a_m^★ t_m k(x_n, x_m) + b^★ \right) = 1$$

Multiplying both sides by $t_n$ (and noting that $t_n^2 = 1$ since $t_n \in \{-1, +1\}$):
$$b^★ = t_n - \sum_{m \in \mathcal{S}} a_m^★ t_m k(x_n, x_m)$$

To ensure numerical stability and minimize round-off errors, we average this relation over the set $\mathcal{M}$ of all support vectors having $0 < a_n^★ < C$:
$$b^★ = \frac{1}{N_{\mathcal{M}}} \sum_{n \in \mathcal{M}} \left( t_n - \sum_{m \in \mathcal{S}} a_m^★ t_m k(x_n, x_m) \right)$$

where $N_{\mathcal{M}}$ is the total number of support vectors in the set $\mathcal{M}$.

#### The Hinge Loss Formulation
The soft-margin optimization problem can be alternatively formulated as an unconstrained minimization of a regularized loss function:
$$\sum_{n=1}^N E_{SV}(y(x_n) t_n) + \lambda \|w\|^2$$

where the loss function $E_{SV}(z)$ is the **Hinge Loss Function**, defined as:
$$E_{SV}(z) = [1 - z]_+ = \max(0, 1 - z)$$

and the regularization parameter is related by $\lambda = (2C)^{-1}$.

<img width="1236" height="506" alt="image" src="https://github.com/user-attachments/assets/dd5c5973-8aa6-4c7e-951c-4cdc5ce0cb23" />

This Hinge Loss formulation illustrates the mathematical origin of sparsity in support vector machines:
*   **Logistic Regression Loss:** The binomial deviance loss $\ln(1 + e^{-z})$ has a non-zero gradient everywhere. Consequently, every single training pattern in the dataset exerts some influence on the final decision boundary.
*   **Support Vector Machine Loss:** The Hinge Loss is exactly zero for any point that lies on the correct side of the margin boundary ($z \ge 1$). This flat region means that these points have zero gradient in the optimization, which is the direct source of the sparse solution where only the support vectors determine the decision boundary.


### 11.1.3 Summary for Soft-Margin SVM

*   **Convex Optimization:** The determination of the model parameters in a support vector machine corresponds to a convex quadratic programming problem. Any local solution is also a global optimum, avoiding the local-minimum issues of neural networks.
*   **Sparsity:** The decision boundary is determined exclusively by the support vectors. Once the model is trained, all non-support vectors can be discarded, and only the sparse subset of support vectors needs to be retained for future predictions.
*   **Regularization Control:** The parameter $C$ acts as an inverse regularization coefficient, providing a direct mechanism to balance model complexity and training error.
*   **Sensitivity to Outliers:** Because the soft-margin penalty increases linearly with the slack variables $\xi_n$, the model is sensitive to outliers and mislabeled data in the training set.
*   **The $\nu$-SVM Variant:** An alternative formulation, known as the $\nu$-SVM, replaces the parameter $C$ with a parameter $\nu \in (0, 1]$. The parameter $\nu$ represents both an upper bound on the fraction of margin errors (points having $\xi_n > 0$) and a lower bound on the fraction of support vectors.
*   **SMO Training Algorithm:** For large-scale datasets, standard quadratic programming solvers are computationally expensive, scaling as $O(N^3)$. The **Sequential Minimal Optimization (SMO)** algorithm takes decomposition methods to the extreme, optimizing just two Lagrange multipliers at each step. This subproblem can be solved analytically, avoiding numerical solvers entirely and scaling between linear and quadratic with the number of data points.

---

## 11.2 Overfitting and Underfitting

Generalization is the central goal of any classifier design. It represents the ability of a trained machine to make accurate predictions when presented with novel patterns that were not seen during training.

### Overfitting
Overfitting occurs when a classifier is overly complex relative to the size and noise level of the training dataset. In this regime, the model's parameters become "tuned" to the random noise on the target values or the accidental correlations of the training samples, rather than the underlying physical process.
*   **Behavior:** The training error is extremely low (or even zero), but the generalization/test error on independent, unseen test data is unacceptably high.
*   **Example:** Fitting a high-order polynomial (such as $M = 9$) to a small dataset of 10 noisy points generated from a sine wave. The curve passes exactly through each training point, achieving zero error, but exhibits wild oscillations between the points.

<img width="1335" height="471" alt="image" src="https://github.com/user-attachments/assets/87835b07-83d1-4252-8df4-7d3d22e18b1f" />

<img width="957" height="565" alt="image" src="https://github.com/user-attachments/assets/6a35b98a-0e6e-4de6-afe3-a95921a9499a" />

### Underfitting
Underfitting occurs when a classifier is too simple or rigid, meaning it does not have enough degrees of freedom to capture the underlying structure or explain the differences between the categories in the data.
*   **Behavior:** The model performs poorly on both the training data and the validation data, exhibiting high error rates in both settings.
*   **Example:** Fitting a first-order polynomial (a straight line, $M = 1$) to highly curved, sinusoidal data.


### The Bias-Variance Trade-off

The expected prediction error of any learning method can be decomposed into three distinct components:
$$\text{Expected Prediction Error} = \text{Irreducible Noise} + \text{Bias}^2 + \text{Variance}$$

where:
1.  **Irreducible Noise ($\sigma_{\epsilon}^2$):** The variance of the target variable around its true mean. It represents the lower limit of the error rate and cannot be avoided, even if we estimate the true function perfectly.
2.  **Bias (Squared Bias):** Measures the accuracy or quality of the match between our model's average prediction and the true generating function. A high bias implies a poor match, meaning the model is too rigid to represent the true relationship (underfitting).
3.  **Variance:** Measures the precision or specificity of the match, representing the expected squared deviation of our estimated model around its own average. A high variance implies that the model's parameters are highly sensitive to the particular random sample of training data selected (overfitting).

#### The Bias-Variance Dilemma
As model complexity is increased (e.g., by adding more parameters, increasing polynomial order, or adding hidden units), the model is better able to adapt to the training data, which reduces the systematic **bias**. However, this increased flexibility makes the model highly sensitive to the random noise in the specific training set, which increases the **variance**. 
The model with the optimal predictive capability is the one that achieves the best balance between bias and variance, minimizing the expected test error.

<img width="811" height="676" alt="image" src="https://github.com/user-attachments/assets/acd037fd-960c-4782-8cad-1eb1d9de21bb" />

#### Bias-Variance in Regression vs. Classification (0-1 Loss)
The behavior of the bias-variance tradeoff differs fundamentally between regression under squared-error loss and classification under 0-1 loss:
*   **Regression (Squared-Error Loss):** The estimation error is strictly additive in the squared bias and variance. Both terms contribute equally to increasing the mean squared error.
*   **Classification (0-1 Loss):** There is a highly nonlinear and multiplicative interaction. Classification errors only occur when the prediction falls on the wrong side of the decision boundary. If at a given point the true probability of class 1 is $0.9$ and our estimated probability is $0.6$, the squared bias is $(0.6 - 0.9)^2 = 0.09$, which is considerable. However, because we still place the prediction on the correct side of the boundary ($> 0.5$), the misclassification error is exactly zero.

<img width="561" height="726" alt="image" src="https://github.com/user-attachments/assets/3536368b-6c6a-43e2-80f4-2d50d814a2bc" />

For this reason, **low variance is generally far more important for classification than low bias**. In classification, we need not be particularly concerned if our estimation is biased, so long as the variance is kept low.


### Practical Techniques to Detect and Avoid Overfitting

#### 1. Validation Set (Hold-out Set)
If data is plentiful, the available dataset is randomly partitioned into three parts:
1.  **Training Set:** Used to determine the model coefficients.
2.  **Validation Set:** Used to estimate the prediction error for model selection, optimizing hyperparameters like the regularization parameter $\lambda$ or tree depth.
3.  **Test Set:** Kept in a vault, and used strictly at the end of the analysis to provide an unbiased estimate of the generalization error of the final chosen model.

#### 2. Cross-Validation
When data is limited, we partition the training set randomly into $S$ roughly equal-sized groups (folds). For each fold, we train the model on the remaining $S - 1$ folds and evaluate its prediction error on the held-out fold. The average of these $S$ validation errors provides our cross-validation estimate of prediction error.
*   **Leave-One-Out Cross-Validation (LOOCV):** The limit where $S = N$. This estimator is approximately unbiased for the expected prediction error, but can have high variance because the $N$ training sets are extremely similar to one another.
*   **$5$- or $10$-Fold Cross-Validation:** Recommended as a good compromise. If the learning curve has a steep slope at the given training set size, five- or tenfold cross-validation will slightly overestimate the true prediction error (biased upward), but it has significantly lower variance than LOOCV.

<img width="917" height="559" alt="image" src="https://github.com/user-attachments/assets/ac76d86f-8efb-4eda-ae07-a1e1ca516203" />

##### The "Wrong" and "Right" Way to Do Cross-Validation
A common and catastrophic error in published papers occurs when predictors are screened or selected using the entire dataset before applying cross-validation:
*   **The Wrong Way:** Screen all 5000 predictors to select the top 100 that correlate best with the class labels across all $N$ samples. Then perform cross-validation to estimate the error of a classifier built on these 100 predictors. Because the predictors were selected using all samples, they have "already seen" the held-out validation samples. This leads to a massive downward bias in the estimated cross-validation error (e.g., yielding a 3% estimated error on random noise when the true error is 50%).
*   **The Right Way:** Cross-validation must be applied to the *entire* sequence of modeling steps. In each fold, the predictors must be screened and selected using *only* the $S - 1$ folds of training data. The held-out fold must be completely excluded from the feature selection step.

#### 3. Regularization
Regularization adds a complexity penalty term to the error function to discourage the model parameters from reaching large values:
$$\tilde{E}(w) = E(w) + \frac{\lambda}{2} J(w)$$

*   **$L_2$ Regularization (Weight Decay / Ridge):** $J(w) = w^T w$. This shrinks the coefficients uniformly toward zero, stabilizing the model in the presence of highly correlated variables.
*   **$L_1$ Regularization (Lasso):** $J(w) = \sum |w_j|$. This encourages sparsity by driving some coefficients to exactly zero, performing internal feature selection.

#### 4. Early Stopping
In iterative gradient descent training of neural networks, the training error decreases monotonically. However, the error measured on an independent validation set typically decreases at first, and then increases as the network starts to overfit. 
The **Early Stopping** technique halts training at the point of the minimum validation set error.

<img width="1321" height="627" alt="image" src="https://github.com/user-attachments/assets/b8f682d3-89be-469f-a3a4-2a8d7305af9c" />

<img width="907" height="557" alt="image" src="https://github.com/user-attachments/assets/33031d77-889b-4f2a-a5ca-23150c5325fc" />

Qualitatively, stopping training before gradient descent is complete represents a way of limiting the effective complexity of the network. The weights are initialized with small values, so the hidden units operate in their linear range. As training progresses, the weights grow, and the nonlinearities are expressed. 
It can be shown that early stopping is mathematically equivalent to $L_2$ regularization, where the product of the iteration index and learning rate ($\tau \eta$) behaves like the reciprocal of the regularization parameter ($1 / \lambda$).

#### 5. Information Criteria
Instead of using validation data, we can estimate the expected in-sample error by adding an estimate of the average **optimism** (the amount by which the training error underestimates the true error) to the training error:
*   **Akaike Information Criterion (AIC):**
    $$\text{AIC} = \ln p(\mathcal{D} \mid w^★_{ML}) - M$$
*   **Bayesian Information Criterion (BIC):**
    $$\text{BIC} = \ln p(\mathcal{D} \mid w^★_{ML}) - \frac{1}{2} M \ln N$$
    where $M$ is the number of parameters and $N$ is the dataset size. BIC penalizes model complexity more heavily than AIC.
*   **Minimum Description Length (MDL):** States that we should select the model $h★$ that minimizes the sum of the model's algorithmic complexity and the description length of the training data given the model:
    $$K(h, \mathcal{D}) = K(h) + K(\mathcal{D} \text{ using } h)$$

---

## 11.3 Ensemble Learning Methods

The core concept of ensemble learning is to build a powerful prediction model by combining the strengths of a collection of simpler base models, often called **weak learners**. 

<img width="968" height="691" alt="image" src="https://github.com/user-attachments/assets/59c400a8-fdc9-4ace-ad39-ad5b24d831c7" />


### 11.3.1 Bagging

**Bagging**, derived from **Bootstrap Aggregation**, is a technique designed to reduce the variance of an estimated prediction function. It is highly effective for high-variance, low-bias procedures.

#### The Bagging Process
1.  **Bootstrap Sampling:** Given a training dataset $D$ of size $N$, we randomly draw $B$ bootstrap datasets $D★^b$ ($b = 1, \dots, B$), each of size $N$, with replacement from $D$.
2.  **Training Component Classifiers:** We fit a separate copy of our model $\hat{f}★^b(x)$ to each bootstrap dataset. The component classifiers are of the same general form (e.g., all neural networks or all decision trees); only their parameter values differ because they are trained on different bootstrap samples.
3.  **Aggregation:**
    *   **Regression:** The final bagging prediction is the simple average of the predictions from the $B$ models:
        $$\hat{f}_{bag}(x) = \frac{1}{B} \sum_{b=1}^B \hat{f}★^b(x)$$
    *   **Classification:** Each tree casts a vote for the predicted class. The final bagging classifier selects the class having the majority of the votes:
        $$\hat{G}_{bag}(x) = \arg \max_{k} p_k(x)$$
        where $p_k(x)$ is the proportion of trees predicting class $k$ at $x$.

<img width="522" height="759" alt="image" src="https://github.com/user-attachments/assets/14c07727-cd6a-4c15-a4f6-107c8cf228cb" />

<img width="840" height="703" alt="image" src="https://github.com/user-attachments/assets/d5e924ff-e5c4-4b63-9939-89714e3c1e74" />

#### Mathematical Principle of Bagging (Squared-Error Loss)
A simple analysis shows why bagging reduces mean squared error under squared-error loss: it averages out the variance while leaving the bias unchanged.

Assume our training observations $(x_i, y_i)$ are independently drawn from a population distribution $P$. Let $f_{ag}(x) = \mathbb{E}_{P} \hat{f}★(x)$ be the ideal aggregate estimator, where the bootstrap dataset $Z★$ consists of observations sampled from $P$. We can write:
$$\mathbb{E} [ (Y - \hat{f}★(x))^2 ] = \mathbb{E} [ (Y - f_{ag}(x) + f_{ag}(x) - \hat{f}★(x))^2 ]$$

Expanding the quadratic form, and noting that the cross-product term is zero because $\mathbb{E}[f_{ag}(x) - \hat{f}★(x)] = 0$:
$$\mathbb{E} [ (Y - \hat{f}★(x))^2 ] = \mathbb{E} [ (Y - f_{ag}(x))^2 ] + \mathbb{E} [ (f_{ag}(x) - \hat{f}★(x))^2 ]$$
$$\ge \mathbb{E} [ (Y - f_{ag}(x))^2 ]$$

Thus, the expected squared error of the aggregated predictor $f_{ag}(x)$ is never greater than the expected squared error of any individual estimator $\hat{f}★(x)$. The amount of reduction is equal to the variance of the individual estimates around their average.

#### The Correlation Limitation
In practice, we do not draw bootstrap samples from the true population distribution $P$; instead, we draw them with replacement from the empirical distribution $\hat{P}$ of our single finite training set. Consequently, the bootstrap datasets are not independent.

The variance of the average of $B$ identically distributed random variables, each with variance $\sigma^2$ and positive pairwise correlation $\rho$, is:
$$\text{Var}(\hat{f}_{bag}(x)) = \rho \sigma^2 + \frac{1 - \rho}{B} \sigma^2$$

As the number of bootstrap samples $B \to \infty$, the second term disappears, but the first term $\rho \sigma^2$ remains. The pairwise correlation $\rho$ between the bagged trees limits the benefits of averaging.
*   **The Random Forest Solution:** Random forests improve on bagging by reducing this pairwise correlation $\rho$. At each split in each tree, they select a random subset of $m$ features from the $p$ total features, grown without pruning. This decorrelates the trees, leading to a much larger reduction in variance.

#### Unstable Classifiers
Bagging is highly effective for **unstable classifiers**, which are classifiers where small changes in the training data lead to significantly different decision boundaries. 
*   **Unstable:** Decision trees trained via greedy algorithms (a slight shift in a single training point can lead to a radically different tree topology). Bagging succeeds in smoothing out this variance, dramatically improving prediction.
*   **Stable:** Nearest neighbors or linear discriminant analysis, which are highly stable. Bagging stable procedures can occasionally degrade performance slightly due to the bootstrap sampling.


### 11.3.2 Boosting

**Boosting** is a sequential ensemble method designed to improve the accuracy of any given learning algorithm. It applies a weak learning algorithm to repeatedly modified versions of the training data, producing a sequence of weak classifiers $G_m(x)$ ($m = 1, \dots, M$) that are combined to form a powerful committee.

<img width="1213" height="502" alt="image" src="https://github.com/user-attachments/assets/62b54fc9-817e-4e72-94dc-efd5210ef58f" />

#### The Sequential Reweighting Principle
The principal difference between boosting and bagging is that the base classifiers in boosting are trained in **sequence**, and each base classifier is trained on a **weighted** form of the dataset. 
Initially, all training observations receive equal weight. On each successive iteration, the weights are modified: observations that were misclassified by the previous classifier have their weights increased, while weights are decreased for those that were classified correctly. This forces each successive classifier to focus on the "difficult" or "most informative" patterns.

#### The AdaBoost Algorithm (AdaBoost.M1)
Consider a two-class classification problem where the output variable is coded as $y_i \in \{-1, +1\}$.

##### 1. Initialization
Initialize the observation weights across the training set to be uniform:
$$w_n^{(1)} = \frac{1}{N}, \quad n = 1, \dots, N$$

##### 2. Sequential Training Loop
For $m = 1, \dots, M$:
1.  Fit a classifier $G_m(x)$ to the training data by minimizing the weighted error function:
    $$J_m = \sum_{n=1}^N w_n^{(m)} I(G_m(x_n) \neq y_n)$$
2.  Compute the weighted error rate of this classifier on the training set:
    $$\epsilon_m = \frac{\sum_{n=1}^N w_n^{(m)} I(G_m(x_n) \neq y_n)}{\sum_{n=1}^N w_n^{(m)}}$$
3.  Compute the classifier weight coefficient $\alpha_m$, which weights the contribution of $G_m(x)$ in the final committee:
    $$\alpha_m = \ln \left( \frac{1 - \epsilon_m}{\epsilon_m} \right)$$
    If the classifier is better than random guessing ($\epsilon_m < 0.5$), then $\alpha_m > 0$. More accurate classifiers receive higher weight in the final vote.
4.  Update the data weighting coefficients for the next iteration:
    $$w_n^{(m+1)} = w_n^{(m)} \exp \left\{ \text{sgn}(\alpha_m) \alpha_m I(G_m(x_n) \neq y_n) \right\}$$
    which simplifies to:
    $$w_n^{(m+1)} = w_n^{(m)} \exp \left\{ \alpha_m I(G_m(x_n) \neq y_n) \right\}$$

##### 3. Final Prediction
Combine the predictions of the individual classifiers through a weighted majority vote:
$$G(x) = \text{sign} \left( \sum_{m=1}^M \alpha_m G_m(x) \right)$$

<img width="1090" height="678" alt="image" src="https://github.com/user-attachments/assets/56acd7a5-f615-4486-9c77-a90fb9348613" />

<img width="910" height="784" alt="image" src="https://github.com/user-attachments/assets/4740797c-39ed-4bac-9de9-d2c5246e6194" />

<img width="920" height="660" alt="image" src="https://github.com/user-attachments/assets/fefdcf65-a6ec-45b0-b688-c155f57ac4bf" />

So long as each component classifier is a weak learner (achieving an error rate $\epsilon_m < 0.5$ on its weighted training set), the total training error of the ensemble decreases exponentially fast with the number of component classifiers $M$:
$$E \le \exp \left( -2 \sum_{m=1}^M G_m^2 \right)$$

where $G_m = 1/2 - \epsilon_m > 0$.

#### Statistical Interpretation: Minimizing Exponential Loss

Friedman, Hastie, and Tibshirani showed that AdaBoost is equivalent to **Forward Stagewise Additive Modeling** using the **exponential loss function**:
$$L(y, f(x)) = \exp(-y f(x))$$

##### The Forward Stagewise Additive Model
Stagewise additive modeling builds an additive expansion of the form $f(x) = \sum_{m=1}^M \beta_m b(x; \gamma_m)$ sequentially, adding one basis function at a time without modifying the parameters of previously added terms.
At each step $m$, we solve:
$$(\beta_m, G_m) = \arg \min_{\beta, G} \sum_{n=1}^N \exp \left[ -y_n (f_{m-1}(x_n) + \beta G(x_n)) \right]$$

Letting $w_n^{(m)} = \exp(-y_n f_{m-1}(x_n))$ denote the weights of the observations computed from the current model:
$$(\beta_m, G_m) = \arg \min_{\beta, G} \sum_{n=1}^N w_n^{(m)} \exp \left[ -y_n \beta G(x_n) \right]$$

For any $\beta > 0$, we split the summation into correctly classified and misclassified points:
$$\sum_{n=1}^N w_n^{(m)} e^{-y_n \beta G(x_n)} = e^{-\beta} \sum_{y_n = G(x_n)} w_n^{(m)} + e^{\beta} \sum_{y_n \neq G(x_n)} w_n^{(m)}$$
$$= e^{-\beta} \sum_{n=1}^N w_n^{(m)} + (e^{\beta} - e^{-\beta}) \sum_{n=1}^N w_n^{(m)} I(y_n \neq G(x_n))$$

Since the first term is independent of $G(x)$, the optimal classifier $G_m(x)$ is found by minimizing:
$$G_m = \arg \min_{G} \sum_{n=1}^N w_n^{(m)} I(y_n \neq G(x_n))$$

which is exactly the weak learner's objective at step $m$. 
Substituting this $G_m$ back into our objective and differentiating with respect to $\beta$ yields:
$$\beta_m = \frac{1}{2} \ln \left( \frac{1 - \epsilon_m}{\epsilon_m} \right)$$

which matches the AdaBoost coefficient $\alpha_m$ exactly.

##### The Population Minimizer of Exponential Loss
The appropriateness of this criterion is addressed by seeking its population minimizer:
$$f★(x) = \arg \min_{f(x)} \mathbb{E}_{Y \mid x} \left[ e^{-Y f(x)} \right]$$

For a binary target $Y \in \{-1, +1\}$, the conditional expectation is:
$$\mathbb{E}_{Y \mid x} \left[ e^{-Y f(x)} \right] = P(Y = 1 \mid x) e^{-f(x)} + P(Y = -1 \mid x) e^{f(x)}$$

Differentiating with respect to $f(x)$ and setting the derivative to zero:
$$-P(Y = 1 \mid x) e^{-f(x)} + P(Y = -1 \mid x) e^{f(x)} = 0 \implies e^{2f(x)} = \frac{P(Y = 1 \mid x)}{P(Y = -1 \mid x)}$$
$$f★(x) = \frac{1}{2} \ln \left( \frac{P(Y = 1 \mid x)}{P(Y = -1 \mid x)} \right)$$

Thus, the additive expansion produced by AdaBoost is estimating **one-half the log-odds ratio** of the class posterior probabilities. This justifies using its sign as the classification decision rule.

#### Overfitting, Shrinkage, and Robustness in Boosting
*   **Overfitting:** Boosting increase model complexity at each iteration. If run to convergence, boosting can overfit, particularly on small or noisy datasets. 
*   **Early Stopping:** We monitor the prediction risk on a validation set as a function of $M$, stopping when the validation risk reaches a minimum to find the optimal iteration index $M^★$.
*   **Binomial Deviance Loss:** The exponential loss $\exp(-yf)$ penalizes large negative margins exponentially, making it highly sensitive to outliers and mislabeled data. The **Binomial Deviance Loss** (cross-entropy) $\ln(1 + e^{-yf})$ provides a linear penalty for large negative margins, making it far more robust in noisy settings.
*   **Shrinkage (Slow Learning):** Introducing a learning rate parameter $\nu \in (0, 1]$ to scale each new term:
    $$f_m(x) = f_{m-1}(x) + \nu \beta_m G_m(x)$$
    substantially improves generalization. The parameter $\nu$ acts as a regularizer. A small value of $\nu$ (e.g., $0.1$ or $0.05$) requires a larger number of iterations $M$, but achieves a lower minimum test error than un-shrunken boosting, analogous to the $L_1$ penalty of the lasso.
*   **Subsampling (Stochastic Gradient Boosting):** At each iteration, we sample a fraction $\eta$ (typically $0.5$) of the training observations without replacement to grow the next tree. This decorrelates the base learners, reducing variance and improving accuracy.
