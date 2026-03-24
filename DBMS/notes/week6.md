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





---





# 6.3: Relational Database Design/8: Case Study



## **Objectives**
*   To design the schema for a Library Information System (LIS).



## **Library Information System (LIS)**

<img width="1139" height="672" alt="image" src="https://github.com/user-attachments/assets/0ac1466d-9068-49ca-8fcf-49cbedf0aba9" />


### **1. Introduction**
The objective is to design a relational database schema for a Library Information System of an Institute based on a provided specification document.

**Tasks to be carried out:**
*   Identify the Entity Sets with attributes.
*   Identify the Relationships.
*   Build the initial set of relational schema.
*   Refine the set of schema with FDs that hold on them.
*   Finalize the design of the schema.

### **2. LIS Specification Excerpts**
*   An institute library has 200,000+ books and 10,000+ members.
*   Books are regularly issued by members on loan and returned after a period.
*   The library needs an LIS to manage the books, the members, and the issue-return process.
*   **Book Details:**
    *   Title, author (only the first author is maintained), publisher, year of publication.
    *   ISBN number (unique for the publication).
    *   Accession number (unique number for the specific copy of the book in the library).
    *   There may be multiple copies of the same book.
*   **Member Categories:**
    *   Undergraduate students, post graduate students, research scholars, and faculty members.
*   **Student Details:** Name, roll number, department, gender, mobile number, date of birth, and degree (undergrad, grad, doctoral).
*   **Faculty Details:** Name, employee id, department, gender, mobile number, and date of joining.
*   **Library Rules for Issue:**
    *   A book may be issued to a member if it is not already issued to someone else.
    *   A book may not be issued to a member if another copy of the same book is already issued to the same member.
*   **Member Quotas:**
    | Member Type | Max Books | Max Duration (Days) |
    | :--- | :--- | :--- |
    | Undergraduate (ug) | 2 | 14 |
    | Graduate (pg) | 4 | 30 |
    | Research Scholar (rs) | 6 | 90 |
    | Faculty (fc) | 10 | 180 |

### **3. Initial Identification: Entity Sets**

#### **3.1. Entity Set: books**
*   **Attributes:** `title`, `author_name` (composite), `publisher`, `year`, `ISBN_no`, `accession_no` (unique).

#### **3.2. Entity Set: students**
*   **Attributes:** `member_no` (unique), `name` (composite), `roll_no` (unique), `department`, `gender`, `mobile_no` (nullable), `dob`, `degree`.

#### **3.3. Entity Set: faculty**
*   **Attributes:** `member_no` (unique), `name` (composite), `id` (unique), `department`, `gender`, `mobile_no` (nullable), `doj`.

#### **3.4. Entity Set: members**
*   A unique membership number is issued to every member across all four categories.

#### **3.5. Entity Set: quota**
*   Defines `max_books` and `max_duration` for each `member_type`.

#### **3.6. Entity Set: staff (Speculated)**
*   Required to manage the LIS.
*   **Attributes:** `name` (composite), `id` (unique), `gender`, `mobile_no`, `doj`.

### **4. Initial Identification: Relationships**

#### **4.1. Relationship: book_issue**
*   **Involved Entity Sets:** `members` (referenced by `member_no`) and `books` (referenced by `accession_no`).
*   **Relationship Attribute:** `doi` (date of issue).
*   **Relationship Type:** Many-to-one from `books` (a copy is issued to at most one member).

--- 📸 INSERT IMAGE: [Diagram showing the relationship 'book_issue' connecting members and books with attribute 'doi' | Timestamp 17:59 in week7t.pdf] ---

---

## **Initial Relational Schema**
From the ER identification, the first-cut schemas are:
*   `books(title, author_fname, author_lname, publisher, year, ISBN_no, accession_no)`
*   `book_issue(member_no, accession_no, doi)`
*   `members(member_no, member_type)`
*   `quota(member_type, max_books, max_duration)`
*   `students(member_no, student_fname, student_lname, roll_no, department, gender, mobile_no, dob, degree)`
*   `faculty(member_no, faculty_fname, faculty_lname, id, department, gender, mobile_no, doj)`
*   `staff(staff_fname, staff_lname, id, gender, mobile_no, doj)`

---

## **Schema Refinement**

### **1. Refinement of 'books'**
*   **Functional Dependencies:**
    *   `ISBN_no` → `title, author_fname, author_lname, publisher, year`
    *   `accession_no` → `ISBN_no`
*   **Problem:** Redundancy of book information across multiple copies.
*   **Decomposition (BCNF):**
    *   `book_catalogue(title, author_fname, author_lname, publisher, year, ISBN_no)`
        *   **Key:** `ISBN_no`
    *   `book_copies(ISBN_no, accession_no)`
        *   **Key:** `accession_no`

### **2. Refinement of 'book_issue'**
*   `book_issue(member_no, accession_no, doi)`
*   **FD:** `member_no, accession_no` → `doi`
*   **Key:** `(member_no, accession_no)`. Note: `accession_no` alone is sufficient if many-to-one is strict.
*   In BCNF.

