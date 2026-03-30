# 8.1: Pillars of Machine Learning 


## 1. The Three Pillars of Machine Learning

The theory of machine learning rests upon three foundational mathematical pillars. To understand any machine learning problem in detail, an understanding of each of these three pillars is required.

### The Pillars:

1. **Linear Algebra**
2. **Probability**
3. **Optimization**

---

## 2. Motivating Example: Height and Weight Prediction

Consider a dataset capturing the height (in centimeters) and weight (in kilograms) of people in a room. When plotted on a two-dimensional plane, each point represents one particular person.

<img width="333" height="206" alt="image" src="https://github.com/user-attachments/assets/90591739-f954-47b1-9587-8319293fa142" />

### Goal:

If a new person enters the room and their height is known, how can we predict their weight?

### Observations:

- There appears to be a **linear relation** between height and weight.
- As height increases, weight also seems to increase overall on average.
- If a line is found that "best represents" this relation, the weight for a given height can be predicted by finding the corresponding value on that line.

---

## 3. Pillar 1: Linear Algebra (Structure)

Linear algebra is used to understand the **structure or relationship between data**.

- **Assumption:** In the example, we assume height and weight are linearly related.
- **Purpose:** It handles the structural relationship between parameters of interest.
- **Complexity:** While relationships can be non-linear, the linear relationship is the simplest structural model. Understanding linear relationships well often allows for the understanding of non-linear models as well.

---

## 4. Pillar 2: Probability (Uncertainty)

Probability is the language used to **model noise or uncertainty in data**.

- **The Reality of Data:** In the height-weight dataset, not every point falls exactly on a single line.
- **Definition of Noise:** Everything that does not pertain to the linear relationship is considered noise.
- **Sources of Noise:**

  - Measurement errors.
  - Incorrect measurements.
  - Missing measurements.
- **Purpose:** It is essential to not only understand structure but also to handle the uncertainty inherent in real-world data.

---

## 5. Pillar 3: Optimization (Decisions)

Optimization is the mathematical tool that allows us to **convert data to decisions**.

### The Problem of Choice:

Even with a structural model (Linear Algebra) and a noise model (Probability), there isn't a single way to describe the relationship.

- There are **infinite possibilities** for lines that can relate height and weight.
- We need a way to determine which line **best** represents the relationship in the observed data.


### The Role of Optimization:

- **The Notion of "Best":** Optimization is used the moment there is a goal to find the "best" way to do certain things.
- **Action:** It involves picking one specific model (e.g., one specific line) out of infinite possibilities based on a criteria of best fit.
- **Summary:** It is the bridge that turns modeled data into a final decision or prediction.

---

## 6. Conclusion

While linear algebra and probability provide the framework for understanding data structure and noise, optimization provides the mechanism to navigate those possibilities and arrive at a solution. The goal of this course section is to understand the basic ideas of optimization to help navigate the world of machine learning and data science.







---






# 8.2: Introduction to Optimization

## Why Optimization in Machine Learning?

Optimization is the process used to convert **data** into **decisions**. In the context of supervised learning, it is fundamental for finding the "best" possible outcome for a given task.

### The Notion of "Best"
The term "best" is a vague notion that requires mathematical formalization depending on the specific paradigm:
* **Least Loss:** Defining a loss function and finding the classifier among all possibilities that minimizes this loss.
* **Maximum Reward:** In other paradigms, the goal is to find the parameter that maximizes a defined reward.

Optimization is essential because machine learning algorithms always aim to make decisions in the best possible way, which translates to finding a minimum or a maximum. 

---

## The Cow and Grass Example: A Mathematical Framework

To illustrate the components of an optimization problem, consider the following scenario involving a field represented as a grid where the origin $(0,0)$ is the left-bottom part. 

### 1. Problem Setup
* **Initial Position:** A cow is located at the coordinates $(20, 30)$.
* **Rope Restriction:** The cow is tied to a rope of length **10 units**. This restricts the cow's movement to a circle of radius 10 centered at $(20, 30)$.
* **Fence Restriction:** A perpendicular fence passes through the point $(25, 0)$. The cow cannot move further away (to the right) from this fence. 
* **Target (Grass):** There is grass located at the position $(40, 40)$.

**The Question:** How close can the cow get to the grass given these restrictions?

<img width="1336" height="539" alt="image" src="https://github.com/user-attachments/assets/c495e1ea-21f7-470c-b38c-237f96e8f800" />

### 2. Measuring the Goal (The Objective)
To answer "how close," we measure the distance from the cow's position $(x_1, x_2)$ to the grass $(40, 40)$.
Using the squared distance formula:
$$(x_1 - 40)^2 + (x_2 - 40)^2$$

Our goal is to **minimize** this distance.

### 3. Incorporating Restrictions (The Constraints)
* **Rope Constraint:** The distance from the cow $(x_1, x_2)$ to its starting point $(20, 30)$ must be less than or equal to the rope length (10 units).
    $$(x_1 - 20)^2 + (x_2 - 30)^2 \leq 10^2$$
* **Fence Constraint:** The cow must remain on the left side of the fence at $x = 25$.
    $$x_1 \leq 25$$

