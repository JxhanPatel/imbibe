# **5.1: Relational Database Design/1**


## **1. Recap**
*   Relational algebra was discussed with accompanying examples.
*   Tuple relational and domain relational calculus were introduced.
*   The equivalence of algebra and calculus was illustrated.
*   The Design Process for Database Systems was introduced.
*   The E-R Model for real-world representation with entities, entity sets, attributes, and relationships was elucidated.
*   ER Diagram notation for ER Models was illustrated.
*   The translation of ER Models to Relational Schema and extended features of the ER Model were discussed.
*   Various design issues were deliberated upon.



## **2. Objectives**
*   To identify the features of good relational design.
*   To familiarize with the First Normal Form (1NF).



## **3. Features of Good Relational Design**

**Good Relational Design Objectives:**
*   Reflects the real-world structure of the problem.
*   Can represent all expected data over time.
*   Avoids redundant storage of data items.
*   Provides efficient access to data.
*   Supports the maintenance of data integrity over time.
*   Clean, consistent, and easy to understand.
*   **Note:** These objectives are sometimes contradictory.

**What is a Good Schema?**
*   **Example 1 (Poor Design):** An `instructor_with_department` table combining all information into a single flat structure.
    *   **ID:** Acts as the Key.
    *   **building, budget:** These represent **Redundant Information** because they are properties of the department, not the instructor, and are repeated for every instructor in that department.
    *   **name, salary, dept_name:** These fields contain **No Redundant Information** in this context.

<img width="944" height="548" alt="image" src="https://github.com/user-attachments/assets/cbc4fb7d-d9ef-4dd4-a6e7-4e3bdc2ca6d8" />

*   **Example 2 (Good Design):** Segregating information into two distinct tables: `instructor` and `department`.
*   **Example 3 (Combining without Redundancy):** Consider combining `sec_class(sec_id, building, room_number)` and `section(course_id, sec_id, semester, year)` into one relation: `section(course_id, sec_id, semester, year, building, room_number)`. In this specific case, there is no repetition of information.

---

## **4. Redundancy and Anomaly**

*   **Redundancy:** Having multiple copies of the same data in the database. This problem arises when a database is not normalized.
*   **Anomaly:** Inconsistencies that can arise due to data changes in a database during insertion, deletion, and update operations.
*   These problems occur in poorly planned, un-normalized databases where all data is stored in one table (a flat-file database).

**Kinds of Anomalies:**
1.  **Insertions Anomaly:** Occurs when the insertion of a data record is not possible without adding some additional unrelated data to the record.
    *   *Example:* We cannot add an instructor in `instructor_with_department` if the department does not yet have an assigned building or budget.
2.  **Deletion Anomaly:** Occurs when the deletion of a data record results in losing some unrelated information that was stored as part of the record.
    *   *Example:* If we delete the last instructor of a department from `instructor_with_department`, we lose the building and budget information for that department.
3.  **Update Anomaly:** Occurs when data is changed, necessitating changes across many records, leading to the possibility of some changes being made incorrectly.
    *   *Example:* When the budget changes for a department with a large number of instructors in the `instructor_with_department` table, the application may miss updating some of them.

**Observations on Design:**
*   $\text{Redundancy } \Rightarrow \text{ Anomaly}$.
*   Separate relations for `instructor` and `department` are better than a combined `instructor_with_department` table.
*   **Dependency causes redundancy:** `dept_name` uniquely decides `building` and `budget`; a department cannot have two different budgets or buildings.
*   $\text{Normalization } \Rightarrow \text{ Good Decomposition}$.

---

## **5. Decomposition**

*   **Decomposition** involves partitioning a relation into smaller relations to remove or minimize redundancy.
*   *Example:* `instructor_with_department` can be decomposed into `instructor` and `department`.
*   **Functional Dependency Rule:** If a schema existed as `(dept_name, building, budget)`, then `dept_name` would be a candidate key.
  
Notation: $\text{deptname } \rightarrow \text{ building, budget}$.
*   In a combined table, if `dept_name` is not a candidate key, information may be repeated, indicating the need to decompose.

**Lossy vs. Lossless Decomposition:**
*   Not all decompositions are good.
*   **Lossy Decomposition:** A decomposition that cannot preserve information because it is impossible to reconstruct the original relation.
    *   *Example:* Decomposing `employee(ID, name, street, city, salary)` into `employee1(ID, name)` and `employee2(name, street, city, salary)` is lossy if names can be duplicates.

