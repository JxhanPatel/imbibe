# 4.1 Introduction to Estimation

## 1. The Problem of Parameter Estimation

The problem of searching for patterns in data is a fundamental one and has a long and successful history. One approach to this problem is to use the samples to estimate the unknown probabilities and probability densities, and to use the resulting estimates as if they were the true values. In typical supervised pattern classification problems, the estimation of the prior probabilities $P(\omega_{i})$ presents no serious difficulties. However, estimation of the class-conditional densities is quite another matter. The number of available samples always seems too small, and serious problems arise when the dimensionality of the feature vector $x$ is large. If we know the number of parameters in advance and our general knowledge about the problem permits us to parameterize the conditional densities, then the severity of these problems can be reduced significantly.

Suppose, for example, that we can reasonably assume that $p(x|\omega_{i})$ is a normal density with mean $\mu_{i}$ and covariance matrix $\Sigma_{i}$, although we do not know the exact values of these quantities. This knowledge simplifies the problem from one of estimating an unknown function $p(x|\omega_{i})$ to one of estimating the parameters $\mu_{i}$ and $\Sigma_{i}$. The problem of parameter estimation is a classical one in statistics, and it can be approached in several ways. We shall consider two common and reasonable procedures, maximum likelihood estimation and Bayesian estimation.

## 2. Comparison of Estimation Paradigms

Although the results obtained with these two procedures are frequently nearly identical, the approaches are conceptually quite different.

### 2.1 Maximum Likelihood Estimation

- Maximum likelihood and several other methods view the parameters as quantities whose values are fixed but unknown.
- The best estimate of their value is defined to be the one that maximizes the probability of obtaining the samples actually observed.
- Maximum likelihood estimation methods have a number of attractive attributes.
- First, they nearly always have good convergence properties as the number of training samples increases.
- Further, maximum likelihood estimation often can be simpler than alternate methods, such as Bayesian techniques.

### 2.2 Bayesian Estimation

- In contrast, Bayesian methods view the parameters as random variables having some known *a priori* distribution.
- Observation of the samples converts this to a posterior density, thereby revising our opinion about the true values of the parameters.
- In the Bayesian case, a typical effect of observing additional samples is to sharpen the *a posteriori* density function, causing it to peak near the true values of the parameters.
- This phenomenon is known as **Bayesian learning**.

> [!NOTE] In either case, we use the posterior densities for our classification rule.
>
>

## 3. Detailed Introductory Principles

### 3.1 Density Estimation

One role for the distributions is to model the probability distribution $p(x)$ of a random variable $x$, given a finite set $x_{1}, \ldots, x_{N}$ of observations. This problem is known as **density estimation**. The problem of density estimation is fundamentally ill-posed because there are infinitely many probability distributions that could have given rise to the observed finite data set. Any distribution $p(x)$ that is nonzero at each of the data points $x_{1}, \ldots, x_{N}$ is a potential candidate. The issue of choosing an appropriate distribution relates to the problem of **model selection**.

<img width="806" height="772" alt="image" src="https://github.com/user-attachments/assets/3a76a32c-e99e-4340-a03c-a2ba00d0b1b3" />



### 3.2 The Parametric Approach

Parametric distributions are so-called because they are governed by a small number of adaptive parameters, such as the mean and variance in the case of a Gaussian, for example. To apply such models to the problem of density estimation, we need a procedure for determining suitable values for the parameters, given an observed data set. One limitation of the parametric approach is that it assumes a specific functional form for the distribution, which may turn out to be inappropriate for a particular application.

### 3.3 The General Principle of Maximum Likelihood

Suppose that we separate a collection of samples according to class, so that we have $c$ sets,

$$
\mathcal{D}_{1}, \ldots, \mathcal{D}_{c},
$$

with the samples in

$$
\mathcal{D}_{j}
$$

having been drawn independently according to the probability law $p(x|\omega_{j})$. We say such samples are **i.i.d. (independent and identically distributed)** random variables. We assume that $p(x|\omega_{j})$ has a known parametric form and is therefore determined uniquely by the value of a parameter vector $\theta_{j}$ . We shall assume that samples in $\mathcal{D}{i}$ give no information about $\theta*{j}$ if $i \ne j$; that is, we assume that the parameters for the different classes are functionally independent. This permits us to work with each class separately and to simplify our notation by deleting indications of class distinctions.

### 3.4 The General Theory of Bayesian Estimation

The computation of the posterior probabilities $P(\omega_{i}|x)$ lies at the heart of Bayesian classification. Bayes' formula allows us to compute these probabilities from the prior probabilities $P(\omega_{i})$ and the class-conditional densities $p(x|\omega_{i})$, but we must proceed when these quantities are unknown. Part of this information might be prior knowledge, such as knowledge of the functional forms for unknown densities and ranges for the values of unknown parameters. Any information we might have about $\theta$ prior to observing the samples is assumed to be contained in a known prior density $p(\theta)$. Observation of the samples converts this to a posterior density $p(\theta|\mathcal{D})$, which, we hope, is sharply peaked about the true value of $\theta$.

