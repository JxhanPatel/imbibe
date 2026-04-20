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