<img width="704" height="483" alt="image" src="https://github.com/user-attachments/assets/7e258f7f-aff2-43a3-813f-eee5408898ee" />

*   **Lossless-Join Decomposition:** A decomposition of relation $R$ into $R_1, R_2$ such that a natural join of the two smaller relations returns the original relation.
    *   **Conditions:**
        1.  $R_1 \cup R_2 = R$.
        2.  $R_1 \cap R_2 \neq \emptyset$.
        3.  For all instances $r \in R$, where $r_1 = \Pi_{R1}(r)$ and $r_2 = \Pi_{R2}(r)$, then $r_1 \bowtie r_2 = r$.

<img width="842" height="468" alt="image" src="https://github.com/user-attachments/assets/fd3f4344-7e98-4067-9922-19408e824d41" />

---

## **6. First Normal Form (1NF)**

*   **Atomic Domain:** A domain is atomic if its elements are considered indivisible units.
*   **Non-atomic Domain Examples:**
    *   Sets of names or composite attributes.
    *   Identification numbers like `CS101` that can be broken into parts (e.g., extracting "CS" to find a department).
*   **Encoding information in application programs** instead of the database is a consequence of non-atomic domains and is considered a bad idea.

**Formal Definition of 1NF:**
A relational schema $R$ is in First Normal Form (1NF) if:
1.  The domains of all attributes of $R$ are atomic.
2.  The value of each attribute contains only a single value from that domain.

**Issues with Non-1NF Designs (Telephone Number Examples):**
1.  **Composite/Multi-valued Field:** Storing multiple numbers in one field (e.g., "555-861, 192-122") violates 1NF.
2.  **Multiple Attributes for One Concept:** Using `Telephone Number 1` and `Telephone Number 2` attributes.
    *   Creates arbitrary and meaningless ordering.
    *   Makes searching difficult (which attribute to query?).
    *   Limits capacity (why only two numbers?).
3.  **Duplicate Rows:** Repeating `Customer ID`, `First Name`, and `Surname` for each phone number.
    *   Redundant information.
    *   Primary key must change from `ID` to `(ID, Telephone Number)`.

**1NF Solution:**
*   Decompose into two relations: `Customer Name(Customer ID, First Name, Surname)` and `Customer Telephone Number(Customer ID, Telephone Number)`.
*   This establishes a one-to-many relationship between parent and child relations.
*   This decomposition helps attain 1NF and satisfies requirements for higher normal forms like 2NF and 3NF.










---


# **5.2: Relational Database Design/2**



## **1. Recap**
*   Identified the features of good relational design.
*   Familiarized with the First Normal Form (1NF).
*   Discussed the basic issues of why normalization is needed to reduce redundancy and anomaly.



## **2. Objectives**
*   To Introduce **Functional Dependencies (FDs)**.



## **3. Goal: Devise a Theory for Good Relations**
*   Decide whether a particular relation $R$ is in "good" form.
*   In the case that a relation $R$ is not in "good" form, decompose it into a set of relations $\{R_1, R_2, ..., R_n\}$ such that:
    *   Each relation is in good form.
    *   The decomposition is a **lossless-join decomposition**.
*   The theory is based on:
    *   Functional dependencies.
    *   Multivalued dependencies.
    *   Other dependencies.



## **4. Functional Dependencies (FDs)**

**Definition:**
*   Constraints on the set of legal relations.
*   Require that the value for a certain set of attributes determines uniquely the value for another set of attributes.
*   A functional dependency is a generalization of the notion of a key.
*   Functional dependencies are an encoding of constraints or business rules that exist in the real world.

**Formal Definition:**
*   Let $R$ be a relation schema, $\alpha \subseteq R$ and $\beta \subseteq R$.
*   The functional dependency or FD $\alpha \rightarrow \beta$ holds on $R$ if and only if for any legal relations $r(R)$, whenever any two tuples $t_1$ and $t_2$ of $r$ agree on the attributes $\alpha$, they also agree on the attributes $\beta$.
*   **Mathematical Expression:**
    $$t_1[\alpha] = t_2[\alpha] \Rightarrow t_1[\beta] = t_2[\beta]$$.

<img width="1042" height="745" alt="image" src="https://github.com/user-attachments/assets/6dbfc1df-600f-4b2b-8bd2-7991ec4dfbbf" />

