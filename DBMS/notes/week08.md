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






---





# 8.2: Algorithms and Data Structures/2: Data Structures



## **Objectives**
*   Introduction to Data Structures.
*   Review of linear data structures: array, list, stack, and queue.
*   Review of search: linear and binary.



## **1. Data Structure Overview**

### **1.1. Definition**
*   **Data Structure:** A data structure specifies the way of organizing and storing in-memory data that enables efficient access and modification of the data.
*   Broadly classified into two categories:
    1.  **Linear Data Structures**.
    2.  **Non-linear Data Structures**.

### **1.2. Key Operations in Data Management**
For applications relating to data management, a data structure typically acts as a container for data and supports five key operations:
1.  **Create:** Initialize the structure.
2.  **Insert:** Add a data element.
3.  **Delete:** Remove a data element.
4.  **Find / Search:** Locate a data element by value.
5.  **Close:** Wrap up and release the data structure when no longer required.


---

## **2. Linear Data Structures**

### **2.1. Definition**
*   A linear data structure has data elements arranged in a linear or sequential manner such that each member element is connected to its previous and next element.
*   Elements are traversable through a single run.
*   Examples include Array, Linked List, Queue, and Stack.

### **2.2. Array**
*   **Storage:** Data elements are stored at contiguous memory locations.
*   **Access:** Allows random access using an index, which is fast with a cost of $O(1)$.
*   **Flexibility:** Arrays have fixed sizes and are not flexible. If created too large, memory is wasted; if too small, some elements cannot be accommodated.
*   **Insertion and Removal:**
    *   Adding or removing from the **end** of an array is easy ($O(1)$).
    *   Adding or removing at an **any arbitrary position** is costly ($O(n)$) because elements must be moved to maintain consecutive memory locations.

<img width="896" height="140" alt="image" src="https://github.com/user-attachments/assets/7e4605f8-921a-4bf8-aae0-2f7e9eba7a85" />

### **2.3. Linked List**
*   **Storage:** Elements are not required to be stored at contiguous locations. A new element can be stored anywhere in memory where free space is available.
*   **Mechanism:** Each element is stored in a **node** consisting of two parts:
    1.  **Info:** Stores the actual element.
    2.  **Link:** Stores the location (pointer or reference) of the next node.
*   **Header:** A link pointing to the first node of the list.
*   **Access:** Random access is not possible; elements must be accessed sequentially ($O(n)$).
*   **Flexibility:** Size is flexible; it grows or shrinks as elements are inserted or deleted.
*   **Insertion and Removal:**
    *   Efficient ($O(1)$) for arbitrary positions provided the location of the node is already known, as no elements need to be physically moved.

### **2.4. Queue and Stack**
*   **Queue:** A **FIFO** (First In First Out) data structure. The element inserted first is removed first.
*   **Stack:** A **LIFO** (Last In First Out) data structure. The element inserted last is removed first.
*   The definition of these structures is based on their **operations** (e.g., push/pop for stacks) rather than the underlying container (which could be an array or a linked list).

---

## **3. Search Algorithms**

### **3.1. Linear Search**
*   **Process:** The algorithm starts with the first element and compares it with the given key value. If they match, it returns "yes." If not, it proceeds sequentially through the list until a match is found or the full list is traversed.
*   **Example:**
    *   Input: `inputArr = ['a', 'c', 'a', 'd', 'e', 'm', 'i', 'c', 's']`
    *   Search Key: `'i'`
    *   Result: Match found at Step 7.
*   **Complexity:** $O(n)$.

### **3.2. Binary Search**
*   **Constraint:** Requires a **sorted list** as input.
*   **Process:**
    1.  Compare key $k$ with the middle element.
    2.  If matches, return the index.
    3.  If $k >$ middle, repeat on the right sub-list.
    4.  If $k <$ middle, repeat on the left sub-list.
