# 10.1 Relation between Primal and Dual Problem, KKT Conditions

Duality is at the center of the underlying theory of linear programming. Introducing the dual problem is an elegant idea and fundamental for applications. Every linear program has a dual; if the original problem is a minimization, its dual is a maximization. The minimum in the "primal problem" equals the maximum in its dual.

## 10.1.1 The Primal and Dual Problems (Linear Programming)

The theory begins with a given primal problem and starts from the same $A$, $b$, and $c$, but reverses everything.

### The Primal-Dual Pair (Strang Formulation)

**Primal (P)**:
$$\min cx \text{ subject to } x \ge 0 \text{ and } Ax \ge b$$.

**Dual (D)**:
$$\max yb \text{ subject to } y \ge 0 \text{ and } yA \le c$$.

There is complete symmetry between the primal and dual problems; the dual of the dual is the original minimum problem.

### Standard Form (Numerical Optimization Formulation)

**Primal**:
$$\min c^T x \text{ subject to } Ax = b, x \ge 0$$.

**Dual**:
$$\max b^T \lambda \text{ subject to } A^T \lambda + s = c, s \ge 0$$.

The variables $(\lambda, s)$ in the dual problem are jointly referred to as dual variables.

## 10.1.2 The Duality Theorem

The whole theory of linear programming hinges on the relation between primal and dual.

### Weak Duality

**Theorem (Weak Duality)**: If $x$ and $y$ are feasible in the primal and dual problems, then $yb \le cx$.
**Proof**: Since the vectors are feasible, they satisfy $Ax \ge b$ and $yA \le c$. Because feasibility includes $x \ge 0$ and $y \ge 0$, we can take inner products without spoiling the inequalities:
$$yAx \ge yb \text{ and } yAx \le cx$$.
Since the left-hand sides are identical, we have $yb \le cx$. This one-sided inequality prohibits the possibility that both problems are unbounded.

### Strong Duality

**Theorem 13.1 (Strong Duality)**:

1. If either problem has a (finite) solution, then so does the other, and the objective values are equal ($cx★ = y★b$).
2. If either problem is unbounded, then the other problem is infeasible.

> [!IMPORTANT]
> Any vectors that achieve $yb = cx$ must be optimal. At that point, the grocer’s price (primal) equals the druggist’s price (dual).

## 10.1.3 KKT Conditions for Linear Programming

The optimality conditions for linear programming are a specialization of the Karush-Kuhn-Tucker (KKT) conditions.

### The KKT System for LP

The first-order necessary conditions for $x★$ to be a solution of the standard form primal are that there exist vectors $\lambda$ and $s$ such that:

1. **Stationarity**: $A^T \lambda + s = c$.
2. **Primal Feasibility**: $Ax = b$ and $x \ge 0$.
3. **Dual Feasibility**: $s \ge 0$.
4. **Complementary Slackness**: $x_i s_i = 0$ for $i=1, \dots, n$ (or $x^T s = 0$).

### Complementary Slackness (Economic Interpretation)

The optimal vectors $x★$ and $y★$ satisfy complementary slackness:

* If $(Ax★)_i > b_i$, then $y_i★ = 0$ (A surplus vitamin has zero price).
* If $(y★A)_j < c_j$, then $x_j★ = 0$ (An overpriced food is omitted from the diet).

## 10.1.4 General KKT Conditions (Constrained Optimization)

For the general nonlinear problem:
$$\min f(x) \text{ subject to } c_i(x) = 0, i \in \mathcal{E}; c_i(x) \ge 0, i \in \mathcal{I}$$.

**Theorem 12.1 (First-Order Necessary Conditions)**: Suppose $x★$ is a local solution and LICQ holds. Then there is a Lagrange multiplier vector $\lambda★$ such that:

