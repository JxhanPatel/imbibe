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

<img width="933" height="509" alt="image" src="https://github.com/user-attachments/assets/071af0e1-e8fa-49e3-84d9-786dff3316d3" />


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

---










# 2.2: Introduction to Relational Model/2

---

## 1. Core Principles of Relations
Before discussing specific operators, it is essential to remember two fundamental principles of the relational model:

1.  **Relation as a Set (Ordering):** A relation is a set. Therefore, the ordering of elements, rows, or tuples in a relation is inconsequential. You can order them in whatever way you want; the instances remain equivalent and identical.
    

<img width="706" height="569" alt="image" src="https://github.com/user-attachments/assets/ba9ea77e-cbb3-4940-ba24-265b3f5ad6fa" />



---

2.  **Uniqueness of Tuples:** A set must always have its elements as unique. All rows and tuples must be distinct. Duplicate rows are not allowed in a correct relation; only one instance of a tuple will exist.

---

## 2. Relational Algebra Operators
Relational algebra is the first language discussed for handling queries. It consists of several operators that take one or more relations as input and produce a relation as output.

### 2.1 Select Operation ($\sigma$)
The **Select** operation is used to select a subset of rows from a relational instance which satisfy a particular condition.

* **Notation:** $$\sigma_{condition}(Relation)$$
* **Condition:** A boolean condition that every tuple being selected from the original relational instance must satisfy.
* **Example:** $$\sigma_{A=B \wedge D > 5}(R)$$
    * This selects only those tuples where attribute $A$ equals attribute $B$ **and** attribute $D$ is greater than $5$.
    * If a conjunction (**and**) is used, both parts must be met. If a disjunction (**or**) is used, any one of the conditions must be satisfied.



<img width="775" height="546" alt="image" src="https://github.com/user-attachments/assets/2f80513f-3b4a-494e-b350-7b0516a9ad16" />



### 2.2 Projection Operation ($\Pi$)
While selection filters rows horizontally, **Projection** takes out some columns (vertical cuts).

* **Notation:** $$\Pi_{L}(R)$$ where $L$ is the list of columns you want to project out (keep).
* **Behavior:** Anything not written in the list is simply erased out.
* **Uniqueness Property:** Because every row in a relation has to be unique, if erasing a column creates duplicate rows, one of them will get deleted.



<img width="792" height="563" alt="image" src="https://github.com/user-attachments/assets/12691a6b-3820-4417-b146-ae84bcf5ee24" />



### 2.3 Union Operation ($\cup$)
If two relations have identical sets of attributes, you can take their union.
* **Behavior:** All tuples are put together, followed by "unification" where duplicates are removed to ensure each row is unique.

### 2.4 Set Difference Operation ($-$)
Identifies what is in one relation but not in another.
* **Notation:** $$R - S$$
* **Definition:** It involves taking out those things in $S$ which are already in $R$.

<img width="767" height="528" alt="image" src="https://github.com/user-attachments/assets/37f51c8c-8b67-4c56-a7c2-e8bf62df72f7" />




### 2.5 Intersection Operation ($\cap$)
Tuples which are common to both relations.
* **Note:** Intersection and Difference are not independent. Intersection can be written in terms of difference:
    $$R \cap S = R - (R - S)$$

### 2.6 Cartesian Product ($\times$)
Takes the columns of one relation and the columns of another and makes all possible combinations.
* **Behavior:** If you have 2 tuples in table one and 4 tuples in table two, you get $2 \times 4 = 8$ tuples.
* **Attribute Naming:** If there are columns with the same name, they are uniquely identified by qualifying the name with the relation (e.g., $R.b$ and $S.b$).


<img width="765" height="537" alt="image" src="https://github.com/user-attachments/assets/0598bc6c-7763-4840-a0d1-c97f14a763d6" />

 

### 2.7 Rename Operation ($\rho$)
Required to change the name of a relation or attribute. 
* **Use Case:** Necessary for self-cross products ($R \times R$) where qualification ($R.a$ and $R.a$) would not be enough to uniquely identify attributes because both relations are the same.

---

## 3. Natural Join ($\bowtie$)
A more specific operation for relational algebra where $R$ and $S$ are relations on schemas $R$ and $S$.

* **Schema:** The result is on the schema $R \cup S$.
* **Logic:** It takes every pair of tuples from $R$ and $S$ (like a Cartesian product) but only keeps those that have the **same value on each attribute** in $R \cap S$ (common attributes).
* **Redundancy:** It is redundant to keep both common columns since they must have identical values, so one is removed and qualified names are not required.


<img width="1427" height="708" alt="image" src="https://github.com/user-attachments/assets/d99981a7-70a4-41cb-a88e-7dc0e4fa0e85" />



---

## 4. Aggregate Operators
These take a relation and, based on one or more columns, perform an aggregation.

* **Examples:** `sum`, `max`, `min`, `avg`, `count`.
* **Distinction:** Relational operators always produce a relation (table). Aggregate operations take a table as input but produce a **single value** as output. They are typically numerical and are not fundamental to "pure" relational algebra but are added for utility.

---

## 5. Summary of Characteristics
* **Input/Output:** Each query input is a table (or set of tables) and each query output is a table.
* **Turing Completeness:** Relational algebra is **not** Turing complete. There are programs for which no relational algebraic solution can be written, necessitating a host language.

### Operator Summary Table

| Operator Type | Examples | Input | Output |
| :--- | :--- | :--- | :--- |
| **Unary** | Select ($\sigma$), Project ($\Pi$) | 1 Relation | 1 Relation |
| **Binary** | Union ($\cup$), Difference ($-$), Product ($\times$), Join ($\bowtie$) | 2 Relations | 1 Relation |
| **Aggregate** | Sum, Max, Avg | 1 Relation | Single Value |


<img width="786" height="571" alt="image" src="https://github.com/user-attachments/assets/5f1bacb2-8fa8-4e58-8776-7b13e94c0347" />


---

