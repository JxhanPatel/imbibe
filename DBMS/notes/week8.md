# 8.1: Algorithms and Data Structures/1: Algorithms and Complexity Analysis



## **1.Overview**

### **1.1. Recap**
*   Reviewed Application Programs across various sectors.
*   Understood typical three-tier architectures, their classification (1-tier to n-tier), and historical evolution.
*   Familiarized with fundamental web technologies (URLs, HTML, HTTP) and technologies like scripting and servlets.
*   Learnt to use SQL from a programming language and build Python Web Applications with PostgreSQL using `psycopg2`.
*   Understood Rapid Application Development (RAD) and issues in application performance and security.
*   Learnt the distinctive features of Mobile Apps.

### **1.2. Objectives**
*   To define **Algorithms** and identify their difference with **Programs**.
*   To analyze algorithms for performance in terms of **time, space, power, etc.**.
*   To introduce **Asymptotic notation** for the representation of complexity.
*   To consider the complexity of **common algorithms**.

---

## **2. Algorithms and Programs**

### **2.1. Algorithm**
*   An algorithm is a finite sequence of well-defined, computer-implementable instructions, typically to solve a class of specific problems or to perform a computation.
*   Algorithms are always unambiguous and are used as specifications for performing calculations, data processing, automated reasoning, and other tasks.
*   **Termination:** An algorithm **must terminate**; given an input, it must produce an output in finite time.

### **2.2. Program**
*   A computer program is a collection of instructions that can be executed by a computer to perform a specific task.
*   A computer program is usually written by a computer programmer in a programming language.
*   A program **implements an algorithm**.
*   A program may or may not terminate (e.g., an Operating System).

---

## **3. Analysis of Algorithms**

### **3.1. Motivation: Why analyze?**
*   **Practical Reasons:** Resources are scarce, there is a greed to do more with less, and a need to avoid performance bugs.
*   **Core Issues:**
    *   **Predict performance:** Determine how much time a specific algorithm (e.g., binary search) takes.
    *   **Compare algorithms:** Evaluate which algorithm is faster (e.g., how quick is Quicksort).
    *   **Provide guarantees:** Ensure operations stay within certain bounds (e.g., Red-Black tree inserts in $O(\log n)$).
    *   **Understand theoretical basis:** For example, knowing that sorting by comparison cannot do better than $\Omega(n \log n)$.

### **3.2. Factors: What to analyze?**
The core issue is that one cannot control what cannot be measured. Analysis typically measures resources used as a function of **input size ($n$)**.
*   **Time:** The most common analysis factor; representative of related factors like power, bandwidth, and processors.
*   **Space:** Widely explored and important for handheld devices.

#### **Examples of Analysis:**
*   **Sum of Natural Numbers:**
    *   *Algorithm:* `int sum(int n) { int s = 0; for(; n > 0; --n) s = s + n; return s; }`.
    *   **Time:** $T(n) = n$ (additions).
    *   **Space:** $S(n) = 2$ (variables $n, s$).
*   **Find a Character in a String:**
    *   *Algorithm:* `int find(char *str, char c) { for(int i = 0; i < strlen(str); ++i) if (str[i] == c) return i; return 0; }`.
    *   **Time:** If $n = strlen(str)$, $T(n) = n(compare) + n * T(strlen(str)) \approx n + n^2 \approx n^2$.
    *   **Space:** $S(n) = 3$ (str, c, i).
*   **Minimum of a Sequence of Numbers:**
    *   **Time:** $T(n) = n - 1$ (comparison of value).
    *   **Space:** Depends on implementation (e.g., $S(n) = n + 3$ if using an array, or $S(n) = 3$ if reading one by one).

### **3.3. Methodologies: How to analyze?**
*   **Counting Models:** Total running time = Sum of (cost $\times$ frequency) for all operations.
    *   *Machine Model:* Random Access Machine (RAM) Computing Model.
*   **Asymptotic Analysis:** Comparing the **growth rate** or how time/space increases with input size, rather than actual times.
*   **Recursive Algorithms:** Analyzed using **Generating Functions** or the **Master Theorem**.