*   **Complexity:** $O(\log n)$.
    *   *Derivation:* The search space is divided by 2 at each step. For $n=16$, it takes 4 steps ($\log_2 16 = 4$) to reach a list of size 1.

<img width="1237" height="609" alt="image" src="https://github.com/user-attachments/assets/79878b83-cc0b-42ad-a932-507e23b40924" />

---

## **4. Performance Summary and Comparison**

### **4.1. Comparison Table (Linear Data Structures)**

| Operation | Array (Unordered) | Array (Ordered) | Linked List (Unordered) | Linked List (Ordered) |
| :--- | :--- | :--- | :--- | :--- |
| **Access** | $O(1)$ | $O(1)$ | $O(n)$ | $O(n)$ |
| **Insert** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Delete** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Search** | $O(n)$ | $O(\log n)$ | $O(n)$ | $O(n)$ |


### **4.2. Complexity of Operations (Average and Worst Case)**

| Data Structure | Average Access | Average Search | Average Insertion | Average Deletion | Worst Access | Worst Search |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Array** | $\Theta(1)$ | $\Theta(n)$ | $\Theta(n)$ | $\Theta(n)$ | $O(1)$ | $O(n)$ |
| **Stack** | $\Theta(n)$ | $\Theta(n)$ | $O(1)$ | $\Theta(1)$ | $O(n)$ | $O(n)$ |
| **Queue** | $\Theta(n)$ | $\Theta(n)$ | $O(1)$ | $\Theta(1)$ | $O(n)$ | $O(n)$ |
| **Singly-Linked List**| $\Theta(n)$ | $\Theta(n)$ | $\Theta(1)$ | $\Theta(1)$ | $O(n)$ | $O(n)$ |
| **Doubly-Linked List**| $\Theta(n)$ | $\Theta(n)$ | $\Theta(1)$ | $\Theta(1)$ | $O(n)$ | $O(n)$ |

<img width="1216" height="552" alt="image" src="https://github.com/user-attachments/assets/684ff02e-5954-4c49-8c47-ba5e34a363fb" />



---






# 8.3: Algorithms and Data Structures/3: Data Structures/2


## **Objectives**
*   Introducing Non-linear Data Structures - graph, tree, hash table.
*   Exploring Binary Search Tree.
*   Comparing Linear and Non-Linear Data Structures.



## **2. Non-linear Data Structures**

### **2.1. Motivation: Why Non-linear?**
From the study of Linear data structures, we observe:
*   All have space complexity $O(n)$, which is optimal.
*   Linked lists have a space overhead (approximately 100% in terms of pointers).
*   Complexity is often identical for Worst as well as Average case.
*   They offer satisfactory complexity for some operations while being unsatisfactory on others.

**Comparison of Linear Data Structures:**
| Operation | Array (Unordered) | Array (Ordered) | Linked List (Unordered) | Linked List (Ordered) |
| :--- | :--- | :--- | :--- | :--- |
| **Access** | $O(1)$ | $O(1)$ | $O(n)$ | $O(n)$ |
| **Insert** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Delete** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ |
| **Search** | $O(n)$ | $O(\log n)$ | $O(n)$ | $O(n)$ |


**Conclusion:** Non-Linear data structures can be used to trade-off between these extremes and achieve a balanced good performance for all operations.

<img width="790" height="255" alt="image" src="https://github.com/user-attachments/assets/c62b927f-3840-4cf2-a877-3e4dc4e93f15" />

### **2.2. General Definition**
*   Data items are **not arranged in a sequence**.
*   Each element may have **multiple paths** to connect to other elements.
*   Traversing through the elements is not possible in one run.

### **2.3. Common Non-Linear Structures**

#### **2.3.1. Graph**
*   A collection of vertices $V$ (storing elements) and connecting edges (links) $E$: $G = <V, E>$ where $E \subseteq V \times V$.
*   **Variants:** Undirected/Directed, Cyclic/Acyclic, Unweighted/Weighted, Disconnected/Connected.
*   **Examples:** ER Diagrams, Networks (Electrical/Water), Facebook Friendships, Knowledge Graphs.

