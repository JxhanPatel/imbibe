# 6.1 Probabilistic Perspectives on Linear Regression

The goal in the curve fitting problem is to be able to make predictions for the target variable $t$ given some new value of the input variable $x$ on the basis of a set of training data comprising $N$ input values $\mathbf{x} = (x_1, \dots, x_N)^T$ and their corresponding target values $\mathbf{t} = (t_1, \dots, t_N)^T$. We can express our uncertainty over the value of the target variable using a probability distribution. For this purpose, we shall assume that, given the value of $x$, the corresponding value of $t$ has a Gaussian distribution with a mean equal to the value $y(x, \mathbf{w})$ of the polynomial curve. Thus we have $p(t|x, \mathbf{w}, \beta) = \mathcal{N}(t|y(x, \mathbf{w}), \beta^{-1})$ where we have defined a precision parameter $\beta$ corresponding to the inverse variance of the distribution.

<img width="865" height="314" alt="image" src="https://github.com/user-attachments/assets/6a731d74-5086-471e-9287-bddba5648c25" />

The linear model either assumes that the regression function $E(Y|X)$ is linear, or that the linear model is a reasonable approximation. In the case of a Gaussian conditional distribution, the conditional mean will be simply $E[t|x] = \int t p(t|x) dt = y(x, \mathbf{w})$. If we assume a squared loss function, then the optimal prediction, for a new value of $x$, will be given by the conditional mean of the target variable.

If the data are assumed to be drawn independently from the distribution, then the likelihood function is given by $p(\mathbf{t}|\mathbf{x}, \mathbf{w}, \beta) = \prod_{n=1}^N \mathcal{N}(t_n|y(x_n, \mathbf{w}), \beta^{-1})$. Taking the logarithm of the likelihood function, and making use of the standard form for the univariate Gaussian, we have $\ln p(\mathbf{t}|\mathbf{w}, \beta) = \frac{N}{2} \ln \beta - \frac{N}{2} \ln(2\pi) - \beta E_D(\mathbf{w})$, where the sum-of-squares error function is defined by $E_D(\mathbf{w}) = \frac{1}{2} \sum_{n=1}^N {t_n - \mathbf{w}^T \phi(x_n)}^2$.

## 6.1.1 Goodness of Maximum Likelihood Estimator for Linear Regression

The maximum likelihood estimator (MLE) has several properties and "goodness" measures based on its statistical behavior:

### 1. Equivalence to Least Squares

Maximizing likelihood is equivalent, so far as determining $\mathbf{w}$ is concerned, to minimizing the sum-of-squares error function. The sum-of-squares error function has arisen as a consequence of maximizing likelihood under the assumption of a Gaussian noise distribution. The solution for $\mathbf{w}$ is given by the normal equations $\mathbf{w}_{ML} = (\Phi^T \Phi)^{-1} \Phi^T \mathbf{t}$.

### 2. The Gauss-Markov Theorem

One of the most famous results in statistics asserts that the least squares estimates of the parameters $\beta$ have the smallest variance among all linear unbiased estimates. If we have any other linear estimator $\tilde{\theta} = \mathbf{c}^T \mathbf{y}$ that is unbiased for $a^T \beta$, then $Var(a^T \hat{\beta}) \le Var(\tilde{\theta})$. This implies that the least squares estimator has the smallest mean squared error of all linear estimators with no bias.

### 3. Bias in Parameter Estimation

* **Mean:** On average, the maximum likelihood estimate will obtain the correct mean $E[\mu_{ML}] = \mu$.
* **Variance:** The maximum likelihood approach systematically underestimates the variance of the distribution. It is straightforward to show that $E[\sigma_{ML}^2] = (\frac{N-1}{N})\sigma^2$, so it underestimates the true variance by a factor $(N-1)/N$. This is an example of a phenomenon called bias.
* **Unbiased Variance:** The following estimate for the variance parameter is unbiased: $\tilde{\sigma}^2 = \frac{N}{N-1}\sigma_{ML}^2 = \frac{1}{N-1}\sum_{n=1}^N (x_n - \mu_{ML})^2$.

### 4. Convergence and Consistency

MLE methods nearly always have good convergence properties as the number of training samples increases. In the limit $N \to \infty$, the maximum likelihood solution for the variance equals the true variance of the distribution that generated the data. Asymptotic likelihood theory says that if the model is correct, then $\hat{\beta}$ is consistent (i.e., converges to the true $\beta$).

### 5. Over-fitting Limitations

