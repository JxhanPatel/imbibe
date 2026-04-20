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














---











# 12.2: Query Processing and Optimization/2: Optimization


## **Objectives**
*   To understand the basic issues for optimizing queries.
*   To understand how transformation of Relational Expressions can create alternates for optimization.

---

## **1. Introduction to Query Optimization**

### **1.1 Definition and Purpose**
Query optimization is the process of selecting the most efficient query-evaluation plan from among the many strategies usually possible for processing a given query, especially if the query is complex. It is the responsibility of the system to construct a query-evaluation plan that minimizes the cost of query evaluation.

### **1.2 Aspects of Optimization**
1.  **Relational-Algebra Level:** The system attempts to find an expression that is equivalent to the given expression but more efficient to execute.
2.  **Detailed Strategy Selection:** Choosing the specific algorithms to use for executing an operation (e.g., hash join vs. merge join) and choosing specific indices.

### **1.3 Importance of Optimization**
The difference in cost between a good strategy and a bad strategy is often substantial and may be several orders of magnitude. Evaluation plan costs can range from seconds to days. Consequently, it is worthwhile for the system to spend a substantial amount of time selecting a good strategy, even if the query is executed only once.

### **1.4 Evaluation Plan**
An evaluation plan defines exactly what algorithm is used for each operation and how the execution of the operations is coordinated.
*   **Annotated Query Tree:** Evaluation plans are often represented as a tree where nodes are physical operators (sort, hash join, etc.) and edges are marked as pipelined or materialized.
*   **Pipelined Edges:** The output of the producer is sent directly to the consumer without being written to disk.
*   **Materialized Edges:** The output is written to disk and then read by the consumer.

<img width="913" height="426" alt="image" src="https://github.com/user-attachments/assets/d385ec21-dbcc-426e-8d2f-686c257ed05a" />

### **1.5 Steps in Cost-Based Query Optimization**
1.  Generate logically equivalent expressions using equivalence rules.
2.  Annotate the resultant expressions to generate alternative query-evaluation plans.
3.  Estimate the cost of each evaluation plan and choose the one with the least estimated cost.

Plan cost estimation is based on:
*   Statistical information about relations (e.g., number of tuples, number of distinct values).
*   Statistics estimation for intermediate results to compute the cost of complex expressions.
*   Cost formulae for algorithms computed using these statistics.

> [!NOTE]
> Most database systems provide a way to view the chosen evaluation plan using a variation of the `explain <query>` command or a graphical user interface (GUI).

---

## **2. Transformation of Relational Expressions**

### **2.1 Equivalence of Expressions**
Two relational-algebra expressions are said to be equivalent if the two expressions generate the same set of tuples on every legal database instance.
*   The order of tuples is irrelevant.
*   In SQL, inputs and outputs are multisets of tuples, so the number of duplicates must be preserved for equivalence.

### **2.2 Equivalence Rules**
These rules specify how to transform an expression into a logically equivalent one.

#### **Rules for Selections**
1.  **Deconstruction of Conjunctive Selections:** $\sigma_{\theta_1 \wedge \theta_2}(E) \equiv \sigma_{\theta_1}(\sigma_{\theta_2}(E))$.
2.  **Commutativity of Selection:** $\sigma_{\theta_1}(\sigma_{\theta_2}(E)) \equiv \sigma_{\theta_2}(\sigma_{\theta_1}(E))$.

#### **Rules for Projections**
3.  **Cascading Projections:** Only the last in a sequence of projection operations is needed: $\Pi_{L_1}(\Pi_{L_2}(\dots(\Pi_{L_n}(E))\dots)) \equiv \Pi_{L_1}(E)$.

#### **Rules for Joins and Products**
4.  **Combining Selections with Products/Joins:**
    *   $\sigma_\theta(E_1 \times E_2) \equiv E_1 \bowtie_\theta E_2$.
    *   $\sigma_{\theta_1}(E_1 \bowtie_{\theta_2} E_2) \equiv E_1 \bowtie_{\theta_1 \wedge \theta_2} E_2$.
