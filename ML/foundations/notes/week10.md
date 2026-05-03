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
