## 1. Sets and Functions
Before diving into calculus, it is necessary to establish the language of sets and sequences used to define continuity and limits.

### **1.1 Common Math Sets**
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

#### Intervals and Cartesian Products
Sets of $d$-dimensional vectors are constructed using the Cartesian product of real sets:
*   **$\mathbb{R}^d$**: The space of all column vectors with $d$ real components.
*   **Closed Interval $[a, b]$**: $\{x \in \mathbb{R} : a \le x \le b\}$.
*   **Open Interval $(a, b)$**: $\{x \in \mathbb{R} : a < x < b\}$.
*   **Higher Dimensional Intervals**: $[a, b]^d = \{\mathbf{x} \in \mathbb{R}^d : x_i \in [a, b], \forall i \in \{1, ..., d\}\}$.

> [!NOTE]
> In ML, the input $\mathbf{x}$ is typically a tuple in $\mathbb{R}^d$, where each dimension represents a specific feature (e.g., area, number of rooms).




### 1.2 Metric Spaces and Topology
#### Distance Functions and Balls
* A metric space is a set $X$ paired with a distance function $D: X \times X \to \mathbb{R}$.
* In these foundations, we primarily use $\mathbb{R}^d$ with the Euclidean distance.

> [!IMPORTANT]
> **Euclidean Distance (L2 Norm)**
> For vectors $\mathbf{x}, \mathbf{y} \in \mathbb{R}^d$:
> $$D(\mathbf{x}, \mathbf{y}) = \|\mathbf{x} - \mathbf{y}\| = \sqrt{\sum_{i=1}^{d} (x_i - y_i)^2}$$

* **Open Ball**: $B(\mathbf{x}, \epsilon) = \{\mathbf{y} \in \mathbb{R}^d : D(\mathbf{x}, \mathbf{y}) < \epsilon\}$
* **Closed Ball**: $\overline{B}(\mathbf{x}, \epsilon) = \{\mathbf{y} \in \mathbb{R}^d : D(\mathbf{x}, \mathbf{y}) \le \epsilon\}$

<img width="191" height="190" alt="image" src="https://github.com/user-attachments/assets/7d7ba5a9-da02-4205-838a-2af92f449f7d" />




### 1.3 Set Logic and Sequences
#### Set Operations and De Morgan's Laws
* Let $V$ be the universal set. For any set $A \subseteq V$, the complement is $A^c = \{x \in V : x \notin A\}$.

> [!NOTE]
> **De Morgan's Laws**
> 1. $(A \cup B)^c = A^c \cap B^c$
> 2. $(A \cap B)^c = A^c \cup B^c$

Sets exist within a defined Universe ($V$). The following example illustrates set operations using a universe $V = [0, 10]$ and two sets, $A = [2, 5]$ and $B = [4, 7]$:
1. Union ($A \cup B$): The set of all elements in either $A$ or $B$.
- Example: $[2, 5] \cup [4, 7] = [2, 7]$.
2. Intersection ($A \cap B$): The set of elements common to both $A$ and $B$.
- Example: $[2, 5] \cap [4, 7] = [4, 5]$.
3. Complement ($A^c$ or $V \setminus A$): All elements in the universe not in set $A$.
- Example: $A^c = [0, 10] \setminus [2, 5] = [0, 2) \cup (5, 10]$.



#### Sequences 
* A **sequence** is an ordered list of elements (vectors) in a space $\mathbb{R}^d$, typically indexed by natural numbers $n \in \mathbb{Z}_+$.
* **Notation**: $\{\mathbf{x}n\}{n=1}^{\infty}$ or simply $\mathbf{x}_1, \mathbf{x}_2, \mathbf{x}_3, \dots$
* In Machine Learning, sequences often represent the iterative updates of model parameters (weights) during the optimization process (e.g., Gradient Descent steps).

#### Sequence Convergence
##### Formal Definition of a Limit
A sequence $\{\mathbf{x}_n\}$ is said to **converge** to a limit $\mathbf{x}^★ \in \mathbb{R}^d$ if the elements of the sequence become arbitrarily close to $\mathbf{x}^★$ as $n$ increases toward infinity.

#### Mathematical Derivations / Proof Steps
To prove that $\lim_{n \to \infty} \mathbf{x}_n = \mathbf{x}^★$, the following $\epsilon-N$ condition must be satisfied:
1. Fix an arbitrary precision $\epsilon > 0$.
2. Define an open ball $B(\mathbf{x}^★, \epsilon) = \{\mathbf{y} \in \mathbb{R}^d : D(\mathbf{y}, \mathbf{x}^★) < \epsilon\}$.
3. Identify a threshold index $N \in \mathbb{Z}_+$ such that for every index $n$ greater than or equal to $N$, the sequence element $\mathbf{x}_n$ lies within that ball.