1. $\nabla_x \mathcal{L}(x★, \lambda★) = \nabla f(x★) - \sum_{i \in \mathcal{E} \cup \mathcal{I}} \lambda_i★ \nabla c_i(x★) = 0$.
2. $c_i(x★) = 0$ for $i \in \mathcal{E}$.
3. $c_i(x★) \ge 0$ for $i \in \mathcal{I}$.
4. $\lambda_i★ \ge 0$ for $i \in \mathcal{I}$.
5. $\lambda_i★ c_i(x★) = 0$ for $i \in \mathcal{E} \cup \mathcal{I}$.

## 10.1.5 Duality in Nonlinear Programming

For the special case where $f$ and $-c_i$ are convex functions, we define the dual objective $q(\lambda)$:
$$q(\lambda) = \inf_x \mathcal{L}(x, \lambda) = \inf_x [f(x) - \lambda^T c(x)]$$.

The **Dual Problem** is:
$$\max q(\lambda) \text{ subject to } \lambda \ge 0$$.

> [!NOTE]
> The function $q(\lambda)$ is always concave, even if the primal objective $f$ is not convex.

## 10.1.6 Sensitivity and Shadow Prices

The Lagrange multipliers (dual variables) answer the key question: How does the minimum cost change if we change $b$ or $c$?.

* **Shadow Prices**: The components of $y★$ (or $\lambda★$) are shadow prices. They give the rate of change of the minimum cost with respect to changes in the constraint $b$.
* **Sensitivity Formula**: For small perturbations $\Delta b$, the change in optimal objective is:
  $$\Delta \text{Cost} = y★ \Delta b$$.
  $$\epsilon \lambda_j \text{ for } \Delta b = \epsilon e_j$$.

<img width="1050" height="958" alt="image" src="https://github.com/user-attachments/assets/2d369f8a-e522-4b1a-9cb5-17ac1b0c3ea9" />

### Comparison of Primal and Dual Components

| Primal Problem          | Dual Problem                 |
| :---------------------- | :--------------------------- |
| Cost coefficients $c$   | Right-hand side bounds       |
| Right-hand side $b$     | Cost coefficients            |
| Constraint matrix $A$   | Transpose matrix $A^T$       |
| Minimize objective      | Maximize objective           |
| Variable $x_j★ = 0$     | Slack in dual constraint $j$ |
| Slack in constraint $i$ | Dual variable $y_i★ = 0$     |




---





# 10.2 Probability Exponential Family

The exponential family of distributions constitutes a small number of key discrete-type and continuous-type distributions that arise frequently in applications. These distributions, including the Bernoulli, Poisson, and Gaussian, are linked through limiting properties and shared mathematical structures such as the memoryless property.

## 10.2.1 The Exponential Distribution

A random variable $T$ has the **exponential distribution** with parameter $\lambda > 0$ if its probability density function (pdf) is given by:

$$
f_T(t) = \begin{cases} \lambda e^{-\lambda t} & t \ge 0 \ 0 & else \end{cases}
$$

### 1. Cumulative Distribution Function (CDF)

The CDF evaluated at a $t \ge 0$ is:

$$
F_T(t) = \int_{-\infty}^{t} f_T(s)ds = \int_{0}^{t} \lambda e^{-\lambda s} ds = -e^{-\lambda s} |_0^t = 1 - e^{-\lambda t}
$$

The complementary CDF, $F_T^c(t) = P{T > t}$, is:

$$
F_T^c(t) = \begin{cases} e^{-\lambda t} & t \ge 0 \ 1 & t < 0 \end{cases}
$$

<img width="1344" height="1612" alt="image" src="https://github.com/user-attachments/assets/12894224-8c38-47af-93a0-3d9cbcc0364c" />

### 2. Moments of the Exponential Distribution

Using integration by parts, the $n^{th}$ moment of $T$ is found to be $E[T^n] = \frac{n}{\lambda} E[T^{n-1}]$.

