# 11.1: Backup & Recovery/1: Backup/1



## **Recap**
*   Concurrent transactions, serializability issues, and ACID properties were discussed.
*   Learnt the forms of serializability - conflict and view.
*   Conflict serializability can be ensured by an acyclic precedence graph.
*   View Serializability is a weaker serializability system providing better concurrency; however, testing for view serializability is NP-complete.
*   With proper planning, a database can be recovered back to a consistent state from an inconsistent state in the face of system failures via cascaded or cascadeless rollback.
*   Understood the locking mechanism and protocols.
*   Realized that deadlock is a peril of locking and needs to be handled through rollback.
*   Explained how to detect, prevent, and recover from deadlock.
*   Introduced a time-based protocol that avoids deadlocks.

## **Objectives**
*   To understand the need for having a backup.
*   To learn about different strategies of backup and their suitability.





## **1. What is Backup and Recovery?**

### **1.1. Backup**
*   A **Backup** of a database is a representative copy of data containing all necessary contents of a database such as data files and control files.
*   Unexpected database failures, especially those due to factors beyond control, are unavoidable. Hence, it is important to keep a backup of the entire database.
*   **Major Types of Backup:**
    *   **Physical Backup:** A copy of physical database files such as data, control files, log files, and archived redo logs.
    *   **Logical Backup:** A copy of logical data that is extracted from a database consisting of tables, procedures, views, functions, etc.

### **1.2. Recovery**
*   **Recovery** is the process of restoring the database to its latest known consistent state after a system failure occurs.

### **1.3. Database Log**
*   A Database Log records all transactions in a sequence.
*   Recovery using logs is quite popular in databases.
*   A typical log file contains information about transactions to execute, transaction states, and modified values.

---

## **2. Why is Backup Necessary?**

*   **Disaster Recovery:** Data loss can occur due to various reasons like hardware failures, malware attacks, environmental & physical factors, or simple human error.
*   **Client Side Changes:** Clients may want to modify existing applications to serve dynamic business needs. Developers might need to restore a previous version of the database to address such requirements.
*   **Auditing:** From an auditing perspective, you need to know what your data or schema looked like at some point in the past (e.g., in the event of a lawsuit).
*   **Downtime:** Without backup, system failures lead to data loss, which results in application downtime and a bad business reputation.

---

## **3. Backup Data: Types**

*   **Business Data:** Includes personal information of clients, employees, contractors, etc., along with details about places, things, events, and rules related to the business.
*   **System Data:** Includes the specific environment/configuration of the system used for specialized development purposes, log files, software dependency data, and disk images.
*   **Media Files:** Photographs, videos, sounds, graphics, etc. These are typically much larger in size.

---

## **4. Backup Strategies**

### **4.1. Full Backup**
*   **Definition:** Backs up everything. This is a complete copy storing all objects: tables, procedures, functions, views, indexes, etc.
*   **Requirement:** A full backup must be done at least once before any other type of backup.
*   **Frequency:** Done on a daily basis if:
    *   $24/7$ availability is not required or is not affected by backups.
    *   The backup data is not too large.
    *   Administrators are not available daily, making it a goal to reduce media required for a restore to a bare minimum.

| **Advantages** | **Disadvantages** |
| :--- | :--- |
| Recovery involves a consolidated read from a single backup. | Takes the largest amount of time among all types. |
| No dependency between consecutive backups; losing one day's backup doesn't affect others. | Results in the longest system downtime during the process. |
| Relatively easy to setup, configure, and maintain. | Uses the largest amount of storage media per backup. |

### **4.2. Incremental Backup**
*   **Definition:** Targets only those files or items that have changed since the **last backup**.
*   **Example:** In a 2 TB database with 5% daily change, the amount backed up is only slightly more than the actual changed data.
*   **Typical Schedule:** Full backup once a week (e.g., Friday), and incremental backups for the rest of the week (Saturday–Thursday).

| **Advantages** | **Disadvantages** |
| :--- | :--- |
| Less storage used per backup. | Requires more effort and time during recovery. |
| Downtime due to backup is minimized. | Recovery needs the full backup + **all** intermediate incrementals. |
| Considerable cost reduction over full backups. | If any intermediate incremental is lost, recovery cannot be $100\%$. |

