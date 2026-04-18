# 9.1: Indexing and Hashing/1: Indexing/1


## **Recap**
*   Need for algorithm analysis, Asymptotic complexity, and Worst-case, average-case and best-case analysis.
*   Reviewed Linear Data Structures; array, list, stack, queue; and linear and binary search.
*   Reviewed Non-linear Data Structures - graph, tree, hash table; Binary Search Tree; and compared Linear and Non-Linear Data Structures.
*   Understood the range of Physical Storage Media.
*   Studied about Magnetic Disks and Magnetic Tape.
*   Glimpsed through Other Storage and the Future of Storage.
*   Familiarized with the organization for database files.
*   Understood how records and relations are organized in files.
*   Learnt how databases keep their own information in Data-Dictionary Storage – the metadata database of a database.
*   Understood the mechanisms for fast access of a database store.

---

## **Objectives**
*   To understand the reasons for which we need to index database tables.
*   To learn about the ordered indexes and Indexed Sequential Access Mechanism (ISAM).


---

## **1. Concepts of Indexing**

### **1.1 Search Records: Motivation Example**
Consider a table: `Faculty(Name, Phone)`.
*   **Searching on Name:** To get the phone number for 'Pabitra Mitra', if the table is not sorted, the system must search sequentially (Order $n$). If an index on "Name" is available (sorted on Name), we can search 'Pabitra Mitra' using binary search and navigate on the pointer rec\# to find the record.
*   **Searching on Phone:** To get the name of the faculty having phone number $= 84772$, if the table is not sorted on Phone, we must search linearly. A "Phone" index (sorted on Phone) allows searching '$84772$' and navigating on the pointer rec\#
*   **Constraint:** We can keep the actual records sorted on 'Name' or on 'Phone' (called the primary index), but not on both.

<img width="797" height="415" alt="image" src="https://github.com/user-attachments/assets/818f36eb-1199-4409-93d3-c35ac79f6f3b" />

### **1.2 Basic Concepts**
*   Indexing mechanisms are used to speed up access to desired data.
*   **Search Key:** Attribute or set of attributes used to look up records in a file.
*   **Index Entry (or Index Record):** Consists of a search-key value and a pointer.
    *   The pointer consists of the identifier of a disk block and an offset within the disk block.
*   Index files are typically much smaller than the original file.
*   **Two basic kinds of indices:**
    1.  **Ordered indices:** Search keys are stored in sorted order.
    2.  **Hash indices:** Search keys are distributed uniformly across buckets using a hash function.

### **1.3 Index Evaluation Metrics**
*   **Access types:** Supported efficiently (e.g., records with a specified value or in a specified range).
*   **Access time:** Time taken to find a data item or set of items.
*   **Insertion time:** Time to find the correct place to insert and update the index structure.
*   **Deletion time:** Time to find the item to delete and update the index structure.
*   **Space overhead:** Additional space occupied by the index structure.

---

## **2. Ordered Indices**

### **2.1 Definitions**
*   **Primary index (Clustering index):** In a sequentially ordered file, the index whose search key also defines the sequential order of the file.
    *   The search key of a primary index is usually but not necessarily the primary key.
*   **Secondary index (Non-clustering index):** An index whose search key specifies an order different from the sequential order of the file.
*   **Index-sequential file:** An ordered sequential file with a primary index.

### **2.2 Dense Index Files**
*   **Dense index:** An index record appears for every search-key value in the file.
*   **Dense clustering index:** The index record contains the search-key value and a pointer to the first data record with that value. Subsequent records with the same value follow sequentially.
*   **Dense non-clustering index:** The index must store a list of pointers to all records with the same search-key value.

<img width="971" height="472" alt="image" src="https://github.com/user-attachments/assets/22cc2971-7e8f-4e21-9f3e-28211504d721" />
<img width="944" height="429" alt="image" src="https://github.com/user-attachments/assets/32a9404e-142d-4460-8391-91590ef29251" />

### **2.3 Sparse Index Files**
*   **Sparse index:** Contains index records for only some search-key values.
*   **Applicability:** Only if the relation is stored in sorted order of the search key (clustering index).
*   **To locate a record with search-key value $K$:**
    1.  Find the index record with the largest search-key value $\le K$.
    2.  Search the file sequentially starting at the record pointed to by that index record.