* **Mean**: $E[T] = \frac{1}{\lambda}$.
* **Second Moment**: $E[T^2] = \frac{2}{\lambda^2}$.
* **Variance**: $Var(T) = E[T^2] - E[T]^2 = \frac{2}{\lambda^2} - \frac{1}{\lambda^2} = \frac{1}{\lambda^2}$.
* **Standard Deviation**: $\sigma_T = \frac{1}{\lambda}$.

## 10.2.2 The Memoryless Property

The exponential distribution is the only continuous distribution to possess the **memoryless property**.
**Property**: For $s, t \ge 0$, $P(T > s+t | T > s) = P{T > t}$.

> [!NOTE]
> If $T$ is the lifetime of a component, the memoryless property implies that if the component is still working after $s$ time units, the probability it continues to work for $t$ additional units is the same as the probability a new component lasts $t$ units.

## 10.2.3 Connection to Other Distributions

The exponential distribution serves as the continuous-time analog of the geometric distribution.

### 1. Limit of Scaled Geometric Distributions

If $L_h$ is a geometrically distributed random variable with parameter $p = \lambda h$, then the distribution of the scaled variable $T_h = hL_h$ converges to the exponential distribution with parameter $\lambda$ as $h \to 0$.

### 2. Poisson Process Intercount Times

In a Poisson process with rate $\lambda$, the intercount times $U_1, U_2, \dots$ are mutually independent, exponentially distributed random variables with parameter $\lambda$.

### 3. Erlang Distribution

The sum of $r$ independent, exponentially distributed random variables with parameter $\lambda$ has the **Erlang distribution** with parameters $r$ and $\lambda$.

* **Pdf**: $f_{T_r}(t) = \frac{\lambda^r t^{r-1} e^{-\lambda t}}{(r-1)!}$ for $t \ge 0$.

<img width="660" height="660" alt="image" src="https://github.com/user-attachments/assets/93a2394c-fe39-4ea6-b50c-b33cfc29cc6d" />

## 10.2.4 Maximum Likelihood Estimation (MLE)

To estimate the parameter $\lambda$ from $n$ independent observed lifetimes $(u_1, \dots, u_n)$, we maximize the likelihood function:

$$
f_\lambda(u_1, \dots, u_n) = \prod_{i=1}^n \lambda e^{-\lambda u_i} = \lambda^n e^{-\lambda \sum u_i}
$$

**Derivation**:
Taking the derivative with respect to $\lambda$:

$$
\frac{d}{d\lambda}(\lambda^n e^{-\lambda t}) = (n - t\lambda)\lambda^{n-1} e^{-\lambda t}
$$

where $t = \sum u_i$.
Setting the derivative to zero yields the **Maximum Likelihood Estimate**:

$$
\hat{\lambda}*{ML} = \frac{n}{t} = \frac{n}{\sum*{i=1}^n u_i}
$$

## 10.2.5 Summary Table: Key Distributions in the Family

| Distribution                   | Pmf / Pdf                                                        | Mean                  | Variance              |
| :----------------------------- | :--------------------------------------------------------------- | :-------------------- | :-------------------- |
| **Bernoulli ($p$)**            | $p^i(1-p)^{1-i}, i \in {0,1}$                                    | $p$                   | $p(1-p)$              |
| **Binomial ($n, p$)**          | $\binom{n}{i}p^i(1-p)^{n-i}$                                     | $np$                  | $np(1-p)$             |
| **Poisson ($\lambda$)**        | $\frac{\lambda^i e^{-\lambda}}{i!}$                              | $\lambda$             | $\lambda$             |
| **Exponential ($\lambda$)**    | $\lambda e^{-\lambda t}, t \ge 0$                                | $1/\lambda$           | $1/\lambda^2$         |
| **Gaussian ($\mu, \sigma^2$)** | $\frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{(u-\mu)^2}{2\sigma^2}}$ | $\mu$                 | $\sigma^2$            |
| **Rayleigh ($\sigma^2$)**      | $\frac{t}{\sigma^2} e^{-\frac{t^2}{2\sigma^2}}, t \ge 0$         | $\sigma \sqrt{\pi/2}$ | $\sigma^2(2 - \pi/2)$ |






