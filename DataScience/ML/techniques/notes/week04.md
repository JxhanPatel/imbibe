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







# 4.3 Bayesian Estimation

## 1. General Principles of Bayesian Estimation

Bayesian methods view the parameters as random variables having some known a priori distribution. Observation of the samples converts this to a posterior density, thereby revising our opinion about the true values of the parameters. In the Bayesian case, a typical effect of observing additional samples is to sharpen the a posteriori density function, causing it to peak near the true values of the parameters. This phenomenon is known as Bayesian learning.

### 1.1 Comparison with Maximum Likelihood
In a frequentist setting, $w$ is considered to be a fixed parameter, whose value is determined by some form of "estimator", and error bars on this estimate are obtained by considering the distribution of possible data sets $\mathcal{D}$. By contrast, from the Bayesian viewpoint there is only a single data set $\mathcal{D}$ (namely the one that is actually observed), and the uncertainty in the parameters is expressed through a probability distribution over $w$.

> [!NOTE]
> In the limit of an infinitely large number of training points, we can expect that our estimate will equal to the true value of the generating function.

## 2. Priors, Posteriors, and Bayes' Theorem

### 2.1 Definitions
*   **Prior Probability ($p(w)$):** We capture our assumptions about $w$, before observing the data, in the form of a prior probability distribution $p(w)$. Any information we might have about $\theta$ prior to observing the samples is assumed to be contained in a known prior density $p(\theta)$.
*   **Likelihood Function ($p(\mathcal{D}|w)$):** The effect of the observed data $\mathcal{D} = \{t_1, \dots, t_N\}$ is expressed through the conditional probability $p(\mathcal{D}|w)$. It expresses how probable the observed data set is for different settings of the parameter vector $w$. Note that the likelihood is not a probability distribution over $w$, and its integral with respect to $w$ does not (necessarily) equal one.
*   **Posterior Probability ($p(w|\mathcal{D})$):** Bayes' theorem then allows us to evaluate the uncertainty in $w$ after we have observed $\mathcal{D}$ in the form of the posterior probability $p(w|\mathcal{D})$. This represents our updated knowledge about $\theta$ after we see the data.
*   **Evidence ($p(\mathcal{D})$):** The denominator in Bayes' theorem is the normalization constant, which ensures that the posterior distribution on the left-hand side is a valid probability density and integrates to one. It can be expressed in terms of the prior distribution and the likelihood function: $p(\mathcal{D}) = \int p(\mathcal{D}|w)p(w) dw$.

### 2.2 Bayes' Theorem in Words
Bayes' formula can be expressed informally in English by saying that:
$$posterior = \frac{likelihood \times prior}{evidence}$$.

Alternatively, given the definition of likelihood:
$$posterior \propto likelihood \times prior$$.

### 2.3 Formal Solution
The posterior density for the unknown parameter vector is given by:
$$p(\theta|\mathcal{D}) = \frac{p(\mathcal{D}|\theta)p(\theta)}{\int p(\mathcal{D}|\theta)p(\theta) d\theta}$$.
By the independence assumption for samples drawn independently according to the unknown probability density $p(x)$:
$$p(\mathcal{D}|\theta) = \prod_{k=1}^{n} p(x_k|\theta)$$.

<img width="792" height="774" alt="image" src="https://github.com/user-attachments/assets/0ee393d3-9fac-48f3-8f46-74fbf2eadcfd" />

## 3. The Class-Conditional Density

The key equation links the desired class-conditional density $p(x|\mathcal{D})$ to the posterior density $p(\theta|\mathcal{D})$ for the unknown parameter vector:
$$p(x|\mathcal{D}) = \int p(x|\theta)p(\theta|\mathcal{D}) d\theta$$.
This equation directs us to average $p(x|\theta)$ over the possible values of $\theta$. If $p(\theta|\mathcal{D})$ peaks very sharply about some value $\hat{\theta}$, we obtain $p(x|\mathcal{D}) \simeq p(x|\hat{\theta})$, i.e., the result we would obtain by substituting the estimate $\hat{\theta}$ for the true parameter vector.