**Example Workout: Hold vs. Not Hold**
Consider $r(A, B)$ with the following instance:
| A | B |
| :--- | :--- |
| 1 | 4 |
| 1 | 5 |
| 3 | 7 |

*   On this instance, **$A \rightarrow B$ does NOT hold** because tuples (1, 4) and (1, 5) match on $A$ but differ on $B$.
*   However, **$B \rightarrow A$ does hold** on this instance because every value in column $B$ is distinct, ensuring matching $B$ values (if they existed) would match on $A$.
*   **Note:** We cannot have tuples like (2, 4), or (3, 5), or (4, 7) added to the current instance if $B \rightarrow A$ is to continue holding.

---

## **5. Keys and Functional Dependencies**

*   $K$ is a **superkey** for relation schema $R$ if and only if $K \rightarrow R$ holds on $R$.
*   $K$ is a **candidate key** for $R$ if and only if:
    1.  $K \rightarrow R$ holds.
    2.  For no $\alpha \subset K$, $\alpha \rightarrow R$ holds (minimal).
*   Functional dependencies allow us to express constraints that cannot be expressed using superkeys alone.

---

## **6. Practical Examples of FDs**

**University Schema `inst_dept` Example:**
$inst\_dept(ID, name, salary, dept\_name, building, budget)$
*   **Expected to hold:**
    *   $dept\_name \rightarrow building$.
    *   $dept\_name \rightarrow budget$.
    *   $ID \rightarrow salary$.
    *   $ID \rightarrow dept\_name$.
*   **NOT expected to hold:**
    *   $dept\_name \rightarrow salary$ (Department name does not decide instructor salary).

**Student Data Example:**
| StudentID | Semester | Lecture | TA |
| :--- | :--- | :--- | :--- |
| 1234 | 6 | Numerical Methods | John |
| 1221 | 4 | Numerical Methods | Smith |
| 1234 | 6 | Visual Computing | Bob |
| 1201 | 2 | Numerical Methods | Peter |
| 1201 | 2 | Physics II | Simon |

*   **FDs observed:**
    *   $StudentID \rightarrow Semester$.
    *   $StudentID, Lecture \rightarrow TA$.
    *   $\{StudentID, Lecture\} \rightarrow \{TA, Semester\}$.

<img width="811" height="422" alt="image" src="https://github.com/user-attachments/assets/b6786f5e-df43-48be-ad82-788dcdb167d6" />

**Employee Data Example:**
*   $EmployeeID \rightarrow EmployeeName$.
*   $EmployeeID \rightarrow DepartmentID$.
*   $DepartmentID \rightarrow DepartmentName$.

---

## **7. Satisfaction vs. Holding**

*   **Satisfies:** If a relation $r$ is legal under a set $F$ of functional dependencies, we say that $r$ **satisfies** $F$.
*   **Holds:** We say that $F$ **holds** on $R$ if **all** legal relations on $R$ satisfy the set of functional dependencies $F$.
*   **Caution:** A specific instance of a relation schema may satisfy a functional dependency by chance (e.g., if all instructor names happen to be unique), but in such cases we **do not** say that the FD holds on the schema $R$.

---

## **8. Trivial Functional Dependencies**
*   A functional dependency is **trivial** if it is satisfied by all instances of a relation.
*   In general, $\alpha \rightarrow \beta$ is trivial if $\beta \subseteq \alpha$.
*   **Examples:**
    *   $ID, name \rightarrow ID$.
    *   $name \rightarrow name$.

---

## **9. Closure of a Set of FDs (Introduction)**
*   Given a set of Functional Dependencies $F$, we can infer new dependencies.
*   The set of all functional dependencies logically implied by $F$ is called the **Closure Set $F^+$**.
*   **Example:**
    *   If $F = \{A \rightarrow B, B \rightarrow C\}$, then by transitivity, $A \rightarrow C$ is logically implied.
    *   $F^+ = \{A \rightarrow B, B \rightarrow C, A \rightarrow C\}$ (plus all trivial dependencies).

---
# Quick Questions

<img width="1148" height="3394" alt="image" src="https://github.com/user-attachments/assets/407735a2-71cd-433f-9348-dc2ae886dbcf" />

---



# 5.3 Relational Database Design/3





## **Objectives**
*   To develop the theory of functional dependencies.
*   To understand how a schema can be decomposed for a ‘good’ design using functional dependencies.