$$\forall \epsilon > 0, \exists N \in \mathbb{Z}_+ \text{ s.t. } \forall n \ge N, \|\mathbf{x}_n - \mathbf{x}^★\| < \epsilon$$

> [!IMPORTANT]
> **Core Theorem of Convergence**
> A sequence converges if and only if for every $\epsilon > 0$, there exists a point $N$ in the sequence after which all subsequent terms are "trapped" within the $\epsilon$-radius of the limit $\mathbf{x}^★$.

<img width="358" height="316" alt="image" src="https://github.com/user-attachments/assets/142322f8-e11d-4680-a81d-c421bef2d6aa" />
Analysis Note:
The visual proves that convergence is a "tail property." The initial values of the sequence do not affect the existence of a limit; only the behavior as $n \to \infty$ determines if the sequence stays within the specified distance $\epsilon$.

### Behavior of Sequences
#### Convergence vs. Divergence
| Behavior | Mathematical Condition | Example in $\mathbb{R}^2$ |
| :--- | :--- | :--- |
| **Convergent** | $\lim_{n \to \infty} \mathbf{x}_n = \mathbf{x}^★$ | $\mathbf{x}_n = (1 + \frac{4}{2^n}, 3 - \frac{4}{2^n}) \to (1, 3)$ |
| **Divergent (Trend)** | $\|\mathbf{x}_n\| \to \infty$ | $\mathbf{x}_n = (n, n^2)$ |
| **Divergent (Oscillatory)** | No single $\mathbf{x}^★$ exists | $\mathbf{x}_n = (\cos(\frac{\pi}{2}n), \sin(\frac{\pi}{2}n))$ |

> [!NOTE]
> **Intuition behind Oscillation**
> In the oscillatory example above, the sequence jumps between $(0,1)$, $(-1,0)$, $(0,-1)$, and $(1,0)$. Because it never "settles" into a single $\epsilon$-ball, it is divergent even though it is bounded.



### 1.4 Vector Spaces

A **Vector Space** ($V$) is a mathematical structure consisting of a set of elements (vectors) that are closed under two main operations: vector addition and scalar multiplication.

#### **Core Properties and Operations**
* **Elements**: If $V$ is a vector space, then $u, v \in V$ are vectors.
* **Scalars**: Typically, we use real numbers ($\alpha, \beta \in \mathbb{R}$) as scalars.
* **Closure under Linear Combination**: For any vectors $u, v \in V$ and scalars $\alpha, \beta \in \mathbb{R}$, the linear combination must also be in the space:
    $$\alpha u + \beta v \in V$$
* **Zero Vector**: A vector space must contain a zero vector, denoted as $\theta \in V$.



#### **Standard Example: $\mathbb{R}^d$**
The set of $d$-dimensional real vectors ($\mathbb{R}^d$) is the most common example of a vector space used in this course. It is defined as the Cartesian product of $\mathbb{R}$ with itself $d$ times:
$$\mathbb{R}^d = \mathbb{R} \times \mathbb{R} \times \dots \times \mathbb{R}$$

#### **Inner Products and Norms**
Vector spaces often include additional structures to measure size and direction:
* **Dot Product (Inner Product)**: Defined for two vectors $x, y \in \mathbb{R}^d$ as:
    $$x \cdot y = x^T y = \sum_{i=1}^{d} x_i y_i$$
* **Norm (Length)**: The squared norm of a vector $x$ is the dot product of the vector with itself:
    $$\|x\|^2 = x \cdot x = x^T x = \sum_{i=1}^{d} x_i^2$$
* **Orthogonality**: Two vectors $x$ and $y$ are considered **perpendicular** or **orthogonal** if their dot product is zero:
    $$x \cdot y = x^T y = 0$$





### 1.5 Functions and Graphs

A function is a mapping between two sets that assigns each element of the first set to exactly one element of the second set.

#### **1. Definitions and Notation**
* **Function Notation**: Represented as $f: A \rightarrow B$, where $A$ is the domain and $B$ is the co-domain.
* **Domain ($A$)**: The set of all possible input values.
* **Co-domain ($B$)**: The set representing the potential output values.
* **Graph of a Function ($G_f$)**: The set of all pairs $(x, f(x))$ where $x$ is in the domain.
    * Mathematically: $G_f = \{(x, f(x)) : x \in \mathbb{R}^d\} \subseteq \mathbb{R}^{d+1}$

#### **2. Types of Functions by Dimension**
* **1-Dimensional Function**: A function $f: \mathbb{R} \rightarrow \mathbb{R}$.
    * *Example*: $f(x) = x^2$.
    * The graph $G_f$ is a curve in a 2D plane ($\mathbb{R}^2$).
  
 <img width="847" height="469" alt="image" src="https://github.com/user-attachments/assets/2594eaac-7ab8-420c-a00b-d0fa0b1e1b77" />