## 4. Bayesian Parameter Estimation: Gaussian Case

### 4.1 The Univariate Case: $p(\mu|\mathcal{D})$
Consider the case where $\mu$ is the only unknown parameter: $p(x|\mu) \sim \mathcal{N}(\mu, \sigma^2)$. We assume the prior knowledge about $\mu$ is expressed by a known prior density: $p(\mu) \sim \mathcal{N}(\mu_0, \sigma_0^2)$.

#### Derivation of the Posterior
The a posteriori density $p(\mu|\mathcal{D})$ is found using Bayes' formula:
$$p(\mu|\mathcal{D}) = \alpha \prod_{k=1}^{n} p(x_k|\mu)p(\mu)$$.
Since the product of two exponentials of quadratic functions is another exponential of a quadratic function, $p(\mu|\mathcal{D})$ is again a normal density. If we write $p(\mu|\mathcal{D}) \sim \mathcal{N}(\mu_n, \sigma_n^2)$, the parameters are:
$$\mu_n = \left(\frac{n\sigma_0^2}{n\sigma_0^2 + \sigma^2}\right)\overline{x}_n + \frac{\sigma^2}{n\sigma_0^2 + \sigma^2}\mu_0$$.
$$\sigma_n^2 = \frac{\sigma_0^2\sigma^2}{n\sigma_0^2 + \sigma^2}$$.
where $\overline{x}_n$ is the sample mean.

> [!IMPORTANT]
> $\mu_n$ always lies somewhere between $\overline{x}_n$ and $\mu_0$. As $n \to \infty$, $\mu_n$ approaches the sample mean and $\sigma_n^2$ approaches $\sigma^2/n$.

### 4.2 The Univariate Case: $p(x|\mathcal{D})$
To obtain the "class-conditional" density $p(x|\mathcal{D})$, we integrate:
$$p(x|\mathcal{D}) = \int p(x|\mu)p(\mu|\mathcal{D}) d\mu$$.
Performing the integration shows that $p(x|\mathcal{D})$ is normally distributed with mean $\mu_n$ and variance $\sigma^2 + \sigma_n^2$:
$$p(x|\mathcal{D}) \sim \mathcal{N}(\mu_n, \sigma^2 + \sigma_n^2)$$.
In effect, the conditional mean $\mu_n$ is treated as if it were the true mean, and the known variance is increased to account for the additional uncertainty in $\mu$.

### 4.3 The Multivariate Case
For the multivariate Gaussian $p(x|\mu) \sim \mathcal{N}(\mu, \Sigma)$ and $p(\mu) \sim \mathcal{N}(\mu_0, \Sigma_0)$, where $\Sigma$, $\Sigma_0$, and $\mu_0$ are assumed to be known. After observing $n$ samples, $p(\mu|\mathcal{D}) \sim \mathcal{N}(\mu_n, \Sigma_n)$ where:
$$\Sigma_n^{-1} = n\Sigma^{-1} + \Sigma_0^{-1}$$.
$$\Sigma_n^{-1}\mu_n = n\Sigma^{-1}\hat{\mu}_n + \Sigma_0^{-1}\mu_0$$.
The final result for the class-conditional density is:
$$p(x|\mathcal{D}) \sim \mathcal{N}(\mu_n, \Sigma + \Sigma_n)$$.

## 5. Recursive Bayes Learning

The posterior density satisfies the recursion relation:
$$p(\theta|\mathcal{D}^n) = \frac{p(x_n|\theta)p(\theta|\mathcal{D}^{n-1})}{\int p(x_n|\theta)p(\theta|\mathcal{D}^{n-1}) d\theta}$$.
This is an incremental or on-line learning method, where learning goes on as the data is collected. Repeated use of this equation produces the sequence of densities $p(\theta), p(\theta|x_1), p(\theta|x_1, x_2)$, and so forth. The Bayesian paradigm leads very naturally to a sequential view of the inference problem.

