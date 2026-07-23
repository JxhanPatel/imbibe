# 5.1 Supervised Learning and Linear Regression

## 1. Introduction to Supervised Learning

### 1.1 Labeled Data and Training Sets
In a typical machine learning approach, a large set of $N$ observations $\{x_{1},...,x_{N}\}$ called a training set is used to tune the parameters of an adaptive model. The categories of the data in the training set are known in advance, typically by hand-labelling them. We can express the category of an observation using a target vector $t$, which represents the identity of the corresponding observation. Applications in which the training data comprises examples of the input vectors along with their corresponding target vectors are known as supervised learning problems.

### 1.2 Learning by Example
The system observes inputs and outputs and assembles a training set $\mathcal{T} = \{(x_{i}, y_{i})\}{i=1}^{N}$. The learning algorithm has the property that it can modify its input/output relationship $\hat{f}$ in response to differences $y_{i} - \hat{f}(x_{i})$ between the original and generated outputs. This process is known as learning by example.

### 1.3 Classification and Regression
Supervised learning problems can be divided into two main categories based on the nature of the output:
*   **Classification:** Cases in which the aim is to assign each input vector to one of a finite number of discrete categories are called classification problems. An example is the recognition of handwritten digits where the identity of the digit $0,...,9$ is produced as output.
*   **Regression:** If the desired output consists of one or more continuous variables, then the task is called regression. An example would be the prediction of the yield in a chemical manufacturing process in which the inputs consist of the concentrations of reactants, the temperature, and the pressure.

<img width="797" height="208" alt="image" src="https://github.com/user-attachments/assets/6cb61a15-3e8a-4f21-baf5-7ee9870b08f4" />

## 2. Linear Regression

Linear regression assumes that the regression function $E(Y|X)$ is linear in the inputs $X_{1},...,X_{p}$. The linear regression model has the form:
$$f(X) = \beta_{0} + \sum_{j=1}^{p} X_{j}\beta_{j}$$.
Functionally, we can also fit data using a polynomial function of the form:
$$y(x,w) = w_{0} + w_{1}x + w_{2}x^{2} + ... + w_{M}x^{M} = \sum_{j=0}^{M} w_{j}x^{j}$$.
Functions which are linear in the unknown parameters are called linear models.

<img width="700" height="465" alt="image" src="https://github.com/user-attachments/assets/9c98072b-acee-484b-b1e0-30d5daa07061" />

### 2.1 Error Functions (Loss Functions)
The values of the coefficients are determined by fitting the model to the training data by minimizing an error function that measures the misfit between the function and the training data points.

#### Sum-of-Squares Error
One simple choice of error function is given by the sum of the squares of the errors between the predictions $y(x_{n},w)$ and the corresponding target values $t_{n}$:
$$E(w) = \frac{1}{2} \sum_{n=1}^{N} \{y(x_{n},w) - t_{n}\}^{2}$$.
The factor of $1/2$ is included for later convenience. This is a nonnegative quantity that would be zero if, and only if, the function $y(x,w)$ were to pass exactly through each training data point.

#### Residual Sum of Squares (RSS)
In the method of least squares, we pick the coefficients $\beta$ to minimize the residual sum of squares:
$$RSS(\beta) = \sum_{i=1}^{N} (y_{i} - x_{i}^{T}\beta)^{2}$$.
In matrix notation, this is written as:
$$RSS(\beta) = (y - X\beta)^{T}(y - X\beta)$$.

<img width="798" height="312" alt="image" src="https://github.com/user-attachments/assets/c28a242a-a660-45b6-b66f-b1af610a07c2" />

### 2.2 Overfitting and Model Complexity
The problem of choosing the order $M$ of a polynomial is an example of model selection. 
*   **Under-fitting:** Constant ($M=0$) and first order ($M=1$) polynomials give poor fits to the data and capabilitiesCaping the oscillations of the underlying function.
*   **Over-fitting:** When a much higher order polynomial (such as $M=9$) is used, we obtain an excellent fit to the training data, and the polynomial may pass exactly through each data point so that $E(w^★) = 0$. However, the fitted curve oscillates wildly and gives a very poor representation of the generating function. This behavior is known as over-fitting.

<img width="847" height="674" alt="image" src="https://github.com/user-attachments/assets/bef201ae-3c25-40a9-a61b-58bbdca91528" />

### 2.3 Generalization and Performance Assessment
The goal of running a machine learning algorithm is to achieve good generalization, which is the ability to categorize correctly new examples that differ from those used for training.

#### Root-Mean-Square (RMS) Error
Generalization performance can be measured using the root-mean-square (RMS) error:
$$E_{RMS} = \sqrt{2E(w^★)/N}$$.
The division by $N$ allows for the comparison of different sizes of data sets, and the square root ensures $E_{RMS}$ is measured on the same scale as the target variable $t$.

#### Training Error vs. Test Error
*   **Training Error:** The average loss over the training sample. It consistently decreases with model complexity, typically dropping to zero if complexity is increased enough.
*   **Test Error (Generalization Error):** The expected prediction error over an independent test set. 
*   **Tradeoff:** There is some intermediate model complexity that gives minimum expected test error. For $M=9$, the training error may go to zero, but the test error becomes very large due to wild oscillations.

<img width="680" height="593" alt="image" src="https://github.com/user-attachments/assets/facf1545-f768-436f-a5e1-86a548cf096d" />

