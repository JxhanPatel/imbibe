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