<img width="739" height="425" alt="image" src="https://github.com/user-attachments/assets/ad5bc105-91ca-4681-bcca-d6818943c1cc" />

## 6. Non-informative Priors

In some applications, we may have little idea of what form the distribution should take. We may then seek a form of prior distribution, called a noninformative prior, which is intended to have as little influence on the posterior distribution as possible. This is sometimes referred to as "letting the data speak for themselves".

*   **Location Parameters:** If a density takes the form $p(x|\mu) = f(x - \mu)$, then $\mu$ is a location parameter. Reflecting translation invariance implies that $p(\mu)$ is constant.
*   **Scale Parameters:** If a density takes the form $p(x|\sigma) = \frac{1}{\sigma} f(\frac{x}{\sigma})$, then $\sigma$ is a scale parameter. Reflecting scale invariance implies $p(\sigma) \propto 1/\sigma$.
*   **Improper Priors:** If the domain of $\lambda$ is unbounded, a constant prior distribution cannot be correctly normalized; such priors are called improper. Improper priors can often be used provided the corresponding posterior distribution is proper.





---





# 4.4 Gaussian Mixture Models

## 1. Motivation: Multimodal Data and Limitations of Single Gaussians

While the Gaussian distribution has some important analytical properties, it suffers from significant limitations when it comes to modelling real data sets. A single Gaussian distribution is intrinsically unimodal (i.e., has a single maximum) and so is unable to provide a good approximation to multimodal distributions. 

### 1.1 The "Old Faithful" Example
Consider the "Old Faithful" data set, which comprises 272 measurements of the eruption of the geyser at Yellowstone National Park. The data set forms two dominant clumps, and a simple Gaussian distribution is unable to capture this structure. However, a linear superposition of two Gaussians gives a better characterization of the data set. 

<img width="865" height="396" alt="image" src="https://github.com/user-attachments/assets/60e009a7-df1a-4f91-a3a2-f010480fe767" />

### 1.2 Approximating Complex Densities
By using a sufficient number of Gaussians, and by adjusting their means and covariances as well as the coefficients in the linear combination, almost any continuous density can be approximated to arbitrary accuracy. Such superpositions, formed by taking linear combinations of more basic distributions, are known as mixture distributions.

## 2. Mathematical Formulation of the GMM

A superposition of K Gaussian densities is called a mixture of Gaussians and takes the form:
$$p(x) = \sum_{k=1}^{K} \pi_{k} \mathcal{N}(x|\mu_{k}, \Sigma_{k})$$.

### 2.1 Mixing Coefficients
The parameters $\pi_{k}$ are called mixing coefficients. They must satisfy the following requirements to be valid probabilities:
*   $0 \le \pi_{k} \le 1$.
*   $\sum_{k=1}^{K} \pi_{k} = 1$.

Each Gaussian density $\mathcal{N}(x|\mu_{k}, \Sigma_{k})$ is called a component of the mixture and has its own mean $\mu_{k}$ and covariance $\Sigma_{k}$.

<img width="873" height="390" alt="image" src="https://github.com/user-attachments/assets/af172872-7fab-41a5-a341-5fef1a51a2ac" />

## 3. The Generative Story

A mixture of Gaussians is best described in terms of its generative model. The process of generating an observation $x$ consists of two steps:
1.  First, generate a discrete variable that determines which of the component Gaussians to use. This is done by choosing one of the components at random with probability given by the mixing coefficients $\pi_{k}$.
2.  Then, generate an observation vector $x$ from the chosen Gaussian density.

This technique of generating random samples distributed according to the model is known as ancestral sampling.

## 4. Latent Variable Framework

The Gaussian mixture model can be formulated in terms of discrete latent variables. This provides deeper insight into the distribution and motivates the Expectation-Maximization (EM) algorithm.

