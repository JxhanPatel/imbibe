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
>
>
