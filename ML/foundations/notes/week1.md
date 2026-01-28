### 1. What is Machine Learning?
**Machine Learning (ML)** is the study of computer algorithms that improve automatically through **experience** and the use of **data**. Instead of requiring a human to write explicit rules for every scenario, an ML algorithm uses a broad blueprint and example data to construct a tool (a model) that performs a specific task.

#### **The Hierarchy of Task Abstraction**
A "task" is defined as any process that converts an **input** into an **output**. Tasks can be handled at three levels of abstraction:

| Level | Method | Description |
| :--- | :--- | :--- |
| **Lowest** | **Manual Labor** | A human directly operates on the input to create an output. |
| **Middle** | **Programming** | A human constructs a tool (software) that explicitly defines rules for the computer to follow. |
| **Highest** | **Machine Learning** | A human provides a broad outline and data; the system then constructs the tool itself. |

#### **Why Use ML?**
ML is preferred over human labor or standard programming when:
*   The task cannot be performed at the required **scale, speed, or cost** by humans.
*   The human cannot explicitly express the **mathematical rules** required to transform input to output (e.g., face detection).
*   The exact rules are unknown, but there is sufficient **example data** to learn from.

---

### 2. Core Mathematical Concepts and Notation
In ML, data is almost always represented as a collection of **vectors**.

<img width="764" height="544" alt="image" src="https://github.com/user-attachments/assets/bcecc799-e616-46f4-9c59-85702d97f40e" />

**Formula for Vector Length (Norm):**
$$\|x\| = \sqrt{x_1^2 + x_2^2 + \dots + x_d^2}$$

---

### 3. Machine Learning Models and Tasks
Models are mathematical simplifications or approximations of reality. They are generally categorized into **Predictive** and **Probabilistic** models.

#### **A. Supervised Learning (Labeled Data)**
The algorithm is provided with both inputs ($x$) and the correct outputs/labels ($y$).

1.  **Regression:** Predicts **continuous** real values (e.g., predicting a house price based on area and rooms).
    *   **Common Model:** Linear Parameterization ($f(x) = w^Tx + b$).
    *   **Regression Loss Formula (Mean Squared Error):**
        $$Loss = \frac{1}{n}\sum_{i=1}^{n} (f(x_i) - y_i)^2$$

2.  **Classification:** Predicts **discrete** labels or categories (e.g., predicting if an email is "Spam" or "Not Spam").
    *   **Common Model:** Linear Separator ($f(x) = \text{sign}(w^Tx + b)$).
    *   **Classification Loss Formula (Miss-classification Rate):**
        $Loss = \frac{1}{n} \sum_{i=1}^n 1(f(x_i) \neq y_i)$
        *(Where the indicator function $$1(\cdot)$$ is 1 if the prediction is wrong and 0 if correct.)*

#### **B. Unsupervised Learning (Unlabeled Data)**
The algorithm is only given inputs ($x$) and must identify inherent patterns or groupings.

1.  **Dimensionality Reduction:** Compresses and simplifies data. It uses an **Encoder** ($f$) to reduce dimensions and a **Decoder** ($g$) to reconstruct the original data.
    *   **Goal:** $g(f(x_i)) \approx x_i$.
2.  **Density Estimation:** Creates a **Probabilistic Model** that scores reality. It identifies regions where data points are most concentrated.
    *   **Goal:** Assign high probability to events likely to occur in the data.
    *   **Density Estimation Loss Formula (Negative Log Likelihood):**
        $$Loss = \frac{1}{n} \sum_{i=1}^n -\log(P(x_i))$$

---

### 4. Data Management: Training, Validation, and Testing
To ensure a model generalizes well to unseen data, the dataset is split into three parts:

| Data Set | Purpose | Percentage (Approx.) |
| :--- | :--- | :--- |
| **Training Data** | Used to identify patterns and learn the model parameters. | ~70-80% |
| **Validation Data** | Used for **Model Selection** (e.g., choosing between linear or quadratic models) and hyperparameter tuning. | ~15-20% |
| **Test Data** | Used as the "final exam" to measure the model’s real performance on **unseen data**. | ~15-20% |

---

### 5. Visualizing Concepts
*   **Regression Visual:** A scatter plot with a "best fit" line passing through the points. Dotted lines from the points to the line represent the **residual errors** being minimized.
*   **Classification Visual:** A 2D plot where points are colored by category (e.g., Red for +1, Blue for -1). A line or curve (boundary) separates the regions.
*   **Density Estimation Visual:** A contour plot (or level curves) showing where data is most concentrated, similar to isotherms or isobars in geography.