#### **2.3.2. Tree**
*   A **connected acyclic graph** representing hierarchical relationships.
*   **Variants:** Rooted/Unrooted, Binary/n-ary, Balanced/Unbalanced.
*   **Examples:** Composite Attributes, Family Genealogy, Search Trees.

#### **2.3.3. Hash Table**
*   Implements an **associative array** abstract data type.
*   Uses a **hash function** to compute an index (hash code) into an array of buckets or slots.
*   **Examples:** Associative arrays, Database indexing, Caches.

---

## **3. Tree Terminology and Properties**

### **3.1. Key Definitions**
*   **Root:** The node at the top; only one per tree.
*   **Parent:** The predecessor of any node. Every node except the Root has a unique parent.
*   **Child:** The descendant of a node.
*   **Leaf:** A node with no children.
*   **Internal Node:** A node with at least one child.
*   **Siblings:** Nodes having the same parent.
*   **Arity (Degree):** Number of children of a node. The maximum arity of a node defines the arity of the tree.
*   **Levels:** Root is at Level 0. Level is the distance from the root.
*   **Height:** The maximum level in a tree.

### **3.2. Binary Tree Facts**
*   A tree with $n$ nodes has $n-1$ edges.
*   The maximum number of nodes at level $l$ is $2^l$.
*   If $h$ is the height of a binary tree of $n$ nodes:
    *   $h + 1 \le n \le 2^{h+1} - 1$
    *   $\lceil \log(n+1) \rceil - 1 \le h \le n - 1$
    *   $O(\log n) \le h \le O(n)$
*   For a $k$-ary tree, height is $O(\log_k n) \le h \le O(n)$.

---

## **4. Binary Search Tree (BST)**

### **4.1. Concept: Adaptation of Binary Search**
*   Binary search splits an array; conceptually, the **Middle Element** is the Root, and the left/right sub-arrays are sub-trees.
*   By progressing recursively, we form a Binary Search Tree.

<img width="1043" height="637" alt="image" src="https://github.com/user-attachments/assets/e04d1805-740b-4a40-9a48-000cc7dd88c4" />

### **4.2. Definition**
*   **BST Rule:** For any node with value $X$:
    *   Every node in the **left sub-tree** has a value $< X$.
    *   Every node in the **right sub-tree** has a value $> X$.
*   **Node Structure:** Each node consists of an element, a link to the left child (LC), and a link to the right child (RC).

### **4.3. Building a BST**
**Example Sequence:** E, L, P, H, A, N, T
1.  Insert **E**: Becomes Root.
2.  Insert **L**: $L > E$, goes to Right Child.
3.  Insert **P**: $P > E, P > L$, goes to Right Child of L.
4.  Insert **H**: $H > E, H < L$, goes to Left Child of L.
5.  Insert **A**: $A < E$, goes to Left Child of E.
6.  Insert **N**: $N > E, N > L, N < P$, goes to Left Child of P.
7.  Insert **T**: $T > E, T > L, T > P$, goes to Right Child of P.

<img width="1104" height="592" alt="image" src="https://github.com/user-attachments/assets/fd75f321-7925-4606-9e97-5f2021e01994" />
<img width="951" height="366" alt="image" src="https://github.com/user-attachments/assets/84c4ab94-262a-4db7-a240-8ac3c43cfcc3" />
<img width="1036" height="539" alt="image" src="https://github.com/user-attachments/assets/59b1bf0f-84bd-4c43-b50c-e1947f4f586f" />

### **4.4. Searching a Key**
*   **Algorithm:** Compare key with root. If equal, return node. If smaller, recurse left; if larger, recurse right.
*   **Complexity:** $O(h)$ where $h$ is the height.
*   **Worst Case:** $O(n)$. Occurs if keys are inserted in sorted order, creating a **skewed binary search tree** ($h = n-1$).
*   **Best Case:** $O(\log n)$. Occurs if the tree is **balanced**.

