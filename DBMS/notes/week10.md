# 10.1 Transactions/1


## **Recap**
*   Need for indexing database tables.
*   Understood the ordered indexes.
*   Recap of Balanced BST for optimal in-memory search data structures.
*   Issues of external search data structures for persistent data.
*   Explored 2-3-4 Tree as a precursor to $B/B^+$-Tree.
*   Understood the $B^+$ Tree and B Tree for Index files and data files.
*   Explored Static and Dynamic Hashing.
*   Compared Ordered Indexing and Hashing.
*   Studied the use of Bitmap Indices.
*   Learnt to create indexes in SQL.
*   Learnt a set of Ground Rules for Indexing.



## **Objectives**
*   To understand the concept of transaction doing a task in a database and its state.
*   To explore issues in concurrent execution of transactions.



## **1. Transaction Concept**

### **1.1. Definition**
*   A transaction is a unit of program execution that accesses and, possibly updates, various data items.
*   It consists of a couple of one or more sequence of instructions.
*   These instructions can access (read), update (write), and perform typical programming language computations in between.

### **1.2. Example Transaction**
A sample transaction to transfer \$50 from account A to account B:
1.  `read(A)`
2.  `A := A - 50`
3.  `write(A)`
4.  `read(B)`
5.  `B := B + 50`
6.  `write(B)`.

*Explanation:* The system first reads the current balance of A and subtracts 50 (assuming enough balance is available). Account A is debited, and the new value is written back. Then, B is read, the amount is added to its balance, and the new value is written back to credit account B.

### **1.3. Major Issues in Transactions**
1.  **Failures:** Various kinds such as hardware failures, software errors, and system crashes can occur at any point during execution.
2.  **Concurrency:** How to handle multiple transactions working with the same data at the same time to ensure better system performance with necessary restrictions.

---

## **2. Required Properties of a Transaction: ACID**

### **2.1. Atomicity**
*   **Requirement:** "All or Nothing".
*   If a transaction fails at any step (e.g., after step 3 but before step 6 in the \$50 transfer), money will be "lost," leading to an inconsistent database state.
*   The system must ensure that updates of a partially executed transaction are not reflected in the database.
*   Atomicity must be guaranteed in all situations, including power failures and error conditions.

### **2.2. Consistency**
*   **Requirement:** Guarantees committed transaction state.
*   Execution of a transaction in isolation (with no other concurrent transactions) must preserve the consistency of the database.
*   Example: $A+B$ must be unchanged by the execution of the transfer transaction.
*   Consistency constraints include explicitly specified integrity constraints (Primary/Foreign Keys) and implicit constraints (e.g., assets minus liabilities must equal cash-in-hand).
*   A transaction starting its execution must see a consistent database; it may be temporarily inconsistent during execution but must be consistent upon successful termination.

### **2.3. Isolation**
*   **Requirement:** Transactions are independent.
*   Ensures that concurrent execution of transactions leaves the database in the same state as if they were executed sequentially.
*   If a transaction $T_2$ is allowed to access a partially updated database (e.g., between steps 3 and 6 of $T_1$), it will see an inconsistent state where the sum $A+B$ is less than it should be.

### **2.4. Durability**
*   **Requirement:** Committed data is never lost.
*   Once a user is notified that the transaction has completed, updates must persist even in the face of software or hardware failures.
*   This typically means completed transactions are recorded in non-volatile memory.

---

## **3. Transaction States**

Every transaction exists in one of the following states, similar to process states in an Operating System:

*   **Active:** The initial state; the transaction stays here while it is executing.
*   **Partially Committed:** After the final statement has been executed, but before it is verified that updates can be made permanent.
*   **Failed:** After the discovery that normal execution can no longer proceed.
*   **Aborted:** After the transaction has been rolled back and the database restored to its prior state. Two options exist after abortion:
    1.  **Restart:** Only if there was no internal logical error (e.g., hardware/software error).
    2.  **Kill:** Due to internal logical errors or bad input.
*   **Committed:** After successful completion and writing updates to permanent store.
*   **Terminated:** The final state after the transaction has been either committed or killed.

<img width="1007" height="478" alt="image" src="https://github.com/user-attachments/assets/ce096248-7140-4421-b1d9-ee3d6145d7fe" />

---

## **4. Concurrent Executions**

### **4.1. Advantages**
*   **Increased Processor and Disk Utilization:** One transaction can use the CPU while another reads/writes to disk, leading to better throughput.
*   **Reduced Average Response Time:** Short transactions do not have to wait behind long-running ones.

### **4.2. Concurrency Control Schemes**
*   These are mechanisms used to achieve **Isolation**.
*   They control interaction among concurrent transactions to prevent them from destroying database consistency.

---

## **5. Schedules**

### **5.1. Definition**
*   A schedule is a sequence of instructions specifying the chronological order in which instructions of concurrent transactions are executed.
*   It must consist of all instructions of those transactions and preserve the relative order of instructions within each individual transaction.
*   A successfully completed transaction ends with a `commit` instruction; a failed one ends with `abort`.

### **5.2. Serial Schedules**
*   Each serial schedule consists of a sequence of instructions where instructions belonging to one single transaction appear together.
*   For a set of $n$ transactions, there are $n!$ (n factorial) different valid serial schedules.

### **5.3. Schedule Examples and Consistency**
Consider $T_1$ (transfer \$50 from A to B) and $T_2$ (transfer 10% from A to B). Assume initial values: $A = 100, B = 200$ (Sum = 300).

*   **Schedule 1 (Serial: $T_1$ then $T_2$):**
    *   Final: $A = 45, B = 255$. Consistent sum = 300.
*   **Schedule 2 (Serial: $T_2$ then $T_1$):**
    *   Final: $A = 40, B = 260$. Consistent sum = 300.
*   **Schedule 3 (Concurrent/Serializable):**
    *   Instructions are interleaved but result in a consistent state equivalent to Schedule 1.
    *   Preserves the sum $A+B = 300$.
*   **Schedule 4 (Concurrent/Not Serializable):**
    *   *Problem:* The read/write operations are intermixed such that updates are overwritten.
    *   Walkthrough: $T_1$ reads $A(100)$ and computes $50$ locally. $T_2$ reads $A(100)$, computes interest $(10)$, and writes $A(90)$. Then $T_1$ writes its local $A(50)$, overwriting $T_2$'s update. $T_1$ reads $B(200)$, writes $B(250)$, and commits. $T_2$ adds its local temp $(10)$ to $B(200)$ and writes $B(210)$.
    *   Final: $A = 50, B = 210$. Inconsistent sum = 260 (\$40 lost).

<img width="1096" height="564" alt="image" src="https://github.com/user-attachments/assets/3e4935f8-8702-43e7-8a9f-5db145edced4" />
<img width="1096" height="564" alt="image" src="https://github.com/user-attachments/assets/197be93d-c41a-4c7e-a20a-c042dd6df901" />
<img width="1096" height="564" alt="image" src="https://github.com/user-attachments/assets/8a3cd6af-1d78-4ad2-9fbf-e246f86f6f8c" />
<img width="1096" height="564" alt="image" src="https://github.com/user-attachments/assets/929c0152-d0ab-4d9a-87b3-354825bdc95b" />





---