### 2.4 Control of Overfitting: Regularization
A technique used to control over-fitting for complex and flexible models is regularization. This involves adding a penalty term to the error function to discourage the coefficients from reaching large values. The simplest penalty term leads to a modified error function of the form:
$$\tilde{E}(w) = \frac{1}{2} \sum_{n=1}^{N} \{y(x_{n},w) - t_{n}\}^{2} + \frac{\lambda}{2} ||w||^{2}$$.
where $||w||^{2} \equiv w^{T}w = w_{0}^{2} + w_{1}^{2} + ... + w_{M}^{2}$. The coefficient $\lambda$ governs the relative importance of the regularization term compared with the sum-of-squares error term. In statistics, this particular case is called ridge regression. In neural networks, it is known as weight decay.

<img width="881" height="392" alt="image" src="https://github.com/user-attachments/assets/3520cd48-1f91-42d1-b181-0aeeddb2e4cf" />




---






# 5.2 Optimizing the Error Function

## 1. Parameter Optimization

The task of network training involves finding a weight vector $w$ which minimizes a chosen error function $E(w)$. Geometrically, the error function can be viewed as a surface sitting over weight space.

<img width="792" height="333" alt="image" src="https://github.com/user-attachments/assets/7c8dee99-5acd-48b5-9d42-526769a6b545" />

### 1.1 Stationary Points
If we make a small step in weight space from $w$ to $w + \delta w$, the change in the error function is $\delta E \simeq \delta w^{T} \nabla E(w)$. The vector $\nabla E(w)$ points in the direction of the greatest rate of increase of the error function. Because the error $E(w)$ is a smooth continuous function of $w$, its smallest value will occur at a point in weight space such that the gradient of the error function vanishes:
$$\nabla E(w) = 0$$
as otherwise we could make a small step in the direction of $-\nabla E(w)$ and thereby further reduce the error. Points at which the gradient vanishes are called stationary points, and may be further classified into minima, maxima, and saddle points.

### 1.2 Local and Global Minima
The error function typically has a highly nonlinear dependence on the weights and bias parameters, and so there will be many points in weight space at which the gradient vanishes.
*   **Equivalent Minima:** For any point $w$ that is a local minimum, there will be other points in weight space that are equivalent minima due to weight-space symmetries. In a two-layer network with $M$ hidden units, each point in weight space is a member of a family of $M!2^{M}$ equivalent points.
*   **Inequivalent Minima:** There will typically be multiple inequivalent stationary points. A minimum that corresponds to the smallest value of the error function for any weight vector is a global minimum. Any other minima corresponding to higher values of the error function are local minima. 

### 1.3 Iterative Numerical Procedures
Because there is no hope of finding an analytical solution to $\nabla E(w) = 0$, we resort to iterative numerical procedures. These involve choosing some initial value $w^{(0)}$ for the weight vector and moving through weight space in a succession of steps of the form:
$$w^{(\tau+1)} = w^{(\tau)} + \Delta w^{(\tau)}$$
where $\tau$ labels the iteration step. Many algorithms make use of gradient information and require that, after each update, the value of $\nabla E(w)$ is evaluated at the new weight vector $w^{(\tau+1)}$.

## 2. Local Quadratic Approximation

Insight into the optimization problem can be obtained by considering a local quadratic approximation to the error function based on a Taylor expansion around some point $\hat{w}$:
$$E(w) \simeq E(\hat{w}) + (w - \hat{w})^{T}b + \frac{1}{2}(w - \hat{w})^{T}H(w - \hat{w})$$
where cubic and higher terms have been omitted. Here $b$ is the gradient of $E$ evaluated at $\hat{w}$:
$$b \equiv \nabla E|{w=\hat{w}}$$
and the Hessian matrix $H = \nabla \nabla E$ has elements:
$$(H){ij} \equiv \frac{\partial^{2}E}{\partial w_{i}\partial w_{j}}|_{w=\hat{w}}$$
The corresponding local approximation to the gradient is:
$$\nabla E \simeq b + H(w - \hat{w})$$
For points $w$ that are sufficiently close to $\hat{w}$, these expressions give reasonable approximations for the error and its gradient.

### 2.1 Approximation at a Minimum
Consider the particular case of a local quadratic approximation around a point $w^★$ that is a minimum of the error function. In this case there is no linear term, because $\nabla E = 0$ at $w^★$, and the expansion becomes:
$$E(w) = E(w^★) + \frac{1}{2}(w - w^★)^{T}H(w - w^★)$$
where the Hessian $H$ is evaluated at $w^★$. Consider the eigenvalue equation for the Hessian matrix:
$$Hu_{i} = \lambda_{i}u_{i}$$
where the eigenvectors $u_{i}$ form a complete orthonormal set. We expand $(w - w^★)$ as a linear combination of the eigenvectors:
$$w - w^★ = \sum_{i} \alpha_{i} u_{i}$$
The error function can then be written in the form:
$$E(w) = E(w^★) + \frac{1}{2} \sum_{i} \lambda_{i} \alpha_{i}^{2}$$
A matrix $H$ is positive definite if, and only if, $v^{T}Hv > 0$ for all $v$. This holds if, and only if, all of its eigenvalues are positive. In the coordinate system defined by the eigenvectors $u_{i}$, the contours of constant $E$ are ellipses centered on $w^★$.

<img width="801" height="277" alt="image" src="https://github.com/user-attachments/assets/55fa1739-1ca0-4230-8c65-70e10b407744" />

