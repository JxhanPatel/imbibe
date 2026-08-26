# 10.1: Foundations of Margins & Linear Separability

## 1. Linear Discriminant Functions and Affine Decision Surfaces

In two-class pattern classification, a linear discriminant function computes a linear combination of the input features to define a decision boundary. The general linear model has the form:

$$y(\mathbf{x}) = \mathbf{w}^T \phi(\mathbf{x}) + b$$

where:

* $\mathbf{w}$ is the weight vector that determines the orientation of the decision surface.
* $b$ is the bias parameter (the negative of which is called the threshold).
* $\phi(\mathbf{x})$ denotes a fixed, nonlinear feature-space transformation of the $D$-dimensional input vector $\mathbf{x}$.

The decision boundary is defined by the relation $y(\mathbf{x}) = 0$, which geometrically corresponds to a $(D-1)$-dimensional hyperplane within the $D$-dimensional feature space.

### Geometric Properties of the Hyperplane:

* **Orientation:** For any two points $\mathbf{x}_A$ and $\mathbf{x}_B$ lying on the decision surface, we have $y(\mathbf{x}_A) = y(\mathbf{x}_B) = 0$. This implies:

$$\mathbf{w}^T (\mathbf{x}_A - \mathbf{x}_B) = 0$$

Hence, the weight vector $\mathbf{w}$ is orthogonal to every vector lying within the decision surface, thus determining its spatial orientation.

* **Distance from the Origin:** The normal signed distance from the origin of the feature space to the decision surface is given by:

$$\frac{b}{\Vert{}\mathbf{w}\Vert{}}$$

* **Signed Distance of a Point:** The value of $y(\mathbf{x})$ gives a signed measure of the perpendicular distance $r$ of any arbitrary point $\mathbf{x}$ to the decision hyperplane. Letting $\mathbf{x}_{\perp}$ be the orthogonal projection of $\mathbf{x}$ onto the decision surface:

$$\mathbf{x} = \mathbf{x}_{\perp} + r \frac{\mathbf{w}}{\Vert{}\mathbf{w}\Vert{}}$$

Multiplying both sides by $\mathbf{w}^T$ and adding $b$, and noting that $y(\mathbf{x}_{\perp}) = \mathbf{w}^T \mathbf{x}_{\perp} + b = 0$, we obtain:

$$r = \frac{y(\mathbf{x})}{\Vert{}\mathbf{w}\Vert{}}$$

<img width="1327" height="647" alt="image" src="https://github.com/user-attachments/assets/caecdab7-ceb5-40e5-a507-26cdf68418c8" />

## 2. Linear Separability and Feature Space Normalization

Suppose we are given a training dataset comprising $N$ input vectors $\mathbf{x}_1, \dots, \mathbf{x}_N$ with corresponding target values $t_1, \dots, t_N$, where $t_n \in \{-1, +1\}$.

A dataset is said to be linearly separable in feature space if there exists at least one choice of parameters $\mathbf{w}$ and $b$ such that the function $y(\mathbf{x})$ satisfies $y(\mathbf{x}_n) > 0$ for points having $t_n = +1$, and $y(\mathbf{x}_n) < 0$ for points having $t_n = -1$.

Under this condition, all training points satisfy:

$$t_n y(\mathbf{x}_n) > 0 \quad \forall n = 1, \dots, N$$

### Coordinate Augmentation and Sign Inversion Normalization:

To simplify the mathematical treatment of linear inequalities, we can map the $d$-dimensional input space to a $(d+1)$-dimensional augmented space.

* **Augmented Vectors:** We define the augmented feature vector $\mathbf{y}_n$ and the augmented weight vector $\mathbf{a}$ by appending a constant coordinate:

$$\mathbf{y}_n = \begin{bmatrix} 1 \\ \mathbf{x}_n \end{bmatrix}, \quad \mathbf{a} = \begin{bmatrix} w_0 \\ \mathbf{w} \end{bmatrix}$$

This maps the hyperplane equation to $\mathbf{a}^T \mathbf{y}_n = 0$, which passes strictly through the origin in the augmented space.