5.  **Commutativity of Theta-Join:** $E_1 \bowtie_\theta E_2 \equiv E_2 \bowtie_\theta E_1$.
6.  **Associativity of Joins:**
    *   **Natural Join:** $(E_1 \bowtie E_2) \bowtie E_3 \equiv E_1 \bowtie (E_2 \bowtie E_3)$.
    *   **Theta-Join:** $(E_1 \bowtie_{\theta_1} E_2) \bowtie_{\theta_2 \wedge \theta_3} E_3 \equiv E_1 \bowtie_{\theta_1 \wedge \theta_3} (E_2 \bowtie_{\theta_2} E_3)$, where $\theta_2$ involves attributes from only $E_2$ and $E_3$.

#### **Distribution Rules**
7.  **Distribution of Selection over Theta-Join:**
    *   **7.a:** When all attributes in $\theta_0$ involve only attributes of $E_1$: $\sigma_{\theta_0}(E_1 \bowtie_\theta E_2) \equiv (\sigma_{\theta_0}(E_1)) \bowtie_\theta E_2$.
    *   **7.b:** When $\theta_1$ involves only attributes of $E_1$ and $\theta_2$ involves only attributes of $E_2$: $\sigma_{\theta_1 \wedge \theta_2}(E_1 \bowtie_\theta E_2) \equiv (\sigma_{\theta_1}(E_1)) \bowtie_\theta (\sigma_{\theta_2}(E_2))$.
8.  **Distribution of Projection over Theta-Join:**
    *   **8.a:** If $\theta$ involves only attributes from $L_1 \cup L_2$: $\Pi_{L_1 \cup L_2}(E_1 \bowtie_\theta E_2) \equiv (\Pi_{L_1}(E_1)) \bowtie_\theta (\Pi_{L_2}(E_2))$.
    *   **8.b:** If $L_1, L_2$ are attributes from $E_1, E_2$ and $L_3, L_4$ are join attributes of $E_1, E_2$ not in $L_1 \cup L_2$: $\Pi_{L_1 \cup L_2}(E_1 \bowtie_\theta E_2) \equiv \Pi_{L_1 \cup L_2}((\Pi_{L_1 \cup L_3}(E_1)) \bowtie_\theta (\Pi_{L_2 \cup L_4}(E_2)))$.

#### **Rules for Set Operations**
9.  **Commutativity of Union and Intersection:**
    *   $E_1 \cup E_2 \equiv E_2 \cup E_1$.
    *   $E_1 \cap E_2 \equiv E_2 \cap E_1$.
    *   *(Note: Set difference is not commutative)*.
10. **Associativity of Union and Intersection:**
    *   $(E_1 \cup E_2) \cup E_3 \equiv E_1 \cup (E_2 \cup E_3)$.
    *   $(E_1 \cap E_2) \cap E_3 \equiv E_1 \cap (E_2 \cap E_3)$.
11. **Distribution of Selection over Set Operations:** $\sigma_\theta(E_1 - E_2) \equiv \sigma_\theta(E_1) - \sigma_\theta(E_2)$, and similarly for $\cup$ and $\cap$.
12. **Distribution of Projection over Union:** $\Pi_L(E_1 \cup E_2) \equiv (\Pi_L(E_1)) \cup (\Pi_L(E_2))$.

<img width="651" height="459" alt="image" src="https://github.com/user-attachments/assets/ef9324e0-5dd8-45a0-a705-4c37beea2c04" />

### **2.3 Transformation Examples**

#### **Pushing Selections**
*   **Query:** "Find the names of all instructors in the Music department together with the course title of all the courses that the instructors teach.".
*   **Initial Expression:** $\Pi_{name, title}(\sigma_{dept\_name = "Music"}(instructor \bowtie (teaches \bowtie \Pi_{course\_id, title}(course))))$.
*   **Observation:** The intermediate join produces a large result, but only a few tuples (Music department) are needed.
*   **Transformed Expression (Rule 7.a):** $\Pi_{name, title}((\sigma_{dept\_name = "Music"}(instructor)) \bowtie (teaches \bowtie \Pi_{course\_id, title}(course)))$.
*   **Conclusion:** Performing the selection as early as possible reduces the size of the relation to be joined.