## 3. Use of Gradient Information

The use of gradient information can lead to significant improvements in the speed with which the minima can be located. 
*   **Without Gradient Information:** In a quadratic approximation, the error surface is specified by $W(W + 3)/2$ independent elements where $W$ is the dimensionality of $w$. Without gradient information, we would expect to perform $O(W^{2})$ function evaluations, each requiring $O(W)$ steps, leading to a computational effort of $O(W^{3})$.
*   **With Gradient Information:** Because each evaluation of $\nabla E$ brings $W$ items of information, we might find the minimum in $O(W)$ gradient evaluations. Using error backpropagation, each evaluation takes only $O(W)$ steps, and thus the minimum can be found in $O(W^{2})$ steps.

## 4. Gradient Descent Optimization

The simplest approach to using gradient information is to choose the weight update to comprise a small step in the direction of the negative gradient:
$$w^{(\tau+1)} = w^{(\tau)} - \eta \nabla E(w^{(\tau)})$$
where the parameter $\eta > 0$ is the learning rate. 

### 4.1 Batch Methods
Techniques that use the whole data set at once to evaluate $\nabla E$ are called batch methods. At each step, the weight vector is moved in the direction of the greatest rate of decrease of the error function, an approach known as gradient descent or steepest descent. For batch optimization, more efficient methods exist, such as conjugate gradients and quasi-Newton methods, which are faster and ensure the error function decreases at each iteration.

### 4.2 On-line (Stochastic) Gradient Descent
Error functions based on maximum likelihood for independent observations comprise a sum of terms $E(w) = \sum_{n=1}^{N} E_{n}(w)$. On-line gradient descent (also known as sequential or stochastic gradient descent) makes an update based on one data point at a time:
$$w^{(\tau+1)} = w^{(\tau)} - \eta \nabla E_{n}(w^{(\tau)})$$
This update is repeated by cycling through the data in sequence or selecting points at random with replacement. 
*   **Redundancy:** On-line methods handle redundancy in data much more efficiently than batch methods.
*   **Local Minima:** On-line gradient descent has the possibility of escaping from local minima, as a stationary point for the whole data set will generally not be a stationary point for each individual data point.




---





# 5.3: Geometric Interpretation of Linear Regression | Projections, Subspaces & Least Squares

## 1. $N$-Dimensional Geometry of Least Squares

The least squares estimate can be represented geometrically in an $N$-dimensional space where the axes are the values of the observations $t_{1}, ..., t_{N}$. In this formulation, the target values are collected into a vector $\mathfrak{t} = (t_{1}, ..., t_{N})^{T}$ (or $y$ in some notations) which exists as a single point in this $N$-dimensional space.

### 1.1 The Column Subspace
Each basis function $\phi_{j}(x_{n})$, when evaluated at the $N$ data points, can be represented as a vector in the same $N$-dimensional space, denoted by $\varphi_{j}$. 
*   **Basis Vectors:** $\varphi_{j}$ corresponds to the $j^{th}$ column of the design matrix $\Phi$.
*   **Input Vectors:** In an alternate notation, the column vectors of the model matrix $X$ are denoted by $x_{0}, x_{1}, ..., x_{p}$.
*   **The Subspace $S$:** If the number $M$ of basis functions is smaller than the number $N$ of data points, then the $M$ vectors $\varphi_{j}$ will span a linear subspace $S$ of dimensionality $M$. This is also referred to as the column space of $X$.

<img width="772" height="510" alt="image" src="https://github.com/user-attachments/assets/5c4f5d6d-fc83-46da-a46d-afb627123c08" />

### 1.2 The Fitted Vector and Residuals
We define $y$ (or $\hat{y}$) to be an $N$-dimensional vector whose $n^{th}$ element is given by the model prediction $y(x_{n}, w)$.
*   **Subspace Location:** Because $y$ is a linear combination of the vectors $\varphi_{j}$, it can live anywhere in the $M$-dimensional subspace $S$.
*   **Sum-of-Squares Error:** The sum-of-squares error $E(w) = \frac{1}{2}\sum_{n=1}^{N}\{y(x_{n}, w) - t_{n}\}^{2}$ is equal (up to a factor of $1/2$) to the squared Euclidean distance between $y$ and $\mathfrak{t}$.

<img width="802" height="223" alt="image" src="https://github.com/user-attachments/assets/ed7da8ad-258b-4fc0-86e6-b87e77c52611" />

## 2. Orthogonal Projection and the Hat Matrix

The least-squares solution for $w$ corresponds to the choice of $y$ that lies in the subspace $S$ and is closest to $\mathfrak{t}$.

### 2.1 Orthogonality Condition
The minimum distance solution corresponds to the orthogonal projection of $\mathfrak{t}$ onto the subspace $S$. 
*   **The Residual Vector:** We minimize $RSS(\beta) = ||y - X\beta||^{2}$ by choosing $\hat{\beta}$ so that the residual vector $y - \hat{y}$ is orthogonal to the column space of $X$.
*   **Mathematical Expression:** This orthogonality is expressed as:
$$X^{T}(y - X\beta) = 0$$.