* **Sign Normalization:** We replace all augmented samples belonging to class $\omega_2$ (where $t_n = -1$) by their negatives. This "normalization" allows us to discard the class labels and look for a single separating vector $\mathbf{a}$ that satisfies:

$$\mathbf{a}^T \mathbf{y}_n > 0 \quad \forall n = 1, \dots, N$$

<img width="1083" height="600" alt="image" src="https://github.com/user-attachments/assets/53295206-e184-467c-a9a7-fc899f8d4628" />

## 10.1.1 Perceptrons and Margin

### 1. Rosenblatt's Perceptron Model

The perceptron is a generalized linear model that maps an input vector $\mathbf{x}$ to a feature vector $\phi(\mathbf{x})$ and outputs a discrete binary decision using a step activation function:

$$y(\mathbf{x}) = f(\mathbf{w}^T \phi(\mathbf{x}))$$

where the nonlinear activation function $f(a)$ is defined as:

$$f(a) = \begin{cases} +1, & a \ge 0 \\ -1, & a < 0 \end{cases}$$

#### The Perceptron Criterion:

To determine the parameters $\mathbf{w}$, we cannot minimize the total number of misclassified patterns directly. The misclassification count is a piecewise constant function of $\mathbf{w}$ with step discontinuities; its gradient is zero almost everywhere, which prevents gradient descent.

Instead, we minimize the Perceptron Criterion Function, which associates zero error with any correctly classified pattern, and penalizes misclassified patterns linearly:

$$E_P(\mathbf{w}) = -\sum_{n \in \mathcal{M}} \mathbf{w}^T \phi_n t_n$$

where $\mathcal{M}$ denotes the set of all misclassified patterns, and $\phi_n = \phi(\mathbf{x}_n)$. The total error function is piecewise linear in $\mathbf{w}$.

Using stochastic gradient descent, the weight vector is updated sequentially upon encountering a misclassified pattern $\mathbf{x}_n$:

$$\mathbf{w}^{(\tau+1)} = \mathbf{w}^{(\tau)} + \eta \phi_n t_n$$

Because $y(\mathbf{x}, \mathbf{w})$ is invariant to scaling of $\mathbf{w}$, the learning rate $\eta$ can be set to $1$ without loss of generality.

### 2. Incorporating a Margin Constraint

To protect the decision boundary from converging directly on the edge of the training patterns, we can enforce a positive constant margin constraint $b > 0$.

#### Geometric Effect of the Margin:

Rather than solving the weak inequalities $\mathbf{a}^T \mathbf{y}_n > 0$, we require that:

$$\mathbf{a}^T \mathbf{y}_n \ge b > 0$$

This constraint shrinks the feasible solution region. The boundary of the new solution region is insulated from the old boundaries by a perpendicular distance of:

$$\frac{b}{\Vert{}\mathbf{y}_n\Vert{}}$$

<img width="979" height="552" alt="image" src="https://github.com/user-attachments/assets/d01cadce-c4a7-4c5b-908f-7564e66da7a8" />

#### Variable-Increment Perceptron with Margin:

The single-sample learning rule is modified to apply a correction whenever the projection of the normalized sample fails to exceed the margin $b$. The update rule is formulated as:

$$\mathbf{a}(k+1) = \mathbf{a}(k) + \eta(k) \mathbf{y}^k$$

which is executed if and only if:

$$\mathbf{a}^T(k) \mathbf{y}^k \le b$$

For separable data, this algorithm is guaranteed to converge to a separating vector in a finite number of corrections, provided that the sequence of learning rates $\eta(k)$ satisfies the following conditions:

* $\eta(k) \ge 0$
* $\lim_{m \to \infty} \sum_{k=1}^m \eta(k) = \infty$
* $\lim_{m \to \infty} \frac{\sum_{k=1}^m \eta^2(k)}{\left(\sum_{k=1}^m \eta(k)\right)^2} = 0$

## 10.1.2 Maximum Margin: Formulation

### 1. Primal Optimization Problem