---

## **5. Comparison of Linear and Non-Linear Data Structures**

| Feature | Linear Data Structure | Non-Linear Data Structure |
| :--- | :--- | :--- |
| **Arrangement** | Linear/Sequential order; attached to previous/next. | Hierarchical or networked manner. |
| **Levels** | Single level involved. | Multiple levels involved. |
| **Implementation** | Easy. | Complex. |
| **Traversal** | Single way only. | Multiple ways (Depth-First, Breadth-First, Inorder, etc.). |
| **Examples** | Array, Stack, Queue, Linked List. | Trees, Graphs, Skip List, Hash Map. |




---





# **8.4: Storage and File Structure/1: Physical Storage**



## **Objectives**
*   Introduce various Physical Storage Media for high volume, fast, reliable and inexpensive options for data storage for databases.
*   To understand the options of Tertiary Storage for high volume, inexpensive backup options.

---

## **1. Physical Storage Media**

### **1.1. Classification of Physical Storage Media**
Physical storage media are classified based on the following parameters:
*   **Speed** with which data can be accessed.
*   **Cost** per unit of data.
*   **Reliability**:
    *   Data loss on power failure or system crash.
    *   Physical failure of the storage device.

**Storage Differentiation:**
*   **Volatile storage:** Loses contents when power is switched off.
*   **Non-volatile storage:** Contents persist even when power is switched off. Includes secondary and tertiary storage, as well as battery-backed up main-memory.

---

### **1.2. Types of Physical Storage Media**

#### **1.2.1. Cache**
*   Fastest and most costly form of storage.
*   Volatile.
*   Managed by the computer system hardware.

#### **1.2.2. Main Memory**
*   Fast access (10's to 100's of nanoseconds (ns); $1ns = 10^{-9}$ seconds).
*   Generally too small (or too expensive) to store the entire database.
*   Capacities of up to a few Gigabytes widely used currently.
*   Volatile: Contents are usually lost if a power failure or system crash occurs.

#### **1.2.3. Flash Memory**
*   Data survives power failure.
*   Data can be written at a location only once, but location can be erased and written to again.
*   Can support only a limited number (10K – 1M) of write/erase cycles.
*   Erasing of memory has to be done to an entire bank of memory.
*   Reads are roughly as fast as main memory.
*   Writes are slow (few microseconds), erase is slower.
*   Widely used in embedded devices such as digital cameras, phones, and USB keys.

#### **1.2.4. Magnetic Disk**
*   Data is stored on spinning disk, and read/written magnetically.
*   Primary medium for the long-term storage of data; typically stores entire database.
*   Data must be moved from disk to main memory for access, and written back for storage - much slower access than main memory.
*   **Direct-access:** Possible to read data on disk in any order, unlike magnetic tape.
*   Capacities range up to roughly 16–32 TB.
*   Much larger capacity and much lower cost/byte than main memory/flash memory.
*   Survives power failures and system crashes; disk failure can destroy data, but is rare.

#### **1.2.5. Optical Storage**
*   Non-volatile, data is read optically from a spinning disk using a laser.
*   CD-ROM (640 MB) and DVD (4.7 to 17 GB) are most popular forms.
*   Blu-ray disks: 27 GB to 54 GB.
*   **WORM (Write-once, read-many):** Optical disks used for archival storage (CD-R, DVD-R, DVD+R).
*   Multiple write versions (CD-RW, DVD-RW, DVD+RW, and DVD-RAM).
*   Reads and writes are slower than with magnetic disk.
*   Juke-box systems use large numbers of removable disks and automatic mechanisms for loading/unloading large volumes of data.