### 2.2 The Hat Matrix ($H$)
The fitted values at the training inputs are given by $\hat{y} = X\hat{\beta} = X(X^{T}X)^{-1}X^{T}y$.
*   **Definition:** The matrix $H = X(X^{T}X)^{-1}X^{T}$ is called the "hat" matrix because it "puts the hat" on $y$.
*   **Projection Operator:** $H$ is also known as a projection matrix because it computes the orthogonal projection of the target vector onto the subspace spanned by the inputs.

> [!NOTE]
> If the columns of $X$ are not linearly independent (e.g., two inputs are perfectly correlated), $X^{T}X$ is singular and the least squares coefficients are not uniquely defined. However, the fitted values $\hat{y}$ are still the unique projection of $y$ onto the column space of $X$.

## 3. Geometry of Weight Space

In contrast to the $N$-dimensional observation space, the parameters can be viewed in a $p$-dimensional (or $(d+1)$-dimensional) weight space.

### 3.1 Stationary Points
The error function $E(w)$ can be viewed as a surface sitting over the weight space.
*   **Gradient:** The vector $\nabla E(w)$ points in the direction of the greatest rate of increase of the error function.
*   **Minima:** The smallest value of the error will occur at a point where the gradient vanishes, $\nabla E(w) = 0$.

### 3.2 Solution Region for Separable Data
In the context of the two-category linearly-separable case, the weight vector $a$ specifies a point in weight space.
*   **Constraints:** Each sample $y_{i}$ places a constraint on the location of a solution vector; the equation $a^{T}y_{i} = 0$ defines a hyperplane through the origin of weight space.
*   **Intersection of Halfspaces:** A solution vector $a$ such that $a^{T}y_{i} > 0$ for all samples must lie in the intersection of $n$ halfspaces, a region called the solution region.

<img width="933" height="516" alt="image" src="https://github.com/user-attachments/assets/2692544f-557c-4210-96ce-936f4dd10391" />

## 4. Derived Directions and Orthogonalization

The multiple least squares estimates are best understood in terms of successive orthogonalization of the inputs.

### 4.1 Regression by Successive Orthogonalization
The $j^{th}$ multiple regression coefficient $\hat{\beta}{j}$ represents the additional contribution of $x_{j}$ on $y$, after $x_{j}$ has been adjusted for all other predictors $x_{0}, x_{1}, ..., x_{j-1}, x_{j+1}, ..., x_{p}$.
*   **Adjustment:** "Regressing $b$ on $a$" produces a residual vector $b - \hat{\gamma}a$ that is "orthogonalized" with respect to $a$.
*   **Correlation Effect:** If $x_{p}$ is highly correlated with other $x_{k}$, the residual vector $z_{p}$ after orthogonalization will be close to zero, and the coefficient $\hat{\beta}_{p}$ will be very unstable.

### 4.2 QR Decomposition
The process of successive orthogonalization can be represented in matrix form as $X = QR$, where:
*   **$Q$:** An $N \times (p+1)$ orthogonal matrix ($Q^{T}Q = I$).
*   **$R$:** A $(p+1) \times (p+1)$ upper triangular matrix.
The least squares fit is then $\hat{y} = QQ^{T}y$.




---





# 5.4: Gradient descent | Optimization & Stochastic Gradient Descent (SGD)

## 1. Parameter Optimization

The task of network training involves finding a weight vector $w$ which minimizes a chosen function $E(w)$. Geometrically, the error function can be viewed as a surface sitting over weight space.

### 1.1 Stationary Points
If we make a small step in weight space from $w$ to $w + \delta w$ then the change in the error function is $\delta E \simeq \delta w^{T} \nabla E(w)$, where the vector $\nabla E(w)$ points in the direction of greatest rate of increase of the error function. Because the error $E(w)$ is a smooth continuous function of $w$, its smallest value will occur at a point in weight space such that the gradient of the error function vanishes, so that:
$$\nabla E(w) = 0$$
as otherwise we could make a small step in the direction of $-\nabla E(w)$ and thereby further reduce the error. Points at which the gradient vanishes are called stationary points, and may be further classified into minima, maxima, and saddle points.

### 1.2 Local and Global Minima
The error function typically has a highly nonlinear dependence on the weights and bias parameters, and so there will be many points in weight space at which the gradient vanishes. A minimum that corresponds to the smallest value of the error function for any weight vector is said to be a global minimum. Any other minima corresponding to higher values of the error function are said to be local minima.

### 1.3 Iterative Numerical Procedures
Because there is clearly no hope of finding an analytical solution to the equation $\nabla E(w) = 0$ we resort to iterative numerical procedures. Most techniques involve choosing some initial value $w^{(0)}$ for the weight vector and then moving through weight space in a succession of steps of the form:
$$w^{(\tau+1)} = w^{(\tau)} + \Delta w^{(\tau)}$$
where $\tau$ labels the iteration step. Many algorithms make use of gradient information and therefore require that, after each update, the value of $\nabla E(w)$ is evaluated at the new weight vector $w^{(\tau+1)}$.

## 2. Basic Gradient Descent

The approach taken to finding a solution to the set of linear inequalities $a^{t}y_{i} > 0$ will be to define a criterion function $J(a)$ that is minimized if $a$ is a solution vector. This reduces the problem to one of minimizing a scalar function, a problem that can often be solved by a gradient descent procedure. Basic gradient descent is very simple. We start with some arbitrarily chosen weight vector $a(1)$ and compute the gradient vector $\nabla J(a(1))$. The next value $a(2)$ is obtained by moving some distance from $a(1)$ in the direction of steepest descent, i.e., along the negative of the gradient. In general, $a(k+1)$ is obtained from $a(k)$ by the equation:
$$a(k+1) = a(k) - \eta(k) \nabla J(a(k))$$
where $\eta$ is a positive scale factor or learning rate that sets the step size.

