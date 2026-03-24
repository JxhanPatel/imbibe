# 6.1: Relational Database Design/6: Normal Forms

## **Recap**
*   Identified the features of good relational design.
*   Familiarized with the First Normal Form.
*   Introduced the notion and the theory of functional dependencies.
*   Discussed issues in "good" design in the context of functional dependencies.
*   Studied Algorithms for Properties of Functional Dependencies.
*   Understood the Characterization for and Determination of Lossless Join and Determination of Dependency Preservation.


## **Objectives**
*   To Understand the Normal Forms and their Importance in Relational Design – how progressive increase of constraints can minimize redundancy in a schema.


## **Outline**
*   Normal Forms: 1NF, 2NF, 3NF.


## **1. Normalization or Schema Refinement**

### **1.1. Definition and Goals**
*   Normalization or Schema Refinement is a technique of organizing the data in the database.
*   It is a systematic approach of decomposing tables to eliminate data redundancy and undesirable characteristics.
*   Goal of Normalization is to Eliminate Redundancy.
*   Redundancy refers to repetition of same data or duplicate copies of same data stored in different locations.
*   Normalization is used for mainly two purposes:
    1.  Eliminating redundant (useless) data.
    2.  Ensuring data dependencies make sense, that is, data is logically stored.
*   Most common technique for the Schema Refinement is decomposition.

### **1.2. Anomalies**
The undesirable characteristics that normalization aims to eliminate include:
*   **Insertion Anomaly:** Occurs when the insertion of a data record is not possible without adding some additional unrelated data to the record. For example, until a new faculty member, Dr. Newsome, is assigned to teach at least one course, his details cannot be recorded.
*   **Update Anomaly:** Occurs when data is inconsistent because changes are not made to all records containing that data. For example, Employee 519 is shown as having different addresses on different records.
*   **Deletion Anomaly:** Occurs when the deletion of a data record results in losing some unrelated information that was stored as part of the record. For example, all information about Dr. Giddens is lost if he temporarily ceases to be assigned to any courses.

<img width="1031" height="512" alt="image" src="https://github.com/user-attachments/assets/ab711251-2427-46f0-a13d-e65b51599c64" />

---

## **2. Desirable Properties of Decomposition**
When decomposing an original schema into 1NF, 2NF, or 3NF, two properties are critical:
*   **Lossless Join Decomposition Property:** It should be possible to reconstruct the original table.
*   **Dependency Preserving Property:** No functional dependency (or other constraints) should get violated.

<img width="518" height="336" alt="image" src="https://github.com/user-attachments/assets/dad50e28-3ce3-4adb-888d-d1ea9148cbaf" />

---

## **3. Overview of Normal Forms**
*   A normal form specifies a set of conditions that the relational schema must satisfy in terms of its constraints – they offer varied levels of guarantee for the design.
*   Most common normal forms are:
    1.  First Normal Form (1NF)
    2.  Second Normal Form (2NF)
    3.  Third Normal Form (3NF)
*   Informally, a relational database relation is often described as "normalized" if it meets third normal form.
*   Most 3NF relations are free of insertion, update, and deletion anomalies.

### **3.1. Additional Normal Forms**
*   Elementary Key Normal Form (EKNF).
*   Boyce-Codd Normal Form (BCNF).
*   Multivalued Dependencies and Fourth Normal Form (4NF).
*   Essential Tuple Normal Form (ETNF).
*   Join Dependencies and Fifth Normal Form (5NF).
*   Sixth Normal Form (6NF).
*   Domain/Key Normal Form (DKNF).

---

## **4. First Normal Form (1NF)**

### **4.1. Definition**
*   A relation is in First Normal Form if and only if all underlying domains contain atomic values only.
*   It must not have multivalued attributes (MVA).
*   A domain is atomic if its elements are considered to be indivisible units.
*   A relational schema $R$ is in First Normal Form (1NF) if:
    1.  The domains of all attributes of $R$ are atomic.
    2.  The value of each attribute contains only a single value from that domain.

