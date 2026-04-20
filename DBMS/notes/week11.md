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