<img width="870" height="429" alt="image" src="https://github.com/user-attachments/assets/ce02034f-0e9f-4971-8893-f583ae708fcf" />

#### **Multiple Transformations**
*   **Query:** Find the names of all instructors in the Music department who have taught a course in 2009, along with the titles of the courses they taught.
*   **Sequence:**
    1.  Use Join Associativity (Rule 6.a) to change the order of joins.
    2.  Use the "perform selections early" rule to push $\sigma_{dept\_name = "Music"}$ to *instructor* and $\sigma_{year = 2009}$ to *teaches*.
*   **Resulting Tree:** Both relations are filtered first, making them small before the join.

<img width="634" height="411" alt="image" src="https://github.com/user-attachments/assets/458435a6-439f-4a6f-b205-d86066668ca7" />

#### **Pushing Projections**
*   **Strategy:** Eliminate unneeded attributes from intermediate results.
*   **Example:** In the join of *instructor* and *teaches*, if only `name` and `course_id` are needed for subsequent operations, apply $\Pi_{name, course\_id}$ immediately after the selection on *instructor*.
*   **Result:** This reduces the number of columns and thus the size of the intermediate relation.

---

## **3. Join Ordering**
*   Join ordering is critical for reducing the size of temporary results.
*   **Strategy:** Use associativity to choose an order where the temporary relation stored is as small as possible.
*   **Example:** For $(r_1 \bowtie r_2) \bowtie r_3$, if $r_1 \bowtie r_2$ is small but $r_2 \bowtie r_3$ is large, compute $(r_1 \bowtie r_2)$ first.

---

## **4. Enumeration of Equivalent Expressions**

### **4.1 The Procedure**
Query optimizers use equivalence rules to systematically generate expressions equivalent to a given query.
*   **Concept:**
    1.  Start with set $EQ = \{E\}$.
    2.  **Repeat:** Match each expression in $EQ$ with each equivalence rule. If a subexpression matches one side of a rule, create a new expression by transforming that subexpression to the other side of the rule.
    3.  Add the new expression to $EQ$ if not already present.
    4.  **Until** no new expressions can be added.

### **4.2 Challenges and Implementation**
*   **Complexity:** The brute force approach is very expensive in time and space, often becoming exponential.
*   **Optimization of Optimization:** The time taken to optimize must not outweigh the execution benefits.
*   **Sharing Sub-expressions:** Reduce space by using pointers to share common subtrees between equivalent expressions (e.g., when applying join commutativity).
*   **Dynamic Programming:** Drastically reduce time by using dynamic programming (Tableau method) to track the best current cost and prune expressions that won't lead to an optimal plan.





---









# 12.3 RDBMS Performance & Architecture



## **Objectives**
*   To evaluate RDBMS, especially with reference to performance and scalability, as a backbone for data-intensive application development.
*   To understand the role of system and database architecture in performance.
*   To understand options for Scaling Databases.



## **1. RDBMS Performance and Scalability**

### **1.1. What do DBMS Applications Need?**
Database applications have several core requirements to function effectively as a backbone for data-intensive development:

*   **Correctness:** Any given database transaction must change affected data only in allowed ways, which is guaranteed by adhering to ACID Properties.
*   **Throughput, Response Time, & Availability:**
    *   **Throughput** is measured in transactions per second (tps).
    *   **Response Time** is the delay from the submission of a transaction to the return of the result.
    *   **Availability** is measured by the mean time to failure.