### **3. Refinement of 'quota' and 'members'**
*   `quota(member_type, max_books, max_duration)` is in BCNF with Key: `member_type`.
*   `members(member_no, member_type)` is in BCNF with Key: `member_no`.

### **4. Refinement of 'students' and 'faculty'**
*   Both schemas are in BCNF.
*   **Issues:** `member_no` is required for issue/return queries, but repeating student/faculty details with it is unnecessary.

### **5. Specialization Refinement (IS_A Relationship)**
To better support queries (e.g., getting a member's name from an accession number), we exploit the fact that `students` and `faculty` are specializations of `members`.

**New Entity: members**
*   **Attributes:**
    *   `member_no`
    *   `member_class` (‘student’ or ‘faculty’)
    *   `member_type` (ug, pg, rs, fc)
    *   `roll_no` (nullable; present if `member_class` = ‘student’)
    *   `id` (nullable; present if `member_class` = ‘faculty’)

**Specialization:**
*   `students` **IS_A** `members`
*   `faculty` **IS_A** `members`
*   Relationship is **One-to-one**.

**Sample Query Implementation (Refined):**
*   *Task:* Get the name of the member who issued book accession 162715.
```sql
SELECT ((SELECT faculty_fname AS First_Name, faculty_lname AS Last_Name 
         FROM faculty 
         WHERE member_class = 'faculty' AND members.id = faculty.id) 
        UNION 
        (SELECT student_fname AS First_Name, student_lname AS Last_Name 
         FROM students 
         WHERE member_class = 'student' AND members.roll_no = students.roll_no))
FROM members, book_issue
WHERE accession_no = 162715 AND book_issue.member_no = members.member_no;
```


---

## **Final Design of the Schema**
The refined final relational schema is:
1.  `book_catalogue(title, author_fname, author_lname, publisher, year, ISBN_no)`
2.  `book_copies(ISBN_no, accession_no)`
3.  `book_issue(member_no, accession_no, doi)`
4.  `quota(member_type, max_books, max_duration)`
5.  `members(member_no, member_class, member_type, roll_no, id)`
6.  `students(student_fname, student_lname, roll_no, department, gender, mobile_no, dob, degree)`
7.  `faculty(faculty_fname, faculty_lname, id, department, gender, mobile_no, doj)`
8.  `staff(staff_fname, staff_lname, id, gender, mobile_no, doj)`




---






# 4.4: Relational Database Design/9: MVD and 4NF


## **Objectives**
*   To understand multi-valued dependencies arising out of attributes that can have multiple values.
*   To define Fourth Normal Form (4NF) and learn the decomposition algorithm to 4NF.


## **1. Multivalued Dependency (MVD)**

### **1.1. Motivation and Basic Concept**
*   **Problem:** If two or more independent relations are kept in a single relation, then Multivalued Dependency is possible.
*   **Example (Person):** Consider a relation `Person(Man, Phones, Dogs_Like)`. A man's phones are independent of the dogs they like. However, in a 1NF relation, each of a man's phones appears with each of the dogs they like in all combinations.
*   **Notation:** Multivalued dependencies are written with two head arrows. We say $X$ multidetermines $Y$, denoted as $X \rightarrow\rightarrow Y$.

<img width="1102" height="532" alt="image" src="https://github.com/user-attachments/assets/de15f683-2a4f-480e-8a28-639bdda2d350" />

### **1.2. Formal Definition**
Let $R$ be a relation schema and let $\alpha \subseteq R$ and $\beta \subseteq R$. The multivalued dependency $\alpha \rightarrow\rightarrow \beta$ holds on $R$ if in any legal relation $r(R)$, for all pairs for tuples $t_1$ and $t_2$ in $r$ such that $t_1[\alpha] = t_2[\alpha]$, there exist tuples $t_3$ and $t_4$ in $r$ such that:
*   $t_1[\alpha] = t_2[\alpha] = t_3[\alpha] = t_4[\alpha]$
*   $t_3[\beta] = t_1[\beta]$
*   $t_3[R-\beta] = t_2[R-\beta]$
*   $t_4[\beta] = t_2[\beta]$
*   $t_4[R-\beta] = t_1[R-\beta]$

### **1.3. MVD Example: University Courses**
Consider a relation of university courses, the books recommended, and the lecturers teaching the course:
*   **Dependencies:** `course` $\rightarrow\rightarrow$ `book` and `course` $\rightarrow\rightarrow$ `lecturer`.

<img width="1127" height="558" alt="image" src="https://github.com/user-attachments/assets/2840a129-7b10-4cb4-ae0c-0ce252cfbf54" />


| Course | Book | Lecturer | Tuples |
| :--- | :--- | :--- | :--- |
| AHA | Silberschatz | John D | $t_1$ |
| AHA | Nederpelt | William M | $t_2$ |
| AHA | Silberschatz | William M | $t_3$ |
| AHA | Nederpelt | John D | $t_4$ |

*Explanation:* Because $t_1$ and $t_2$ agree on `Course` (AHA), the definition requires that $t_3$ and $t_4$ (swapping the books and lecturers) must also exist in the relation.

### **1.4. Theory of MVDs**
The closure $D^+$ is the set of all functional and multivalued dependencies logically implied by $D$.

| Name | Rule |
| :--- | :--- |
| **Complementation** | If $X \rightarrow\rightarrow Y$, then $X \rightarrow\rightarrow (R - (X \cup Y))$. |
| **Augmentation** | If $X \rightarrow\rightarrow Y$ and $W \supseteq Z$, then $WX \rightarrow\rightarrow YZ$. |
| **Transitivity** | If $X \rightarrow\rightarrow Y$ and $Y \rightarrow\rightarrow Z$, then $X \rightarrow\rightarrow (Z - Y)$. |
| **Replication** | If $X \rightarrow Y$, then $X \rightarrow\rightarrow Y$ (but the reverse is not true). |
| **Coalescence** | If $X \rightarrow\rightarrow Y$ and there is a $W$ such that $W \cap Y$ is empty, $W \rightarrow Z$ and $Y \supseteq Z$, then $X \rightarrow Z$. |

*Source:*

**Trivial MVD:** An MVD $X \rightarrow\rightarrow Y$ in $R$ is called trivial if:
*   $Y \subseteq X$ or
*   $X \cup Y = R$.

---

## **2. Fourth Normal Form (4NF)**

### **2.1. Definition**
A relation schema $R$ is in 4NF with respect to a set $D$ of functional and multivalued dependencies if for all multivalued dependencies in $D^+$ of the form $\alpha \rightarrow\rightarrow \beta$, where $\alpha \subseteq R$ and $\beta \subseteq R$, at least one of the following holds:
1.  $\alpha \rightarrow\rightarrow \beta$ is trivial (that is, $\beta \subseteq \alpha$ or $\alpha \cup \beta = R$).
2.  $\alpha$ is a superkey for schema $R$.

> [!IMPORTANT]
> If a relation is in 4NF, it is also in BCNF.

### **2.2. 4NF Decomposition Algorithm**
1.  **Initialize:** `result := {R}`.
2.  **Compute:** $D^+$.
3.  **Loop:** While there is a schema $R_i$ in `result` that is not in 4NF:
    *   Find a non-trivial MVD $\alpha \rightarrow\rightarrow \beta$ that holds on $R_i$ such that $\alpha$ is not a superkey for $R_i$ and $\alpha \cap \beta = \emptyset$.
    *   Decompose $R_i$ into:
        *   $R_{i,1} = (\alpha \cup \beta)$
        *   $R_{i,2} = (R_i - \beta)$
    *   Replace $R_i$ in `result` with $R_{i,1}$ and $R_{i,2}$.

<img width="747" height="389" alt="image" src="https://github.com/user-attachments/assets/bc3abe41-e2fa-47cb-bd45-9eda298d81e8" />

### **2.3. Example: 4NF Decomposition**
**Schema:** $R = (A, B, C, G, H, I)$
**Dependencies:** $A \rightarrow\rightarrow B$, $B \rightarrow\rightarrow HI$, $CG \rightarrow\rightarrow H$
*   $R$ is not in 4NF because $A \rightarrow\rightarrow B$ and $A$ is not a superkey.
*   **Decomposition:**
    1.  $R_1 = (A, B)$ (In 4NF).
    2.  $R_2 = (A, C, G, H, I)$ (Not in 4NF due to $CG \rightarrow\rightarrow H$).
    3.  Decompose $R_2$ into $R_3 = (C, G, H)$ and $R_4 = (A, C, G, I)$.
    4.  $R_4$ is not in 4NF because $A \rightarrow\rightarrow I$ (derived via transitivity).
    5.  Decompose $R_4$ into $R_5 = (A, I)$ and $R_6 = (A, C, G)$.
*   **Final 4NF result:** $(A, B), (C, G, H), (A, I), (A, C, G)$.
<img width="1094" height="556" alt="image" src="https://github.com/user-attachments/assets/34c7a350-cd95-4c5e-9fed-730b3ab6d55d" />

---

## **3. Case Study: LIS Example for 4NF**
Consider a version of `book_catalogue` where a `book_title` is associated with multiple authors and multiple editions.
*   **Relation:** `book_catalogue(book_title, author_fname, author_lname, edition)`.
*   **MVDs:**
    1.  `book_title` $\rightarrow\rightarrow$ `{author_fname, author_lname}`
    2.  `book_title` $\rightarrow\rightarrow$ `edition`
*   **Condition:** Relation has no functional dependencies, so it is in BCNF but violates 4NF.
*   **Decomposition:**
    *   `book_author(book_title, author_fname, author_lname)`
    *   `book_edition(book_title, edition)`
*   Both are now in 4NF.





---




