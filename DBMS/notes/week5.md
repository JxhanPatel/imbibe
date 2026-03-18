# **5.1: Relational Database Design/1**

---

## **1. Recap**
*   Relational algebra was discussed with accompanying examples.
*   Tuple relational and domain relational calculus were introduced.
*   The equivalence of algebra and calculus was illustrated.
*   The Design Process for Database Systems was introduced.
*   The E-R Model for real-world representation with entities, entity sets, attributes, and relationships was elucidated.
*   ER Diagram notation for ER Models was illustrated.
*   The translation of ER Models to Relational Schema and extended features of the ER Model were discussed.
*   Various design issues were deliberated upon.

---

## **2. Objectives**
*   To identify the features of good relational design.
*   To familiarize with the First Normal Form (1NF).

---

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