*   **Performance Tuning:**
    *   **At Transaction Level:** Involves Concurrency Control and Query Optimization.
    *   **At System Level:** Involves System Architecture and Database Architecture.
    *   **Hardware Tuning:** Adding disks to speed up $I/O$, increasing memory to increase buffer hits, or moving to a faster processor.
    *   **Database System Parameters:** Setting buffer size to avoid paging and setting checkpointing to limit log size.
    *   **Higher level database design:** Modification of schema, indices, and transactions.
*   **Scalability:**
    *   This is the ability to scale up a database to allow it to hold increasing amounts of data without sacrificing performance.
    *   A system should be able to scale with volume of data, number of users, diversity of services, and geographic expanse.
    *   Scalability can be achieved through system architecture, database architecture, scaling expectations, alternate data models, and accommodating hybrid systems.

---

## **2. RDBMS Architecture**

### **2.1. Centralized and Client-Server Systems**

#### **2.1.1. Centralized Architecture**
*   Centralized systems run on a single computer system and do not interact with other computer systems.
*   General-purpose systems typically consist of one to a few CPUs and device controllers connected through a common bus providing access to shared memory.
*   **Single-user systems:** Usually personal computers or workstations with one CPU and one or two hard disks, where the OS may support only one user.
*   **Multi-user systems:** Feature more disks, more memory, multiple CPUs, and a multi-user OS to serve a large number of users connected via terminals (often called server systems).


<img width="727" height="373" alt="image" src="https://github.com/user-attachments/assets/132574f2-d779-49cc-9c30-bad0b79d8a36" />


#### **2.1.2. Client-Server Architecture**
*   Server systems satisfy requests generated by $m$ client systems over a network.
*   Database functionality is divided into:
    *   **Back-end:** Manages access structures, query evaluation, optimization, concurrency control, and recovery.
    *   **Front-end:** Consists of tools such as forms, report-writers, and graphical user interface (GUI) facilities.
*   The interface between front-end and back-end is through SQL or an application program interface (API) like ODBC or JDBC.


<img width="853" height="302" alt="image" src="https://github.com/user-attachments/assets/344ab553-9fc8-4665-826d-8282c1bee7ef" />


<img width="868" height="461" alt="image" src="https://github.com/user-attachments/assets/20e8e8df-5916-4013-a65a-ad1d74c4c622" />

### **2.2. Server System Architectures**

#### **2.2.1. Transaction or Query Servers**
*   These are widely used in relational database systems.
*   The typical transaction cycle involves clients sending SQL requests to the server via remote procedure call (RPC), transactions executing at the server, and results shipping back to the client.
*   Transactional RPC allows multiple RPC calls to form a single transaction.

#### **2.2.2. Data Servers**
*   Commonly used in object-oriented database systems and high-speed LANs where clients have comparable processing power to the server and tasks are compute-intensive.
*   Key issues include page-shipping versus item-shipping, locking, data caching, and lock caching.


<img width="472" height="570" alt="image" src="https://github.com/user-attachments/assets/45d8d6eb-0897-40c0-8729-e74323baa991" />

### **2.3. Parallel Systems**

#### **2.3.1. Overview**
*   Parallel database systems consist of multiple processors and multiple disks connected by a fast interconnection network.
*   **Coarse-grain parallel machines:** Consist of a small number of powerful processors.
*   **Massively parallel (fine-grain) machines:** Utilize thousands of smaller processors.

#### **2.3.2. Performance Measures: Speedup and Scaleup**
*   **Speedup:** Executing a fixed-sized problem on a system that is $N$-times larger to reduce elapsed time.
    *   $\text{Speedup} = \frac{\text{small system elapsed time}}{\text{large system elapsed time}}$.
    *   Speedup is linear if it equals $N$.
*   **Scaleup:** Increasing both the problem size and the system size by $N$ to handle larger tasks.
    *   $\text{Scaleup} = \frac{\text{small system small problem elapsed time}}{\text{big system big problem elapsed time}}$.
    *   Scaleup is linear if it equals $1$.