The key equation links the desired class-conditional density $p(x|\mathcal{D})$ to the posterior density $p(\theta|\mathcal{D})$ for the unknown parameter vector:

$$
p(x|\mathcal{D})=\int p(x|\theta)p(\theta|\mathcal{D})\,d\theta
$$

If $p(\theta|\mathcal{D})$ peaks very sharply about some value $\hat{\theta}$, we obtain

$$
p(x|\mathcal{D})\simeq p(x|\hat{\theta}),
$$

i.e., the result we would obtain by substituting the estimate $\hat{\theta}$ for the true parameter vector.

## 4. Methodological Differences and Preferences

In virtually every case, maximum likelihood and Bayes solutions are equivalent in the asymptotic limit of infinite training data. However, since practical pattern recognition problems invariably have a limited set of training data, it is natural to ask when they may be expected to differ.

| Criterion | Maximum Likelihood (ML) | Bayesian Estimation |
| --- | --- | --- |
| **Computational Complexity** | Often preferred; requires differential calculus or gradient search. | Can be high; requires complex multidimensional integration. |
| **Interpretability** | Easier to interpret; returns the single best model from the provided set. | Weighted average of models; results can be more complicated to understand. |
| **Use of Information** | Solution must be of assumed parametric form. | Uses more information via the full $p(\theta) |
| **Uncertainty** | Views parameters as fixed but unknown. | Reflects the remaining uncertainty in possible models. |

> [!IMPORTANT] A maximum likelihood estimator is a MAP estimator for the uniform or "flat" prior. As such, a MAP estimator finds the peak (mode) of a posterior density. The drawback of MAP estimators is that if we choose an arbitrary nonlinear transformation of the parameter space, the density changes, and the MAP solution may no longer be appropriate.












---





# 4.2 Maximum Likelihood Estimation

## 1. General Principles of Maximum Likelihood

Maximum likelihood estimation (MLE) is a widely used frequentist approach to parameter estimation where parameters are viewed as quantities whose values are fixed but unknown. The best estimate of their value is defined to be the one that maximizes the probability of obtaining the samples actually observed.

### 1.1 Attributes of Maximum Likelihood
*   **Convergence:** MLE methods nearly always have good convergence properties as the number of training samples increases.
*   **Simplicity:** MLE often can be simpler than alternate methods, such as Bayesian techniques.
*   **Invariance:** If $\hat{\theta}$ is the maximum likelihood estimate of $\theta$, then for any differentiable function $\tau(\cdot)$ the maximum likelihood estimate of $\tau(\theta)$ is $\tau(\hat{\theta})$.

### 1.2 Mathematical Formulation
Suppose that we have a set $\mathcal{D} = \{x_{1},...,x_{n}\}$ of n training samples drawn independently from the probability density $p(x|\theta)$. Since the samples were drawn independently and identically distributed (i.i.d.), the joint density is the product of the individual densities:
$$p(\mathcal{D}|\theta) = \prod_{k=1}^{n} p(x_{k}|\theta)$$

Viewed as a function of $\theta$, $p(\mathcal{D}|\theta)$ is called the likelihood of $\theta$ with respect to the set of samples. The maximum likelihood estimate $\hat{\theta}$ is, by definition, the value that maximizes $p(\mathcal{D}|\theta)$.

#### The Log-Likelihood Function
For analytical purposes, it is usually easier to work with the logarithm of the likelihood because it is a monotonically increasing function. Maximizing the log-likelihood is equivalent to maximizing the likelihood itself. We define the log-likelihood function $l(\theta)$ as:
$$l(\theta) = \ln p(\mathcal{D}|\theta) = \sum_{k=1}^{n} \ln p(x_{k}|\theta)$$

#### Necessary Conditions for a Maximum
If $p(\mathcal{D}|\theta)$ is a well-behaved, differentiable function, $\hat{\theta}$ can be found using differential calculus. The set of necessary conditions for the maximum likelihood estimate can be obtained from the set of p equations where the gradient is zero:
$$\nabla_{\theta}l = \sum_{k=1}^{n} \nabla_{\theta} \ln p(x_{k}|\theta) = 0$$

## 2. Maximum Likelihood for the Gaussian Distribution

The multivariate normal or Gaussian density is a common choice for class-conditional densities due to its analytical tractability.

### 2.1 Univariate Case
For a single real-valued variable x, the Gaussian distribution is governed by the mean $\mu$ and variance $\sigma^{2}$. The log-likelihood function is:
$$\ln p(\mathfrak{x}|\mu, \sigma^{2}) = -\frac{1}{2\sigma^{2}} \sum_{n=1}^{N} (x_{n} - \mu)^{2} - \frac{N}{2} \ln \sigma^{2} - \frac{N}{2} \ln(2\pi)$$