---
---



# 11.1 Parameter Estimation: EM Algorithm


## 11.1.1 Principles of Parameter Estimation

Sometimes when we devise a probability model for some situation we have a reason to use a particular type of probability distribution, but there may be a parameter that has to be selected. A common approach is to collect some data and then estimate the parameter using the observed data.

### 1. The Likelihood Function

* **Discrete Case**: Suppose an experiment is accurately modeled by a probability model with a random variable $X$, and that the pmf of $X$ is $p_{\theta}$, where $\theta$ ★ is a parameter whose value is not known before the experiment is performed. When the experiment is performed, suppose we observe a particular value $k$ for $X$. According to the probability model, the probability of $k$ being the observed value for $X$, before the experiment was performed, would have been $p_{\theta}(k)$. It is said that the **likelihood** that $X = k$ is $p_{\theta}(k)$.
* **Continuous Case**: For continuous-type observations, the probability of a specific observation $u$ is zero for any value of $\theta$ ★. however, if $f_{\theta}$ is a continuous pdf for each value of $\theta$ ★, then for $\epsilon$ sufficiently small, $f_{\theta}(u) \approx \frac{1}{\epsilon} P{u - \frac{\epsilon}{2} < X < u + \frac{\epsilon}{2}}$. That is, $f_{\theta}(u)$ is proportional to the probability that the observation is in an $\epsilon$-width interval centered at $u$, where the constant of proportionality, namely $\frac{1}{\epsilon}$, is the same for all $\theta$ ★. In this context, $f_{\theta}(u)$ is called the **likelihood of the observation $u$**.

### 2. Maximum Likelihood Estimate (MLE)

The maximum likelihood estimate of $\theta$ ★ for observation $k$ (or $u$), denoted by $\hat{\theta}_{ML}$, is the value of $\theta$ ★ that maximizes the likelihood with respect to $\theta$ ★.

> [!NOTE]
> Intuitively, the maximum likelihood estimate is the value of $\theta$ ★ that best explains the observed value, or makes it the least surprising.

<img width="1103" height="389" alt="image" src="https://github.com/user-attachments/assets/9e49e3d5-0424-4a79-90d7-47c0d13d7f39" />

## 11.1.2 MLE for Discrete-Type Variables

### 1. Bernoulli/Binomial Distribution (Bent Coin)

Suppose a coin shows heads with probability $p$ each time it is flipped. A student flips the coin $n$ times and heads shows on $k$ of the flips.

* **Likelihood**: $p_X(k) = \binom{n}{k} p^k (1-p)^{n-k}$.
* **Estimation**: To find $\hat{p}_{ML}$, we maximize $p^k (1-p)^{n-k}$. The derivative with respect to $p$ is:
  $$\frac{d(p^k(1-p)^{n-k})}{dp} = (k - np)p^{k-1}(1-p)^{n-k-1}$$.
* **Result**: The derivative is zero if $p = \frac{k}{n}$. Therefore, **$\hat{p}_{ML}(k) = \frac{k}{n}$**.

### 2. Geometric Distribution

Suppose $X$ has the geometric distribution with unknown parameter $p$, and a particular value $k$ is observed.

* **Likelihood**: $p_X(k) = (1-p)^{k-1}p$ for $k \ge 1$.
* **Estimation**: Differentiating yields $((1-p)^{k-1}p)' = (1-kp)(1-p)^{k-2}$.
* **Result**: The likelihood is increasing in $p$ for $0 \le p \le \frac{1}{k}$ and decreasing for $\frac{1}{k} \le p \le 1$. Therefore, **$\hat{p}_{ML} = \frac{1}{k}$**.

### 3. Poisson Distribution

It is assumed that $X$ has a Poisson distribution with parameter $\lambda \ge 0$, and $X = k$ is observed.