For a linearly separable dataset in feature space, there exist infinite hyperplanes that separate the classes perfectly. To find the unique hyperplane that minimizes the generalization error, we define the margin to be the smallest perpendicular distance between the decision boundary and any of the training samples.

The perpendicular distance of any point $\mathbf{x}_n$ from the hyperplane is given by:

$$\frac{t_n y(\mathbf{x}_n)}{\Vert{}\mathbf{w}\Vert{}} = \frac{t_n (\mathbf{w}^T \phi(\mathbf{x}_n) + b)}{\Vert{}\mathbf{w}\Vert{}}$$

We seek parameters $\mathbf{w}$ and $b$ that maximize the margin of the closest point in the dataset:

$$\arg \max_{\mathbf{w}, b} \left\{ \frac{1}{\Vert{}\mathbf{w}\Vert{}} \min_n \left[ t_n (\mathbf{w}^T \phi(\mathbf{x}_n) + b) \right] \right\}$$

<img width="1306" height="631" alt="image" src="https://github.com/user-attachments/assets/f8747414-3b9c-42f4-a5bb-d15380649d24" />

#### Establishing the Canonical Representation:

Because the distance is invariant under the scaling transformation $\mathbf{w} \to \kappa \mathbf{w}$ and $b \to \kappa b$, we can use this scaling freedom to set the distance of the closest point to the decision boundary such that:

$$t_n (\mathbf{w}^T \phi(\mathbf{x}_n) + b) = 1$$

For this closest point, the constraint is said to be active. Consequently, all training observations must satisfy the inequality constraints:

$$t_n (\mathbf{w}^T \phi(\mathbf{x}_n) + b) \ge 1, \quad n = 1, \dots, N$$

This is known as the canonical representation of the separating hyperplane.

Maximizing the margin $\Vert{}\mathbf{w}\Vert{}^{-1}$ under these constraints is mathematically equivalent to minimizing the squared norm $\Vert{}\mathbf{w}\Vert{}^2$. We formulate this as a constrained Primal Optimization Problem:

$$\min_{\mathbf{w}, b} \frac{1}{2} \Vert{}\mathbf{w}\Vert{}^2$$

$$\text{subject to } t_n (\mathbf{w}^T \phi(\mathbf{x}_n) + b) \ge 1, \quad n = 1, \dots, N$$

This is a convex quadratic programming problem involving a quadratic objective function subject to linear inequality constraints.

### 2. Lagrangian Formulation and the Wolfe Dual

To solve this constrained minimization problem, we introduce Lagrange multipliers $a_n \ge 0$, with one multiplier for each inequality constraint, to construct the Primal Lagrangian function:

$$L(\mathbf{w}, b, \mathbf{a}) = \frac{1}{2} \Vert{}\mathbf{w}\Vert{}^2 - \sum_{n=1}^N a_n \left\{ t_n (\mathbf{w}^T \phi(\mathbf{x}_n) + b) - 1 \right\}$$

where $\mathbf{a} = (a_1, \dots, a_N)^T$ is the vector of Lagrange multipliers. We must minimize $L(\mathbf{w}, b, \mathbf{a})$ with respect to the primal variables $\mathbf{w}$ and $b$, and maximize it with respect to the dual variables $\mathbf{a}$.

#### Stationarity Conditions:

Setting the partial derivatives of $L(\mathbf{w}, b, \mathbf{a})$ with respect to $\mathbf{w}$ and $b$ to zero yields:

$$\frac{\partial L}{\partial \mathbf{w}} = 0 \implies \mathbf{w} = \sum_{n=1}^N a_n t_n \phi(\mathbf{x}_n)$$

$$\frac{\partial L}{\partial b} = 0 \implies \sum_{n=1}^N a_n t_n = 0$$

#### Deriving the Wolfe Dual Lagrangian:

By substituting these stationarity equations back into the primal Lagrangian, we eliminate the primal parameters $\mathbf{w}$ and $b$. This yields the Wolfe Dual Lagrangian representation:

$$\tilde{L}(\mathbf{a}) = \sum_{n=1}^N a_n - \frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N a_n a_m t_n t_m k(\mathbf{x}_n, \mathbf{x}_m)$$