#### Maximum Likelihood Solutions
1.  **Estimate for the Mean:** Maximizing with respect to $\mu$ yields the sample mean, which is the arithmetic average of the observed values:
    $$\mu_{ML} = \frac{1}{N} \sum_{n=1}^{N} x_{n}$$
2.  **Estimate for the Variance:** Maximizing with respect to $\sigma^{2}$ yields the sample variance measured with respect to the sample mean $\mu_{ML}$:
    $$\sigma_{ML}^{2} = \frac{1}{N} \sum_{n=1}^{N} (x_{n} - \mu_{ML})^{2}$$

### 2.2 Multivariate Case
For a D-dimensional vector x, the log-likelihood for a multivariate Gaussian distribution is:
$$\ln p(X|\mu, \Sigma) = -\frac{ND}{2} \ln(2\pi) - \frac{N}{2} \ln|\Sigma| - \frac{1}{2} \sum_{n=1}^{N} (x_{n} - \mu)^{T} \Sigma^{-1} (x_{n} - \mu)$$

The maximum likelihood solutions are:
*   **Mean Vector:** $\hat{\mu} = \frac{1}{n} \sum_{k=1}^{n} x_{k}$.
*   **Covariance Matrix:** $\hat{\Sigma} = \frac{1}{n} \sum_{k=1}^{n} (x_{k} - \hat{\mu})(x_{k} - \hat{\mu})^{t}$.

## 3. Bias in Maximum Likelihood Estimates

A significant limitation of the maximum likelihood approach is that it can yield biased estimates.

### 3.1 Bias in Variance Estimation
The maximum likelihood estimate for the variance $\sigma^{2}$ systematically underestimates the true variance of the distribution. This occurs because the variance is measured relative to the sample mean $\mu_{ML}$ and not relative to the true mean $\mu$.
$$\mathbb{E}[\sigma_{ML}^{2}] = \left(\frac{N-1}{N}\right) \sigma^{2}$$

### 3.2 Unbiased Estimator
An unbiased estimate for the variance parameter is given by:
$$\tilde{\sigma}^{2} = \frac{N}{N-1} \sigma_{ML}^{2} = \frac{1}{N-1} \sum_{n=1}^{N} (x_{n} - \mu_{ML})^{2}$$
Note that the bias of the maximum likelihood solution becomes less significant as the number N of data points increases. In the limit $N \rightarrow \infty$, the MLE for variance equals the true variance.

<img width="818" height="350" alt="image" src="https://github.com/user-attachments/assets/fe9ec043-0862-48ea-b6ff-f114cf7e8431" />

## 4. MLE for Discrete Distributions

### 4.1 Bernoulli Distribution
For a binary variable x where $p(x=1|\mu) = \mu$, the log-likelihood function is:
$$\ln p(\mathcal{D}|\mu) = \sum_{n=1}^{N} \{x_{n} \ln \mu + (1-x_{n}) \ln(1-\mu)\}$$
The maximum likelihood estimator is the fraction of observations of heads in the data set:
$$\mu_{ML} = \frac{m}{N}$$
where m is the number of observations of $x=1$.

### 4.2 Multinomial Distribution
For a variable taking K states with probabilities $\mu_{k}$, the maximum likelihood solution for $\mu_{k}$ is:
$$\mu_{k}^{ML} = \frac{m_{k}}{N}$$
where $m_{k}$ is the number of observations for which $x_{k} = 1$.

## 5. Limitations and Practical Issues

### 5.1 Sensitivity to Model Error
If the assumed model is very poor, the maximum likelihood classifier is not guaranteed to be the best, even among the assumed model set. If the noise in the data is large compared to the signal separating categories, MLE will find directions of noise rather than signal [ESL Page 67 Figure 3.9].

### 5.2 Singularities
In mixture models such as Gaussian Mixture Models (GMMs), MLE can suffer from singularities where a component 'collapses' onto a specific data point, causing the likelihood to go to infinity.

### 5.3 Robustness
MLE estimation of a Gaussian is not robust to outliers because the sum-of-squares criterion places high influence on large distances. BASE-ing a model on a heavy-tailed distribution like Student's t-distribution provides a more robust model.

### 5.4 Computational Complexity
*   **Sample Mean:** $O(nd)$ calculations.
*   **Sample Covariance Matrix:** $O(d^{2}n)$ calculations.
*   **Matrix Inversion:** $O(d^{3})$ calculations.
*   **Missing Data:** When features are missing, the Expectation-Maximization (EM) algorithm is used to iteratively find maximum likelihood estimates.

> [!IMPORTANT]
> In the asymptotic limit of infinite training data, maximum likelihood and Bayes solutions are equivalent. However, for finite data sets, MLE may lead to severe over-fitting.



---