* **d-Dimensional Function**: A function $f: \mathbb{R}^d \rightarrow \mathbb{R}$.
    * These functions map a $d$-dimensional vector input to a single real number output.
    * For $d=2$, the graph exists in 3D space ($\mathbb{R}^3$).



#### **3. Visualizing 2-Dimensional Functions ($f: \mathbb{R}^2 \rightarrow \mathbb{R}$)**
Visualizing 3D surfaces on 2D screens can be difficult, so two primary methods are used:

* **Contour Plots**:
    * These represent a function by drawing lines (isohypses/contours) where the function value is constant: $f(x) = c$.
    * *Example*: For $f(x_1, x_2) = x_1^2 + x_2^2$, the contours are concentric circles.
 <img width="903" height="493" alt="image" src="https://github.com/user-attachments/assets/223c7f32-9746-4106-ad3b-f1f1f46d7af8" />



* **Heat Maps**:
    * A heat map uses a color gradient to represent the value of the function at every point.
    * While a heat map technically contains "infinite" contours and more data, a contour map is often clearer for identifying the specific geometric shape of the function's levels.
 
<img width="916" height="556" alt="image" src="https://github.com/user-attachments/assets/ee06fe3a-69e9-4343-9416-1fddc1d08ce3" />

<img width="914" height="596" alt="image" src="https://github.com/user-attachments/assets/f7ee6323-aaf7-4bb7-9aa2-7256c57e05d2" />


---

# Univariate Calculus
## 2. Continuity and Differentiability

### 2.1 Continuity of functions

#### **Theory of Continuity**

A function $f: \mathbb{R} \to \mathbb{R}$ is considered **continuous at a point** $x^★$ if the following three conditions are met:

1.  The function $f(x^★)$ is defined at that point.
2.  The limit of the function as $x$ approaches $x^★$ exists: $\lim_{x \to x^★} f(x) = L$.
3.  The value of the limit is exactly equal to the function's value at that point: $\lim_{x \to x^★} f(x) = f(x^★)$.

**Visual Interpretation:**
Intuitively, a function is continuous if you can draw its graph without lifting your pen from the paper. If there is a "jump," a "hole," or a "vertical asymptote" at $x^★$, the function is discontinuous at that point.



#### **Formal Definition (Limit-based)**

A function $f$ is continuous at $x^★$ if for every sequence $x_n$ that converges to $x^★$, the sequence of function values $f(x_n)$ converges to $f(x^★)$. 

$$\lim_{n \to \infty} x_n = x^★ \implies \lim_{n \to \infty} f(x_n) = f(x^★)$$

---

#### **Examples of Continuity and Discontinuity**

**1. Example of a Continuous Function:**
* **Function:** $f(x) = x^2$
* **Analysis at $x^★ = 1$:**
    * $f(1) = 1^2 = 1$ (Defined)
    * As $x$ gets closer and closer to 1 (e.g., 0.9, 0.99, 1.01), $x^2$ gets closer and closer to 1.
    * Since $\lim_{x \to 1} x^2 = 1 = f(1)$, the function is continuous at $x = 1$.



**2. Example of Discontinuity (The "Jump"):**
* **Function:** $f(x) = \text{sign}(x)$ where:
    * $f(x) = 1$ if $x > 0$
    * $f(x) = -1$ if $x < 0$
    * $f(x) = 0$ if $x = 0$
* **Analysis at $x^★ = 0$:**
    * If we approach 0 from the positive side ($x = 0.1, 0.01$), the limit is 1.
    * If we approach 0 from the negative side ($x = -0.1, -0.01$), the limit is -1.
    * Because the limit from the left does not equal the limit from the right, the limit does not exist at $x=0$.
    * **Conclusion:** The function is discontinuous at $x = 0$.



**3. Example of Discontinuity (The "Hole"):**
* **Function:** $f(x) = \frac{x^2 - 1}{x - 1}$
* **Analysis at $x^★ = 1$:**
    * At $x = 1$, the function is $\frac{0}{0}$, which is undefined.
    * However, for $x \neq 1$, $f(x) = \frac{(x-1)(x+1)}{x-1} = x + 1$.
    * $\lim_{x \to 1} f(x) = 2$, but since $f(1)$ is not defined, the function is discontinuous at $x = 1$.



**4. Example of Discontinuity (The "Removable" vs "Essential"):**
* If we redefine the "Hole" example as $f(x) = x + 1$ for $x \neq 1$ and $f(1) = 3$:
    * The limit $\lim_{x \to 1} f(x) = 2$.
    * The value $f(1) = 3$.
    * Since $2 \neq 3$, the function is still discontinuous.



#### **Types of Discontinuity Summary**
* **Point Discontinuity:** A single point is missing or displaced (a "hole").
* **Jump Discontinuity:** The function "jumps" from one value to another at a specific point.
* **Infinite Discontinuity:** The function goes to infinity (e.g., $1/x$ at $x=0$).