### **4.2. Redundancy and Drawbacks in 1NF**
*   Non-atomic values complicate storage and encourage redundant (repeated) storage of data.
*   Example of non-atomic domains include set of names, composite attributes, or identification numbers like CS101 that can be broken up into parts.
*   Redundancy exists when the Left Hand Side (LHS) of a non-trivial functional dependency is not a superkey.
    *   Let $X \rightarrow Y$ be a non-trivial FD over $R$. If $X$ is not a superkey of $R$, then redundancy exists between the $X$ and $Y$ attribute sets.
    *   Because $X$ is not a candidate key, $X$ values can duplicate, and the corresponding $Y$ value would duplicate also.

<img width="986" height="504" alt="image" src="https://github.com/user-attachments/assets/cf8c4eb0-c4b4-47ec-a0e1-e2f361d692e8" />

<img width="996" height="521" alt="image" src="https://github.com/user-attachments/assets/2619605f-2d1a-4623-9cbd-4450b9906ba9" />

---

## **5. Second Normal Form (2NF)**

### **5.1. Definition**
*   Relation $R$ is in Second Normal Form (2NF) if and only if:
    1.  $R$ is in 1NF.
    2.  $R$ contains no partial dependency.

### **5.2. Partial Dependency**
*   Let $R$ be a relational schema and $X, Y, A$ be the attribute sets over $R$ where $X$ is any Candidate Key, $Y$ is a proper subset of a Candidate Key, and $A$ is a Non-Prime Attribute.
*   $(Y \rightarrow A)$ is a Partial dependency only if:
    *   $Y$: Proper subset of Candidate Key.
    *   $A$: Non-Prime Attribute.
*   **Prime attribute:** An attribute that is a part of a candidate key of the relation.
*   **Non-prime attribute:** An attribute that is not a member of any candidate key.

### **5.3. Example Workout**
*   Consider `STUDENT(Sid, Sname, Cname)` where `{SID, Cname}` is the primary key.
*   Functional dependency: `SID` $\rightarrow$ `Sname`.
*   This is a partial dependency because `SID` is a proper subset of the candidate key `{SID, Cname}` and `Sname` is a non-prime attribute.
*   **Normalization:** Decompose into $R_1(\text{SID, Sname})$ with key `SID` and $R_2(\text{SID, Cname})$ with key `{SID, Cname}`.
*   The resulting relations are Lossless Join, in 2NF, and Dependency Preserving.

<img width="996" height="459" alt="image" src="https://github.com/user-attachments/assets/cb5c4d3e-a3fa-40e5-8060-60e2a3266451" />

---

## **6. Third Normal Form (3NF)**

### **6.1. Definition**
*   A relation $R$ is in 3NF if and only if:
    1.  $R$ is in 2NF.
    2.  $R$ should not contain transitive dependencies (OR, Every non-prime attribute of $R$ is non-transitively dependent on every key of $R$).
*   Alternatively, $R$ is in 3NF iff for each of its non-trivial functional dependencies $X \rightarrow A$, at least one of the following conditions holds:
    1.  $A \subseteq X$ (The FD is trivial).
    2.  $X$ is a superkey of $R$.
    3.  Each attribute $A$ in $A - X$ is a prime attribute (contained in some candidate key).
*   A relation in 3NF is naturally in 2NF.

### **6.2. Transitive Dependency**
*   A transitive dependency is a functional dependency which holds by virtue of transitivity.
*   It can occur only in a relation that has three or more attributes.
*   Let $A, B, C$ designate three distinct attributes. Suppose the following hold:
    1.  $A \rightarrow B$
    2.  It is not the case that $B \rightarrow A$
    3.  $B \rightarrow C$
*   Then $A \rightarrow C$ is a transitive dependency.

<img width="996" height="509" alt="image" src="https://github.com/user-attachments/assets/99bfecfc-4ca5-49f8-9c94-f16a0058b7a3" />