The use of maximum likelihood can lead to severe over-fitting if complex models are trained using data sets of limited size. Over-fitting can be understood as a general property of maximum likelihood. For a finite data set, the maximum likelihood result can lead to unreasonable predictions, such as predicting future observations with 100% certainty based on a very small sample.

<img width="1196" height="358" alt="image" src="https://github.com/user-attachments/assets/5f2dde3c-f044-4ba3-a823-8e8fb1320683" />


## 6.1.2 Bayesian Modeling for Linear Regression

The Bayesian approach avoids the over-fitting problem of maximum likelihood and allows model complexity to be determined using the training data alone.

### 1. Parameter Distribution and Prior

In Bayesian learning, we consider the parameters $\mathbf{w}$ to be random variables. We capture our assumptions about $\mathbf{w}$, before observing the data, in the form of a prior probability distribution $p(\mathbf{w})$. For the linear regression model, the likelihood function $p(\mathbf{t}|\mathbf{w})$ is the exponential of a quadratic function of $\mathbf{w}$. The corresponding conjugate prior is therefore given by a Gaussian distribution of the form $p(\mathbf{w}) = \mathcal{N}(\mathbf{w}|\mathbf{m}_0, \mathbf{S}_0)$.

### 2. Posterior Distribution

Bayes' theorem is used to convert the prior probability into a posterior probability by incorporating the evidence provided by the observed data. The posterior distribution is proportional to the product of the likelihood function and the prior. Due to the choice of a conjugate Gaussian prior, the posterior is also Gaussian: $p(\mathbf{w}|\mathbf{t}) = \mathcal{N}(\mathbf{w}|\mathbf{m}_N, \mathbf{S}_N)$.

* $\mathbf{m}_N = \mathbf{S}_N(\mathbf{S}_0^{-1}\mathbf{m}_0 + \beta\Phi^T \mathbf{t})$.
* $\mathbf{S}_N^{-1} = \mathbf{S}_0^{-1} + \beta\Phi^T \Phi$.

### 3. Predictive Distribution

In a fully Bayesian approach, we make predictions by integrating over the whole of parameter space. The predictive distribution is written in the form $p(t|x, \mathbf{x}, \mathbf{t}) = \int p(t|x, \mathbf{w}) p(\mathbf{w}|\mathbf{x}, \mathbf{t}) d\mathbf{w}$. Integration shows that the predictive distribution is a Gaussian $p(t|x, \mathbf{x}, \mathbf{t}) = \mathcal{N}(t|m(x), s^2(x))$ where the variance $s^2(x) = \beta^{-1} + \phi(x)^T \mathbf{S} \phi(x)$ depends on $x$.

* The first term $\beta^{-1}$ represents the uncertainty due to noise on the target variables.
* The second term $\phi(x)^T \mathbf{S} \phi(x)$ arises from the uncertainty in the parameters $\mathbf{w}$.

<img width="612" height="725" alt="image" src="https://github.com/user-attachments/assets/fc60fbe0-d1a3-40d1-a59d-863a8ef54b13" />

### 4. Bayesian Learning and Model Complexity

* **Sequential Nature:** The Bayesian paradigm leads very naturally to a sequential view of the inference problem, where the posterior distribution at any stage acts as the prior for the subsequent data point.
* **Automatic Adaptation:** In a Bayesian model, the effective number of parameters adapts automatically to the size of the data set. There is no difficulty in employing models for which the number of parameters greatly exceeds the number of data points.
* **Regularization Connection:** Maximization of the log posterior is equivalent to minimization of the sum-of-squares error function with the addition of a quadratic regularization term. For a zero-mean isotropic Gaussian prior $p(\mathbf{w}|\alpha) = \mathcal{N}(\mathbf{w}|\mathbf{0}, \alpha^{-1}\mathbf{I})$, the regularization parameter is $\lambda = \alpha/\beta$.

### 5. Summary Table: ML vs. Bayesian View

| Feature                | Maximum Likelihood (ML)                          | Bayesian Estimation                           |
| ---------------------- | ------------------------------------------------ | --------------------------------------------- |
| **Parameter Status**   | Fixed but unknown.                               | Random variables.                             |
| **Output**             | Point estimate $p(x \mid \hat{\theta})$.         | Distribution over parameters and predictions. |
| **Complexity Control** | Fixed basis functions or cross-validation.       | Automatic adaptation to data size.            |
| **Prior Knowledge**    | Not directly relevant (used for initialization). | Expressed via prior density $p(\theta)$.      |

> [!NOTE]
> For a finite data set, the posterior mean for $\mu$ always lies between the prior mean and the maximum likelihood estimate. In the limit of an infinitely large data set, the Bayesian and maximum likelihood results will agree.