*   **Comparison:**
    *   Less space and less maintenance overhead for insertions/deletions.
    *   Generally slower than a dense index for locating records.
*   **Tradeoff:** A good compromise is a sparse index with one index entry per block of the file, corresponding to the least search-key value in the block.

<img width="1020" height="511" alt="image" src="https://github.com/user-attachments/assets/496ed6dc-ca78-4189-8288-ba401a00444c" />
<img width="995" height="511" alt="image" src="https://github.com/user-attachments/assets/c5987ad5-1aa8-4101-8cb2-32df0e95ff20" />

### **2.4 Secondary Indices**
*   Secondary indices must be **dense**, containing an index entry for every search-key value and a pointer to every record in the file.
*   **Non-unique search keys:** One way to implement these is using an extra level of indirection. The index record points to a **bucket** that contains pointers to all actual records with that search-key value.

<img width="1023" height="490" alt="image" src="https://github.com/user-attachments/assets/f605013f-94b3-4780-b8d8-17f6a2554121" />


### **2.5 Multilevel Index**
*   If the primary index is too large to fit in main memory, searching it becomes expensive (e.g., binary search on $b$ blocks requires $\lceil \log_2(b) \rceil$ random $I/O$ operations).
*   **Solution:** Treat the primary index kept on disk as a sequential file and construct a sparse index on it.
    *   **Outer index:** A sparse index of the primary index.
    *   **Inner index:** The original primary index file.
*   This can be extended to multiple levels if the outer index is still too large.


<img width="520" height="518" alt="image" src="https://github.com/user-attachments/assets/8202da70-0d25-401a-9a4a-548c30fd49e7" />


---

## **3. Index Update**

### **3.1 Deletion**
*   If the deleted record was the only record with its search-key value, the search-key is deleted from the index.
*   **Dense indices:** Deletion of the search-key is similar to file record deletion.
*   **Sparse indices:** If an entry for the search-key exists, it is replaced with the next search-key value in the file (in search-key order). If that next value already has an entry, the deleted entry is simply removed.

### **3.2 Insertion**
*   Perform a lookup using the search-key value of the new record.
*   **Dense indices:** If the search-key value does not appear, insert an index entry at the appropriate position.
*   **Sparse indices:** If the index stores an entry for each block, no change is needed unless a new block is created.
    *   If a new block is created, the first search-key value appearing in the new block is inserted into the index.
*   **Multilevel indices:** Insertion/deletion algorithms are simple extensions where the lowest level is updated first, and changes propagate to higher levels as if the lower level were a file of records.






---








# 9.2: Indexing and Hashing/2: Indexing/2



## **Objectives**
*   To recap Balanced Binary Search Trees as options for optimal in-memory search data structures.
*   To understand the issues relating to external search data structures for persistent data.
*   To study 2-3-4 Tree as a precursor to $B/B^+$-Tree for an efficient external data structure for database and index tables.




## **1. Search Data Structures Recap**

### **1.1 In-Memory Search Complexity**
How to search a key in a list of $n$ data items?
*   **Linear Search:** $O(n)$. For example, finding '28' in an unordered list of 16 items might take 16 comparisons.
*   **Binary Search:** $O(\log n)$. Finding '28' in a sorted array of 16 items takes 4 comparisons (e.g., checking 25, 36, 30, 28).
*   **Binary Search Tree (BST):** Search is performed recursively on the left or right subtrees.

*   <img width="791" height="424" alt="image" src="https://github.com/user-attachments/assets/f73db50f-cd32-47e9-a542-547edd9fddee" />


### **1.2 Complexity Comparison Table**
Worst-case time with $n$ data items:

| Data Structure | Search | Insert | Delete | Remarks |
| :--- | :--- | :--- | :--- | :--- |
| **Unordered Array** | $O(n)$ | $O(1)$ | $O(1)$ | Insert/Delete time is |
| **Ordered Array** | $O(\log n)$| $O(n)$ | $O(n)$ | the time after the |
| **Unordered List** | $O(n)$ | $O(1)$ | $O(1)$ | location has been |
| **Ordered List** | $O(n)$ | $O(1)$ | $O(1)$ | ascertained by Search. |
| **Binary Search Tree**| $O(h)$ | $O(1)$ | $O(1)$ | |