where the kernel function is defined as the inner product in the feature space:

$$k(\mathbf{x}_n, \mathbf{x}_m) = \phi(\mathbf{x}_n)^T \phi(\mathbf{x}_m)$$

The dual optimization problem requires maximizing the dual Lagrangian:

$$\max_{\mathbf{a}} \tilde{L}(\mathbf{a})$$

$$\text{subject to } a_n \ge 0, \quad n = 1, \dots, N$$

$$\text{and } \sum_{n=1}^N a_n t_n = 0$$

This dual formulation allows the model to be expressed entirely in terms of kernel evaluations, enabling efficient classification in high-dimensional or infinite-dimensional feature spaces.

### 3. Karush-Kuhn-Tucker (KKT) Conditions and Sparsity

Because the constraints are linear inequalities, the optimal solution must satisfy the Karush-Kuhn-Tucker (KKT) conditions:

* $a_n \ge 0$
* $t_n y(\mathbf{x}_n) - 1 \ge 0$
* $a_n \left\{ t_n y(\mathbf{x}_n) - 1 \right\} = 0$

#### The Principle of Sparsity:

The third KKT condition is the complementary slackness condition. It dictates that for every training point $n$, either the Lagrange multiplier $a_n = 0$, or the point lies exactly on the margin slab boundary such that $t_n y(\mathbf{x}_n) = 1$.

* **Non-Support Vectors ($a_n = 0$):** Points that lie strictly outside the margin boundary have inactive constraints. Their Lagrange multipliers are zero, meaning they do not contribute to the weight vector expansion and play no role in making predictions for new test points.
* **Support Vectors ($a_n > 0$):** Points that lie exactly on the maximum margin hyperplanes in feature space have active constraints. These points are called Support Vectors. The entire classification boundary is determined exclusively by this sparse subset of training points.

### 4. Dual Prediction and the Bias Parameter

Once the optimal Lagrange multipliers $\mathbf{a}^{\star}$ are determined, the classification of a new input vector $\mathbf{x}$ is computed using the dual predictive function:

$$y(\mathbf{x}) = \sum_{n=1}^N a_n^{\star} t_n k(\mathbf{x}, \mathbf{x}_n) + b^{\star}$$

#### Solving for the Bias Parameter $b^{\star}$:

Any active support vector $\mathbf{x}_n \in \mathcal{S}$ must satisfy $t_n y(\mathbf{x}_n) = 1$. Substituting the dual predictive function:

$$t_n \left( \sum_{m \in \mathcal{S}} a_m^{\star} t_m k(\mathbf{x}_n, \mathbf{x}_m) + b^{\star} \right) = 1$$

To obtain a numerically stable estimate, we multiply through by $t_n$ (noting that $t_n^2 = 1$) and average the equations over the entire set $\mathcal{S}$ of support vectors:

$$b^{\star} = \frac{1}{N_{\mathcal{S}}} \sum_{n \in \mathcal{S}} \left( t_n - \sum_{m \in \mathcal{S}} a_m^{\star} t_m k(\mathbf{x}_n, \mathbf{x}_m) \right)$$

where $N_{\mathcal{S}}$ is the total number of active support vectors.






---






## 10.2 Optimization Theory & Dual Formulation

### 10.2.1 Constrained Optimization

To understand the mathematical foundations of support vector machines, we must first establish the theory of constrained optimization using Lagrange multipliers.

#### 1. Equality Constraints

Consider the problem of minimizing a function $f(x)$ subject to an equality constraint of the form $g(x) = 0$.

* **Geometric Representation:** The constraint equation $g(x) = 0$ represents a $D-1$ dimensional surface within a $D$-dimensional space.
* **The Gradient Relation:** At any point on the constraint surface, the gradient $\nabla g(x)$ is orthogonal to the surface. For any step $dx$ along the constraint surface, the change in $g(x)$ is zero, so $dx^T \nabla g(x) = 0$.
* **Stationary Condition:** We seek a point $x^\star$ on the constraint surface such that $f(x)$ is minimized. For any displacement $dx$ that lies along the constraint surface, the change in $f(x)$ must also vanish, so $dx^T \nabla f(x) = 0$.
* **Proportionality:** Since both $\nabla f(x)$ and $\nabla g(x)$ are orthogonal to the constraint surface, they must be parallel (or anti-parallel) vectors. Thus, there must exist a parameter $\lambda \neq 0$ such that:

$$\nabla f(x) + \lambda \nabla g(x) = 0$$

where $\lambda$ is called a Lagrange multiplier.


<img width="1237" height="469" alt="image" src="https://github.com/user-attachments/assets/84ad5a8d-3d3a-41ad-b9c6-5b12e7dfe62e" />

We can formulate this condition by introducing the Lagrangian function:

$$L(x, \lambda) = f(x) + \lambda g(x)$$

By setting the gradient of the Lagrangian with respect to both $x$ and $\lambda$ to zero:

$$\nabla_x L(x, \lambda) = 0 \implies \nabla f(x) + \lambda \nabla g(x) = 0$$

$$\frac{\partial L}{\partial \lambda} = 0 \implies g(x) = 0$$

we recover the constrained stationary conditions.

#### 2. Inequality Constraints

Now consider the problem of minimizing $f(x)$ subject to an inequality constraint of the form $g(x) \ge 0$. This defines a region of space called the feasible region.
The solution point $x^\star$ can fall into one of two distinct regimes:

##### Case A: The Active Constraint (Boundary Solution)

The optimal solution lies on the boundary of the feasible region where $g(x^\star) = 0$.

* **Objective Gradient direction:** Since the minimum of $f(x)$ lies outside the feasible region, the gradient $\nabla f(x^\star)$ points inward toward the interior of the feasible region (the direction of decreasing $f(x)$ points outward).
* **Constraint Gradient direction:** The gradient of the constraint $\nabla g(x^\star)$ always points outward (toward the region where $g(x) > 0$).
* **Opposing Directions:** Therefore, the gradients must point in opposite directions, meaning there exists some $\lambda > 0$ such that:

$$\nabla f(x^\star) - \lambda \nabla g(x^\star) = 0$$


<img width="1219" height="446" alt="image" src="https://github.com/user-attachments/assets/34fae9a4-5c04-43e8-94d6-b34d8f90bb4c" />

##### Case B: The Inactive Constraint (Interior Solution)

The optimal solution lies strictly in the interior of the feasible region where $g(x^\star) > 0$.

* **Unconstrained Stationary Point:** The constraint is inactive, so the gradient of the objective function must vanish independently:

$$\nabla f(x^\star) = 0$$

This corresponds to setting $\lambda = 0$ in our joint formulation.

##### The Karush-Kuhn-Tucker (KKT) Conditions

To unify both cases into a single optimization framework, we write the Lagrangian function as:

$$L(x, \lambda) = f(x) - \lambda g(x)$$

The stationary point must satisfy the KKT conditions:

$$\lambda \ge 0$$

$$g(x) \ge 0$$

$$\lambda g(x) = 0$$

The condition $\lambda g(x) = 0$ is known as the complementary slackness condition. It guarantees that when the constraint is inactive ($g(x) > 0$), the multiplier $\lambda$ is exactly zero, and when the constraint is active ($g(x) = 0$), the multiplier $\lambda$ can be strictly positive.

> [!NOTE]
> If we have multiple inequality constraints $g_i(x) \ge 0$ for $i = 1, \dots, N$, we introduce a separate Lagrange multiplier $a_i \ge 0$ for each constraint, defining the Lagrangian:
> $$L(x, a) = f(x) - \sum_{i=1}^N a_i g_i(x)$$
> 
> 

### 10.2.2 Formulating the Dual Problem

We now apply constrained optimization theory to derive the dual representation of the maximum margin classifier for a linearly separable dataset.

#### 1. The Primal Formulation

We are given a training dataset of $N$ observations $\phi(x_1), \dots, \phi(x_N)$ with target labels $t_n \in \{-1, +1\}$. We assume the data is linearly separable, so there exists at least one hyperplane satisfying:

$$t_n (w^T \phi(x_n) + b) \ge 1 \quad \forall n = 1, \dots, N$$

We seek to minimize the squared norm of the weight vector subject to these linear inequality constraints:

$$\min_{w, b} \frac{1}{2} \Vert{}w\Vert{}^2$$

$$\text{subject to } t_n (w^T \phi(x_n) + b) - 1 \ge 0, \quad n = 1, \dots, N$$

#### 2. The Primal Lagrangian

We introduce a vector of Lagrange multipliers $a = (a_1, \dots, a_N)^T \ge 0$ to construct the Primal Lagrangian:

$$L(w, b, a) = \frac{1}{2} \Vert{}w\Vert{}^2 - \sum_{n=1}^N a_n \{ t_n (w^T \phi(x_n) + b) - 1 \}$$

This Lagrangian must be minimized with respect to the primal variables $w$ and $b$, and maximized with respect to the dual variables $a_n \ge 0$.

#### 3. Deriving the Wolfe Dual

To eliminate the primal variables $w$ and $b$ analytically, we find the stationary points of $L(w, b, a)$ by taking partial derivatives and setting them to zero:

$$\frac{\partial L}{\partial w} = 0 \implies w - \sum_{n=1}^N a_n t_n \phi(x_n) = 0 \implies w = \sum_{n=1}^N a_n t_n \phi(x_n)$$

$$\frac{\partial L}{\partial b} = 0 \implies \sum_{n=1}^N a_n t_n = 0$$

We substitute these stationary relations back into the Primal Lagrangian:

$$L(w, b, a) = \frac{1}{2} \left( \sum_{n=1}^N a_n t_n \phi(x_n) \right)^T \left( \sum_{m=1}^N a_m t_m \phi(x_m) \right) - \sum_{n=1}^N a_n t_n \left( \sum_{m=1}^N a_m t_m \phi(x_m) \right)^T \phi(x_n) - b \sum_{n=1}^N a_n t_n + \sum_{n=1}^N a_n$$

Using the relation $\sum_{n=1}^N a_n t_n = 0$, the term involving $b$ vanishes:

$$b \sum_{n=1}^N a_n t_n = 0$$

We rewrite the remaining terms:

$$L(w, b, a) = \frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N a_n a_m t_n t_m \phi(x_n)^T \phi(x_m) - \sum_{n=1}^N \sum_{m=1}^N a_n a_m t_n t_m \phi(x_n)^T \phi(x_m) + \sum_{n=1}^N a_n$$

Combining the double summation terms yields the Wolfe Dual Lagrangian:

$$\tilde{L}(a) = \sum_{n=1}^N a_n - \frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N a_n a_m t_n t_m k(x_n, x_m)$$

where $k(x_n, x_m) = \phi(x_n)^T \phi(x_m)$ is the kernel function.
The dual optimization problem requires maximizing this function with respect to $a$:

$$\max_{a} \tilde{L}(a)$$

$$\text{subject to } a_n \ge 0, \quad n = 1, \dots, N$$

$$\text{and } \sum_{n=1}^N a_n t_n = 0$$

> [!IMPORTANT]
> The dual representation is formulated entirely in terms of the kernel function $k(x_n, x_m)$. We do not need to explicitly evaluate the high-dimensional feature vectors $\phi(x)$, allowing us to operate in infinite-dimensional spaces using the kernel trick.





---

# 10.3 Support Vector Machines (SVM)

### 10.3.1 Support Vector Machine

#### 1. Dual Prediction Function

Once we solve the dual quadratic programming problem to find the optimal Lagrange multipliers $a^\star = (a_1^\star, \dots, a_N^\star)^T$, we can substitute the weight vector relation $w^\star = \sum_{n=1}^N a_n^\star t_n \phi(x_n)$ into our linear model.
The prediction for a new test point $x$ is computed as:

$$y(x) = w^{\star T} \phi(x) + b^\star = \sum_{n=1}^N a_n^\star t_n k(x, x_n) + b^\star$$

#### 2. KKT Complementary Slackness and Sparsity