### 4.1 The Latent Variable $z$
We introduce a $K$-dimensional binary random variable $z$ having a 1-of-K representation in which a particular element $z_{k}$ is equal to 1 and all other elements are equal to 0. The values of $z_{k}$ therefore satisfy $z_{k} \in \{0,1\}$ and $\sum_{k} z_{k} = 1$. There are $K$ possible states for the vector $z$ according to which element is nonzero.

### 4.2 Joint and Marginal Distributions
The marginal distribution over $z$ is specified in terms of the mixing coefficients $\pi_{k}$:
$$p(z_{k}=1) = \pi_{k}$$.
Because $z$ uses a 1-of-K representation, this can be written as:
$$p(z) = \prod_{k=1}^{K} \pi_{k}^{z_{k}}$$.

The conditional distribution of $x$ given a particular value for $z$ is a Gaussian:
$$p(x|z_{k}=1) = \mathcal{N}(x|\mu_{k}, \Sigma_{k})$$.
This can also be written in the form:
$$p(x|z) = \prod_{k=1}^{K} \mathcal{N}(x|\mu_{k}, \Sigma_{k})^{z_{k}}$$.

The joint distribution is given by $p(z)p(x|z)$, and the marginal distribution of $x$ is obtained by summing the joint distribution over all possible states of $z$:
$$p(x) = \sum_{z} p(z)p(x|z) = \sum_{k=1}^{K} \pi_{k} \mathcal{N}(x|\mu_{k}, \Sigma_{k})$$.

<img width="763" height="171" alt="image" src="https://github.com/user-attachments/assets/1f8476fd-9991-49a6-807b-e63aa3072134" />

## 5. Responsibilities

An important quantity is the conditional probability of $z$ given $x$, which we denote as $\gamma(z_{k}) \equiv p(z_{k}=1|x)$. This can be found using Bayes' theorem:
$$\gamma(z_{k}) = \frac{\pi_{k} \mathcal{N}(x|\mu_{k}, \Sigma_{k})}{\sum_{j=1}^{K} \pi_{j} \mathcal{N}(x|\mu_{j}, \Sigma_{j})}$$.

### 5.1 Interpretation
*   $\pi_{k}$ is the prior probability of $z_{k} = 1$.
*   $\gamma(z_{k})$ is the corresponding posterior probability once we have observed $x$.
*   $\gamma(z_{k})$ can be viewed as the responsibility that component $k$ takes for "explaining" the observation $x$.

<img width="881" height="433" alt="image" src="https://github.com/user-attachments/assets/de2c9b56-bb91-4223-a8bb-214697960fb8" />




---






# 4.5 Likelihood of Gaussian Mixture Models (GMM) | Mixing Proportions & Convexity

## 1. The Gaussian Mixture Likelihood Function

A mixture of Gaussians is a linear superposition of Gaussian components. The probability density for a single observation $x$ is given by:

$$
p(x) = \sum_{k=1}^{K} \pi_{k} \mathcal{N}(x|\mu_{k}, \Sigma_{k})
$$

where each Gaussian density $\mathcal{N}(x|\mu_{k}, \Sigma_{k})$ is called a component of the mixture and has its own mean $\mu_{k}$ and covariance $\Sigma_{k}$. 

For a data set of $N$ observations $X = \{x_{1}, ..., x_{N}\}$ assumed to be drawn independently from the distribution, the likelihood function is given by the joint density:

$$
p(X|\pi, \mu, \Sigma) = \prod_{n=1}^{N} \sum_{k=1}^{K} \pi_{k} \mathcal{N}(x_{n}|\mu_{k}, \Sigma_{k})
$$


The log of the likelihood function is:

$$
\ln p(X|\pi, \mu, \Sigma) = \sum_{n=1}^{N} \ln { \sum_{k=1}^{K} \pi_{k} \mathcal{N}(x_{n}|\mu_{k}, \Sigma_{k})}
$$


## 2. Mixing Proportions (Mixing Coefficients)

The parameters $\pi_{k}$ are called mixing coefficients or mixing parameters. They represent the prior probability of picking the $k^{th}$ component.