### **1.3 The Need for Balancing**
*   For a BST of $n$ nodes, $\log n \le h < n$, where $h$ is the height.
*   **Bad Tree:** $h \sim O(n)$, occurring if keys are inserted in a skewed manner.
*   **Desired Tree:** A balanced BST where $h \sim O(\log n)$.

**Balancing Guarantees:**
*   **Worst-case (AVL Tree):** Self-balancing BST where $|h_L - h_R| \le 1$. Rebalancing is done via rotations.
*   **Randomized (Randomized BST / Skip List):** Guarantees $O(\log n)$ search time probabilistically.
*   **Amortized (Splay Tree):** Recently accessed elements are quicker to access again.

### **1.4 Scaling Issues for Databases**
While these structures have optimal $O(\log n)$ complexity, they are:
*   Good for in-memory operations and small volumes of data.
*   Complex due to rotations or randomized operations.
*   **Inefficient for external storage:** They do not scale for external data structures (persistent data on disk).

---

## **2. 2-3-4 Trees**

### **2.1 Core Concepts**
A 2-3-4 tree is a precursor to B-Trees and B+ Trees, designed to maintain balance differently than a standard BST.

**Properties:**
*   **Uniform Leaf Depth:** All leaves are at the same depth (bottom level).
*   **Height:** $h \sim O(\log n)$.
*   **Sorted Order:** All data is kept in sorted order.
*   **Node Types:** Every node (leaf or internal) is a 2-node, 3-node, or 4-node based on the number of links (children).

### **2.2 Node Structures**
1.  **2-node:** Contains 1 data item (S) and 2 links.
    *   Left link: Search keys < S.
    *   Right link: Search keys > S.
2.  **3-node:** Contains 2 data items (S, L) and 3 links.
    *   Left link: < S.
    *   Middle link: > S and < L.
    *   Right link: > L.
3.  **4-node:** Contains 3 data items (S, M, L) and 4 links.
    *   Links represent ranges: < S; between S and M; between M and L; > L.

<img width="754" height="384" alt="image" src="https://github.com/user-attachments/assets/de820a3d-0a4c-457c-8a71-adfff79cf767" />

### **2.3 Search Operation**
Search is a simple and natural extension of search in a standard BST, navigating the multiple links based on the value comparisons at each node.

### **2.4 Insertion and Node Splitting**
Insertion is performed at the leaf level. If a node is full, it must be split.

**Splitting a 4-node:**
*   A 4-node (S, M, L) is split into two 2-nodes (S) and (L).
*   The middle element (M) is pushed up to the parent node.
*   If the root is a 4-node and needs to split, a new root is created, increasing the tree height by 1.

<img width="809" height="376" alt="image" src="https://github.com/user-attachments/assets/a6c65343-c62f-4dee-ad53-95c236472bed" />


**Early vs. Late Splitting Strategies:**
*   **Early Strategy:** Split any 4-node encountered while searching down the tree for the insertion point. This ensures that the parent of the leaf will always have room for a pushed-up element.
*   **Late Strategy:** Split nodes only when an insertion actually overflows a leaf.
*   Both are valid and have $O(h)$ complexity, though they lead to different tree structures.

### **2.5 Insertion Example (Early Strategy)**
Insert sequence: 10, 30, 60, 20, 50, 40, 70, 80, 15, 90, 100.
1.  **10, 30, 60:** Forms a 4-node root.
2.  **Insert 20:** Root is a 4-node, split it before inserting. Middle element (30) becomes the new root. 10 and 60 become 2-node children. Insert 20 into the (10) node to make it a 3-node (10, 20).
3.  **Insert 50:** Added to the (60) node to make it a 3-node (50, 60).
4.  **Insert 40:** Added to the (50, 60) node to make it a 4-node (40, 50, 60).
5.  **Insert 70:** Encounter 4-node (40, 50, 60). Split it. 50 moves up to the root (30, 50). 40 and 60 become 2-node children. Insert 70 into the (60) node.