#### **1.2.6. Tape Storage**
*   Non-volatile, used primarily for backup (to recover from disk failure) and for archival data.
*   **Sequential-access:** Much slower than disk.
*   Very high capacity (40 to 300 TB tapes available).
*   Tape can be removed from drive; storage costs are much cheaper than disk, but drives are expensive.
*   Tape jukeboxes available for storing massive amounts of data (hundreds of TB to multiple PB).

---

### **1.3. Storage Hierarchy**

<img width="1130" height="614" alt="image" src="https://github.com/user-attachments/assets/39eb6fdf-e1f4-4460-aa32-42ed1574a350" />

*   **Primary storage:** Volatile, very fast (Cache, Main Memory).
*   **Secondary storage (on-line storage):** Non-volatile, moderately fast (Flash memory, Magnetic disk).
*   **Tertiary storage (off-line storage):** Non-volatile, slow (Optical disk, Magnetic tapes).

---

## **2. Magnetic Disk Details**

### **2.1. Mechanism**

<img width="746" height="615" alt="image" src="https://github.com/user-attachments/assets/5aedb5a8-f28c-4208-a22b-9d171bd12970" />

*   **Read-write head:** Positioned very close to the platter surface to read or write magnetically encoded information.
*   **Tracks:** Surface of platter divided into circular tracks (50K-100K tracks per platter).
*   **Sectors:** Each track is divided into sectors, the smallest unit of data read or written (typically 512 bytes).
*   **Head-disk assemblies:** Multiple disk platters (1 to 5) on a single spindle with one head per platter mounted on a common arm.
*   **Cylinder:** Cylinder $i$ consists of the $i^{th}$ track of all the platters.

<img width="480" height="567" alt="image" src="https://github.com/user-attachments/assets/186bb072-8fc6-457a-8410-8d50a8b84f48" />

### **2.2. Disk Controller and Subsystems**
*   **Disk Controller:** Interfaces between the computer system and the disk drive hardware.
    *   Accepts high-level commands to read or write a sector.
    *   Initiates arm movement to the right track.
    *   Attaches checksums to verify correct read back.
*   **Disk Subsystem:** Performs remapping of bad sectors.
*   **Interface Standards:** ATA, SATA, SCSI, SAS, and variants.
*   **Network Storage:**
    *   **Storage Area Networks (SAN):** Connects disks via high-speed network to multiple servers.
    *   **Network Attached Storage (NAS):** Provides a file system interface using networked file system protocols.

### **2.3. Performance Measures**
*   **Access Time:** Time from a read/write request issue to start of data transfer.
    *   **Seek Time:** Time to reposition arm over correct track (4 to 10 ms).
    *   **Rotational Latency:** Time for sector to appear under the head (4 to 11 ms for 5400 to 15000 rpm).
*   **Data-transfer Rate:** Rate for data retrieval or storage (typically 25 to 100 MB/s).
*   **Reliability:** Measured by **Mean Time To Failure (MTTF)**.
    *   Theoretical MTTF for new disks ranges from 500,000 to 1,200,000 hours.

---

## **3. Cloud Storage**

### **3.1. Definition**
*   Purchased from a third-party vendor who owns and operates capacity delivered over the Internet in a pay-as-you-go model.
*   Accessed via traditional storage protocols or directly via an API.
*   **Options:** Google Drive, Amazon Drive, OneDrive, Evernote, Dropbox.

### **3.2. Cloud vs. Traditional Storage**

| Parameters | Cloud Storage | Traditional Storage |
| :--- | :--- | :--- |
| **Cost** | Cheaper per GB than external drives. | High hardware/infrastructure costs; upgrades add costs. |
| **Reliability** | Highly reliable; less time to get functioning. | Requires high initial effort and is less reliable. |
| **File Sharing** | Supports dynamic sharing with network access. | Requires physical drives and established networks. |
| **Accessibility** | Access files from anywhere. | Restricted to local access. |
| **Backup/Recovery** | Safe from on-site disasters. | Locally stored data/backups susceptible to unexpected events. |