---

### 2.2: Differentiability of Functions


#### 1. Definition of Differentiability
A function $f: \mathbb{R} \to \mathbb{R}$ is differentiable at a point $x^★$ in reals if the following limit exists:

$$\lim_{x \to x^★} \frac{f(x) - f(x^★)}{x - x^★}$$

When we say the limit exists, it means that it converges to something. If it does exist, that particular limit is called the **derivative of $f$ at $x^★$**, which is denoted by $f'(x^★)$.

#### 2. Relationship with Continuity
* If a function $f$ is not continuous at $x^★$, it is not differentiable at $x^★$.
* However, the converse is not necessarily true: You can have a function which is **not differentiable but still continuous**.

#### 3. Alternative Expression for the Derivative
An alternative expression for the derivative, which is easier to interpret, is:

$$f'(x^★) = \lim_{h \to 0} \frac{f(x^★ + h) - f(x^★)}{h}$$

These two expressions are exactly the same.

---

#### 4. Geometric Interpretation (Slope)
The slope of a function at a point is simply given by $f'(x^★)$. 



<img width="640" height="361" alt="image" src="https://github.com/user-attachments/assets/2484ae02-f92e-4201-b6e3-545f5e8e1b2c" />
<img width="558" height="363" alt="image" src="https://github.com/user-attachments/assets/8e01b0a8-ad0f-48ce-b646-9159ed935bd6" />

> **Analysis Note:**
> As $h$ is reduced (e.g., from 1 to $1/2$ to $1/4$), the blue line segment joining $(x^★, f(x^★))$ and $(x^★+h, f(x^★+h))$ becomes a closer approximation of the black curve. When $h$ is practically zero, the slope of the resulting red line—representing the ratio of the vertical length to the horizontal length—corresponds exactly to the derivative $f'(x^★)$.

---

#### 5. Examples of Differentiability and Non-Differentiability

**Example 1: The Absolute Value Function**
$f(x) = |x|$ from $\mathbb{R}$ to $\mathbb{R}$. To show this is not differentiable at $x^★ = 0$, consider two sequences:

1.  **Sequence 1:** $x_i = \{1, 1/2, 1/4, \dots\}$ which converges to $0$.
    * $\frac{f(x_i) - f(0)}{x_i - 0} = \frac{x_i}{x_i} = 1$. This sequence converges to $1$.
2.  **Sequence 2:** $x_i = \{-1, -1/2, -1/4, \dots\}$ which also converges to $0$.
    * $\frac{f(x_i) - f(0)}{x_i - 0} = \frac{1 - 0}{-1} = -1$. This sequence converges to $-1$.

Since two sequences converging to $x^★$ yield different values for the limit, the limit does not exist. Therefore, $f(x) = |x|$ is **not differentiable at $x^★ = 0$**.

**Example 2: Piecewise Discontinuous Function**

$$f(x) = \begin{cases} 4x + 2 & \text{if } x \ge 2 \\ 2x + 8 & \text{if } x < 2 \end{cases}$$

* Approaching from the left: $\lim_{x \to 2^-} f(x) = 12$.
* Approaching from the right: $\lim_{x \to 2^+} f(x) = 10$.

Since the function is not continuous at $x^★ = 2$, it is **clearly not differentiable at $x^★ = 2$**.

**Example 3: Piecewise Continuous but Non-Differentiable Function**

$$f(x) = \begin{cases} 4x + 2 & \text{if } x \ge 2 \\ 2x + 6 & \text{if } x < 2 \end{cases}$$

This function is continuous at $x^★ = 2$, but we check the limit:
* From the positive side ($x \to 2^+$): $\frac{f(x) - f(2)}{x - 2} = 4$.
* From the negative side ($x \to 2^-$): $\frac{f(x) - f(2)}{x - 2} = 2$.

Because the limits from the two sequences are different ($4$ and $2$), the limit does not exist. Thus, the function is **not differentiable at $x^★ = 2$**.



---


## 3.1: Derivative and Linear Approximation

### 1. Definition and Formula
For a univariate function $f: \mathbb{R} \rightarrow \mathbb{R}$, the linear approximation of the function $f$ around a point $x$ close to $x^\star$ is given by:

$$f(x) \approx f(x^\star) + f'(x^\star)(x - x^\star)$$

This expression is denoted as the linear approximation of $f$ around $x^\star$ evaluated at $x$, often written as:
$$L_{x^\star}f(x)$$

> [!IMPORTANT]
> This is a valid approximation only when $x$ is close to $x^\star$.

---

### 2. Geometric Interpretation: Tangent Lines
The linear approximation $L_{x^\star}f$ is geometrically represented as a **tangent line** to the graph of $f$ (denoted as $G_f$) at the point $(x^\star, f(x^\star))$.

