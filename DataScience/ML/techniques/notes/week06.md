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






---






# 6.2 Cross-validation for minimizing MSE

## 1. Principles of Cross-Validation

Probably the simplest and most widely used method for estimating prediction error is cross-validation. This method directly estimates the expected extra-sample error $Err = E[L(Y, \hat{f}(X))]$, which represents the average generalization error when the mapping $\hat{f}(X)$ is applied to an independent test sample drawn from the joint distribution of $X$ and $Y$. Cross-validation is an empirical approach that evaluates the performance of a classifier or regression model experimentally. Unlike methods that rely only on the training data, cross-validation seeks a measure of performance that does not suffer from bias due to over-fitting.

## 2. K-Fold Cross-Validation

Ideally, if enough data were available, a validation set would be set aside to assess the performance of a prediction model. When data are scarce, K-fold cross-validation finesses this problem by using part of the available data to fit the model and a different part to test it.

### 2.1 The Partitioning Process

The data is split into $K$ roughly equal-sized parts. For each part $k$, the model is fit to the other $K-1$ parts of the data, and the prediction error of the fitted model is calculated when predicting the $k^{th}$ part of the data. This procedure is repeated for $k = 1, 2, \dots, K$, and the $K$ resulting estimates of prediction error are combined.

<img width="1204" height="334" alt="image" src="https://github.com/user-attachments/assets/53373ade-3f5a-44dc-a720-bc5541b6e046" />

### 2.2 Mathematical Formulation

Let $\kappa: {1, \dots, N} \mapsto {1, \dots, K}$ be an indexing function that indicates the partition to which observation $i$ is allocated by the randomization. Denote by $\hat{f}^{-k}(x)$ the fitted function computed with the $k^{th}$ part of the data removed. The cross-validation estimate of prediction error is defined as:

$$
CV(\hat{f}) = \frac{1}{N} \sum_{i=1}^{N} L(y_i, \hat{f}^{-\kappa(i)}(x_i))
$$

## 3. Minimizing Mean Squared Error (MSE) via Cross-Validation

In regression problems, the common choice for the loss function $L$ is the squared error $(Y - \hat{f}(X))^2$. In this context, the cross-validation estimate becomes the cross-validated Mean Squared Error (MSE).

### 3.1 Model Selection and Tuning Parameters

Cross-validation is used for model selection by estimating the performance of different models to choose the best one. Typically, a model has a tuning parameter (or parameters) $\alpha$ that varies the model's complexity. Denote by $\hat{f}^{-k}(x, \alpha)$ the $\alpha^{th}$ model fit with the $k^{th}$ part of the data removed. The cross-validation error curve as a function of $\alpha$ is:

$$
CV(\hat{f}, \alpha) = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{f}^{-\kappa(i)}(x_i, \alpha))^2
$$

The goal is to find the tuning parameter $\hat{\alpha}$ that minimizes this $CV(\hat{f}, \alpha)$. This $\hat{\alpha}$ corresponds to the minimum of the average test error curve. Once $\hat{\alpha}$ is chosen, the final model $f(x, \hat{\alpha})$ is fit to all available data.


### 3.2 The One-Standard-Error Rule

Because the tradeoff curve is estimated with error, a conservative approach known as the "one-standard-error" rule is often used. Under this rule, one picks the most parsimonious model (the simplest model) whose error is within one standard error of the minimum of the cross-validation error curve.

## 4. Specific Types of Cross-Validation

### 4.1 Leave-One-Out Cross-Validation (LOOCV)

The limit where $K = N$ is known as leave-one-out cross-validation. In this case, $\kappa(i) = i$, and for each observation $i$, the fit is computed using all data except the $i^{th}$. LOOCV is approximately unbiased for the true expected prediction error but can have high variance because the $N$ training sets used are very similar to one another.

### 4.2 Generalized Cross-Validation (GCV)

For linear fitting methods where the predictions can be written as $\hat{y} = Sy$, generalized cross-validation provides a convenient approximation to leave-one-out cross-validation under squared-error loss. The GCV approximation is given by:

$$
GCV(\hat{f}) = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{y_i - \hat{f}(x_i)}{1 - trace(S)/N} \right]^2
$$

The quantity $trace(S)$ is the effective number of parameters in the model. GCV is computationally advantageous when $trace(S)$ is easier to compute than the individual diagonal elements $S_{ii}$ of the smoother matrix.

## 5. Bias and Variance Considerations

The choice of $K$ involves a tradeoff between bias and variance.

* **High K (e.g., K=N):** The cross-validation estimator has low bias as an estimate of expected error, but its variance can be high.
* **Low K (e.g., K=5 or 10):** The estimator has lower variance but may be biased upward if the "learning curve" for the model has a steep slope at the given training set size. In such cases, cross-validation will overestimate the true prediction error.

Five-fold or tenfold cross-validation are generally recommended as a good compromise.

## 6. Methodological Requirements

### 6.1 The "Right Way" to Perform Cross-Validation

A common pitfall occurs when predictors are screened using the entire dataset before applying cross-validation. If the top predictors are chosen based on their correlation with class labels over all samples, they have an "unfair advantage" because they have "already seen" the samples that will be left out in the cross-validation folds.

**Correct Procedure:** Cross-validation must be applied to the entire sequence of modeling steps. Samples must be "left out" before any selection or filtering steps are applied.

### 6.2 Computational Drawbacks

One major drawback of cross-validation is that the number of training runs is increased by a factor of $K$. This can be problematic for models where the training process is itself computationally expensive. Furthermore, exploring combinations of settings for multiple complexity parameters could require an exponential number of training runs.