The optimal solution must satisfy the KKT complementary slackness conditions:

$$a_n^\star \ge 0$$

$$t_n y(x_n) - 1 \ge 0$$

$$a_n^\star \{ t_n y(x_n) - 1 \} = 0$$

This implies that for every training pattern $n$:

* **Non-Support Vectors:** If the point lies strictly outside the margin boundary ($t_n y(x_n) > 1$), then its constraint is inactive, which forces its Lagrange multiplier to be exactly zero ($a_n^\star = 0$). These points do not contribute to the dual prediction model.
* **Support Vectors:** If the Lagrange multiplier is strictly positive ($a_n^\star > 0$), then the constraint must be active, meaning the point lies exactly on the margin boundary:

$$t_n y(x_n) = 1 \implies t_n (w^{\star T} \phi(x_n) + b^\star) = 1$$

These critical points are called Support Vectors. Because most training patterns lie outside the margin boundary in well-behaved problems, their multipliers are zero. The final decision boundary is determined strictly by a sparse subset of support vectors, making the model highly computationally efficient during inference.


#### 3. Solving for the Bias Parameter $b^\star$

To find the optimal bias parameter $b^\star$, we select any support vector $x_n \in S$ (for which $a_n^\star > 0$) and satisfy its active boundary equation:

$$t_n \left( \sum_{m \in S} a_m^\star t_m k(x_n, x_m) + b^\star \right) = 1$$

Multiplying both sides by $t_n$ (and noting that $t_n^2 = 1$ since $t_n \in \{-1, +1\}$):

$$\sum_{m \in S} a_m^\star t_m k(x_n, x_m) + b^\star = t_n \implies b^\star = t_n - \sum_{m \in S} a_m^\star t_m k(x_n, x_m)$$

To ensure numerical stability and minimize round-off errors, we average this relation over the entire set $S$ containing $N_S$ support vectors:

$$b^\star = \frac{1}{N_S} \sum_{n \in S} \left( t_n - \sum_{m \in S} a_m^\star t_m k(x_n, x_m) \right)$$

### 10.3.2 Soft Margin SVM

To handle non-separable datasets where class distributions overlap in feature space, we relax the strict margin constraints by introducing non-negative slack variables $\xi_n \ge 0$ for each training observation.

#### 1. Slack Variables and Physical Meanings

The relaxed constraints are formulated as:

$$t_n (w^T \phi(x_n) + b) \ge 1 - \xi_n, \quad \xi_n \ge 0, \quad n = 1, \dots, N$$

The slack variable $\xi_n$ measures the deviation of the point $x_n$ from its ideal margin boundary:

* $\xi_n = 0$: The point lies on or outside the correct margin boundary.
* $0 < \xi_n \le 1$: The point lies inside the margin boundary but on the correct side of the decision boundary.
* $\xi_n > 1$: The point lies on the incorrect side of the decision boundary (misclassified).


<img width="1187" height="501" alt="image" src="https://github.com/user-attachments/assets/29cc0aa8-3439-4be6-94ac-54ebdfd9d603" />

#### 2. Primal Optimization Problem (Soft Margin)

We introduce a regularization parameter $C > 0$ that controls the trade-off between maximizing the margin and minimizing the slack penalty:

$$\min_{w, b, \xi} \frac{1}{2} \Vert{}w\Vert{}^2 + C \sum_{n=1}^N \xi_n$$

$$\text{subject to } t_n (w^T \phi(x_n) + b) \ge 1 - \xi_n \quad \text{and} \quad \xi_n \ge 0, \quad n = 1, \dots, N$$

#### 3. The Primal Lagrangian and Box Constraints

We introduce two sets of non-negative Lagrange multipliers, $a_n \ge 0$ and $\mu_n \ge 0$, to construct the Primal Lagrangian:

$$L(w, b, \xi, a, \mu) = \frac{1}{2} \Vert{}w\Vert{}^2 + C \sum_{n=1}^N \xi_n - \sum_{n=1}^N a_n \{ t_n (w^T \phi(x_n) + b) - 1 + \xi_n \} - \sum_{n=1}^N \mu_n \xi_n$$