---

## **4. Flash-Based Storage and SSDs**

### **4.1. Flash Storage Types**
*   **NAND flash:** Predominant for storage; cheaper than NOR flash; requires page-at-a-time reads.
*   **Wear Leveling:** Remapping logical page addresses to physical page addresses to distribute erase cycles.

### **4.2. Solid-State Drives (SSD) vs. HDD**
*   SSDs use flash-based memory with no moving parts.

| Parameters | SSD | HDD |
| :--- | :--- | :--- |
| **Technology** | Integrated circuit using Flash memory. | Mechanical parts (spinning disks/platters). |
| **Access Time** | 0.1 ms. | 5.5 - 8.0 ms. |
| **Avg. Seek Time** | 0.08 - 0.16 ms. | < 10 ms. |
| **Random I/O** | 6000 io/s. | 400 io/s. |
| **Reliability** | Failure rate < 0.5%. | Failure rate 2 - 5%. |
| **Energy** | 2 to 5 watts. | 6 to 15 watts. |

---

## **5. Future of Storage**
*   **DNA Digital Storage:** Encoding binary data into synthetic strands of DNA using nucleotides (A, T, G, C). Extremely high density (1 exabyte could fit in a palm) and stable, but currently expensive and slow.
*   **Quantum Memory:** Uses quantum superposition and quantum bits (qubits), providing more flexibility for quantum algorithms than classical storage.
in terms of DNA and Quantum.





---





# 8.5: Storage and File Structure/2: File Structure


## **Objectives**
*   To familiarize with the organization for database files.
*   To understand how records and relations are organized in files.
*   To learn how databases keep their own information in Data-Dictionary Storage – the metadata database of a database.
*   To understand the mechanisms for fast access of a database store.




## **1. File Organization**

### **1.1. Basic Concepts**
*   A database is a collection of files.
*   A file is a sequence of records.
*   A record is a sequence of fields.
*   **One approach:**
    *   Assume record size is fixed.
    *   Each file has records of one particular type only.
    *   Different files are used for different relations.
*   A database file is partitioned into fixed-length storage units called **blocks**.
*   Blocks are units of both storage allocation and data transfer.

### **1.2. Fixed-Length Records**
*   **Simple approach:** Store record $i$ starting from byte $n*(i-1)$, where $n$ is the size of each record.
*   **Access:** Simple, but records may cross blocks.
*   **Modification:** Do not allow records to cross block boundaries.

<img width="531" height="357" alt="image" src="https://github.com/user-attachments/assets/af09d467-63aa-4b2c-922d-6fabf068b938" />

*   **Deletion of record $i$ Alternatives:**
    1.  Move records $i+1, \dots, n$ to $i, \dots, n-1$ (Compaction).
    2.  Move record $n$ to $i$.
    3.  Do not move records, but link all free records on a **free list**.

<img width="1185" height="425" alt="image" src="https://github.com/user-attachments/assets/a88ab841-93b7-4c33-81c4-2b1505d799b8" />
<img width="1200" height="427" alt="image" src="https://github.com/user-attachments/assets/d2065f3a-f764-4471-8c76-6761238707f2" />

### **1.3. Free Lists**
*   Store the address of the first deleted record in the file header.
*   Use this first record to store the address of the second deleted record, and so on.
*   Addresses act as pointers to the location of a record.
*   **Space efficiency:** Reuse space for normal attributes of free records to store pointers (no pointers stored in in-use records).

<img width="650" height="407" alt="image" src="https://github.com/user-attachments/assets/2f18f970-4cdd-4a72-8876-2e0822ebda8f" />

### **1.4. Variable-Length Records**
*   **Arise from:**
    *   Storage of multiple record types in a file.
    *   Record types that allow variable lengths for fields like strings (`varchar`).
    *   Record types that allow repeating fields.