<img width="781" height="400" alt="image" src="https://github.com/user-attachments/assets/718d7694-f78c-427f-9815-50cdb0e13365" />
<img width="775" height="351" alt="image" src="https://github.com/user-attachments/assets/b81d8e75-1c72-42e5-ac6a-38a50370d19a" />
<img width="788" height="340" alt="image" src="https://github.com/user-attachments/assets/88518146-4a6b-4bcd-9a1a-1bf5c35fdbd3" />
<img width="755" height="351" alt="image" src="https://github.com/user-attachments/assets/655a3065-c6af-4483-9e33-75efa466c0ca" />
<img width="677" height="347" alt="image" src="https://github.com/user-attachments/assets/ab47a952-bcbf-44a2-a429-5a9176885dd2" />

---

## **3. Observations and Transition to B-Trees**
*   2-3-4 trees waste some space (holding only 1-3 items per node) but provide a powerful framework for external data.
*   They easily generalize to larger nodes (e.g., $n=100$).
*   **Definition:** A 2-3-4 Tree is essentially a B-Tree where $n=4$.







---









# 9.3: Indexing and Hashing/3: Indexing/3


## **Objectives**
*   To understand the design of $B^+$ Tree Index Files as a generalization of 2-3-4 Tree.
*   To understand the fundamentals of B-Tree Index Files.



## **1. $B^+$ Tree Index Files**

### **1.1 The $B^+$ Tree Concept**
*   **Definition:** A balanced binary search tree that maintains efficiency despite insertion and deletion of data.
*   **Form:** A rooted tree where every path from the root to a leaf is of the same length.
*   **Generalization:** It is a generalization of the 2-3-4 Tree following a multi-level index format.
*   **Node Occupancy:**
    *   Each non-leaf node (other than root) has between $\lceil n/2 \rceil$ and $n$ children.
    *   Each leaf node has between $\lceil (n-1)/2 \rceil$ and $n-1$ values.
    *   If the root is not a leaf, it has at least 2 children.
*   **Data Pointers:** Leaf nodes contain the actual data pointers.
*   **Sequential Access:** Leaf nodes are linked together using a linked list (pointer $P_n$) to support efficient sequential processing.

<img width="704" height="392" alt="image" src="https://github.com/user-attachments/assets/539d80b7-b3bc-417a-8558-50cfbfefda70" />

### **1.2 Node Structure**
A typical $B^+$ Tree node has the following structure:
$$P_1, K_1, P_2, K_2, \dots, P_{n-1}, K_{n-1}, P_n$$
*   **$K_i$:** Search-key values, stored in ordered sequence: $K_1 < K_2 < \dots < K_{n-1}$.
*   **$P_i$:**
    *   **In leaf nodes:** $P_1$ to $P_{n-1}$ point to file records with search-key values $K_i$; $P_n$ points to the next leaf node.
    *   **In non-leaf nodes:** Pointers to children subtrees.

### **1.3 Queries on $B^+$ Trees**
The procedure for finding a record with search-key value $V$:
1.  **Set $C = root$ node**.
2.  **While $C$ is not a leaf node:**
    *   Let $i = $ smallest number such that $V \le C.K_i$.
    *   If no such $i$ exists, let $P_m = $ last non-null pointer; set $C = C.P_m$.
    *   Else if $V = C.K_i$, set $C = C.P_{i+1}$.
    *   Else set $C = C.P_i$.
3.  **In leaf node $C$:**
    *   If there is an $i$ such that $K_i = V$, follow pointer $P_i$ to the record.
    *   Else, no record exists.

**Height Complexity:** For $K$ search-key values, height is no more than $\lceil \log_{\lceil n/2 \rceil}(K) \rceil$. With $n=100$, 1 million search keys require only 4 node accesses.

### **1.4 Updates on $B^+$ Trees**

#### **1.4.1 Insertion**
1.  **Find leaf node $L$** for the search-key value.
2.  If room exists in $L$, insert (key, pointer) in order.
3.  **If $L$ is full (Splitting):**
    *   Take $n$ pairs (existing + new), place first $\lceil n/2 \rceil$ in $L$, and the rest in new node $L'$.
    *   Insert the least key value of $L'$ and a pointer to $L'$ into the parent.
    *   **Propagation:** If the parent is full, split it recursively up to the root.

<img width="600" height="119" alt="image" src="https://github.com/user-attachments/assets/41d5b8d9-8c2a-480c-819f-127f9f07fec2" />