We compute the partial derivatives of $L$ with respect to the primal variables $w, b, \xi_n$ and set them to zero:

$$\frac{\partial L}{\partial w} = 0 \implies w = \sum_{n=1}^N a_n t_n \phi(x_n)$$

$$\frac{\partial L}{\partial b} = 0 \implies \sum_{n=1}^N a_n t_n = 0$$

$$\frac{\partial L}{\partial \xi_n} = 0 \implies C - a_n - \mu_n = 0 \implies a_n + \mu_n = C$$

Since both $a_n \ge 0$ and $\mu_n \ge 0$, the relation $a_n + \mu_n = C$ forces the multipliers $a_n$ to satisfy the box constraints:

$$0 \le a_n \le C$$

#### 4. Dual Formulation (Soft Margin)

Substituting these stationary conditions back into the Primal Lagrangian eliminates the primal variables, yielding the exact same dual objective function as the separable case:

$$\tilde{L}(a) = \sum_{n=1}^N a_n - \frac{1}{2} \sum_{n=1}^N \sum_{m=1}^N a_n a_m t_n t_m k(x_n, x_m)$$

The only difference is that the Lagrange multipliers are now constrained by the upper bound $C$:

$$\max_{a} \tilde{L}(a)$$

$$\text{subject to } 0 \le a_n \le C, \quad n = 1, \dots, N$$

$$\text{and } \sum_{n=1}^N a_n t_n = 0$$

#### 5. KKT Conditions and Physical Interpretation

The optimal solution must satisfy the following joint set of KKT conditions:

$$a_n \ge 0$$

$$t_n y(x_n) - 1 + \xi_n \ge 0$$

$$a_n \{ t_n y(x_n) - 1 + \xi_n \} = 0$$

$$\mu_n \ge 0$$

$$\xi_n \ge 0$$

$$\mu_n \xi_n = 0 \implies (C - a_n) \xi_n = 0$$

Using these relations, we can classify the physical states of any training point $x_n$ based on its optimal multiplier $a_n$:

| Multiplier Value | Slack Variable | Point Location | State Description |
| --- | --- | --- | --- |
| $a_n = 0$ | $\xi_n = 0$ | Outside the margin boundary | **Non-Support Vector:** The constraint is inactive; the point has no influence on the decision boundary. |
| $0 < a_n < C$ | $\xi_n = 0$ | Exactly on the margin boundary | **Margin-Bound Support Vector:** Since $a_n < C \implies \mu_n > 0 \implies \xi_n = 0$. These points are used to compute the bias parameter $b^\star$. |
| $a_n = C$ | $\xi_n > 0$ | Inside the margin boundary | **Non-Bound Support Vector:** Since $a_n = C \implies \mu_n = 0 \implies \xi_n \ge 0$. Points can lie on the correct side of the decision boundary ($0 < \xi_n \le 1$) or be misclassified ($\xi_n > 1$). |

#### 6. The Hinge Loss Perspective

We can rewrite the soft-margin optimization problem in an unconstrained form using a loss function called the Hinge Loss. The primal objective is equivalent to minimizing:

$$\sum_{n=1}^N E_{HP}(y(x_n) t_n) + \lambda \Vert{}w\Vert{}^2$$

where the hinge loss function is defined as:

$$E_{HP}(u) = [1 - u]_+ = \max(0, 1 - u)$$

and the regularization parameter is related by $\lambda = 1 / (2C)$.


<img width="1180" height="486" alt="image" src="https://github.com/user-attachments/assets/97cdf706-9a08-4c21-8b91-a54c5e913abf" />

This view illustrates the difference between SVMs and logistic regression:

* **Logistic Regression:** The cross-entropy loss $\ln(1 + e^{-z})$ has a non-zero gradient everywhere, meaning that every training point in the dataset exerts some influence on the final decision boundary.
* **Support Vector Machines:** The Hinge Loss is exactly zero for any point on the correct side of the margin ($z \ge 1$). This flat region is the mathematical source of the model's sparsity, ensuring the decision boundary depends only on the support vectors.