---

## **4. Counting Models and Asymptotic Analysis**

### **4.1. Factorial Example (Counting Model)**
*   **Recursive Factorial:**
    *   Time $T(n) = n - 1$ (multiplications).
    *   Space $S(n) = n + 1$ (due to $n$ in recursive call stack).
*   **Iterative Factorial:**
    *   Time $T(n) = n$ (multiplications).
    *   Space $S(n) = 2$ (variables $n, t$).

### **4.2. Function Approximation ($\sim$ Notation)**
*   Estimate running time/memory as a function of input size $N$, ignoring lower-order terms when $N$ is large.
*   $f(n) \sim g(n)$ means $\lim_{N \to \infty} \frac{f(n)}{g(n)} = 1$.

### **4.3. Asymptotic Notation (Big-O)**
*   $O(g(n)) = \{f(n) : \text{there exist positive constants } c \text{ and } n_0 \text{ such that } 0 \le f(n) \le cg(n), \text{ for all } n > n_0\}$.
*   It provides an **upper bound** on a function within a constant factor.
*   Saying a running time is $O(n^2)$ means the **worst-case** running time is bounded from above by a quadratic function.

---

## **5. Complexity Classifications**

### **5.1. Common Order-of-Growth Classifications**
The following set of functions suffices to describe the growth of most common algorithms:

| Order of Growth | Name | Typical Code Framework | Example | $T(2N)/T(N)$ |
| :--- | :--- | :--- | :--- | :--- |
| **1** | Constant | Statement | Add two numbers | 1 |
| **$\log N$** | Logarithmic | Divide in half | Binary Search | $\sim 1$ |
| **$N$** | Linear | Single loop | Find maximum | 2 |
| **$N \log N$** | Linearithmic | Divide and conquer | Mergesort | $\sim 2$ |
| **$N^2$** | Quadratic | Double loop | Check all pairs | 4 |
| **$N^3$** | Cubic | Triple loop | Check all triples | 8 |
| **$2^N$** | Exponential | Exhaustive search | Check all subsets | $T(N)$ |

<img width="826" height="331" alt="image" src="https://github.com/user-attachments/assets/4879ccce-c046-4c35-8f39-85f6861febeb" />

---

## **6. Scenarios: Where to analyze?**

Analysis identifies data configurations or scenarios:
*   **Best Case:** Minimum running time on an input.
*   **Worst Case:** Running time guarantee for any input of size $n$.
*   **Average Case:** Expected running time for a random input of size $n$.
*   **Probabilistic Case:** Expected running time of a randomized algorithm.
*   **Amortized Case:** Worst-case running time for any sequence of $n$ operations.

---

## **7. Complexity Cheat Sheets**

### **7.1. Common Data Structure Operations (Average Case)**
*   **Array:** Access $O(1)$, Search $O(n)$, Insertion $O(n)$, Deletion $O(n)$.
*   **Hash Table:** Search $O(1)$, Insertion $O(1)$, Deletion $O(1)$.
*   **Binary Search Tree:** Access $O(\log n)$, Search $O(\log n)$, Insertion $O(\log n)$, Deletion $O(\log n)$.
*   **B-Tree:** Access $O(\log n)$, Search $O(\log n)$, Insertion $O(\log n)$, Deletion $O(\log n)$.

### **7.2. Array Sorting Algorithms**
| Algorithm | Time (Best) | Time (Average) | Time (Worst) | Space (Worst) |
| :--- | :--- | :--- | :--- | :--- |
| **Quicksort** | $\Omega(n \log n)$ | $\Theta(n \log n)$ | $O(n^2)$ | $O(\log n)$ |
| **Mergesort** | $\Omega(n \log n)$ | $\Theta(n \log n)$ | $O(n \log n)$ | $O(n)$ |
| **Heapsort** | $\Omega(n \log n)$ | $\Theta(n \log n)$ | $O(n \log n)$ | $O(1)$ |
| **Insertion Sort**| $\Omega(n)$ | $\Theta(n^2)$ | $O(n^2)$ | $O(1)$ |