#### **1.4.2 Deletion**
1.  **Find leaf node $L$** and remove the entry.
2.  Shift remaining entries in $L$ left to close gaps.
3.  **If $L$ is underfull (Coalescing/Merging):**
    *   If the underfull node and a sibling fit in one node, **merge** them and recursively delete the pointer from the parent.
    *   Otherwise, **redistribute** pointers between the node and sibling so both satisfy the minimum occupancy requirement.

<img width="775" height="455" alt="image" src="https://github.com/user-attachments/assets/823c1c64-41d0-4cbe-b4d9-c81ab33d0031" />

### **1.5 $B^+$ Tree Extensions**

#### **1.5.1 $B^+$ Tree File Organization**
*   Leaf nodes store actual data records instead of just pointers to records.
*   Solves the degradation problem of sequential data files.
*   Each node is at least half full; records take more space than pointers, reducing fanout at the leaf level.

#### **1.5.2 Indexing Strings**
*   **Variable length:** Use space utilization as the criterion for splitting instead of the number of pointers.
*   **Prefix compression:** Internal nodes store only the prefix needed to distinguish subtrees (e.g., "Silb" to separate "Silas" and "Silberschatz").

---

## **2. B-Tree Index Files**

### **2.1 B-Tree vs. $B^+$ Tree**
*   **Non-redundancy:** Search-key values appear only once in a B-Tree. In $B^+$ Trees, values in non-leaf nodes are repeated in leaf nodes.
*   **Non-leaf Node Pointers:** Since keys in non-leaf nodes appear nowhere else, non-leaf nodes must include an additional pointer $B_i$ to the record/bucket for that key.

<img width="701" height="258" alt="image" src="https://github.com/user-attachments/assets/2032db22-56e5-4be4-b345-0faa1f238c2b" />

### **2.2 Comparison and Evaluation**

| Advantage of B-Tree | Disadvantage of B-Tree |
| :--- | :--- |
| May use fewer tree nodes. | Non-leaf nodes are larger (due to record pointers), reducing fanout and increasing depth. |
| Possible to find value before reaching leaf. | Only a small fraction of values are found early. |
| | Insertion and deletion are more complicated. |

**Conclusion:** Advantages of B-Trees typically do not outweigh the disadvantages; $B^+$ Trees are used almost universally in practice.




---








# **9.4: Indexing and Hashing/4: Hashing**


## **Objectives**
*   To explore various hashing schemes – Static and Dynamic Hashing.
*   To compare Ordered Indexing and Hashing.
*   To understand the Bitmap Indices.




## **1. Static Hashing**

### **1.1. Hash Function**
*   A hash function $h$ maps data of arbitrary size (from domain $D$) to fixed-size values (say, integers from $0$ to $N > 0$).
*   $h: D \rightarrow [0..N]$.
*   Given key $k$, $h(k)$ is called hash values, hash codes, digests, or simply hashes.
*   If for two keys $k_1 \ne k_2$, we have $h(k_1) = h(k_2)$, we say a collision has occurred.
*   A hash function should be Collision Free and Fast.

<img width="805" height="413" alt="image" src="https://github.com/user-attachments/assets/06895db0-21f1-42e6-8cc5-6b5cf6679c68" />


### **1.2. Static Hashing Definition**
*   A bucket is a unit of storage containing one or more records (a bucket is typically a disk block).
*   In a hash file organization we obtain the bucket of a record directly from its search-key value using a hash function.
*   Hash function $h$ is a function from the set of all search-key values $K$ to the set of all bucket addresses $B$.
*   Hash function is used to locate records for access, insertion as well as deletion.
*   Records with different search-key values may be mapped to the same bucket; thus entire bucket has to be searched sequentially to locate a record.

### **1.3. Example of Hash File Organization**
*   Consider a hash file organization of the `instructor` file using `dept_name` as the key.
*   There are 10 buckets.
*   The binary representation of the $i^{th}$ character is assumed to be the integer $i$.
*   The hash function returns the sum of the binary representations of the characters modulo 10.
*   **Examples:**
    *   $h(\text{Music}) = 1$
    *   $h(\text{History}) = 2$
    *   $h(\text{Physics}) = 3$
    *   $h(\text{Elec. Eng.}) = 3$


