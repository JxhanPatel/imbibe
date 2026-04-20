# 12.1: Query Processing and Optimization/1: Processing

## **Recap**
*   Learnt the importance of backup and analysed different backup strategies.
*   Failures may be due to a variety of sources; each needs a strategy for handling.
*   A proper mix and management of volatile, non-volatile, and stable storage can guarantee recovery from failures and ensure Atomicity, Consistency, and Durability.
*   Log-based recovery is efficient and effective.
*   Learnt how Hot backup of transaction log helps in recovering a consistent database.
*   Studied the recovery algorithms for concurrent transactions.
*   Recovery based on operation logging supplements log-based recovery.
*   Planning for Backup.
*   Understood RAID - array of redundant disks in parallel to enhance speed and reliability.

## **Objectives**
*   To understand the overall flow for Query Processing.
*   To define the Measures of Query Cost.
*   To understand the algorithms for processing Selection Operations, Sorting, Join Operations, and a few Other Operations.

---

## **1. Overview of Query Processing**

### **1.1. Basic Steps**
Query processing refers to the range of activities involved in extracting data from a database. The basic steps are:
1.  **Parsing and translation:** Translate the query into its internal form, which is then translated into relational algebra. The parser checks syntax and verifies relations.
2.  **Optimization:** Amongst all equivalent evaluation plans, the system chooses the one with the lowest cost. Cost is estimated using statistical information from the database catalog, such as the number of tuples in each relation and the size of tuples.
3.  **Evaluation:** The query-execution engine takes a query-evaluation plan, executes that plan, and returns the answers to the query.

<img width="793" height="454" alt="image" src="https://github.com/user-attachments/assets/0b0d4988-0993-4de2-92e4-2b1298bb42c9" />

### **1.2. Evaluation Plan**
*   An annotated expression specifying a detailed evaluation strategy is called an evaluation-plan.
*   A relational-algebra expression can be evaluated in many ways, using different algorithms for each operation.
*   For example, one could use an index on salary to find instructors with $\text{salary} < 75000$, or perform a complete relation scan and discard instructors with $\text{salary} \ge 75000$.

<img width="886" height="441" alt="image" src="https://github.com/user-attachments/assets/f6d0c4a7-35b9-4b1d-a8e0-79466df948d6" />

---

## **2. Measures of Query Cost**

*   Cost is generally measured as the total elapsed time for answering a query.
*   The number of block transfers from disk and the number of seeks are used as the primary cost measures.
*   Let:
    *   $t_T$: time to transfer one block.
    *   $t_S$: time for one seek.
    *   $b$: number of block transfers.
    *   $S$: number of seeks.
*   **Total Cost Formula:** $b * t_T + S * t_S$.
*   Several factors are often ignored for simplicity:
    *   CPU costs (though real systems take this into account).
    *   The cost of writing the final output to disk.
*   Note: The cost to write a block is greater than the cost to read a block because data is read back after being written to ensure the write was successful.

---

## **3. Selection Operation**

### **3.1. File / Index Scan Algorithms**

| A# | Algorithm | Cost Estimate | Reason |
| :--- | :--- | :--- | :--- |
| **A1** | Linear Search | $t_S + b_r \times t_T$ | One initial seek plus $b_r$ block transfers. |
| **A1** | Linear Search, Eq. on Key | Average: $t_S + (b_r / 2) \times t_T$ | Scan can terminate once the unique record is found. |
| **A2** | Primary Index, Eq. on Key | $(h_i + 1) \times (t_T + t_S)$ | Traverses index height plus one I/O to fetch the record. |
| **A3** | Primary Index, Eq. on Non-key | $h_i \times (t_T + t_S) + b \times t_T$ | Seeks for index levels and first block; $b$ leaf blocks are read sequentially. |
| **A4** | Secondary Index, Eq. on Key | $(h_i + 1) \times (t_T + t_S)$ | Similar to primary index. |
| **A4** | Secondary Index, Eq. on Non-key | $(h_i + n) \times (t_T + t_S)$ | Each of the $n$ records may be on a different block, requiring a seek per record. |
| **A5** | Primary Index, Comparison | $h_i \times (t_T + t_S) + b \times t_T$ | Identical to case A3 (equality on non-key). |
| **A6** | Secondary Index, Comparison | $(h_i + n) \times (t_T + t_S)$ | Identical to case A4 (equality on non-key). |