#### **2.3.3. Barriers to Parallelism**
Speedup and scaleup are often sublinear due to:
*   **Startup costs:** The cost of starting up multiple processes may dominate computation time at high degrees of parallelism.
*   **Interference:** Processes compete for shared resources (system bus, disks, locks), leading to wait times.
*   **Skew:** Variance in service times means the overall execution time is determined by the slowest task.

<img width="1108" height="566" alt="image" src="https://github.com/user-attachments/assets/c9fc80fb-22c1-4934-8bf9-184db27d78fc" />

#### **2.3.4. Interconnection Topologies**
*   **Bus:** Components communicate via a single bus; does not scale well.
*   **Mesh:** Nodes arranged in a grid; each connected to adjacent components; scales better.
*   **Hypercube:** Components numbered in binary and connected if representations differ by one bit; reduces communication delays to $\log n$ links.

#### **2.3.5. Parallel Database Architectures**
*   **Shared memory:** Processors share a common memory.
*   **Shared disk:** Processors share a common disk.
*   **Shared nothing:** Processors share neither common memory nor common disk.
*   **Hierarchical:** A hybrid of the above architectures.


<img width="1053" height="578" alt="image" src="https://github.com/user-attachments/assets/8395501d-e72f-4cff-860b-0e69a5861f07" />

### **2.4. Distributed Systems**
*   Data is spread over multiple machines (sites or nodes) interconnected by a network.
*   **Homogeneous Distributed Databases:** Use the same software and schema on all sites; data may be partitioned to provide a view of a single database.
*   **Heterogeneous Distributed Databases:** Use different software or schemas; the goal is to integrate existing databases for useful functionality.
*   **Transactions:**
    *   **Local transaction:** Accesses data only at the site where it was initiated.
    *   **Global transaction:** Accesses data at a different site or across several different sites.

| Advantages of Distributed Systems | Disadvantages of Distributed Systems |
| :--- | :--- |
| **Sharing data:** Users at one site can access data at others. | **Added complexity:** Coordination among sites is difficult. |
| **Autonomy:** Sites retain control over local data. | **Software cost:** Development is more expensive. |
| **Availability:** Redundancy allows functioning if one site fails. | **Bugs/Overhead:** Higher potential for errors and processing overhead. |

---

## **3. Scaling Databases**

### **3.1. The Challenge of Web Applications**
*   Web-based applications and social media (Facebook, Twitter) have large data needs, leading to the rise of cloud-based solutions like Amazon S3.
*   Hooking a traditional RDBMS to these applications can be problematic due to scaling issues.

### **3.2. Vertical vs. Horizontal Scaling**

| Parameters | Horizontal Scaling (Scaling Out) | Vertical Scaling (Scaling Up) |
| :--- | :--- | :--- |
| **Definition** | Adding additional nodes (smaller/cheaper servers) to infrastructure. | Adding additional resources (more power) to a single current machine. |
| **Complexity** | Adds operational complexity (deciding machine roles). | Dataset size may eventually exceed the limits of a single machine. |
| **Hardware** | Easier from a hardware perspective. | Initial costs can be very high. |
| **Resilience** | Increased resilience and fault tolerance; fewer periods of downtime. | Performance increases but limited by single-point constraints. |
| **Software** | May require significant software changes to manage multiple nodes. | Less need for software changes; simpler process communication and maintenance. |

### **3.3. Scaling Out RDBMS Approaches**
*   **Master/Slave:** All writes go to the master; reads are performed against replicated slave databases.
    *   *Issue:* Critical reads may be incorrect if writes haven't propagated; large datasets make duplication difficult.
*   **Sharding (Partitioning):** Distributes data across different processors; scales well for reads and writes.
    *   *Issue:* Not transparent; application must be partition-aware; relationships/joins across partitions are difficult; loss of referential integrity.
*   **Other Options:**
    *   Multi-Master replication.
    *   "INSERT only" strategies (avoiding UPDATES/DELETES).
    *   Eliminating JOINs through de-normalization to reduce query time.
    *   In-memory databases.












---















# 12.4: Non-Relational DBMS: NOSQL