### **6.3. Example Workout**
*   Consider `Sup_City(SID, Status, City)` where `SID` is the primary key and dependencies are `SID` $\rightarrow$ `City` and `City` $\rightarrow$ `Status`.
*   `SID` $\rightarrow$ `Status` is a transitive dependency.
*   **Normalization:** Decompose into $SC(\text{SID, City})$ and $CS(\text{City, Status})$.
*   The resulting relations are Lossless Join, in 3NF, and Dependency Preserving.

<img width="991" height="515" alt="image" src="https://github.com/user-attachments/assets/b051d22e-58bf-4cdc-9cfa-5aeb3d9dad47" />

### **6.4. Redundancy in 3NF**
*   There is some redundancy remaining in this schema.
*   Problems include repetition of information (e.g., the relationship between an instructor and a department) and the potential need for null values to represent relationships when certain attribute values (like a student ID) are missing.

<img width="1003" height="522" alt="image" src="https://github.com/user-attachments/assets/48e4383a-caeb-4dfd-abcb-6ddae5802e6c" />



---







# 6.2: Relational Database Design/7: Normal Forms



## **Objectives**
*   To Learn the Decomposition Algorithm for a Relation to 3NF.
*   To Learn the Decomposition Algorithm for a Relation to BCNF.




## **1. Decomposition to 3NF**

### **1.1. Motivation for 3NF Decomposition**
*   There are some situations where BCNF is not dependency preserving.
*   Efficient checking for FD violation on updates is important.
*   **Solution:** Define a weaker normal form, called Third Normal Form (3NF).
    *   Allows some redundancy (with resultant problems).
    *   But functional dependencies can be checked on individual relations without computing a join.
    *   There is always a lossless-join, dependency-preserving decomposition into 3NF.

### **1.2. 3NF Definition**
A relational schema $R$ is in 3NF if for every functional dependency $X \rightarrow A$ associated with $R$, at least one of the following holds:
1.  $A \subseteq X$ (that is, the FD is trivial).
2.  $X$ is a superkey of $R$.
3.  $A$ is part of some candidate key (not just superkey!).

*Note:* A relation in 3NF is naturally in 2NF.

### **1.3. Testing for 3NF**
*   **Optimization:** Need to check only FDs in $F$, need not check all FDs in $F^+$.
*   Use attribute closure to check for each dependency $\alpha \rightarrow \beta$, if $\alpha$ is a superkey.
*   If $\alpha$ is not a superkey, we have to verify if each attribute in $\beta$ is contained in a candidate key of $R$.
*   This test is rather more expensive, since it involves finding candidate keys.
*   Decomposition into 3NF can be done in polynomial time.

### **1.4. 3NF Decomposition Algorithm**

<img width="659" height="512" alt="image" src="https://github.com/user-attachments/assets/70642ead-1210-474d-983d-6f9f3187f9f1" />

**Algorithm Steps:**
1.  Let $F_c$ be a canonical cover for $F$.
2.  $i := 0$.
3.  For each functional dependency $\alpha \rightarrow \beta$ in $F_c$ do:
    *   If none of the schemas $R_j, 1 \le j \le i$ contains $\alpha\beta$, then:
        *   $i := i + 1$
        *   $R_i := \alpha\beta$.
4.  If none of the schemas $R_j, 1 \le j \le i$ contains a candidate key for $R$, then:
    *   $i := i + 1$
    *   $R_i := \text{any candidate key for } R$.
5.  **Optionally, remove redundant relations:**
    *   Repeat:
        *   If any schema $R_j$ is contained in another schema $R_k$:
            *   Delete $R_j$
            *   $R_j = R_i$
            *   $i = i - 1$
    *   Until no more $R_j$s can be deleted.
6.  Return $(R_1, R_2, \dots, R_i)$.

**Properties upon decomposition:**
*   Each relation schema $R_j$ is in 3NF.
*   Decomposition is **Dependency Preserving** and **Lossless Join**.

### **1.5. 3NF Decomposition Example**
*   **Relation schema:** `cust_banker_branch = (customer_id, employee_id, branch_name, type)`.
*   **Functional Dependencies:**
    1.  `customer_id, employee_id → branch_name, type`
    2.  `employee_id → branch_name`
    3.  `customer_id, branch_name → employee_id`.
