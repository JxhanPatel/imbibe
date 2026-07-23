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
*   **Over-fitting:** When a much higher order polynomial (such as $M=9$) is used, we obtain an excellent fit to the training data, and the polynomial may pass exactly through each data point so that $E(w★) = 0$. However, the fitted curve oscillates wildly and gives a very poor representation of the generating function. This behavior is known as over-fitting.

<img width="847" height="674" alt="image" src="https://github.com/user-attachments/assets/bef201ae-3c25-40a9-a61b-58bbdca91528" />

### 2.3 Generalization and Performance Assessment
The goal of running a machine learning algorithm is to achieve good generalization, which is the ability to categorize correctly new examples that differ from those used for training.

#### Root-Mean-Square (RMS) Error
Generalization performance can be measured using the root-mean-square (RMS) error:
$$E_{RMS} = \sqrt{2E(w★)/N}$$.
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
Consider the particular case of a local quadratic approximation around a point $w★$ that is a minimum of the error function. In this case there is no linear term, because $\nabla E = 0$ at $w★$, and the expansion becomes:
$$E(w) = E(w★) + \frac{1}{2}(w - w★)^{T}H(w - w★)$$
where the Hessian $H$ is evaluated at $w★$. Consider the eigenvalue equation for the Hessian matrix:
$$Hu_{i} = \lambda_{i}u_{i}$$
where the eigenvectors $u_{i}$ form a complete orthonormal set. We expand $(w - w★)$ as a linear combination of the eigenvectors:
$$w - w★ = \sum_{i} \alpha_{i} u_{i}$$
The error function can then be written in the form:
$$E(w) = E(w★) + \frac{1}{2} \sum_{i} \lambda_{i} \alpha_{i}^{2}$$
A matrix $H$ is positive definite if, and only if, $v^{T}Hv > 0$ for all $v$. This holds if, and only if, all of its eigenvalues are positive. In the coordinate system defined by the eigenvectors $u_{i}$, the contours of constant $E$ are ellipses centered on $w★$.

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