### 2.1 Algorithm: Basic Gradient Descent
1. **begin** initialize $a$, criterion $\theta$, $\eta(\cdot)$, $k=0$
2. **do** $k \leftarrow k+1$
3. $a \leftarrow a - \eta(k) \nabla J(a)$
4. **until** $\eta(k) \nabla J(a) < \theta$
5. **return** $a$
6. **end**

> [!CAUTION]
> If $\eta(k)$ is too small, convergence is needlessly slow, whereas if $\eta(k)$ is too large, the correction process will overshoot and can even diverge.

## 3. Local Quadratic Approximation

Insight into the optimization problem, and into the various techniques for solving it, can be obtained by considering a local quadratic approximation to the error function. Consider the Taylor expansion of $E(w)$ around some point $\hat{w}$ in weight space:
$$E(w) \simeq E(\hat{w}) + (w - \hat{w})^{T}b + \frac{1}{2}(w - \hat{w})^{T}H(w - \hat{w})$$
where cubic and higher terms have been omitted. Here $b$ is defined to be the gradient of $E$ evaluated at $\hat{w}$:
$$b \equiv \nabla E|{w=\hat{w}}$$
and the Hessian matrix $H = \nabla \nabla E$ has elements:
$$(H){ij} \equiv \frac{\partial E}{\partial w_{i}\partial w_{j}}|_{w=\hat{w}}$$.
From the expansion, the corresponding local approximation to the gradient is given by:
$$\nabla E \simeq b + H(w - \hat{w})$$.

### 3.1 Approximation at a Minimum
Consider the particular case of a local quadratic approximation around a point $w^★$ that is a minimum of the error function. In this case there is no linear term, because $\nabla E = 0$ at $w^★$, and the expansion becomes:
$$E(w) = E(w^★) + \frac{1}{2}(w - w^★)^{T}H(w - w^★)$$.
In the coordinate system whose basis vectors are given by the eigenvectors $\{u_{i}\}$ of the Hessian matrix, the contours of constant $E$ are ellipses centred on the origin.


## 4. Newton's Algorithm

An alternative approach is choosing $a(k + 1)$ to minimize the second-order expansion, which is Newton’s algorithm. The weight update is:
$$a(k+1) = a(k) - H^{-1} \nabla J$$.

### 4.1 Algorithm: Newton Descent
1. **begin** initialize $a$, criterion
2. **do**
3. $a \leftarrow a - H^{-1} \nabla J(a)$
4. **until** $H^{-1} \nabla J(a) < \theta$
5. **return** $a$
6. **end**

Generally speaking, Newton’s algorithm will usually give a greater improvement per step than the simple gradient descent algorithm, even with the optimal value of $\eta(k)$. However, Newton’s algorithm is not applicable if the Hessian matrix $H$ is singular.

<img width="963" height="623" alt="image" src="https://github.com/user-attachments/assets/5d0a508d-c9db-4ed4-ad13-52e59d64bc1f" />

## 5. Batch Gradient Descent

The simplest approach to using gradient information is to choose the weight update to comprise a small step in the direction of the negative gradient, so that:
$$w^{(\tau+1)} = w^{(\tau)} - \eta \nabla E(w^{(\tau)})$$
where the parameter $\eta > 0$ is known as the learning rate. After each such update, the gradient is re-evaluated for the new weight vector and the process repeated. The error function is defined with respect to a training set, and so each step requires that the entire training set be processed in order to evaluate $\nabla E$. Techniques that use the whole data set at once are called batch methods. At each step the weight vector is moved in the direction of the greatest rate of decrease of the error function, and so this approach is known as gradient descent or steepest descent.

## 6. On-line (Stochastic) Gradient Descent (SGD)

Error functions based on maximum likelihood for a set of independent observations comprise a sum of terms, one for each data point:
$$E(w) = \sum_{n=1}^{N} E_{n}(w)$$.
On-line gradient descent, also known as sequential gradient descent or stochastic gradient descent, makes an update to the weight vector based on one data point at a time, so that:
$$w^{(\tau+1)} = w^{(\tau)} - \eta \nabla E_{n}(w^{(\tau)})$$.
This update is repeated by cycling through the data either in sequence or by selecting points at random with replacement.

### 6.1 Advantages of On-line Methods
*   **Efficiency with Redundancy:** On-line methods handle redundancy in the data much more efficiently than batch methods. If a data set is doubled by duplicating every data point, batch methods will require double the computational effort to evaluate the gradient, whereas on-line methods will be unaffected.
*   **Escaping Local Minima:** On-line gradient descent offers the possibility of escaping from local minima, since a stationary point with respect to the error function for the whole data set will generally not be a stationary point for each data point individually.

### 6.2 Convergence and Learning Rate
The learning rate $\gamma_{r}$ for batch learning is usually taken to be a constant, and can also be optimized by a line search that minimizes the error function at each update. With online learning $\gamma_{r}$ should decrease to zero as the iteration $r \rightarrow \infty$. This learning is a form of stochastic approximation; results in this field ensure convergence if $\gamma_{r} \rightarrow 0, \sum_{r} \gamma_{r} = \infty$ and $\sum_{r} \gamma_{r}^{2} < \infty$ (satisfied, for example, by $\gamma_{r} = 1/r$).

