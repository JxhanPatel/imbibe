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