## **1. Functional Dependency Theory**

### **1.1 Functional Dependencies: Armstrong’s Axioms**
Given a set of Functional Dependencies $F$, we can infer new dependencies by the **Armstrong’s Axioms**:
*   **Reflexivity:** if $\beta \subseteq \alpha$, then $\alpha \rightarrow \beta$.
*   **Augmentation:** if $\alpha \rightarrow \beta$, then $\gamma\alpha \rightarrow \gamma\beta$.
*   **Transitivity:** if $\alpha \rightarrow \beta$ and $\beta \rightarrow \gamma$, then $\alpha \rightarrow \gamma$.

These axioms can be repeatedly applied to generate new FDs and added to $F$. A new FD obtained by applying the axioms is said to be **logically implied** by $F$. The process of generations of FDs terminates after a finite number of steps, and we call it the **Closure Set $F^+$** for FDs $F$. This is the set of all FDs logically implied by $F$.
*   Clearly, $F \subseteq F^+$.
*   These axioms are **Sound** (generate only functional dependencies that actually hold) and **Complete** (eventually generate all functional dependencies that hold).

### **1.2 Additional Derived Rules**
The following rules can be inferred from basic Armstrong’s axioms:
*   **Union:** if $\alpha \rightarrow \beta$ holds and $\alpha \rightarrow \gamma$ holds, then $\alpha \rightarrow \beta\gamma$ holds.
*   **Decomposition:** if $\alpha \rightarrow \beta\gamma$ holds, then $\alpha \rightarrow \beta$ holds and $\alpha \rightarrow \gamma$ holds.
*   **Pseudotransitivity:** if $\alpha \rightarrow \beta$ holds and $\gamma\beta \rightarrow \delta$ holds, then $\alpha\gamma \rightarrow \delta$ holds.

### **1.3 Examples of Closure $F^+$**
**Example 1:**
*   $F = \{A \rightarrow B, B \rightarrow C\}$.
*   $F^+ = \{A \rightarrow B, B \rightarrow C, A \rightarrow C\}$.

**Example 2:**
*   $R = (A, B, C, G, H, I)$.
*   $F = \{A \rightarrow B, A \rightarrow C, CG \rightarrow H, CG \rightarrow I, B \rightarrow H\}$.
*   **Some members of $F^+$:**
    *   $A \rightarrow H$ (by transitivity from $A \rightarrow B$ and $B \rightarrow H$).
    *   $AG \rightarrow I$ (by augmenting $A \rightarrow C$ with $G$ to get $AG \rightarrow CG$ and then transitivity with $CG \rightarrow I$).
    *   $CG \rightarrow HI$ (by augmenting $CG \rightarrow I$ with $CG$ to infer $CG \rightarrow CGI$, and augmenting $CG \rightarrow H$ with $I$ to infer $CGI \rightarrow HI$, then transitivity).

<img width="897" height="400" alt="image" src="https://github.com/user-attachments/assets/66d57d5a-3e5f-4030-918c-b1ab9d0b5893" />

---

## **2. Closure of Attribute Sets**

### **2.1 Definition and Algorithm**
Given a set of attributes $\alpha$, the **closure of $\alpha$ under $F$** (denoted by $\alpha^+$) is the set of attributes that are functionally determined by $\alpha$ under $F$.

**Algorithm to compute $\alpha^+$:**
```text
result := alpha;
while (changes to result) do
    for each beta -> gamma in F do
        begin
            if beta ⊆ result then result := result ∪ gamma;
        end
```


### **2.2 Attribute Closure Example**
*   $R = (A, B, C, G, H, I)$.
*   $F = \{A \rightarrow B, A \rightarrow C, CG \rightarrow H, CG \rightarrow I, B \rightarrow H\}$.
*   **Computing $(AG)^+$:**
    1.  `result` = $AG$.
    2.  `result` = $ABCG$ ($A \rightarrow C$ and $A \rightarrow B$).
    3.  `result` = $ABCGH$ ($CG \rightarrow H$ and $CG \subseteq ABCG$).
    4.  `result` = $ABCGHI$ ($CG \rightarrow I$ and $CG \subseteq ABCGH$).

**Testing for Candidate Key:**
1.  **Is $AG$ a superkey?** Does $AG \rightarrow R$? (Check if $(AG)^+ \supseteq R$).
2.  **Is any subset of $AG$ a superkey?**
    *   Does $A \rightarrow R$? (Check if $(A)^+ \supseteq R$).
    *   Does $G \rightarrow R$? (Check if $(G)^+ \supseteq R$).