## **Objectives**
*   To understand issues in Big Data.
*   To understand the approach of NOSQL and CAP theorem viz-a-viz ACID.
*   To take tour of common types of NOSQL database.



## **1. What is Big Data?**

### **1.1. Evolution of Storage Capacity**
*   In 1986, global information storage capacity was 2.6 exabytes (99% analog).
*   In 2002, the "beginning of the digital age" occurred, where digital storage reached 50%.
*   By 2007, capacity reached 295 exabytes (94% digital).

### **1.2. The 5V's (Characteristics) of Big Data**
*   **Volume:** The quantity of generated and stored data. The size determines potential insight and if it is considered big data.
*   **Velocity:** The speed at which data is generated and processed to meet demands and challenges.
*   **Variety:** The type and nature of the data (structured, semi-structured, unstructured).
*   **Variability:** The inconsistency of data flows can be periodic with daily, seasonal, and event-triggered peak data loads.
*   **Veracity:** The data quality of captured data can vary greatly, affecting accurate analysis.


<img width="1077" height="557" alt="image" src="https://github.com/user-attachments/assets/2a96ad8c-7178-4c1c-9e77-c141772b0314" />

---

## **2. What is NOSQL?**

### **2.1. Basic Concepts**
*   **Definition:** A NoSQL database provides a mechanism for storage and retrieval of data that is modeled in means other than the tabular relations used in relational databases.
*   **Meaning:** Stands for "Not Only SQL". It is also referred to as Non-Relational DBMS (NDBMS) and Multi-Model Databases.
*   **Origin:** The term was introduced by Carl Strozzi in 1998 for his lightweight open-source relational database and re-introduced by Eric Evans for open-source distributed databases.
*   **Historical Context:** These are evolutions of older models that existed since the late 1960s:
    *   **Network Database Model (NDBMS):** Introduced in 1969; schema viewed as a graph where object types are nodes and relationship types are arcs.
    *   **Hierarchical Database Model (HDBMS):** Organized into a tree-like structure of records connected through links.

### **2.2. Advantages and Disadvantages**

| **Advantages** | **Disadvantages** |
| :--- | :--- |
| Non-relational; don't require schema. | Don't fully support relational features (no join, group by, order by except within partitions). |
| Replicated to multiple nodes (fault-tolerant) and can be partitioned. | No referential integrity constraints across partitions. |
| No single point of failure; down nodes easily replaced. | No declarative query language (like SQL) $\rightarrow$ more programming. |
| Horizontally scalable; cheap and easy to implement (open-source). | Relaxed ACID (CAP theorem) $\rightarrow$ fewer guarantees. |
| Massive write performance and fast key-value access. | No easy integration with other applications that support SQL. |

### **2.3. The "Perfect Storm"**
The rise of NOSQL resulted from three factors coming together:
1. Large datasets.
2. Acceptance of alternatives.
3. Dynamically-typed data.

> [!NOTE]
> NOSQL is not a backlash against RDBMS. For applications like net banking, RDBMS is still used due to strict guarantees. SQL is a rich query language that cannot be rivaled by current NOSQL offerings.

---

## **3. CAP Theorem**

### **3.1. The Three Properties**
For any system sharing data, the CAP theorem defines three properties:
1.  **Consistency (C):** All clients always have the same view of the data. All copies have the same value.
2.  **Availability (A):** Each client can always read and write. Reads and writes always succeed.
3.  **Partition-tolerance (P):** System properties hold even when network failures prevent machines from communicating.

### **3.2. Brewer’s CAP Theorem**
*   It is "impossible" to guarantee all three properties simultaneously for a system sharing data.
*   You can have at most **two** of these three properties.
*   **Tradeoffs:**
    *   **CA:** Traditional RDBMS (MySQL, Postgres). Prioritize C and A over P.
    *   **AP:** Dynamo, Cassandra, CouchDB. Prioritize A and P (results in eventual consistency).
    *   **CP:** BigTable, MongoDB, HBase. Prioritize C and P (availability may be low).