### 2.1 Requirements for Mixing Proportions
To be valid probabilities, the mixing coefficients must satisfy the following constraints:
*   $0 \le \pi_{k} \le 1$ for all $k$.
*   $\sum_{k=1}^{K} \pi_{k} = 1$.

### 2.2 Maximum Likelihood Solution for $\pi_{k}$
The maximum likelihood estimate $\hat{P}(\omega_{i})$ (or $\pi_{k}$) must satisfy the condition that it is the average over the entire data set of the estimate derived from each sample. To maximize the log-likelihood with respect to $\pi_{k}$ while enforcing the summation constraint, a Lagrange multiplier $\lambda$ is used to maximize the quantity:
$$\ln p(X|\pi, \mu, \Sigma) + \lambda \left( \sum_{k=1}^{K} \pi_{k} - 1 \right)$$.
Setting the derivative to zero yields:
$$0 = \sum_{n=1}^{N} \frac{\mathcal{N}(x_{n}|\mu_{k}, \Sigma_{k})}{\sum_{j} \pi_{j} \mathcal{N}(x_{n}|\mu_{j}, \Sigma_{j})} + \lambda$$.
Multiplying by $\pi_{k}$ and summing over $k$ shows that $\lambda = -N$. Rearranging gives the M-step re-estimation equation:
$$\pi_{k} = \frac{N_{k}}{N}$$
where $N_{k} = \sum_{n=1}^{N} \gamma(z_{nk})$ is the effective number of points assigned to cluster $k$.

## 3. Issues of Convexity and Logarithms

Maximizing the log-likelihood for a Gaussian mixture is a more complex problem than for a single Gaussian.

### 3.1 Non-Convexity and Local Maxima
The difficulty arises from the presence of the summation over $k$ that appears inside the logarithm. Because the logarithm function no longer acts directly on the Gaussian, setting the derivatives to zero does not obtain a closed-form solution. The nonlinearity of the mixture function causes the error/likelihood function to be non-convex. Consequently, the likelihood function can have multiple local maxima, and optimization algorithms like EM are not guaranteed to find the largest of these maxima. 

### 3.2 Identifiability
A density $p(x|\theta)$ is said to be identifiable if $\theta \ne \theta'$ implies that there exists an $x$ such that $p(x|\theta) \ne p(x|\theta')$. For a $K$-component mixture, there are a total of $K!$ equivalent solutions corresponding to the $K!$ ways of assigning $K$ sets of parameters to $K$ components. This means that for any given point in parameter space, there are $K! - 1$ additional points giving rise to the same distribution, which represents a form of nonidentifiability.

<img width="765" height="778" alt="image" src="https://github.com/user-attachments/assets/f26c826d-4ee2-428e-9327-bb3fb474f78b" />

## 4. Singularities in the Likelihood Function

The maximization of the log-likelihood function for GMMs is not a well-posed problem due to the presence of singularities. 

### 4.1 Component Collapse
If one of the components of the mixture model has its mean $\mu_{j}$ exactly equal to one of the data points $x_{n}$, that point contributes a term of the form $\mathcal{N}(x_{n}|x_{n}, \sigma_{j}^{2}I) = \frac{1}{(2\pi)^{1/2}}\frac{1}{\sigma_{j}}$. In the limit $\sigma_{j} \to 0$, this term goes to infinity, and thus the log-likelihood also goes to infinity. 

### 4.2 Comparison with Single Gaussians
This problem does not arise in the case of a single Gaussian distribution. If a single Gaussian collapses onto a point, the multiplicative factors from other data points go to zero exponentially fast, causing the overall likelihood to go to zero. In a mixture, however, other components can provide finite probability to all data points while one component shrinks onto a specific point to provide an increasing additive value to the log-likelihood.

<img width="807" height="272" alt="image" src="https://github.com/user-attachments/assets/7906afd5-9814-4b06-9873-0773fdd12a5e" />