## 7. Comparison Summary

| Method | Data Usage | Update Frequency | Complexity per Update |
| :--- | :--- | :--- | :--- |
| **Batch Gradient Descent** | Entire training set | Once per epoch | $O(W)$ where $W$ is total weights |
| **Stochastic Gradient Descent** | Single data point | For every observation | $O(1)$ relative to training set size |
| **Newton's Method** | Entire training set | Once per epoch | $O(W^3)$ due to Hessian inversion |






---





# 5.5: Kernel regression | non-linear regression with the kernel trick

## 1. Introduction to Kernel Smoothing and Localization

Kernel methods achieve flexibility in estimating the regression function $f(X)$ over the domain $\mathbb{R}^{p}$ by fitting a different but simple model separately at each query point $x_{0}$. This is done by using only those observations close to the target point $x_{0}$ to fit the simple model, and in such a way that the resulting estimated function $\hat{f}(X)$ is smooth in $\mathbb{R}^{p}$. This localization is achieved via a weighting function or kernel $K_{\lambda}(x_{0},x_{i})$, which assigns a weight to $x_{i}$ based on its distance from $x_{0}$. The kernels $K_{\lambda}$ are typically indexed by a parameter $\lambda$ that dictates the width of the neighborhood. These memory-based methods require in principle little or no training; all the work gets done at evaluation time.

<img width="790" height="601" alt="image" src="https://github.com/user-attachments/assets/253a8e8c-b357-49b0-bd5f-b5310d65e6be" />

## 2. The Nadaraya-Watson Model

The kernel regression model can be motivated starting with kernel density estimation. Suppose we have a training set $\{x_{n},t_{n}\}$ and we use a Parzen density estimator to model the joint distribution $p(x,t)$, so that:
$$p(x,t)=\frac{1}{N}\sum_{n=1}^{N}f(x-x_{n},t-t_{n})$$
where $f(x,t)$ is the component density function, and there is one such component centred on each data point. We now find an expression for the regression function $y(x)$, corresponding to the conditional average of the target variable conditioned on the input variable, which is given by:
$$y(x) = \mathbb{E}[t|x] = \int_{-\infty}^{\infty} tp(t|x) dt = \frac{\int tp(x,t)dt}{\int p(x,t)dt} = \frac{\sum_{n} \int tf(x-x_{n},t-t_{n})dt}{\sum_{m} \int f(x-x_{m},t-t_{m})dt}$$.

Assuming for simplicity that the component density functions have zero mean, so that $\int_{-\infty}^{\infty} f(x,t)t dt = 0$ for all values of $x$, we obtain:
$$y(x) = \frac{\sum_{n}g(x-x_{n})t_{n}}{\sum_{m}g(x-x_{m})} = \sum_{n}k(x,x_{n})t_{n}$$.
The kernel function $k(x,x_{n})$ is given by:
$$k(x,x_{n}) = \frac{g(x-x_{n})}{\sum_{m}g(x-x_{m})}$$
where $g(x) = \int_{-\infty}^{\infty} f(x,t)dt$. 

This result is known as the Nadaraya-Watson model, or kernel regression. For a localized kernel function, it has the property of giving more weight to the data points $x_{n}$ that are close to $x$. The kernel $k(x,x_{n})$ satisfies the summation constraint $\sum_{n=1}^{N} k(x,x_{n}) = 1$.

<img width="1104" height="406" alt="image" src="https://github.com/user-attachments/assets/c86f749c-9a77-49f0-9ee5-4ad8ffcc7bca" />

## 3. Dual Representations and the Kernel Trick

Many linear models for regression can be reformulated in terms of a dual representation in which the kernel function arises naturally. Consider a linear regression model whose parameters are determined by minimizing a regularized sum-of-squares error function given by:
$$J(w) = \frac{1}{2}\sum_{n=1}^{N}\{w^{T}\phi(x_{n})-t_{n}\}^{2}+\frac{\lambda}{2}w^{T}w$$
where $\lambda \ge 0$. Setting the gradient of $J(w)$ with respect to $w$ equal to zero shows that the solution for $w$ takes the form of a linear combination of the vectors $\phi(x_{n})$:
$$w = \sum_{n=1}^{N}a_{n}\phi(x_{n}) = \Phi^{T}a$$.