<img width="325" height="352" alt="image" src="https://github.com/user-attachments/assets/99b00aeb-10e5-4ea2-95b3-60ca03f8488e" />



---

### 3. Step-by-Step Examples

#### **Example 1: Function $f(x) = x^2$ around $x^\star = 1$**
1.  **Identify the function and point**: $f(x) = x^2$ and $x^\star = 1$.
2.  **Compute the derivative**: $f'(x) = 2x$.
3.  **Evaluate at the point**:
    * $f(x^\star) = f(1) = 1^2 = 1$.
    * $f'(x^\star) = f'(1) = 2(1) = 2$.
4.  **Apply the formula**:
    $$f(x) \approx 1 + 2(x - 1)$$
    $$f(x) \approx 1 + 2x - 2$$
    $$f(x) \approx 2x - 1 \quad (\text{around } x = 1)$$

#### **Example 2: Trigonometric Function $\sin(x)$ around $x^\star = 0$**
1.  **Function and point**: $f(x) = \sin(x)$ around $x^\star = 0$.
2.  **Derivatives and evaluations**:
    * $f'(x) = \cos(x)$.
    * $f'(x^\star) = \cos(0) = 1$.
    * $f(x^\star) = \sin(0) = 0$.
3.  **Linear Approximation**:
    $$f(x) \approx f(x^\star) + f'(x^\star)(x - x^\star)$$
    $$f(x) \approx 0 + 1(x - 0)$$
    $$f(x) \approx x$$

#### **Example 3: Exponential Function $e^x$ around $x^\star = 0$**
1.  **Function and point**: $f(x) = e^x$ around $x^\star = 0$.
2.  **Linear Approximation**:
    $$e^x \approx e^0 + (x - 0) \cdot 1$$
    $$e^x \approx 1 + x \quad (\text{around } x = 0)$$

#### **Example 4: Numerical Approximation of $e^{0.017}$**
**Problem**: Compute the approximate value of $e^{0.017}$.
1.  **Setup**: The closest known value is $e^0$, so set $f(x) = e^x$ and $a = 0$.
2.  **Values**: $f(0) = 1$ and $f'(0) = 1$.
3.  **Linear Model**: $L(x) = f(0) + f'(0)(x - 0) = 1 + x$.
4.  **Calculation**:
    $$L(0.017) = 1 + 0.017 = 1.017$$
   
*Note: The actual value of $e^{0.017}$ is approximately 1.017.*

---

### 4. Summary Table of Linear Approximations around $x^\star = 0$

| Function $f(x)$ | Approximation $L_0 f(x)$ |
| :--- | :--- |
| $\sin(x)$ | $x$ |
| $e^x$ | $1+x$ |
| $\ln(1+x)$ | $x$ |

---

### 5. Transition to Multivariate Case
The univariate linear approximation serves as the foundation for multivariate calculus. By viewing a multidimensional function as a collection of multiple one-dimensional functions (holding other variables constant), we can extend this concept to partial derivatives and gradients.

For a 2D function $f(x_1, x_2)$, the approximation around $v = (v_1, v_2)$ is:
$$f(y_1, y_2) \approx f(v_1, v_2) + \frac{\partial f}{\partial x_1}(v)(y_1 - v_1) + \frac{\partial f}{\partial x_2}(v)(y_2 - v_2)$$

---



## 2.4: Univariate Calculus: Applications and Advanced Rules

Now we will take a look at some applications or advanced rules that we have. We will not go into greater detail but they will be still useful for understanding the basic terms that we have been discussing.

### Why Linear Approximations?

The first thing is why linear approximations? That is the first question that comes to mind. Linear approximations are fine, but what is so special about them? We have been approximating $f(x)$ by the linear approximation of $f$ around some point $x^{\star}$.

$$f(x) \approx f(x^{\star}) + f'(x^{\star})(x - x^{\star})$$

This is what you have been doing, but you could argue that there is nothing special about linear functions and you can go higher. In fact, you could do that, and that is the fundamental idea behind higher order approximations.

#### Higher Order Approximations
The linear approximation would simply say that $f(x)$ is approximately equal to $f(x^{\star}) + f'(x^{\star})(x - x^{\star})$. But you could go to higher order approximations and have a **quadratic approximation** instead, which would say:

$$f(x) \approx f(x^{\star}) + f'(x^{\star})(x - x^{\star}) + \frac{1}{2} f''(x^{\star})(x - x^{\star})^2$$

Both of these are valid around $x \approx x^{\star}$. The second expression is a better approximation than the first, but it comes at a cost: it is significantly more complex. It has a quadratic term here as opposed to just a linear term here.