<img width="559" height="416" alt="image" src="https://github.com/user-attachments/assets/6a466bf1-b683-4787-be9b-a4b5aa6dc212" />


### **1.4. Ideal Hash Functions**
*   Worst hash function maps all search-key values to the same bucket; this makes access time proportional to the number of search-key values in the file.
*   An ideal hash function is **uniform**, i.e., each bucket is assigned the same number of search-key values from the set of all possible values.
*   Ideal hash function is **random**, so each bucket will have the same number of records assigned to it irrespective of the actual distribution of search-key values in the file.
*   Typical hash functions perform computation on the internal binary representation of the search-key.

### **1.5. Handling of Bucket Overflows**
*   Bucket overflow can occur because of:
    1.  Insufficient buckets.
    2.  Skew in distribution of records. This can occur due to multiple records having the same search-key value or a chosen hash function producing a non-uniform distribution.
*   Overflow is handled by using **overflow buckets**.
*   **Overflow chaining:** The overflow buckets of a given bucket are chained together in a linked list.
*   This scheme is called **closed hashing**.
*   An alternative, called **open hashing** (or open addressing), which does not use overflow buckets, is not suitable for database applications because it does not support deletes efficiently.

<img width="797" height="428" alt="image" src="https://github.com/user-attachments/assets/ec17316c-0250-4636-9bbf-636e137bb1d9" />


### **1.6. Hash Indices**
*   Hashing can be used not only for file organization, but also for index-structure creation.
*   A hash index organizes the search keys, with their associated record pointers, into a hash file structure.
*   Strictly speaking, hash indices are always secondary indices.
*   If the file itself is organized using hashing, a separate primary hash index on it using the same search-key is unnecessary.

<img width="512" height="447" alt="image" src="https://github.com/user-attachments/assets/fd55951b-100a-4961-8cf6-addcc8c628cb" />

### **1.7. Deficiencies of Static Hashing**
*   In static hashing, function $h$ maps search-key values to a fixed set $B$ of bucket addresses. Databases grow or shrink with time.
*   If the initial number of buckets is too small and the file grows, performance will degrade due to too many overflows.
*   If space is allocated for anticipated growth, a significant amount of space will be wasted initially (and buckets will be underfull).
*   Periodic re-organization of the file with a new hash function is expensive and disrupts normal operations.
*   Better solution: Allow the number of buckets to be modified dynamically.

---

## **2. Dynamic Hashing**

### **2.1. Overview**
*   Good for a database that grows and shrinks in size.
*   Allows the hash function to be modified dynamically.
*   **Extendable hashing** is one form of dynamic hashing.
*   Hash function generates values over a large range - typically b-bit integers, with $b=32$.
*   At any time, use only a prefix of the hash function to index into a table of bucket addresses.
*   Let the length of the prefix be $i$ bits, $0 \le i \le 32$.
*   **Bucket address table size** $= 2^i$. Initially $i=0$.
*   Value of $i$ grows and shrinks as the size of the database grows and shrinks.
*   Multiple entries in the bucket address table may point to the same bucket.
*   The actual number of buckets is $< 2^i$.

<img width="727" height="490" alt="image" src="https://github.com/user-attachments/assets/5f1f1663-9e07-4146-96c4-150fd4d79ea8" />

### **2.2. Use of Extendable Hash Structure**
*   Each bucket $j$ stores a value $i_j$.
*   All the entries that point to the same bucket have the same values on the first $i_j$ bits.
*   **To locate the bucket containing search-key $K_j$:**
    1.  Compute $h(K_j) = X$.
    2.  Use the first $i$ high order bits of $X$ as a displacement into the bucket address table and follow the pointer to the appropriate bucket.
*   **To insert a record with search-key value $K_j$:**
    1.  Follow same procedure as look-up and locate the bucket, say $j$.
    2.  If there is room in the bucket $j$, insert the record in the bucket.
    3.  Else the bucket must be split and insertion re-attempted.

### **2.3. Splitting and Deleting**
*   **Splitting a bucket $j$:**
    *   **If $i > i_j$:** More than one pointer points to bucket $j$. Allocate a new bucket $z$, set $i_j = i_z = (i_j + 1)$, and update the bucket address table.
    *   **If $i = i_j$:** Only one pointer points to bucket $j$. Increment $i$ by 1, double the size of the bucket address table, and replace each original entry with two entries pointing to the same bucket.