*   $b_r$ denotes the number of blocks in the file.
*   $h_i$ denotes the height of the index.
*   $n$ is the number of records fetched.

### **3.2. Complex Selections**
*   **Conjunction ($\sigma_{\theta_1 \wedge \theta_2 \wedge \dots \wedge \theta_n}(r)$):** Select the combination of conditions and algorithms (A1 through A6) that results in the least cost.
*   **Disjunction ($\sigma_{\theta_1 \vee \theta_2 \vee \dots \vee \theta_n}(r)$):** If indices are available for all conditions, use them and take the union of record pointers (identifiers); otherwise, use a linear scan.
*   **Negation ($\sigma_{\neg\theta}(r)$):** Generally use a linear scan; if very few records satisfy the negation and an index is applicable to $\theta$, use the index to find satisfying records and fetch them.

---

## **4. Sorting**

*   Sorting is essential because SQL queries can specify sorted output and several relational operations (like joins) can be implemented efficiently on sorted inputs.
*   **In-memory:** Techniques like quicksort are used if the relation fits in memory.
*   **External Data:** **External sort-merge** is used for relations that do not fit in memory.
*   **Run Generation:** First, sorted "runs" are created, each containing some records of the relation.
*   **Merge Pass:** These runs are then merged in one or more passes to produce the final sorted output.

<img width="491" height="454" alt="image" src="https://github.com/user-attachments/assets/7b60e6fb-aaee-490c-800e-66a80d0eb389" />

---

## **5. Join Operation**

Several algorithms exist to implement joins; the choice is based on cost estimates.

### **5.1. Nested-Loop Join**
*   To compute $\theta$-join $r \bowtie_\theta s$, for each tuple $t_r$ in $r$ (outer relation), scan each tuple $t_s$ in $s$ (inner relation) and test the join condition.
*   **Worst-case Cost:** $n_r * b_s + b_r$ block transfers plus $n_r + b_r$ seeks, where $n$ is the number of tuples and $b$ is the number of blocks.

### **5.2. Block Nested-Loop Join**
*   A variant where every **block** of the outer relation is paired with every **block** of the inner relation.
*   **Algorithm:** For each block $B_r$ of $r$, and for each block $B_s$ of $s$, check pairs $(t_r, t_s)$ where $t_r \in B_r$ and $t_s \in B_s$.
*   **Cost:** $\lceil b_r / (M-2) \rceil * b_s + b_r$ block transfers plus $2 * \lceil b_r / (M-2) \rceil$ seeks.

### **5.3. Indexed Nested-Loop Join**
*   Index lookups replace file scans if the join is an equi-join or natural join and an index is available on the inner relation's join attribute.
*   For each tuple $t_r$ in the outer relation, use the index to look up matching tuples in $s$.

### **5.4. Join Cost Example**
*   **Scenario:** Join `student` (outer, 100 blocks, 5000 tuples) and `takes` (inner, 400 blocks, 10000 tuples).
*   `takes` has a primary $B^+$-tree index on ID (height 4).
*   **Block Nested-Loop Cost:** $400 * 100 + 100 = 40,100$ transfers plus 200 seeks.
*   **Indexed Nested-Loop Cost:** $100 + 5000 * 5 = 25,100$ transfers and seeks.

---

## **6. Other Operations**

*   **Duplicate Elimination:** Can be implemented via hashing or sorting. Sorting brings duplicates adjacent to each other; hashing places them in the same bucket.
*   **Projection:** Perform projection on each tuple, followed by duplicate elimination.
*   **Aggregation:** Similar to duplicate elimination. Sorting or hashing brings tuples in the same group together before applying functions.
    *   *Optimization:* For `sum`, `min`, `max`, `count`, keep partial aggregate values during run generation or merge steps.
*   **Set Operations & Outer Joins:** Also processed using similar sorting or hashing techniques.
