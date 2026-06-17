# Week 1 – Python Refreshers

These notes consolidate Python fundamentals, algorithmic efficiency, and introductory object-oriented programming concepts covered in Week 1, with an emphasis on *why* efficiency matters.

---

## Learning Objectives

By the end of this week, you should be able to:

* [ ] Compare Python execution environments (Jupyter, Colab, IDEs).
* [ ] Analyze multiple GCD algorithms and their time–space trade-offs.
* [ ] Explain primality testing optimizations from $O(n)$ to $O(\sqrt{n})$.
* [ ] Implement basic Object-Oriented Programming (OOP) using classes and special methods.
* [ ] Articulate the real-world importance of algorithmic efficiency.

---

## 1. Python Development Environments

Python can be run in different environments depending on workflow, scale, and collaboration needs.

### Common Environments

**Manual / Terminal**

* Write code in a text editor (e.g., Emacs, VS Code).
* Execute via the command line using `python file.py`.

**IDEs (Integrated Development Environments)**

* Examples: Replit, PyCharm, VS Code.
* Combine editing, execution, and debugging in one interface.

**Jupyter Notebooks**

* Web-based, cell-oriented interface.
* Mix code, Markdown, math, and plots.

**Google Colab**

* Cloud-hosted Jupyter environment by Google.
* No local setup required; supports free GPU/TPU access.

---

## 2. Greatest Common Divisor (GCD) Algorithms

The GCD problem demonstrates how algorithmic insight can drastically improve efficiency.

### Approach 1: List-Based Intersection

Compute all factors of both numbers and select the largest common one.

* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(n)$

---

### Approach 2: Most Recent Common Factor (MRCF)

Tracks only the most recent common factor while iterating.

* **Optimization:** Eliminates factor lists
* **Time Complexity:** $O(n)$
* **Space Complexity:** $O(1)$

---

### Approach 3: Backward Iteration

Iterate from $\min(m, n)$ down to $1$ and return the first common divisor.

* **Worst Case:** $O(n)$
* **Practical Note:** Often faster if a large common factor exists

---

### Approach 4: Euclid’s Subtraction Algorithm

Uses the identity:
$gcd(a, b) = gcd(b, a-b)$ where $a > b$

* **Worst-Case Time Complexity:** $O(\max(m, n))$

> This approach performs poorly when $m$ and $n$ differ significantly.

---

### Approach 5: Euclid’s Remainder Algorithm

Uses:

$gcd(m, n) = \gcd(n, m \bmod n)$

```python
def gcd(m, n):
    if n == 0:
        return m
    return gcd(n, m % n)
```

* **Time Complexity:** $O(\log \min(m, n))$
* **Space Complexity:** $O(\log n)$ (recursive stack)

---

## 3. Primality Testing & Optimization

A number $n > 1$ is prime if it has no divisors other than $1$ and itself.

### Linear Search Method

Check all integers from $2$ to $n - 1$.

* **Time Complexity:** $O(n)$

---

### Square Root Optimization

If $d$ divides $n$, then $\frac{n}{d}$ is also a divisor.

* **Observation:** At least one divisor must satisfy $d \le \sqrt{n}$
* **Time Complexity:** $O(\sqrt{n})$

---

## 4. Object-Oriented Programming (OOP) in Python

OOP supports modularity and **information hiding**, allowing internal changes without affecting external code.

### Example: `Point` Class

```python
class Point:
    def __init__(self, a=0, b=0):
        """Constructor: initializes x and y coordinates."""
        self.x = a
        self.y = b

    def translate(self, deltax, deltay):
        """Moves the point by the given offsets."""
        self.x += deltax
        self.y += deltay

    def __str__(self):
        """Returns a human-readable string representation."""
        return f"({self.x}, {self.y})"

```

### Special Methods

* `__init__()` — constructor
* `__str__()` — string representation via `print()`

---

## 5. The “Why” Factor: Importance of Efficiency

Why does $O(\log n)$ outperform $O(n)$ so dramatically?

| Problem        | Inefficient Approach   | Efficient Approach          | Impact                                |
| -------------- | ---------------------- | --------------------------- | ------------------------------------- |
| Birthday Guess | Linear search (O(n)) | Binary search (O(log n)) | $\le 9$ guesses                       |
| Aadhaar Search | Linear search (O(n)) | Binary search (O(log n)) | $\sim 3200$ years → $\sim 50$ minutes |

> **Important**
> Efficient algorithms often require the appropriate data structure (e.g., sorted data for binary search).

---

## Technical Glossary

| Term                   | Definition                           |
| ---------------------- | ------------------------------------ |
| **GCD**                | Greatest Common Divisor              |
| **MRCF**               | Most Recent Common Factor            |
| **Euclid’s Algorithm** | GCD algorithm using remainders       |
| **Amortized Time**     | Average cost per operation over time |
| **Information Hiding** | Internal changes do not affect usage |