### 4. The Final Mathematical Optimization Problem
$$\min_{x_1, x_2} (x_1 - 40)^2 + (x_2 - 40)^2$$
$$\text{subject to:}$$
$$(x_1 - 20)^2 + (x_2 - 30)^2 \leq 100$$
$$x_1 \leq 25$$

---

## General Form of an Optimization Problem

In a broad mathematical framework, an optimization problem is structured as follows:

$$\min f(x)$$

$$\text{subject to:}$$

$$g_i(x) \leq 0, \quad \text{for } i = 1, \dots, k$$

$$h_j(x) = 0, \quad \text{for } j = 1, \dots, l$$

Where $x \in \mathbb{R}^d$ (a $d$-dimensional real space).

### Components of the Problem

| Term | Definition | Example (Cow Problem) |
| :--- | :--- | :--- |
| **Objective** | The function $f(x)$ we are trying to minimize or maximize. | Distance to the grass. |
| **Variable / Parameter** | The value $x$ we are optimizing over. | The position $(x_1, x_2)$ of the cow. |
| **Inequality Constraints** | Restrictions of the form $g_i(x) \leq 0$. | Rope and fence restrictions. |
| **Equality Constraints** | Restrictions of the form $h_j(x) = 0$. | (e.g., a specific budget limit). |

### Points for Reflection
1.  **Standard Form:** Can any inequality (e.g., $\geq$) or equality constraint be rewritten to fit the $g(x) \leq 0$ or $h(x) = 0$ format? 
2.  **Maximization vs. Minimization:** Is it possible to convert a maximization problem (like maximizing reward) into an equivalent minimization problem? If so, how? 





---





# 8.3: Solving an Unconstrained Optimization Problem | Part 1

## Introduction to Unconstrained Optimization

The general form of an optimization problem includes an objective and a bunch of constraints. To understand how to solve these, it makes sense to start with an **unconstrained optimization problem**, where constraints ($g_i(x)$ and $h_j(x)$) are absent. 
### A Simple Example
Consider minimizing a real number $x$:
$$\min_{x \in \mathbb{R}} (x - 5)^2$$

* **Logical Solution:** By observation, $x = 5$ is the solution because the squared function is non-negative and $(5-5)^2 = 0$. No other point can achieve a lower value. 
* **Traditional School Method:**
    1.  Let $f(x) = (x - 5)^2$.
    2.  Compute the first derivative: $f'(x) = 2(x - 5)$.
    3.  Set $f'(x) = 0 \implies 2(x - 5) = 0 \implies x^★ = 5$. 

### Limitations of the Derivative Method
The traditional method of setting the derivative to zero does not generalize to all problems. For example:
$$\min_{x \in \mathbb{R}} 3x^6 + 2x^5 + 3x^3 + 5x^2 + 2$$
Taking the derivative results in a degree-5 equation: $18x^5 + 10x^4 + 9x^2 + 10x = 0$. There is no easy way to solve this, and it is not even clear if the resulting $x$ is the minimum. 


---

## Developing a Systematic Iterative Procedure

To enable a computer to solve optimization problems, we need a systematic, iterative procedure. 

### The General Iterative Approach
1.  **Start with an arbitrary initial guess** $x_0 \in \mathbb{R}$ (e.g., $x_0 = 10$).
2.  **Update the guess** over $T$ rounds (e.g., $T = 1000$):
    $$x_{t+1} = x_t + d$$
    Where $d$ is a **direction** that hopefully moves the current point closer to the minimum.

### Determining a Good Direction
In the case of $f(x) = (x - 5)^2$:
* If $x > 5$: We want to move left (add a negative value, $d < 0$).
* If $x < 5$: We want to move right (add a positive value, $d > 0$). 

<img width="1323" height="766" alt="image" src="https://github.com/user-attachments/assets/70159964-7efe-4ff5-b715-bfff1336f82a" />

### The Choice of Direction ($d$)
The direction $d$ must be a function of $x$. A specific choice that satisfies the requirements above is the **negative derivative**:
$$d = -f'(x)$$

**Verification for $f(x) = (x - 5)^2$:**
* $f'(x) = 2(x - 5)$
* If $x > 5 \implies f'(x) > 0 \implies d = -f'(x) < 0$ (Correct: Move left).
* If $x < 5 \implies f'(x) < 0 \implies d = -f'(x) > 0$ (Correct: Move right). 

---

## Analyzing the Update Rule: The Oscillation Problem

Using the update rule $x_{t+1} = x_t - f'(x_t)$ on $f(x) = (x - 5)^2$ starting at $x_0 = 10$: 

1.  **Iteration 0:**
    * $x_0 = 10$
    * $f'(10) = 2(10 - 5) = 10$
    * $x_1 = 10 - 10 = 0$
2.  **Iteration 1:**
    * $x_1 = 0$
    * $f'(0) = 2(0 - 5) = -10$
    * $x_2 = 0 - (-10) = 10$
3.  **Iteration 2:**
    * $x_2 = 10 \implies x_3 = 0$

**Observation:** The algorithm oscillates between 10 and 0 and never reaches the minimum at $x = 5$. 

### Diagnosis
The problem is not the direction of movement. Our `−f′(x)` is indeed giving us a negative value when we need to move left and a positive value when we need to move right. The problem is the **amount** you move in this direction. The amount of movement seems high; you are taking a step in the direction given by your negative derivative, but that step is too large so that it misses the minimum.

