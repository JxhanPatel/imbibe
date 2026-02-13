### 1. Foundational Sets, Logic, and Sequences
Before diving into calculus, it is necessary to establish the language of sets and sequences used to define continuity and limits.

#### **Common Math Sets**
| Symbol | Meaning |
| :--- | :--- |
| $\mathbb{R}$ | The set of all **Real Numbers**. |
| $\mathbb{R}^{+}$ | **Positive Real Numbers**, including 0. |
| $\mathbb{Z} / \mathbb{Z}^{+}$ | Integers / Positive integers including 0. |
| $[a, b]$ | **Closed Interval**: Includes boundary points $a$ and $b$. |
| $(a, b)$ | **Open Interval**: Boundary points are excluded. |
| $\mathbb{R}^d$ | **D-dimensional vectors** of real numbers. |

#### **Logic Symbols**
*   **$\forall$**: For all.
*   **$\exists$**: There exists.
*   **$\implies$**: Implies.
*   **$\iff$**: Equivalent to / If and only if.

### Intervals and Cartesian Products
Sets of $d$-dimensional vectors are constructed using the Cartesian product of real sets:
*   **$\mathbb{R}^d$**: The space of all column vectors with $d$ real components.
*   **Closed Interval $[a, b]$**: $\{x \in \mathbb{R} : a \le x \le b\}$.
*   **Open Interval $(a, b)$**: $\{x \in \mathbb{R} : a < x < b\}$.
*   **Higher Dimensional Intervals**: $[a, b]^d = \{\mathbf{x} \in \mathbb{R}^d : x_i \in [a, b], \forall i \in \{1, ..., d\}\}$.

> [!NOTE]
> In ML, the input $\mathbf{x}$ is typically a tuple in $\mathbb{R}^d$, where each dimension represents a specific feature (e.g., area, number of rooms).


#### **Convergence and Limits**
A sequence $x_i$ converges to a point x* if for every radius $\epsilon > 0$, there is an integer $N$ such that all subsequent elements in the sequence ($n \ge N$) stay within a **ball** $B(x^*, \epsilon)$.
*   **Metric Space Distance (Euclidean):** $D(x, y) = \|x - y\| = \sqrt{\sum (x_i - y_i)^2}$.
*   
#### Defining Local Regions (Balls)
*   **Open Ball** $B(\mathbf{x}, \epsilon)$: The set $\{\mathbf{y} \in \mathbb{R}^d : D(\mathbf{x}, \mathbf{y}) < \epsilon\}$.
*   **Closed Ball** $\bar{B}(\mathbf{x}, \epsilon)$: The set $\{\mathbf{y} \in \mathbb{R}^d : D(\mathbf{x}, \mathbf{y}) \le \epsilon\}$.

---

### 2. Univariate Calculus (Functions of One Variable)
Univariate calculus focuses on real-valued functions $f: \mathbb{R} \to \mathbb{R}$.

#### **Continuity and Differentiability**
*   **Continuity:** A function is continuous at x* if for any sequence $x_i$ converging to $x^* $, the values $f(x_i)$ converge to $f(x^*)$.
*   **Differentiability:** A function is differentiable if the limit $lim_{x \to x^* } \frac{f(x) - f(x^*)}{x - x^ * }$ exists.
*   **Key Relationship:** If a function is differentiable at a point, it is **guaranteed to be continuous** there; however, a continuous function (like $f(x) = |x|$) is not necessarily differentiable.

#### **Linear Approximation (The Tangent Line)**
The derivative $f'(a)$ represents the **slope of the tangent line** at point $a$. Near $a$, the function can be approximated as:
$$f(x) \approx f(a) + f'(a)(x - a)$$.

<img width="440" height="250" alt="image" src="https://github.com/user-attachments/assets/fe8a1b4c-c5fd-4a78-92e0-d782df0cb007" />
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/a7eca5e0-c892-408f-85e0-a40aad90c111" />

> **Visual 1: The Tangent Approximation**
> *Description:* A black curve represents a function $f(x) = x^2$. At the point $(1, 1)$, a red straight line touches the curve. This red line (the linear approximation $2x - 1$) stays very close to the curve near $x=1$ but diverges as $x$ moves further away.

---

### 3. Multivariate Calculus (Functions of Multiple Variables)
Machine learning models often deal with $d$-dimensional inputs ($f: \mathbb{R}^d \to \mathbb{R}$).

#### **Geometry of High-Dimensional Space**
*   **Lines:** A line through point $u$ along direction $v$ is the set $\{x : x = u + \alpha v\}$.
*   **Hyperplanes:** A plane normal to vector $w$ with offset $b$ is the set $\{x : w^T x = b\}$.

#### **The Gradient ($\nabla f$)**
The gradient is a vector that packages all **partial derivatives** $(\frac{\partial f}{\partial x_i})$ of a function.
$$\nabla f(x, y) = \left[ \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y} \right]$$.

**Key Properties of the Gradient:**
1.  **Steepest Ascent:** $\nabla f$ points in the direction that increases the function value the fastest.
2.  **Orthogonality:** The gradient at a point is **perpendicular to the contour line** (level set) passing through that point.

#### **Multivariate Linear Approximation**
In the neighborhood of a point $(a, b)$, a function of two variables can be approximated linearly as:
$$L(x, y) = f(a, b) + \frac{\partial f}{\partial x}(a, b)(x - a) + \frac{\partial f}{\partial y}(a, b)(y - b)$$.

---

### 4. Advanced Rules and Optimization
#### **Product and Chain Rules**
These rules can be derived through the lens of linear approximations.
*   **Chain Rule:** If $f(x) = g(h(x))$, then $f'(x) = g'(h(x)) \cdot h'(x)$.

#### **Directional Derivatives**
This measures the rate of change of a function at point $v$ along a **unit vector** direction $u$.
$$\text{Directional Derivative} = \nabla f(v)^T u$$.

#### **Critical Points and Saddle Points**
A point $v$ is a **critical point** if the gradient $\nabla f(v)$ is the zero vector.
| Point Type | Description |
| :--- | :--- |
| **Local Minimum** | The function value is lower than all nearby points. |
| **Local Maximum** | The function value is higher than all nearby points. |
| **Saddle Point** | A point that is a maximum along one direction but a minimum along another. |

<img width="800" height="325" alt="image" src="https://github.com/user-attachments/assets/6a00ee2f-010e-42f2-8d97-6b702c16ec92" />


> **Visual 2: Contour Plots and Gradients**
> *Description:* A 2D plot shows concentric circles (contours of constant function values). A black arrow originating from a point on a circle points directly away from the center, crossing the circle at a 90-degree angle. This arrow represents the gradient, showing the direction of steepest ascent.