*   **Canonical Cover ($F_c$):**
    *   `branch_name` is extraneous in the RHS of the 1st dependency.
    *   $F_c = \{$`customer_id, employee_id → type`, `employee_id → branch_name`, `customer_id, branch_name → employee_id` $\}$.
*   **Generated Schemas:**
    1.  `(customer_id, employee_id, type)`
    2.  `(employee_id, branch_name)`
    3.  `(customer_id, branch_name, employee_id)`.
*   **Resultant simplified 3NF schema:**
    *   `(customer_id, employee_id, type)` (Contains a candidate key of the original schema).
    *   `(customer_id, branch_name, employee_id)`.

---

## **2. Decomposition to BCNF**

### **2.1. BCNF Definition**
A relation schema $R$ is in BCNF with respect to a set $F$ of FDs if for all FDs in $F^+$ of the form $\alpha \rightarrow \beta$, where $\alpha \subseteq R$ and $\beta \subseteq R$, at least one of the following holds:
*   $\alpha \rightarrow \beta$ is trivial (that is, $\beta \subseteq \alpha$).
*   $\alpha$ is a superkey for $R$.

### **2.2. BCNF Decomposition Algorithm**

<img width="707" height="501" alt="image" src="https://github.com/user-attachments/assets/6021a186-2c34-45b7-9943-c729f856ed0d" />

**Algorithm Steps:**
1.  `result := {R}`.
2.  `done := false`.
3.  Compute $F^+$.
4.  While (`not done`) do:
    *   If (there is a schema $R_i$ in result that is not in BCNF), then:
        *   Let $\alpha \rightarrow \beta$ be a nontrivial functional dependency that holds on $R_i$ such that $\alpha \rightarrow \beta$ is not in $F^+$, and $\alpha \cap \beta = \emptyset$.
        *   `result := (result - Ri) ∪ (Ri - β) ∪ (α, β)`.
    *   Else `done := true`.
*Note:* Each $R_i$ is in BCNF, and the decomposition is lossless-join.

### **2.3. BCNF Decomposition Example**
*   $R = (A, B, C)$
*   $F = \{A \rightarrow B, B \rightarrow C\}$
*   $Key = \{A\}$.
*   $R$ is not in BCNF ($B \rightarrow C$ but $B$ is not a superkey).
*   **Decomposition:** $R_1 = (B, C)$ and $R_2 = (A, B)$.

### **2.4. Dependency Preservation in BCNF**
*   It is not always possible to get a BCNF decomposition that is dependency preserving.
*   **Example:** $R = (J, K, L)$, $F = \{JK \rightarrow L, L \rightarrow K\}$.
*   Two candidate keys: $JK$ and $JL$.
*   $R$ is not in BCNF.
*   Any decomposition of $R$ will fail to preserve $JK \rightarrow L$. Testing for $JK \rightarrow L$ would then require a join.

---

## **3. Comparison of BCNF and 3NF**

### **3.1. Capability Summary**
*   It is **always** possible to decompose a relation into a set of relations that are in 3NF such that:
    *   The decomposition is lossless.
    *   The dependencies are preserved.
*   It is **always** possible to decompose a relation into a set of relations that are in BCNF such that:
    *   The decomposition is lossless.
    *   It **may not** be possible to preserve dependencies.

### **3.2. Comparison Table**
| S# | 3NF | BCNF |
| :--- | :--- | :--- |
| 1. | It concentrates on Primary Key | It concentrates on Candidate Key |
| 2. | Redundancy is high as compared to BCNF | 0% redundancy (w.r.t functional dependencies) |
| 3. | It preserves all the dependencies | It may not preserve the dependencies |
| 4. | A dependency $X \rightarrow Y$ is allowed in 3NF if $X$ is a super key or $Y$ is a part of some key | A dependency $X \rightarrow Y$ is allowed if $X$ is a super key |