<img width="786" height="524" alt="image" src="https://github.com/user-attachments/assets/af0722d7-a133-4b03-bfd1-feb71e846362" />

### **3.3. Consistency Models**
*   **Strong Consistency (ACID):** Atomicity, Consistency, Isolation, Durability.
*   **Weak Consistency (BASE):**
    *   **B**asically **A**vailable.
    *   **S**oft-state.
    *   **E**ventual consistency: When no updates occur for a long time, all updates eventually propagate and nodes become consistent.

---

## **4. Types of NOSQL Databases**

### **4.1. Key-value Stores**
*   **Concept:** Work by matching keys with values, similar to a dictionary. No structure or relations.
*   **Data Model:** Global collection of Key-value pairs. Attributes can be single-valued or multi-valued like a set.
*   **Basic API access:** `get(key)`, `put(key, value)`, `delete(key)`, `execute(key, operation, parameters)`.
*   **Pros:** Very fast, very scalable, simple data model, fault-tolerant.
*   **Cons:** Cannot model complex data structures like objects.
*   **Examples:** DynamoDB (Amazon), Redis (Salvatore Sanfilippo), Voldemort (LinkedIn), SimpleDB (Amazon), MemcacheDB.

### **4.2. Document Stores**
*   **Concept:** Inspired by Lotus Notes. Collection of documents (JSON, XML, or semi-structured formats).
*   **Features:** Allows deep nesting and complex structures (document within a document).
*   **Querying:** Manipulations with objects in collections (find, delete, update) via selections or MapReduce.
*   **Examples:** MongoDB (10gen), Couchbase/CouchDB.


<img width="1100" height="324" alt="image" src="https://github.com/user-attachments/assets/c7925534-69e7-4faa-b861-2df9296bb725" />

### **4.3. Column Stores**
*   **Concept:** Based on Google's BigTable paper. Like column-oriented RDBMS but with a twist.
*   **Data Model:** Collection of **Column Families**. 
    *   Column family = (key, value) where value = set of related columns.
    *   Indexed by row key, column key, and timestamp (vital for eventual consistency).
*   **Data Locality:** All columns in a family are stored together on disk, so multiple rows can be retrieved in one read operation.
*   **Examples:** BigTable (Google), Cassandra (Apache/originally Facebook), HBase (Apache), PNUTS (Yahoo).

### **4.4. Graph Stores**
*   **Concept:** Focus on modeling interconnectivity (structure) of data. Inspired by mathematical Graph Theory $G=(E, V)$.
*   **Data Model:** Property Graph consisting of nodes and edges. 
    *   Nodes: May have properties (including ID).
    *   Edges: May have labels or roles. 
    *   Both can have key-value pairs.
*   **Interfaces:** Vary from single-step to full recursion/path expressions.
*   **Examples:** Neo4j, OrientDB, InfoGrid, FlockDB, Pregel.

---

## **5. Relational vs. Non-Relational Comparison**

| **Parameter** | **Relational** | **Non-Relational** |
| :--- | :--- | :--- |
| **Data Model** | Structured data (tables). | Unstructured and semi-structured (XML, JSON). Dynamic schema. |
| **Speed/Complexity**| Faster for simple operations; cheaper and less complex. | Supports more operations; highly complex internally and costlier. |
| **Scalability** | Distributed architecture incurs data integrity issues at all levels. | Better scalability via sharding and partitioning in distributed systems. |
| **Consistency** | Enforces strong consistency (ACID). | Enforces eventual consistency (BASE). |
| **Integration** | Fits into Enterprise IT stack; secure and robust. | Designed for agility in modern cloud-based Web 2.0 applications. |



<img width="617" height="634" alt="image" src="https://github.com/user-attachments/assets/9939a890-ced6-4cf6-8990-f1a68617302e" />

<img width="855" height="648" alt="image" src="https://github.com/user-attachments/assets/1a9c4ff9-1a76-4fb5-bd0a-d6da968a9292" />