*   **Representation:**
    *   Attributes are stored in order.
    *   Variable length attributes represented by fixed size **(offset, length)**, with actual data stored after all fixed length attributes.
    *   **Null values:** Represented by a null-value bitmap.

<img width="1054" height="219" alt="image" src="https://github.com/user-attachments/assets/60c56039-ed4e-4e28-993d-f62ce208d699" />

### **1.5. Slotted Page Structure**
The **Slotted Page header** contains:
*   Number of record entries.
*   End of free space in the block.
*   An array containing the location and size of each record.
*   Records can be moved around within a page to keep them contiguous; entry in the header must be updated.
*   Pointers should not point directly to a record; instead, they point to the entry for the record in the header.

<img width="820" height="294" alt="image" src="https://github.com/user-attachments/assets/9bdf5af8-c3bd-42c4-b002-12a610aa48df" />

---

## **2. Organization of Records in Files**

*   **Heap:** A record can be placed anywhere in the file where there is space.
*   **Sequential:** Store records in sequential order, based on the value of the search key of each record.
    *   Suitable for applications requiring sequential processing of the entire file.
    *   Records are ordered by a **search-key**.
    *   **Deletion:** Use pointer chains.
    *   **Insertion:** Locate the position; insert in free space or in an **overflow block** if no space exists.
    *   Need to reorganize the file from time to time to restore sequential order.

<img width="715" height="483" alt="image" src="https://github.com/user-attachments/assets/41ffb2f5-4208-4ad2-8f79-ff4cfbefd222" />

*   **Hashing:** A hash function computed on some attribute of each record; the result specifies in which block of the file the record should be placed.
*   **Multitable Clustering File Organization:** Records of several different relations are stored in the same file.
    *   **Motivation:** Store related records on the same block to minimize $I/O$.
    *   Good for queries involving `department` $\bowtie$ `instructor`.
    *   Bad for queries involving only `department`.
    *   Results in variable size records.

<img width="1091" height="611" alt="image" src="https://github.com/user-attachments/assets/c79d7daa-1ef0-45aa-ac15-ee9864d6fafa" />

---

## **3. Data Dictionary Storage**

The **Data Dictionary** (System Catalog) stores **metadata** (data about data) such as:
*   **Information about relations:** Names, types and lengths of attributes, views, and integrity constraints.
*   **User and accounting information:** Including passwords.
*   **Statistical and descriptive data:** Number of tuples in each relation.
*   **Physical file organization information:** How a relation is stored (sequential/hash/...) and its physical location.
*   **Information about indices**.

### **3.1. Relational Representation of Metadata**

<img width="728" height="583" alt="image" src="https://github.com/user-attachments/assets/492b8f93-951c-4f17-bea8-c06382951f12" />


---

## **4. Storage Access**

*   A database file is partitioned into fixed-length storage units called **blocks**.
*   Database systems seek to minimize the number of block transfers between the disk and memory.
*   **Buffer:** Portion of main memory available to store copies of disk blocks.
*   **Buffer Manager:** Subsystem responsible for allocating buffer space in main memory.

### **4.1. Buffer Manager Tasks**
*   Programs call the buffer manager when they need a block from disk.
*   If the block is already in the buffer, return the address in main memory.
*   If not in the buffer:
    1.  Allocate space (possibly replacing/throwing out another block).
    2.  Write back the replaced block **only if it was modified**.
    3.  Read the block from disk to buffer and return the address.

### **4.2. Buffer Replacement Policies**
*   **Least Recently Used (LRU strategy):** Replace the block that hasn't been used for the longest period of time. Uses past patterns as a predictor for future references.
*   **Pinned block:** Memory block that is not allowed to be written back to disk.
*   **Toss-immediate strategy:** Frees the space occupied by a block as soon as the final tuple of that block has been processed.
*   **Most recently used (MRU) strategy:** System pins the block currently being processed; unpinned after the final tuple is processed, becoming the most recently used block.
