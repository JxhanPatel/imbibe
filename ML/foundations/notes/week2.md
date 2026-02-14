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

---

#### **Types of Discontinuity Summary**
* **Point Discontinuity:** A single point is missing or displaced (a "hole").
* **Jump Discontinuity:** The function "jumps" from one value to another at a specific point.
* **Infinite Discontinuity:** The function goes to infinity (e.g., $1/x$ at $x=0$).