* **Likelihood**: $\frac{\lambda^k e^{-\lambda}}{k!}$.
* **Estimation**: Maximizing $\lambda^k e^{-\lambda}$ for $k \ge 1$ involves the derivative $\frac{d(\lambda^k e^{-\lambda})}{d\lambda} = (k-\lambda)\lambda^{k-1}e^{-\lambda}$.
* **Result**: The likelihood is maximized at **$\hat{\lambda}_{ML}(k) = k$**.

## 11.1.3 MLE for Continuous-Type Variables

### 1. Uniform Distribution

Suppose $X$ is drawn at random from the uniform distribution on the interval $[0, b]$, where $b$ is to be estimated, and $X = u$ is observed.

* **Likelihood**: $f_b(u) = \frac{1}{b} I_{{0 \le u \le b}}$.
* **Estimation**: The function is zero if $b < u$, jumps to $\frac{1}{u}$ at $b = u$, and then decreases as $b$ increases.
* **Result**: **$\hat{b}_{ML}(u) = u$**.

### 2. Normal (Gaussian) Distribution

In the case of independent observations assumed to follow a normal distribution, the discrepancies between model and observations are $\epsilon_j = \phi(x; t_j) - y_j$.

* **Likelihood**: Under the assumption of independent and identically distributed errors with variance $\sigma^2$ and normal density $g_\sigma(\epsilon) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp(-\frac{\epsilon^2}{2\sigma^2})$, the likelihood of a set of observations is:
  $$p(y; x, \sigma) = \prod_{j=1}^m g_\sigma(\phi(x; t_j) - y_j)$$.
* **Result**: Maximizing $p(y; x, \sigma)$ with respect to $x$ provides the maximum likelihood estimate.

## 11.1.4 Estimation of Signal Ascent Rate (Drone Altimeter Example)

Observations $X_1, \dots, X_T$ are assumed to be $X_t = bt + W_t$, where $b$ is the unknown rate of ascent and $W_t$ are independent $N(0, 1)$ noise variables.

* **Joint PDF (Likelihood)**:
  $$f_{X_1, \dots, X_T}(u_1, \dots, u_T) = \frac{1}{(2\pi)^{T/2}} \exp(-\sum_{t=1}^T \frac{(u_t - bt)^2}{2})$$.
* **Estimation**: The estimator $\hat{b}*{ML}$ minimizes the quadratic function $\sum*{t=1}^T \frac{(u_t - bt)^2}{2}$. Setting the derivative with respect to $b$ to zero:
  $$\frac{d}{db}(\sum_{t=1}^T \frac{(u_t - bt)^2}{2}) = b \sum_{t=1}^T t^2 - \sum_{t=1}^T u_t t = 0$$.
* **Result**: **$\hat{b}*{ML} = \frac{\sum*{t=1}^T u_t t}{\sum_{t=1}^T t^2}$**.

> [!IMPORTANT]
> An estimator is called **unbiased** if the mean of the estimator is equal to the parameter being estimated. Since $E[X_t] = bt$, the expectation $E[\hat{b}_{ML}] = b$, confirming the MLE in this case is unbiased.

## 11.1.5 Summary of MLE Properties

| Distribution              | Parameter  | Observed Value | ML Estimate ($\hat{\theta}_{ML}$) |
| :------------------------ | :--------- | :------------- | :-------------------------------- |
| **Binomial ($n, p$)**     | $p$        | $k$ successes  | $k/n$                             |
| **Uniform ($[1, n]$)**    | $n$        | $k$            | $k$                               |
| **Geometric ($p$)**       | $p$        | $k$            | $1/k$                             |
| **Poisson ($\lambda$)**   | $\lambda$  | $k$            | $k$                               |
| **Uniform ($[0, b]$)**    | $b$        | $u$            | $u$                               |
| **Rayleigh ($\sigma^2$)** | $\sigma^2$ | $u$            | $u^2/2$                           |