### 3.1 The Gram Matrix
Instead of working with the parameter vector $w$, we can reformulate the algorithm in terms of the parameter vector $a$, giving rise to a dual representation. We define the Gram matrix $K = \Phi\Phi^{T}$, which is an $N \times N$ symmetric matrix with elements:
$$K_{nm} = \phi(x_{n})^{T}\phi(x_{m}) = k(x_{n},x_{m})$$
where $k(x,x')$ is the kernel function. The solution for $a$ is:
$$a = (K + \lambda I_{N})^{-1}t$$.
Substituting this back into the linear regression model, we obtain the prediction for a new input $x$:
$$y(x) = w^{T}\phi(x) = a^{T}\Phi\phi(x) = k(x)^{T}(K + \lambda I_{N})^{-1}\mathfrak{t}$$
where $k(x)$ has elements $k_{n}(x) = k(x_{n},x)$.

> [!NOTE]
> The advantage of the dual formulation is that it is expressed entirely in terms of the kernel function $k(x,x')$. We can therefore work directly in terms of kernels and avoid the explicit introduction of the feature vector $\phi(x)$, which allows us implicitly to use feature spaces of high, even infinite, dimensionality. This is known as the kernel trick or kernel substitution.

## 4. Radial Basis Functions (RBF) and Kernels

Radial basis functions combine the ideas of basis expansions and kernel methods by treating the kernel functions $K_{\lambda}(\xi, x)$ as basis functions. This leads to the model:
$$f(x) = \sum_{j=1}^{M} K_{\lambda_{j}}(\xi_{j},x)\beta_{j} = \sum_{j=1}^{M} D\left(\frac{||x-\xi_{j}||}{\lambda_{j}}\right)\beta_{j}$$
where each basis element is indexed by a location parameter $\xi_{j}$ and a scale parameter $\lambda_{j}$. 

The Nadaraya-Watson kernel regression estimator can be viewed as an expansion in renormalized radial basis functions:
$$\hat{f}(x_{0}) = \sum_{i=1}^{N} y_{i} \frac{K_{\lambda}(x_{0},x_{i})}{\sum_{n=1}^{N} K_{\lambda}(x_{0},x_{n})} = \sum_{i=1}^{N} y_{i} h_{i}(x_{0})$$
with a basis function $h_{i}$ located at every observation and coefficients $y_{i}$.

<img width="820" height="467" alt="image" src="https://github.com/user-attachments/assets/bd088860-cd14-4b67-ae66-564b297544b2" />

## 5. Kernel Properties for Regression

Suppose we consider approximation of the regression function in terms of a set of basis functions $\{h_{m}(x)\}$, $m=1,2,...,M$:
$$f(x) = \sum_{m=1}^{M}\beta_{m}h_{m}(x)+\beta_{0}$$.
To estimate $\beta$ and $\beta_{0}$ we minimize:
$$H(\beta,\beta_{0}) = \sum_{i=1}^{N}V(y_{i}-f(x_{i}))+\frac{\lambda}{2}\sum \beta_{m}^{2}$$
for some general error measure $V(r)$. For any choice of $V(r)$ the solution has the form:
$$\hat{f}(x) = \sum_{i=1}^{N}\hat{a}_{i}K(x,x_{i})$$
with $K(x,y) = \sum_{m=1}^{M}h_{m}(x)h_{m}(y)$. 

### 5.1 Penalized Least Squares Case
For concreteness, in the case $V(r) = r^{2}$ (penalized least squares), the predicted values at an arbitrary $x$ satisfy:
$$\hat{f}(x) = h(x)^{T}\hat{\beta} = \sum_{i=1}^{N}\hat{\alpha}{i}K(x,x_{i})$$
where $\hat{\alpha} = (HH^{T} + \lambda I)^{-1}y$. Only the inner product kernel $K(x_{i},x_{i'})$ need be evaluated, at the $N$ training points for each $i, i'$ and at points for predictions there.

## 6. Summary of Kernel Regression Methods

| Method | Characteristics |
| :--- | :--- |
| **Nadaraya-Watson** | Weighted average of $y_i$ using a kernel; weights sum to 1. |
| **Local Linear Regression** | Fits a straight line locally to remove boundary bias; automatically modifies the kernel. |
| **Dual Linear Regression** | Expresses the solution for $w$ as a linear combination of training inputs $\phi(x_n)$. |
| **RBF Networks** | Uses Gaussian kernels as basis functions; parameters learned via least squares or clustering. |
| **Kernelized Ridge Regression** | Minimizes penalized RSS entirely in terms of the Gram matrix $K$. |
| **Gaussian Processes** | Bayesian framework where the kernel defines the prior covariance between function values. |

> [!IMPORTANT]
> A necessary and sufficient condition for a function $k(x,x')$ to be a valid kernel is that the Gram matrix $K$, whose elements are given by $k(x_{n},x_{m})$, should be positive semidefinite for all possible choices of the set $\{x_{n}\}$.




---





# 5.6: Probabilistic View of Linear Regression | Error Functions

## 1. A Statistical Model for the Joint Distribution

The additive error model is a useful approximation to the truth. For most systems, the input-output pairs $(X, Y)$ will not have a deterministic relationship $Y = f(X)$. Generally, there will be other unmeasured variables that also contribute to $Y$, including measurement error. The additive model assumes that we can capture all these departures from a deterministic relationship via the error $\epsilon$:
$$Y = f(X) + \epsilon$$
where the random error $\epsilon$ has $E(\epsilon) = 0$ and is independent of $X$. Note that for this model, $f(x) = E(Y|X=x)$, and in fact the conditional distribution $Pr(Y|X)$ depends on $X$ only through the conditional mean $f(x)$.

<img width="890" height="335" alt="image" src="https://github.com/user-attachments/assets/8f3f48dc-ea34-4bc0-8184-45b308175986" />

## 2. Maximum Likelihood and Least Squares

The sum-of-squares error function can be motivated as the maximum likelihood solution under an assumed Gaussian noise model. We assume that the target variable $t$ is given by a deterministic function $y(x, w)$ with additive Gaussian noise so that:
$$t = y(x, w) + \epsilon$$
where $\epsilon$ is a zero mean Gaussian random variable with precision (inverse variance) $\beta$. Thus we can write:
$$p(t|x, w, \beta) = \mathcal{N}(t|y(x, w), \beta^{-1})$$
where the precision parameter $\beta$ is related to the variance by $\beta^{-1} = \sigma^{2}$.

### 2.1 The Likelihood Function
Consider a data set of inputs $X = \{x_1, \dots, x_N\}$ with corresponding target values $t_1, \dots, t_N$. Making the assumption that these data points are drawn independently from the distribution, we obtain the expression for the likelihood function as a function of the adjustable parameters $w$ and $\beta$:
$$p(t|X, w, \beta) = \prod_{n=1}^{N} \mathcal{N}(t_{n}|w^{T}\phi(x_{n}), \beta^{-1})$$

### 2.2 Derivation of the Log Likelihood
Taking the logarithm of the likelihood function, and making use of the standard form for the univariate Gaussian, we have:
$$\ln p(t|w, \beta) = \sum_{n=1}^N \ln \mathcal{N}(t_n|w^T\phi(x_n), \beta^{-1})$$
$$\ln p(t|w, \beta) = \frac{N}{2} \ln \beta - \frac{N}{2} \ln(2\pi) - \beta E_D(w)$$
where the sum-of-squares error function is defined by:
$$E_D(w) = \frac{1}{2} \sum_{n=1}^N \{t_n - w^T\phi(x_n)\}^2$$

> [!IMPORTANT]
> Maximizing likelihood is equivalent, so far as determining $w$ is concerned, to minimizing the sum-of-squares error function defined by $E_D(w)$. The sum-of-squares error function has arisen as a consequence of maximizing likelihood under the assumption of a Gaussian noise distribution.

### 2.3 Maximum Likelihood Solutions
*   **Solution for $w$:** The gradient of the log likelihood function with respect to $w$ takes the form:
    $$\nabla \ln p(t|w, \beta) = \sum_{n=1}^N \{t_n - w^T\phi(x_n)\}\phi(x_n)^T$$
    Setting this gradient to zero and solving for $w$ yields the normal equations for the least squares problem:
    $$w_{ML} = (\Phi^T\Phi)^{-1}\Phi^T t$$
*   **Solution for $\beta$:** Maximizing with respect to the noise precision parameter $\beta$ gives:
    $$\frac{1}{\beta_{ML}} = \frac{1}{N} \sum_{n=1}^N \{t_n - w_{ML}^T\phi(x_n)\}^2$$
    The inverse of the noise precision is given by the residual variance of the target values around the regression function.

## 3. Generalized Loss Functions (Minkowski Loss)

The squared loss is not the only possible choice of loss function for regression. A generalization of the squared loss is the Minkowski loss, whose expectation is given by:
$$E[L_q] = \iint |y(x) - t|^q p(x, t) dx dt$$
This reduces to the expected squared loss for $q = 2$. The minimum of $E[L_q]$ is given by:
*   **$q = 2$:** The conditional mean.
*   **$q = 1$:** The conditional median.
*   **$q \rightarrow 0$:** The conditional mode.

<img width="842" height="636" alt="image" src="https://github.com/user-attachments/assets/d705e8a0-ee76-4059-90eb-4811b2917786" />

## 4. Regularization and the Maximum Posterior (MAP) View

To control the over-fitting phenomenon, regularization adds a penalty term to the error function. The simplest penalty term takes the form of a sum of squares of all the coefficients:

$$
\tilde{E}(w) = \frac{1}{2} \sum_{n=1}^N \{y(x_n, w) - t_n\}^2 + \frac{\lambda}{2} \|w\|^2
$$

where $\|w\|^2 \equiv w^T w = w_0^2 + w_1^2 + \dots + w_M^2$.

### 4.1 Probabilistic Interpretation
We introduce a prior distribution over the coefficients $w$. Consider a Gaussian distribution of the form:
$$p(w|\alpha) = \mathcal{N}(w|0, \alpha^{-1}I) = \left(\frac{\alpha}{2\pi}\right)^{(M+1)/2} \exp\{-\frac{\alpha}{2}w^T w\}$$
Using Bayes' theorem, the posterior distribution for $w$ is proportional to the product of the prior and the likelihood:
$$p(w|x, t, \alpha, \beta) \propto p(t|x, w, \beta)p(w|\alpha)$$
Maximizing the posterior (MAP) is equivalent to minimizing the negative logarithm of this product. Combining the Gaussian likelihood and prior, the maximum of the posterior is given by the minimum of:
$$\frac{\beta}{2} \sum_{n=1}^N \{y(x_n, w) - t_n\}^2 + \frac{\alpha}{2} w^T w$$
This is equivalent to minimizing the regularized sum-of-squares error function with a regularization parameter given by $\lambda = \alpha/\beta$.

## 5. Geometry of Least Squares

In an $N$-dimensional space whose axes are given by $t_n$, the target values $\mathfrak{t} = (t_1, \dots, t_N)^T$ is a vector. Each basis function $\phi_j(x_n)$ evaluated at the $N$ data points is represented as a vector $\varphi_j$. If $M < N$, the vectors $\varphi_j$ span a linear subspace $S$ of dimensionality $M$. The sum-of-squares error is equal (up to a factor of $1/2$) to the squared Euclidean distance between the fitted vector $y$ and $\mathfrak{t}$. The least-squares solution for $w$ corresponds to the orthogonal projection of $\mathfrak{t}$ onto the subspace $S$.

<img width="1229" height="358" alt="image" src="https://github.com/user-attachments/assets/63842639-2864-4c9f-bd4b-c80e77721b32" />