### **4.3. Differential Backup**
*   **Definition:** Backs up all changes that have occurred since the **most recent full backup**, regardless of intermediate backups.
*   **Function:** "Rolls up" multiple changes into a single job, setting the basis for the next incremental backup.
*   **Example:** If Full is Friday and incrementals are Sat-Mon, a Tuesday Differential backs up everything changed since Friday. Recovery on Wednesday only requires the Full and the Differential, skipping Sat-Mon incrementals.

| **Advantages** | **Disadvantages** |
| :--- | :--- |
| Recoveries require fewer backup sets. | Storage media required may exceed that of incremental backups. |
| Better options when full backups are run rarely (e.g., monthly). | If done after a long time, it can reach the size of a full backup. |

<img width="894" height="356" alt="image" src="https://github.com/user-attachments/assets/a403107f-67d3-4712-99ad-2b21bc66e06a" />

---

## **5. Case Study: Monthly Schedule**

<img width="919" height="422" alt="image" src="https://github.com/user-attachments/assets/541aefd6-c40b-4d4d-82cc-56857498bb75" />

*   **Inference:** With differentials performed weekly and full backups monthly, the maximum number of backups for a complete recovery at any point is: **1 Full + 1 Differential + 6 Incrementals**.
*   **Comparison:** If only incrementals were used after the 1st, a recovery on the last day would need **31 backup sets**.

---

## **6. Hot Backup**

*   **Definition:** Keeping a database up and running while the backup is performed concurrently.
*   **Use Cases:** Highly dynamic operations ($24\times 7$) like asset management, hedge funds, stock trading, or real-time systems (sensors, satellite transmissions).

### **6.1. Evaluation of Hot Backup**
*   **Advantages:** Database is always available; easier point-in-time recovery; efficient for dynamic modular data.
*   **Disadvantages:** May not be feasible for huge monolithic datasets; lower fault tolerance (errors can terminate the whole process); high maintenance and setup cost.

### **6.2. Transactional Logging as Hot Backup**
*   In regular systems, **Cold backup** (Full, Differential, Incremental) is preferred for Data, while **Hot backup** is used for the **Transaction Log**.
*   **Mechanism:** Transactional logging is used when a possibly inconsistent data backup is taken; the log file (backed up after the data file) is used to restore consistency.
*   **Role:** Log backups are vital because they allow the system to infer data backup versions needed for recovery at any given point.






---









# 11.2 Backup & Recovery/2: Recovery/1

---



### **1. Objectives**
*   To understand the possible sources for failure for transactions in a database.
*   To explore storage models (volatile, non-volatile, stable) used for recovery to ensure Atomicity, Consistency, and Durability.
*   To understand recovery schemes based on logging.
*   To focus on single transactions.

---

## **2. Failure Classification**

All database reads/writes occur within a transaction which must satisfy **ACID properties**: **Atomicity** (all or nothing), **Consistency** (preserves database integrity), **Isolation** (execute as if run alone), and **Durability** (results not lost by failure). 

*   **Concurrency Control** guarantees Isolation and contributes to Consistency.
*   The **Application Program** guarantees Consistency.
*   The **Recovery Subsystem** guarantees Atomicity and Durability, and contributes to Consistency.

### **2.1. Types of Failures**
1.  **Transaction Failure:**
    *   **Logical Errors:** The transaction cannot complete due to an internal error condition (e.g., bad input, data not found, overflow, or resource limit exceeded).
    *   **System Errors:** The database system must terminate an active transaction due to an error condition, such as a deadlock.
2.  **System Crash:** A power failure or other hardware or software failure causes the system to crash.
    *   **Fail-stop Assumption:** Non-volatile storage contents are assumed not to be corrupted as a result of a system crash.
    *   Database systems use numerous integrity checks to prevent corruption of disk data.
3.  **Disk Failure:** A head crash or similar failure destroys all or part of disk storage. 
    *   Destruction is assumed to be detectable; disk drives use checksums to detect failures.

---

## **3. Recovery Algorithms**

Recovery algorithms have two distinct parts:
1.  **Normal Actions:** Taken during normal transaction processing to ensure enough information exists to allow recovery from failures.
2.  **Recovery Actions:** Taken after a failure to recover the database contents to a state ensuring atomicity, consistency, and durability.

---

## **4. Storage Structure**

