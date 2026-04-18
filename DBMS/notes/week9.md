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