> The first expression ($f(x^{\star}) + f'(x^{\star})(x - x^{\star})$) is the linear approximation.

> The second expression (including the $\frac{1}{2}f''(x^{\star})$ term) is the quadratic approximation.

#### Special Case: Quadratic Functions
For a special class of functions, quadratic function approximations are exactly equal to the function itself. The classic case for that is $f(x) = x^2$.

**Example:**
Let $f(x) = x^2$.
* $f'(x) = 2x$
* $f''(x) = 2$

Pick any $x^{\star}$:
$$x^2 \approx x^{\star 2} + 2x^{\star}(x - x^{\star}) + \frac{1}{2}(2)(x - x^{\star})^2$$
Expanding this, you end up with the approximation being exactly equal to the function because the function itself is quadratic in nature.

#### Example: $e^x$
This is not true for all functions. For example, you could have $e^x$ instead. If you approximated $e^x$ around $x^{\star} = 0$:

$$e^x \approx e^0 + e^0(x - 0) + \frac{1}{2}e^0(x - 0)^2$$
$$e^x \approx 1 + x + \frac{x^2}{2}$$

If you are all familiar with the **Taylor series**, this is exactly what it is. The linear approximation truncates it at the linear term; the quadratic approximation truncates the Taylor series at the quadratic term. You could in principle go up to cubic, quartic, quintic, and so on.

> [!IMPORTANT]
> In most machine learning applications, you will stop at the linear approximation. There are certain situations where you will go to the quadratic approximation, but in no practical scenario will you ever go beyond quadratic approximations.



### Exercise: Compound Interest Logic
Which is closest to $(1.1)^7$?
The four options are: **1.7, 1.9, 2.1, 2.3**

* **Linear Approximation:** $(1.1)^7$ is simply $1 + 7(0.1) = 1.7$. This makes the simple interest logic clear (every year money grows by 10%, so after 7 years you get 1.7).
* **Quadratic Approximation:** For compound interest, simple interest is no longer a good approximation.
  * $f(x) = (1 + x)^7 \implies f(0) = 1$
  * $f'(x) = 7(1 + x)^6 \implies f'(0) = 7$
  * $f''(x) = 42(1 + x)^5 \implies f''(0) = 42$

$$f(0.1) \approx 1 + 7(0.1) + \frac{1}{2}(42)(0.1)^2$$
$$f(0.1) \approx 1 + 0.7 + 21(0.01) = 1.91$$

The second order approximation gives you $1.91$, which is much closer to the truth than the simple first order approximation of $1.7$.

---

### Product Rule via Linear Approximations
Let $f(x) = g(x)h(x)$. We are working with one-dimensional functions $f: \mathbb{R} \to \mathbb{R}$.
To find $f'(x)$, let's pick $x^{\star} = 0$:

Linear approximation of components:
* $g(x) \approx g(0) + x \cdot g'(0)$
* $h(x) \approx h(0) + x \cdot h'(0)$

$$f(x) \approx [g(0) + x \cdot g'(0)][h(0) + x \cdot h'(0)]$$
$$f(x) \approx g(0)h(0) + x[g'(0)h(0) + h'(0)g(0)] + x^2[g'(0)h'(0)]$$

Ignoring the quadratic term for a linear approximation:
$L_x(f) = g(0)h(0) + x[g'(0)h(0) + h'(0)g(0)]$

By matching terms with $f(0) + x \cdot f'(0)$:
$$f'(0) = g'(0)h(0) + h'(0)g(0)$$
This is the **product rule** derived from first principles based on linear approximations.

---

### Chain Rule via Linear Approximations
Let $f(x) = g(h(x))$. We want $f'(0)$.
1. First, approximate $h$ around 0: $h(x) \approx h(0) + h'(0)x$
2. Then, approximate $g$ around $h(0)$:
$$g(h(x)) \approx g(h(0) + h'(0)x)$$
$$g(h(x)) \approx g(h(0)) + g'(h(0))[h(0) + h'(0)x - h(0)]$$
$$f(x) \approx g(h(0)) + g'(h(0))h'(0)x$$

Matching terms with $f(0) + f'(0)x$:
$$f'(0) = g'(h(0))h'(0)$$
This recovers the **chain rule**.

---

### Advanced Examples

**Example 1:** Linear approximation of $f(x) = \frac{e^{3x}}{\sqrt{1+x}}$ around $x = 0$.
* $e^{3x} \approx 1 + 3x$
* $(1 + x)^{-1/2} \approx 1 - \frac{x}{2}$
* Product: $(1 + 3x)(1 - \frac{x}{2}) = 1 + 3x - \frac{x}{2} - \frac{3x^2}{2}$
* Ignore $x^2$: $1 + \frac{5}{2}x$
The derivative at $x=0$ is $5/2$.

**Example 2 (Exercise):** Give linear approximation of $e^{\sqrt{1+x}}$ around $x = 1$.
$$e^{\sqrt{1+x}} \approx e^{\sqrt{2}} + \frac{e^{\sqrt{2}}}{2\sqrt{2}}(x - 1)$$

---

### Maxima, Minima, and Saddle Points

Linear approximations are the key tool that allow you to get around most of machine learning with its very complex functions.

**Critical Points:**
If $f'(x^{\star}) = 0$, then $L_{x^{\star}}f(x) = f(x^{\star})$.
Even though the left hand side is supposed to be a function of $x$, it is a constant. Such points where $f'(x^{\star}) = 0$ are called **critical points**.

In the neighborhood of $x^{\star}$, if the derivative is not 0 (e.g., $7 + 3x$), there is variation. If the derivative is 0, there is no variation in the linear approximation. This happens when $x^{\star}$ is a:
1. **Maxima**
2. **Minima**
3. **Saddle Point**

**Saddle Point:** A point that is essentially a maxima of one part and a minima of another part. The function reaches a slope of $f'(x) = 0$ and then increases (or decreases) again.

<img width="1081" height="744" alt="image" src="https://github.com/user-attachments/assets/d4b975ca-27f3-4bba-abc3-b302c3650be1" />



Machine learning at its core is an applied optimization problem. We are interested in finding the minima or maxima of functions, and so we are really interested in finding points where the gradient or the derivative is 0.



---


## 2.5  Multivariate Calculus: Lines and Planes in Higher Dimensional Space

### 1. Introduction to Multivariate Calculus
Hello everyone and welcome to another lecture of machine learning foundations. We will continue with our calculus recap. We have been seeing about univariate function univariate calculus which is about functions $f$ from $\mathbb{R}$ to $\mathbb{R}$ where the domain and codomain are both $\mathbb{R}$. 

We will generalize most of these to multivariate functions, which are functions from $\mathbb{R}^d$ to $\mathbb{R}$. The codomain is still $\mathbb{R}$; the function is still real-valued, just that the input domain is $d$-dimensional. This brings us lots of geometric ideas for explaining multivariate calculus ideas. Still, we'll be doing mostly differential calculus, and most of these you should have seen in your Mathematics for Data Science 2.

Before we actually get into the calculus, we should first get the basic geometry of high dimensional space, and the basic sets for doing that are lines and planes.

---

### 2. Geometry of Lines
What is a line? We will be working with $\mathbb{R}^d$ now. A line in $\mathbb{R}^d$ is, first of all, a subset of $\mathbb{R}^d$. 

#### Definition 2a: Line Through a Point Along a Direction
A line is parameterized by two vectors. A line through the point $u$ in $\mathbb{R}^d$ along the direction $v$ in $\mathbb{R}^d$ is simply given by the set of all $d$-dimensional vectors such that $x$ can be written as:
$$x = u + \alpha v$$
for $\alpha$ reals ($\alpha \in \mathbb{R}$).

#### Definition 2b: Line Through Two Points
A line through the points $u$ and $u'$ is simply given by the set of all $x \in \mathbb{R}^d$ such that:
$$x = u + \alpha(u' - u) \text{ for } \alpha \in \mathbb{R}$$
Which is also written as the set of all $x \in \mathbb{R}^d$ such that:
$$x = (1 - \alpha)u + \alpha u' \text{ for } \alpha \in \mathbb{R}$$
As you vary $\alpha$, you get different $(1 - \alpha)u + \alpha u'$ and the union of every such element forms the line.

#### Equivalence of Definitions
You can see from this expression that a line through $u$ and $u'$ is exactly the same as the line through $u$ along the direction $u' - u$. Similarly, the following three sets are really the same:
* Line through $u$ and $u'$
* Line through $u$ along $u' - u$
* Line through $u'$ along $u - u'$

All of these are the same subset of $\mathbb{R}^d$. This is a simple exercise to prove. You can just view $u' - u$ as a vector indicating direction; if you do that, it's exactly the same as the first definition.

#### Example: Line in $\mathbb{R}^2$
Consider a line through the point $(1, 1)$ along the direction $(1, 2)$. This would be the set of all $x \in \mathbb{R}^2$ such that:
$$x = \begin{bmatrix} 1 \\ 1 \end{bmatrix} + \alpha \begin{bmatrix} 1 \\ 2 \end{bmatrix} \text{ where } \alpha \text{ varies over the real line.}$$

<img width="423" height="400" alt="image" src="https://github.com/user-attachments/assets/21e10ca2-cbd3-4ca5-a84e-92f64706fac1" />


It does really look like a line when plotted.

---

### 3. Hyperplanes
The next basic set in $d$-dimensional space is a hyperplane or just a plane. A hyperplane is typically a $(d - 1)$-dimensional hyperplane. This is the default dimension.

#### Definition
A hyperplane normal to the vector $w$ in $\mathbb{R}^d$ with value $b$ in $\mathbb{R}$ is given by the set of all $x$ in $\mathbb{R}^d$ such that:
$$w^T x = b$$
which is the set of $x$ in $\mathbb{R}^d$ such that:
$$\sum_{i=1}^{d} w_i x_i = b$$

This is the standard definition of a plane, which is defined in terms of a vector normal to the plane and value $b$.

#### Example: Hyperplane in $\mathbb{R}^3$
Consider $d = 3$. A hyperplane that is normal to the vector $(1, 1, 1)$ with value $1$ is simply the set of all $x$ in $\mathbb{R}^3$ such that:
$$x_1 + x_2 + x_3 = 1$$

<img width="383" height="373" alt="image" src="https://github.com/user-attachments/assets/7205371d-ddba-441c-87c4-9e31ab4ce403" />


If you can imagine a 3D corner of your room, a piece of paper floating there would look like this corresponding to the hyperplane.

---

### 4. Points, Vectors, and Tuples
In this particular set, we will call it this hyperplane $t$. We can say a statement like this: The point $(0, 1, 0)$ lies on $t$ which is perpendicular or normal to the vector $(1, 1, 1)$.

#### The Distinction
* **Tuple:** Simply what you would store in a programming language (e.g., a three-dimensional vector).
* **Point:** Corresponds to a location (e.g., "I am in Chennai").
* **Vector:** Corresponds to a direction (e.g., "I am going towards Bangalore").

You can imagine a line through Chennai along the direction given by Bangalore minus Chennai. Both are represented by a tuple in $\mathbb{R}^d$, but there is a difference between points and vectors.

> [!IMPORTANT]
> From context, it should be clear whether a tuple is used as a point or as a vector. 
> * $(0, 1, 0)$ is used as a **point** because a vector cannot lie on a plane; only points can.
> * $(1, 1, 1)$ is used as a **vector** because you cannot be perpendicular to a point; you can be perpendicular to a vector.

Algebraically it should be very clear, but geometrically you must understand whether a particular tuple is used as a vector or as a point.

---

### 5. Partial Derivatives
The basic tool that is the basic building block of multivariate calculus are partial derivatives.

#### Concept
The partial derivative of a function with respect to one of its arguments means you view the rest of the arguments as constants. If you fix the other variables, it becomes a one-dimensional function and then you can do the derivative.

#### Notation
I am going to use the sound "dough" ($\partial$) to denote this.

#### Mathematical Definition (2D Case)
For a function $f$ from $\mathbb{R}^2$ to $\mathbb{R}$, the partial derivative with respect to $x_1$ evaluated at a point $v$:
$$\frac{\partial f}{\partial x_1}(v) = \lim_{\alpha \to 0} \frac{f(v_1 + \alpha, v_2) - f(v_1, v_2)}{\alpha}$$

The second variable is kept constant at $v_2$. Similarly for the second variable:
$$\frac{\partial f}{\partial x_2}(v) = \lim_{\alpha \to 0} \frac{f(v_1, v_2 + \alpha) - f(v_1, v_2)}{\alpha}$$

#### General $d$-dimensional Definition
For a general $d$-dimensional function, the $i$-th argument:
$$\frac{\partial f}{\partial x_i}(v) = \lim_{\alpha \to 0} \frac{f(v + \alpha e_i) - f(v)}{\alpha}$$
Where $e_i$ is the coordinate vector with the $i$-th coordinate being equal to $1$ and the rest being $0$.

---

### 6. Gradients
If you have a function $f$ from $\mathbb{R}^d$ to $\mathbb{R}$, for any given point $v$ you have these things:
$$\frac{\partial f}{\partial x_1}(v), \frac{\partial f}{\partial x_2}(v), \dots, \frac{\partial f}{\partial x_d}(v)$$

#### Row Vector
You can imagine putting all of these in a row vector and call it:
$$\frac{\partial f}{\partial x} = \begin{bmatrix} \frac{\partial f}{\partial x_1} & \frac{\partial f}{\partial x_2} & \dots & \frac{\partial f}{\partial x_d} \end{bmatrix}$$

#### Gradient Definition
A gradient of $f$ at $v$ is simply the transpose:
$$\nabla f(v) = \left( \frac{\partial f}{\partial x} \right)^T$$
The gradient is typically written as a column vector. It is a collection of partial derivatives; a $d$-dimensional vector which collects partial derivatives.

#### Examples
1.  **Quadratic:** $f(x) = x_1^2 + x_2^2$
    * $\frac{\partial f}{\partial x_1} = 2x_1$
    * $\frac{\partial f}{\partial x_2} = 2x_2$
    * $\nabla f(v) = \begin{bmatrix} 2v_1 \\ 2v_2 \end{bmatrix}$

2.  **Linear:** $f(x) = x_1 + 2x_2 + 3x_3$
    * $\nabla f(v) = \begin{bmatrix} 1 \\ 2 \\ 3 \end{bmatrix}$
    Because the gradient is a constant, these kind of functions are called linear functions.