Storage media are distinguished by speed, capacity, and resilience to failure.

| Storage Type | Characteristics | Examples |
| :--- | :--- | :--- |
| **Volatile Storage** | Does not survive system crashes; extremely fast access. | Main memory, cache memory. |
| **Non-volatile Storage** | Survives system crashes but may still fail, losing data. | Disk, tape, flash memory, non-volatile (battery-backed) RAM. |
| **Stable Storage** | A mythical/theoretical form of storage that is never lost. | Approximated by maintaining multiple copies on distinct non-volatile media. |

### **4.1. Stable Storage Implementation**
To approximate stable storage, information is replicated in several non-volatile storage media (usually disks) with independent failure modes. 

**Output Protocol (for two copies):**
1.  Write the information onto the $1^{st}$ physical block.
2.  When the $1^{st}$ write is successful, write the same information onto the $2^{nd}$ physical block.
3.  The output is completed only after the second write successfully completes.

<img width="844" height="457" alt="image" src="https://github.com/user-attachments/assets/144afd8d-dc9f-41e0-a54a-35ec17e88314" />

**Failure Recovery for Blocks:**
If the system fails during an output operation, copies may differ. 
*   **Checksums:** Errors in a disk block (like partial writes) are detected using checksums.
*   **Comparison:** If one copy has an error (bad checksum), overwrite it with the other copy.
*   **Inconsistency:** If both have no error but are different, overwrite the second block with the first.
*   **Optimization:** Record in-progress writes on non-volatile RAM to avoid comparing every block during recovery.

---

## **5. Data Access**

*   **Physical Blocks:** Blocks residing on the disk.
*   **System Buffer Blocks:** Blocks residing temporarily in main memory.
*   **Operations:**
    *   `input(B)`: Transfers physical block $B$ to main memory.
    *   `output(B)`: Transfers buffer block $B$ to the disk and replaces the physical block there.