### **2.3 Uses of Attribute Closure**
*   **Testing for superkey:** Compute $\alpha^+$ and check if it contains all attributes of $R$.
*   **Testing functional dependencies:** To check if $\alpha \rightarrow \beta$ holds, check if $\beta \subseteq \alpha^+$.
*   **Computing closure of $F$:** For each $\gamma \subseteq R$, find $\gamma^+$, and for each $S \subseteq \gamma^+$, output $\gamma \rightarrow S$.

---

## **3. Decomposition Using Functional Dependencies**

### **3.1 Boyce-Codd Normal Form (BCNF)**
A relation schema $R$ is in **BCNF** with respect to a set $F$ of FDs if for all FDs in $F^+$ of the form $\alpha \rightarrow \beta$, where $\alpha \subseteq R$ and $\beta \subseteq R$, at least one of the following holds:
1.  $\alpha \rightarrow \beta$ is **trivial** (that is, $\beta \subseteq \alpha$).
2.  $\alpha$ is a **superkey** for $R$.

**Example of schema NOT in BCNF:**
`instr_dept (ID, name, salary, dept_name, building, budget)` is not in BCNF because `dept_name` $\rightarrow$ `building, budget` holds, but `dept_name` is not a superkey.

<img width="1322" height="483" alt="image" src="https://github.com/user-attachments/assets/14c92fe7-6b33-47ea-90a6-82941fffd39b" />


### **3.3 Lossless Join Decomposition**
A decomposition of $R$ into $R_1, R_2$ is a **lossless join** if $R_1 \bowtie R_2 = R$. To identify if a decomposition is lossless using an FD set, the following must hold:
1.  $R_1 \cup R_2 = R$
2.  $R_1 \cap R_2 \neq \emptyset$
3.  **$R_1 \cap R_2 \rightarrow R_1$** or **$R_1 \cap R_2 \rightarrow R_2$** (Common attribute must be a key for at least one relation).

### **3.4 Dependency Preservation**
A decomposition is **dependency preserving** if it is sufficient to test only those dependencies on each individual relation in order to ensure that all functional dependencies hold.

**Example of BCNF non-preservation:**
$R = (C, S, Z)$, $F = \{CS \rightarrow Z, Z \rightarrow C\}$. Key = $CS$.
*   $CS \rightarrow Z$ satisfies BCNF, but $Z \rightarrow C$ violates it.
*   Decompose: $R_1 = (Z, C)$ and $R_2 = (S, Z)$.
*   Intersection $\{Z\}$ is a key for $R_1$ ($Z \rightarrow C$), so it is lossless.
*   However, $CS \rightarrow Z$ cannot be checked without a join. It is **not** dependency preserving.

### **3.5 Third Normal Form (3NF)**
A relation schema $R$ is in **3NF** if for all $\alpha \rightarrow \beta \in F^+$, at least one of the following holds:
1.  $\alpha \rightarrow \beta$ is trivial.
2.  $\alpha$ is a superkey for $R$.
3.  Each attribute $A$ in $\beta - \alpha$ is contained in a candidate key for $R$.

3NF is a **minimal relaxation** of BCNF to ensure dependency preservation.

---

## **4. Goals of Normalization**
Decide whether a relation scheme $R$ is in “good” form; if not, decompose into $\{R_1, R_2, ..., R_n\}$ such that:
*   Each relation scheme is in good form.
*   Decomposition is **lossless-join**.
*   Preferably, decomposition is **dependency preserving**.

> [!CAUTION]
> **Problems with Decomposition:**
> 1. **Lossiness:** Impossible to reconstruct original relation.
> 2. **Joins:** Dependency checking and queries may require joins, becoming expensive.

---

## **5. Shortcomings of BCNF**
There are database schemas in BCNF that are not sufficiently normalized.

**Example:** `inst_info (ID, child_name, phone)`
*   An instructor has more than one phone and multiple children.
*   There are no non-trivial FDs, so it is in BCNF.
*   **Insertion Anomaly:** Adding a phone number requires adding two tuples.
*   **Solution:** Decompose into `inst_child(ID, child_name)` and `inst_phone(ID, phone)`.
*   This suggests the need for **Fourth Normal Form (4NF)**.

