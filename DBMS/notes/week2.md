# 2.1: Introduction to the Relational Model/1


## Outline
* Example of a Relation
* Attributes
* Schema and Instance
* Keys
* Relational Query Languages

---

## 1. Example of a Relation
A relation is essentially a table. Below is an example of the `instructor` relation.

<img width="885" height="575" alt="image" src="https://github.com/user-attachments/assets/4b241bd4-87bc-4971-9384-792d6d5ed02f" />



### Instructor Relation Attributes:
* **ID**
* **name**
* **dept_name**
* **salary**

| ID | name | dept_name | salary |
| :--- | :--- | :--- | :--- |
| 10101 | Srinivasan | Comp. Sci. | 65000 |
| 12121 | Wu | Finance | 90000 |
| 15151 | Mozart | Music | 40000 |
| 22222 | Einstein | Physics | 95000 |
| 32343 | El Said | History | 60000 |
| 33456 | Gold | Physics | 87000 |
| 45565 | Katz | Comp. Sci. | 75000 |
| 58583 | Califieri | History | 62000 |
| 76543 | Singh | Finance | 80000 |
| 76766 | Crick | Biology | 72000 |
| 83821 | Brandt | Comp. Sci. | 92000 |
| 98345 | Kim | Elec. Eng. | 80000 |

---

## 2. Attributes
### 2.1 Attribute Types
* Each attribute has an allowed set of values, called the **domain** of the attribute.
* **Attribute values** are (normally) required to be **atomic**; that is, indivisible.
* The special value **null** is a member of every domain. Indicated that the value is "unknown".
* The **null** value causes complications in the definition of many operations.

---

## 3. Mathematical Definition of a Relation
1. Let $D_1, D_2, \dots, D_n$ be domains.
2. A **relation** $r$ is a subset of $D_1 \times D_2 \times \dots \times D_n$.
   * Thus, a relation is a set of $n$-tuples $(a_1, a_2, \dots, a_n)$ where each $a_i \in D_i$.


### 3.1 Relations are Unordered
* Order of tuples is irrelevant (tuples may be stored in an arbitrary order).
* **Example:** The `instructor` relation with unordered tuples.

| ID | name | dept_name | salary |
| :--- | :--- | :--- | :--- |
| 22222 | Einstein | Physics | 95000 |
| 12121 | Wu | Finance | 90000 |
| 58583 | Califieri | History | 62000 |
| 45565 | Katz | Comp. Sci. | 75000 |
| 98345 | Kim | Elec. Eng. | 80000 |
| 10101 | Srinivasan | Comp. Sci. | 65000 |
| 76543 | Singh | Finance | 80000 |
| 33456 | Gold | Physics | 87000 |
| 83821 | Brandt | Comp. Sci. | 92000 |
| 15151 | Mozart | Music | 40000 |
| 32343 | El Said | History | 60000 |
| 76766 | Crick | Biology | 72000 |

---

## 4. Database Schema and Instance
* **Database Schema** – is the logical structure of the database.
* **Database Instance** – is a snapshot of the data in the database at a given instant in time.

### 4.1 Formal Notation:
* $R = (A_1, A_2, \dots, A_n)$ is a **relation schema**.
  * Example: `instructor_schema = (ID, name, dept_name, salary)`
* $r(R)$ denotes a relation $r$ on the schema $R$.

---

## 5. Keys
Let $K \subseteq R$.

### 5.1 Superkey
$K$ is a **superkey** of $R$ if values for $K$ are sufficient to identify a unique tuple of each possible relation $r(R)$.
* Example: `{ID}` and `{ID, name}` are both superkeys of `instructor`.

### 5.2 Candidate Key
$K$ is a **candidate key** if $K$ is a minimal superkey.
* Example: `{ID}` is a candidate key for `instructor`.

### 5.3 Primary Key
A **primary key** is a candidate key chosen by the database designer as the principal means of identifying tuples within a relation.
* A primary key should choose an attribute whose value never, or very rarely, changes.
  * E.g., address changes, but ID does not change.

### 5.4 Foreign Keys
A relation $r_1$ may include among its attributes the primary key of another relation $r_2$. This attribute is called a **foreign key** from $r_1$, referencing $r_2$.
* The relation $r_1$ is called the **referencing relation**.
* The relation $r_2$ is called the **referenced relation**.

<img width="1337" height="708" alt="image" src="https://github.com/user-attachments/assets/60bb90aa-320b-4bf4-b231-8028b929c988" />


 ---

### 5.5 Referential Integrity Constraint
The value in the foreign key attributes of each tuple in the referencing relation must appear in the primary key attributes of some tuple in the referenced relation.

---

## 6. Schema Diagram for University Database


<img width="1285" height="773" alt="image" src="https://github.com/user-attachments/assets/b79aa452-2f29-4046-9966-9f439434e205" />



---

## 7. Relational Query Languages
Relational query languages are "Pure" languages:
* Relational algebra
* Tuple relational calculus
* Domain relational calculus

### 7.1 Key Characteristics:
* The above 3 pure languages are **equivalent** in computing power.
* We will concentrate on **relational algebra**.
* **Not Turing-machine equivalent**: Not all algorithms can be expressed in Relational Algebra (RA).
* Consists of **6 basic operations**.

---

## Summary
* Introduced the notion of attributes and their types.
* Taken an overview of the mathematical structure of relational model - schema and instance.
* Introduced the notion of keys - primary as well as foreign.