*   **Local Copies:** Each transaction $T_i$ has a private work-area for local copies of data items (e.g., $x_i$ is $T_i$'s local copy of $X$).
    *   `read(X)`: Assigns the value of $X$ from the buffer to local variable $x_i$.
    *   `write(X)`: Assigns the value of $x_i$ to data item $X$ in the buffer block.

<img width="587" height="428" alt="image" src="https://github.com/user-attachments/assets/4d902030-37fa-4010-92a2-43ca210da9c8" />

---

## **6. Log-Based Recovery**

The system maintains a **log** on stable storage, which is a sequence of log records recording all update activities.

### **6.1. Log Record Types**
*   **$<T_i \text{ start}>$:** Transaction $T_i$ has started.
*   **$<T_i, X, V_1, V_2>$:** $T_i$ performed a write on $X$. $V_1$ is the old value (before write), and $V_2$ is the new value (after write).
*   **$<T_i \text{ commit}>$:** $T_i$ has committed.
*   **$<T_i \text{ abort}>$:** $T_i$ has aborted.

### **6.2. Modification Schemes**
1.  **Immediate-Modification:** Allows updates of an uncommitted transaction to be made to the buffer or disk before the transaction commits. An update log record must be written to stable storage before the database item is written.
2.  **Deferred-Modification:** Performs updates to the buffer/disk only at the time of transaction commit.

### **6.3. Transaction Commit**
A transaction is committed when its **commit log record** is output to stable storage. All previous log records of that transaction must have been output already.

---

## **7. Undo and Redo Operations**

### **7.1. Basic Definitions**
*   **Undo:** Sets the data item to the old value $V_1$ using a log record.
*   **Redo:** Sets the data item to the new value $V_2$ using a log record.

### **7.2. Transactional Undo and Redo**
*   **$undo(T_i)$:** Restores all data items updated by $T_i$ to their old values, scanning the log **backwards**.
    *   Writes a **Compensation Log Record (CLR)** (redo-only record) $<T_i, X, V_1>$ for each restoration.
    *   Writes $<T_i \text{ abort}>$ when complete.
*   **$redo(T_i)$:** Sets all data items updated by $T_i$ to the new values, scanning the log **forward**.
    *   No logging is done during redo.

### **7.3. Use Cases**
*   **Rollback:** Undo is used for transaction rollback during normal operation (e.g., due to a logical error).
*   **Failure Recovery:** Both undo and redo are used.
    *   **Undo $T_i$:** If the log contains $<T_i \text{ start}>$ but neither $<T_i \text{ commit}>$ nor $<T_i \text{ abort}>$.
    *   **Redo $T_i$:** If the log contains $<T_i \text{ start}>$ and either $<T_i \text{ commit}>$ or $<T_i \text{ abort}>$.
    *   **Repeating History:** Redoing aborted transactions (which include CLRs) ensures the end result is the same as the original undo.

---

## **8. Checkpoints**

Redoing/undoing the entire log is slow. **Checkpoints** streamline recovery by periodically flushing the state to disk.

### **8.1. Checkpoint Procedure**
1.  Stop all updates.
2.  Output all log records in main memory to stable storage.
3.  Output all modified buffer blocks to disk.
4.  Write a log record **$<checkpoint~L>$** where $L$ is a list of active transactions at that time.

### **8.2. Recovery with Checkpoints**
The system scans backwards to find the most recent $<checkpoint~L>$ record.
*   **Ignore:** Transactions that committed or aborted before the checkpoint.
*   **Redo:** Transactions in $L$ or those that started after the checkpoint if they have a commit or abort record in the log.
*   **Undo:** Transactions in $L$ or those that started after the checkpoint if they lack a commit or abort record.








---






# 11.3: Backup & Recovery/3: Recovery/2



### **1.  Objectives**
*   To understand Transactional Logging with Hot Backup.
*   To focus on concurrent transactions and understand the recovery algorithms.


## **2. Transactional Logging**

### **2.1 Hot Backup: Recap**
*   In systems where high availability is a requirement Hot backup is preferable wherever possible.
*   Hot backup refers to keeping a database up and running while the backup is performed concurrently.
*   Such a system usually has a module or plug-in that allows the database to be backed up while staying available to end users.
*   Databases which stores transactions of asset management companies, hedge funds, high frequency trading companies etc. try to implement Hot backups as these data are highly dynamic and the operations run $24\times7$.
*   Real time systems like sensor and actuator data in embedded devices, satellite transmissions etc. also use Hot backup.

### **2.2 Transactional Logging as Hot Backup**
*   In regular database systems, Hot Backup is mainly used for **Transaction Log Backup**.
*   Cold backup strategies like Differential, Incremental are preferred for **Data backup**.
*   Transactional Logging is used in circumstances where a possibly inconsistent backup is taken, but another file generated and backed up (after the database file has been fully backed up) can be used to restore consistency.
*   The information regarding data backup versions while recovery at a given point can be inferred from the Transactional Log backup set.
*   Thus they play a vital role in database recovery.

### **2.3 Transactional Logging with Recovery: Example**
To understand how Transactional Logging works we consider a chunk of a database just before a backup has been started.

<img width="1141" height="572" alt="image" src="https://github.com/user-attachments/assets/3e44d2d1-dbe8-462f-ae9a-c93237b49d69" />

*   While the backup is in progress, modifications may continue to occur to the database. For example, a request to modify the data at location "4325" to '0' arrives.
*   When a request comes through to modify a part of the DB, the modifications will be written in the given order compulsorily:
    1. Transaction Log
    2. Database (itself).


*   If a crash occurs before writing to the database then the inconsistent backed up file is recovered first, and then the pending modifications in the transaction log (backed up*) are applied to re-establish consistency.
*   **Note:** The Transactional Log itself is backed up using Hot Backup; the Data is backed up incrementally.

#### **Extended Example: Multiple Requests**
*   Consider in the previous scenario before the occurrence of crash, another request modifies the content of location "4321" to '0'. Incidentally, this change gets written in the database itself (Immediate Modification).

<img width="1133" height="566" alt="image" src="https://github.com/user-attachments/assets/d1ba4895-b03f-4d01-b7a7-91185676e000" />

*   The system crashes. Note that this part has already been backed up, and hence, the backup is inconsistent with the database.
*   **Recovery Phase:**
    *   Data recovery is done from the last data backup set (Figure 1).
    *   Log recovery is done from the Transaction Log backup set. It will be same as the current transaction log because of Hot backup.


*   The recovered database is inconsistent. To re-establish consistency all transaction logs generated between the start of the backup and the end of the backup must be replayed.

### **2.4 Recover vs. Restore**
When using transactional logging we distinguish between recover and restore:
*   **Recover:** retrieve from the backup media the database files and transaction logs.
*   **Restore:** reapply database consistency based on the transaction logs.

<img width="1127" height="562" alt="image" src="https://github.com/user-attachments/assets/1a4a228d-c13d-4059-83dd-6486444bb706" />

*   **Note:** an unnecessary log replay might occur for a block (e.g., 4325) depending on the database vendor. Replaying all logs is often faster than determining if a specific activity needs replaying.
*   Once all transaction logs have been replayed, the database is said to have been restored and can be opened for user access.

---

## **3. Recovery Algorithm**

### **3.1 Concurrency Control and Recovery**
*   With concurrent transactions, all transactions share a single disk buffer and a single log.
*   A buffer block can have data items updated by one or more transactions.
*   **Assumption:** if a transaction $T_i$ has modified an item, no other transaction can modify the same item until $T_i$ has committed or aborted.
    *   Updates of uncommitted transactions should not be visible to other transactions.
    *   This is ensured by obtaining exclusive locks on updated items and holding the locks till end of transaction (**strict two-phase locking**).
*   Log records of different transactions may be interspersed in the log.

### **3.2 Data Access: Comparison**

<img width="704" height="530" alt="image" src="https://github.com/user-attachments/assets/e0099938-e153-4e98-9e37-3b86b7384faa" />

<img width="695" height="567" alt="image" src="https://github.com/user-attachments/assets/fcbb1c2d-060d-47a1-904f-2f0e3e7a5ffc" />

### **3.3 Normal Logging and Rollback**
*   **Logging (during normal operation):**
    *   $<T_i \text{ start}>$ at transaction start.
    *   $<T_i, X_j, V_1, V_2>$ for each update.
    *   $<T_i \text{ commit}>$ at transaction end.

*   **Transaction rollback (during normal operation):**
    1. Scan log backwards from the end.
    2. For each log record of $T_i$ of the form $<T_i, X_j, V_1, V_2>$:
        *   Perform the undo by writing $V_1$ to $X_j$.
        *   Write a log record $<T_i, X_j, V_1>$ (**Compensation Log Record or CLR**).
    3. Once the record $<T_i \text{ start}>$ is found, stop the scan and write the log record $<T_i \text{ abort}>$.

### **3.4 Checkpoints Recap**
*   Let the time of checkpointing be $t_{check}$ and the time of system crash be $t_{fail}$.
*   **Transaction Scenarios:**
    *   $T_a$: commits before checkpoint.
    *   $T_b$: starts before checkpoint and commits before system crash.
    *   $T_c$: starts after checkpoint and commits before system crash.
    *   $T_d$: starts after checkpoint and was active at time of system crash.

*   **Recovery Actions:**
    *   $T_a$: Nothing is done.
    *   $T_b$ and $T_c$: **Transaction redo** is performed.
    *   $T_d$: **Transaction undo** is performed.

### **3.5 Redo-Undo Phases Strategy**
Recovery from failure involves two phases:
1.  **Redo phase:** Replay updates of all transactions, whether they committed, aborted, or are incomplete.
2.  **Undo phase:** Undo all incomplete transactions.

| Transaction Type | State at Failure | Strategy |
| :--- | :--- | :--- |
| **$T_1$** | Committed before Checkpoint | Ignore. |
| **$T_2$ / $T_4$** | Committed since Checkpoint | Redo. |
| **$T_3$ / $T_5$** | Running at Failure | Redo then Undo. |

> [!TIP]
> Redo $T_3, T_5$ (which are incomplete) brings the system to the point of failure so they can then be undone to reach a consistent state.

### **3.6 The Algorithms**

#### **Redo Phase Algorithm**
1. Find last $<checkpoint~L>$ record, and set **undo-list** to $L$.
2. Scan forward from the checkpoint record.
3. Whenever a record $<T_i, X_j, V_1, V_2>$ is found, redo it by writing $V_2$ to $X_j$.
4. Whenever a log record $<T_i \text{ start}>$ is found, add $T_i$ to **undo-list**.
5. Whenever a log record $<T_i \text{ commit}>$ or $<T_i \text{ abort}>$ is found, remove $T_i$ from **undo-list**.

*   **Operation mapping:**
    *   INSERT $\rightarrow$ Recovery manager generates an insert from the log.
    *   DELETE $\rightarrow$ Recovery manager generates a delete from the log.
    *   UPDATE $\rightarrow$ Recovery manager generates an update from the log.

#### **Undo Phase Algorithm**
1. Scan log backwards from the end.
2. Whenever a log record $<T_i, X_j, V_1, V_2>$ is found where $T_i$ is in **undo-list**:
    *   Perform undo by writing $V_1$ to $X_j$.
    *   Write a CLR log record $<T_i, X_j, V_1>$.
3. Whenever a log record $<T_i \text{ start}>$ is found where $T_i$ is in **undo-list**:
    *   Write a log record $<T_i \text{ abort}>$.
    *   Remove $T_i$ from **undo-list**.
4. Stop when **undo-list** is empty.

*   **Operation mapping:**
    *   Faulty INSERT $\rightarrow$ deletes data item(s).
    *   Faulty DELETE $\rightarrow$ inserts deleted data item(s) from log.
    *   Faulty UPDATE $\rightarrow$ writes before-update value from log.

---

## **4. Comprehensive Example: Failure Recovery**

<img width="932" height="524" alt="image" src="https://github.com/user-attachments/assets/afe1591c-1a96-48d6-8990-66f4d0fd70ac" />

*   **Scenario:**
    *   $T_1$ had committed before the crash.
    *   $T_0$ had been completely rolled back before the crash.
    *   Checkpoint record contains active list $\{T_0, T_1\}$.
*   **Redo Pass:** Replays every log record since checkpoint.
    *   **undo-list** initially $\{T_0, T_1\}$.
    *   $T_1$ removed (commit found).
    *   $T_2$ added (start found).
    *   $T_0$ removed (abort found).
    *   **Final undo-list:** $\{T_2\}$.
*   **Undo Pass:** Scans backwards.
    *   Finds $T_2$ update on A, restores old value, writes redo-only record.
    *   Finds $T_2$ start record, adds $T_2$ abort, terminates.






---













# 11.4: Backup & Recovery/4: Recovery/3

### **1. Objectives**
*   To understand Recovery with Early Lock Release.
*   To understand how to plan for backup and recovery.


## **2. Recovery with Early Lock Release**

### **2.1. Motivation and Problem**
*   Any index used in processing a transaction, such as a $B^+$-tree, can be treated as normal data.
*   To increase concurrency, the $B^+$-tree concurrency control algorithm often allows locks to be released early, in a non-two-phase manner.
*   **The Problem:** As a result of early lock release, it is possible that a value in a $B^+$-tree node is updated by one transaction $T_1$, which inserts an entry $(V_1, R_1)$, and subsequently updated by another transaction $T_2$, which inserts an entry $(V_2, R_2)$ in the same node.
*   If $T_1$ must be undone, we cannot simply replace the contents of the node with the old value prior to $T_1$’s insert, since that would also undo the insert performed by $T_2$; $T_2$ may still commit or may have already committed.
*   **The Solution:** The only way to undo the effect of the insertion of $(V_1, R_1)$ is to execute a corresponding delete operation.

### **2.2. Core Concepts**
*   **Logical Undo:** Supports high-concurrency locking techniques by executing a counter-operation (like a delete to undo an insert) instead of restoring old physical values.
*   **Repeating History:** Recovery is based on executing exactly the same actions as normal processing, including redo of log records of incomplete transactions, followed by subsequent undo.
*   **System Applicability:** Early lock release is vital for indices and frequently updated system data structures, such as those tracking records of a relation, free space in a block, and free blocks.

<img width="1478" height="826" alt="image" src="https://github.com/user-attachments/assets/2e8c52ed-5b2b-480e-9610-ede89e368ede" />

---

## **3. Logical Undo Logging**

### **3.1. Physical Redo vs. Logical Undo**
*   **Logical Undo Logging:** Undo log records contain the operation to be executed (logical operation). Examples include:
    *   Delete of a tuple to undo an insert of a tuple (allows early lock release on space allocation).
    *   Subtracting a deposited amount to undo a deposit (allows early lock release on bank balances).
*   **Physical Redo:** Redo information is still logged physically (new value for each write) because logical redo is complicated by the fact that the database state on disk may not be "operation consistent" when recovery starts.

### **3.2. Operation Logging Process**
1.  **Start:** When an operation begins, log $\langle T_i, O_j, \text{operation-begin} \rangle$, where $O_j$ is a unique identifier of the operation instance.
2.  **Execution:** While executing, normal update log records are created for all updates. These include old-value (physical undo) and new-value (physical redo) information.
3.  **End:** When the operation completes, $\langle T_i, O_j, \text{operation-end}, U \rangle$ is logged, where $U$ contains information to perform a logical undo.

### **3.3. Operation Logging Rules**
*   If a crash/rollback occurs **before** the operation completes: The operation-end record is not found; the physical undo information is used.
*   If a crash/rollback occurs **after** the operation completes: The operation-end record is found; logical undo is performed using $U$, and physical undo information is ignored.
*   **Redo:** Redo after a crash always uses physical redo information.

<img width="805" height="351" alt="image" src="https://github.com/user-attachments/assets/82a7e49e-669a-4860-b90a-8c142ec2fc67" />

---

## **4. Transaction Rollback with Logical Undo**

The rollback of transaction $T_i$ involves scanning the log backwards and applying these rules:

*   **(a)** If a physical log record $\langle T_i, X, V_1, V_2 \rangle$ is found, perform the undo by writing $V_1$ to $X$ and log a compensation record $\langle T_i, X, V_1 \rangle$.
*   **(b)** If an operation-end record $\langle T_i, O_j, \text{operation-end}, U \rangle$ is found:
    *   Roll back the operation logically using $U$.
    *   Log updates during rollback like normal operations.
    *   Generate an operation-abort record $\langle T_i, O_j, \text{operation-abort} \rangle$.
    *   Skip all preceding log records for $T_i$ until the $\langle T_i, O_j, \text{operation-begin} \rangle$ record is found.
*   **(c)** If a redo-only record is found, ignore it.
*   **(d)** If an operation-abort record is found, skip preceding log records for $T_i$ until $\langle T_i, O_j, \text{operation-begin} \rangle$ to prevent multiple rollbacks of the same operation.
*   **(e)** Stop the scan when $\langle T_i, \text{start} \rangle$ is found.
*   **(f)** Add $\langle T_i, \text{abort} \rangle$ to the log.

<img width="619" height="445" alt="image" src="https://github.com/user-attachments/assets/b4349e09-6519-4aee-8c83-224f71ec82a2" />

---

## **5. Recovery Algorithm with Logical Undo**

Recovery from a system crash takes place in two phases:

### **5.1. Redo Phase**
*   Scan the log forward from the last $\langle \text{checkpoint } L \rangle$ record to the end of the log.
*   Repeat history by physically redoing all updates of all transactions.
*   Create an **undo-list**:
    *   Set to $L$ initially.
    *   Add $T_i$ when $\langle T_i, \text{start} \rangle$ is found.
    *   Delete $T_i$ when $\langle T_i, \text{commit} \rangle$ or $\langle T_i, \text{abort} \rangle$ is found.

### **5.2. Undo Phase**
*   Scan the log backwards, performing undo on log records of transactions in the undo-list.
*   Log records are processed using the logical undo rules (skipping records where an operation-end or operation-abort is found).
*   When $\langle T_i, \text{start} \rangle$ is found for a transaction in the undo-list, write a $\langle T_i, \text{abort} \rangle$ record.
*   The phase terminates when the undo-list is empty.

---

## **6. Planning for Backup and Recovery**

Several factors determine the setup of a Backup and Recovery plan:

*   **Data Importance:** How critical is the information? Business-critical data requires extra copies and easy restoration plans.
*   **Frequency of Change:** How often is the database updated? Critical data modified daily requires a daily backup schedule.
*   **Speed:** How much time is allocated for backup and recovery? This determines the maximum period that can be spent on these tasks.
*   **Equipment:** Availability of necessary software and hardware resources (e.g., storage media, servers).
*   **Employees:** Who is responsible? Ideally, one supervisor and several specialists (system administrators) for actual execution.
*   **Storing:** 
    *   **Online/Offsite:** Allows recovery from natural disasters (fire, flood).
    *   **Onsite:** Essential for quick restoration but has capacity and maintenance bottlenecks.








---