*   **To delete a key value:** Locate it in its bucket and remove it.
*   The bucket itself can be removed if it becomes empty.
*   **Coalescing of buckets** can be done with a "buddy" bucket having the same prefix length.
*   Decreasing bucket address table size is possible but expensive; it should be done only if the number of buckets becomes much smaller than the table size.

<img width="1009" height="530" alt="image" src="https://github.com/user-attachments/assets/2a936dd2-158d-447e-a2bb-3bfb61b64717" />
<img width="983" height="530" alt="image" src="https://github.com/user-attachments/assets/10e03cf6-0301-4b2f-9802-14c1a653fdcb" />
<img width="984" height="532" alt="image" src="https://github.com/user-attachments/assets/6db00fe3-efa4-426d-8ed9-e923bc72d09f" />
<img width="844" height="452" alt="image" src="https://github.com/user-attachments/assets/5bb1ca64-076e-4a7d-b076-23152e2c65df" />

<img width="829" height="446" alt="image" src="https://github.com/user-attachments/assets/f8a1136a-a393-4e05-9798-2c9946c13b15" />

### **2.4. Comparison of Schemes**
*   **Benefits of extendable hashing:**
    *   Hash performance does not degrade with growth of file.
    *   Minimal space overhead.
*   **Disadvantages of extendable hashing:**
    *   Extra level of indirection to find desired record.
    *   Bucket address table may become very big (larger than memory).
    *   Changing the size of the bucket address table is an expensive operation.
*   **Linear hashing** is an alternative mechanism that allows incremental growth of its directory at the cost of more bucket overflows.

---

## **3. Comparison: Ordered Indexing vs. Hashing**
*   **Ordered indices** are preferred if range queries are common.
*   **Hashing** is generally better at retrieving records having a specified value of the key (point queries).
*   **Implementation in Practice:**
    *   PostgreSQL supports hash indices but discourages use due to poor performance.
    *   Oracle supports static hash organization but not hash indices.
    *   SQLServer supports only $B^{+}$ trees.

---

## **4. Bitmap Indices**

### **4.1. Definition and Usage**
*   Bitmap indices are a special type of index designed for efficient querying on multiple keys.
*   Records in a relation are assumed to be numbered sequentially from 0.
*   Applicable on attributes that take on a relatively small number of distinct values (e.g., gender, country, state, income-level).
*   A bitmap is simply an array of bits.

### **4.2. Structure**
*   In its simplest form, a bitmap index on an attribute has a bitmap for each value of the attribute.
*   Bitmap has as many bits as records.
*   In a bitmap for value $v$, the bit for a record is 1 if the record has the value $v$ for the attribute, and is 0 otherwise.

<img width="891" height="450" alt="image" src="https://github.com/user-attachments/assets/25d7a87f-e009-42d9-b790-ecd9f29eb03b" />

### **4.3. Operations**
*   Bitmap indices are useful for queries on multiple attributes; they are not particularly useful for single attribute queries.
*   **Queries are answered using bitmap operations:**
    *   **Intersection (AND):** `100110 AND 110011 = 100010`.
    *   **Union (OR):** `100110 OR 110011 = 110111`.
    *   **Complementation (NOT):** `NOT 100110 = 011001`.
*   **Example:** To find "Males with income level L1," perform `10010 AND 10100 = 10000`.
*   Counting the number of matching tuples is even faster using bitmaps.

### **4.4. Management and Efficiency**
*   Bitmap indices are generally very small compared with relation size.
*   **Deletion:** Handled using an **Existence bitmap** to note if there is a valid record at a location.
    *   `not(A=v)` is computed as: `(NOT bitmap-A-v) AND ExistenceBitmap`.
*   **Nulls:** Bitmaps should be kept for all values, including the null value, to handle SQL null semantics.
*   **CPU Optimization:** Bitmaps are packed into words; a single word AND (basic CPU instruction) computes 32 or 64 bits at once.
*   **Hybrid use:** Bitmaps can be used instead of Tuple-ID lists at leaf levels of $B^{+}$ trees for values with many matching records (worthwhile if $> 1/64$ of records have that value).
