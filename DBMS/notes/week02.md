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











# 2.3: Introduction to SQL/1



#### **Outline**
*   **History of SQL**.
*   **Data Definition Language (DDL)**.
*   **Data Manipulation Language (DML): Query Structure**.

---

#### **1. History of SQL**

*   **IBM developed Structured English Query Language (SEQUEL)** as part of the **System R project**.
*   It was later renamed **Structured Query Language (SQL)**, though it is still commonly pronounced as "SEQUEL".
*   SQL has established itself as the **standard relational database language**.

**ANSI and ISO Standard SQL Timeline:**
| Standard | Key Features Added |
| :--- | :--- |
| **SQL-86** | First formalized by ANSI. |
| **SQL-89** | Added Integrity Constraints. |
| **SQL-92** | Major revision (ISO/IEC 9075 standard); became the De-facto Industry Standard. |
| **SQL:1999** | Added Regular Expression Matching, Recursive Queries, Triggers, Support for Procedural and Control Flow Statements, Non-scalar types (Arrays), and Some OO features (structured types), Embedding SQL in Java (SQL/OLB), and Embedding Java in SQL (SQL/JRT). |
| **SQL:2003** | Added XML features (SQL/XML), Window Functions, Standardized Sequences, and Columns with Auto-generated Values (identity columns). |
| **SQL:2006** | Added ways of importing and storing XML data in an SQL database, manipulating it within the database, and publishing both XML and conventional SQL-data in XML form. |
| **SQL:2008** | Legalizes ORDER BY outside Cursor Definitions; added INSTEAD OF Triggers, TRUNCATE Statement, and FETCH Clause. |
| **SQL:2011** | Added Temporal Data (PERIOD FOR); enhancements for Window Functions and FETCH Clause. |
| **SQL:2016** | Added Row Pattern Matching, Polymorphic Table Functions, and JSON. |
| **SQL:2019** | Added Multidimensional Arrays (MDarray type and operators). |

**Compliance:**
*   SQL is the **de facto industry standard** today for relational or structured data systems.
*   Commercial and open systems may be fully or partially compliant to one or more standards from SQL-92 onward.
*   Not all examples may work on every system; users should check their specific system's SQL documentation.

**Derivatives:**
*   **SPARQL** (pronounced "sparkle") is a recursive acronym for **SPARQL Protocol and RDF Query Language**.
*   It is a semantic query language for databases, able to retrieve and manipulate data stored in **Resource Description Framework (RDF)** format.
*   Standardized by the **W3C Consortium** as a key technology of the semantic web.

---

#### **2. Data Definition Language (DDL)**

The SQL data-definition language (DDL) allows the specification of information about relations, including:
*   The **Schema** for each relation.
*   The **Domain** of values associated with each attribute.
*   **Integrity Constraints**.
*   The set of **Indices** to be maintained for each relation.
*   **Security and Authorization** information for each relation.
*   The **Physical Storage Structure** of each relation on disk.

**Domain Types in SQL:**
*   `char(n)`: Fixed length character string, with user-specified length $n$.
*   `varchar(n)`: Variable length character strings, with user-specified maximum length $n$.
*   `int`: Integer (a finite subset of the integers that is machine-dependent).
*   `smallint`: Small integer (a machine-dependent subset of the integer domain type).
*   `numeric(p, d)`: Fixed point number, with user-specified precision of $p$ digits, with $d$ digits to the right of the decimal point.
*   `real`, `double precision`: Floating point and double-precision floating point numbers, with machine-dependent precision.
*   `float(n)`: Floating point number, with user-specified precision of at least $n$ digits.

<img width="1163" height="662" alt="image" src="https://github.com/user-attachments/assets/cd34fead-1550-4029-a866-439569038a95" />



 
 ---

**Create Table Construct:**
An SQL relation is defined using the `create table` command:
$$\text{create table } r (A_1 D_1, A_2 D_2, ..., A_n D_n, \text{(integrity-constraint}_1), ..., \text{(integrity-constraint}_k));$$
*   $r$ is the name of the relation.
*   Each $A_i$ is an attribute name in the schema of relation $r$.
*   $D_i$ is the data type of values in the domain of attribute $A_i$.

**Example:**
```sql
create table instructor (
    ID char(5),
    name varchar(20),
    dept_name varchar(20),
    salary numeric(8, 2));
```

**Integrity Constraints:**
*   `not null`: Indicates that an attribute cannot take a null value.
*   `primary key (A_1, ..., A_n)`: Specifies that the listed attributes form the primary key for the relation. The primary key attributes are required to be non-null and unique.
*   `foreign key (A_m, ..., A_n) references r`: Specifies that the values of these attributes for any tuple in the relation must correspond to values of the primary key attributes of some tuple in relation $r$.

**Example with Constraints:**
```sql
create table instructor (
    ID char(5),
    name varchar(20) not null,
    dept_name varchar(20),
    salary numeric(8, 2),
    primary key (ID),
    foreign key (dept_name) references department);
```

**Update and Schema Modification Commands:**
*   **Insert (DML):** `insert into instructor values ('10211', 'Smith', 'Biology', 66000);`
*   **Delete (DML):** `delete from student;` (Removes all tuples from the student relation).
*   **Drop Table (DDL):** `drop table r;` (Deletes the table and its schema).
*   **Alter (DDL):** `alter table r add A D;` (Adds attribute $A$ of type $D$ to relation $r$; existing tuples are assigned `null` for the new attribute).
*   **Alter (DDL):** `alter table r drop A;` (Drops attribute $A$ from relation $r$).

---

#### **3. Data Manipulation Language (DML): Query Structure**

**Basic Query Structure:**
A typical SQL query has the form:
$$\text{select } A_1, A_2, ..., A_n$$
$$\text{from } r_1, r_2, ..., r_m$$
$$\text{where } P;$$
*   $A_i$ represents an attribute.
*   $r_i$ represents a relation.
*   $P$ is a predicate.
*   The result of an SQL query is a relation.

**The Select Clause:**
*   Lists the attributes desired in the result of a query.
*   Corresponds to the **projection** operation of the relational algebra.
*   **Case Insensitivity:** SQL names are case insensitive (e.g., $Name \equiv NAME \equiv name$).
*   **Duplicates:** SQL allows duplicates in relations and query results.
    *   To force elimination of duplicates, use `distinct`.
    *   The keyword `all` specifies that duplicates should **not** be removed (default).
*   **Wildcard:** An asterisk (`*`) denotes "all attributes".
*   **Literals:** An attribute can be a literal.
    *   `select '437'` results in a table with one column and one row containing '437'.
    *   `select '437' as FOO` renames the literal column.
*   **Arithmetic Expressions:** Can contain $+$, $-$, $*$, and $/$ operating on constants or attributes.
    *   Example: `select ID, name, salary/12 as monthly_salary from instructor;`

**The Where Clause:**
*   Specifies conditions that the result must satisfy.
*   Corresponds to the **selection predicate** of the relational algebra.
*   **Connectives:** Comparison results can be combined using logical connectives `and`, `or`, and `not`.
*   Comparisons can be applied to results of arithmetic expressions.

**The From Clause:**
*   Lists the relations involved in the query.
*   Corresponds to the **Cartesian product** operation of the relational algebra.
*   It generates every possible pair of tuples from the listed relations.


<img width="624" height="479" alt="image" src="https://github.com/user-attachments/assets/d60f4a66-b21e-4ea2-8de6-3937a4f63304" />




---

#### **4. Summary**
*   **Introduced relational query language**.
*   **Familiarized with data definition and basic query structure**.

---













# 2.4: Introduction to SQL/2

---

## 1. Additional Basic Operations

The basic structure of the `select-from-where` query is extended with additional features to enable writing varied forms of queries and making query writing easier.

### 1.1 Cartesian Product in the From Clause

If multiple relations are specified in the `from` clause, a Cartesian product is formed.

- **Definition:** A combination of all tuples from one relation with all tuples from another.
- **Example:** If we have relations `instructor` and `teaches`, specifying both in the `from` clause with a wildcard `*` in the `select` clause creates a table containing all columns from both, with all possible record combinations.
- **Column Qualification:** If a column name is identical between two relations (e.g., `ID`), it is qualified by the relation name: `instructor.ID` and `teaches.ID`.


### 1.2 Joining Information

The raw Cartesian product is often not useful because it creates many meaningless tuples (where IDs do not match).

- **Condition:** To relate instructor names to course IDs, we match the `ID` from both tables.
- **Query Example:**

```sql
select name, course_id
from instructor, teaches
where instructor.ID = teaches.ID;

```

<img width="646" height="479" alt="image" src="https://github.com/user-attachments/assets/c3016164-3379-4b67-803c-06315d63193c" />



- **Refinement with Additional Selection:**
Find the names of all instructors in the Art department who have taught some course and the course_id of the course they taught:

```sql
select name, course_id
from instructor, teaches
where instructor.ID = teaches.ID and instructor.dept_name = 'Art';

```

---

## 2. The Rename Operation (as Clause)

The `as` clause is used to give a different name to an attribute or a relation.

- **Syntax:** `old-name as new-name`
- **Usage:** It can be used in the `select` clause or the `from` clause.
- **Note:** The `as` keyword is optional; `instructor T` is equivalent to `instructor as T`.

### 2.1 Self-Join Example

Renaming is essential for operations within a single table (self-comparison).

- **Query:** Find the names of all instructors whose salary is greater than at least one instructor in the Computer Science department.

```sql
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept_name = 'Comp. Sci.';

```


---

## 3. String Operations

Strings are used for names, IDs, and descriptions. SQL allows for exact and partial matching.

- **Operator:** `like`
- **Pattern Matching Characters:**

  1. **Percent (%):** Matches any substring (including null strings).
  2. **Underscore (_):** Matches any single character.

### 3.1 Examples

- `'Intro%'` matches any string beginning with "Intro".
- `'%Computer%'` matches any string containing "Computer" as a substring.
- `'___'` matches any string of exactly three characters.
- `'___%'` matches any string of at least three characters.

> [!IMPORTANT]
> Patterns are **case-sensitive** in SQL string matching, whereas general SQL keywords are not.
>
>

---

## 4. Ordering the Display of Tuples

The `order by` clause sorts the result of a query.

- **Default:** Ascending order (`asc`).
- **Descending:** Use `desc`.
- **Multi-Attribute Sorting:** Sorting can be performed on multiple attributes.

```sql
select distinct name
from instructor
order by name desc;

```

### 4.1 Limiting Results

To select a specific number of rows from a large list (e.g., top 10):

- **SQL Server / MS Access:** `select top 10 ...`
- **MySQL:** `... limit 10`
- **Oracle / SQL Standard:** `fetch first 10 rows only`

---

## 5. Where Clause Predicates

### 5.1 Range Comparisons (between)

Simplifies range checks:

```sql
select name
from instructor
where salary between 90000 and 100000;
-- Equivalent to: salary >= 90000 and salary <= 100000

```

### 5.2 Set Membership (in)

Simplifies multiple `OR` conditions:

```sql
select name
from instructor
where dept_name in ('Comp. Sci.', 'Biology');

```

### 5.3 Tuple Comparison

Allows comparing pairs of attributes:

```sql
select name, course_id
from instructor, teaches
where (instructor.ID, dept_name) = (teaches.ID, 'Biology');

```

---

## 6. Duplicates and Multisets

While Relational Algebra is based on set theory (no duplicates), SQL follows **multiset** (bag) semantics.

### 6.1 Multiset Operations

- **Selection:** If a tuple $t_1$ has $c_1$ copies and satisfies condition $\theta$, then all $c_1$ copies are included in the result.
- **Projection:** For each copy of tuple $t_1$ in relation $r_1$, there is a copy of the projection of $t_1$ in the result.
- **Cartesian Product:** If $t_1$ appears $c_1$ times in $r_1$ and $t_2$ appears $c_2$ times in $r_2$, the tuple $(t_1, t_2)$ appears $c_1 \times c_2$ times in $r_1 \times r_2$.


<img width="1303" height="226" alt="image" src="https://github.com/user-attachments/assets/f06fa1fe-f66d-4f20-b8c3-efae2788d09a" />


---







### 2.5: Introduction to SQL/3

#### **Outline**
*   **Set Operations: union, intersect, except**.
*   **Null Values**.
*   **Aggregate Functions: avg, min, max, sum, and count**.
*   **Group By**.
*   **Having**.

---

#### **1. Set Operations**

The SQL operations **union**, **intersect**, and **except** operate on relations and correspond to the mathematical set operations $\cup, \cap,$ and $-$.

**Examples based on the University Schema:**
*   **Find courses that ran in Fall 2009 or in Spring 2010 (Union):**
    ```sql
    (select course_id from section where sem = 'Fall' and year = 2009)
    union
    (select course_id from section where sem = 'Spring' and year = 2010);
    ```
*   **Find courses that ran in Fall 2009 and in Spring 2010 (Intersect):**
    ```sql
    (select course_id from section where sem = 'Fall' and year = 2009)
    intersect
    (select course_id from section where sem = 'Spring' and year = 2010);
    ```
*   **Find courses that ran in Fall 2009 but not in Spring 2010 (Except):**
    ```sql
    (select course_id from section where sem = 'Fall' and year = 2009)
    except
    (select course_id from section where sem = 'Spring' and year = 2010);
    ```

**Duplicates in Set Operations:**
*   Each of the above operations **automatically eliminates duplicates**.
*   To retain all duplicates, use the corresponding multiset versions: **union all**, **intersect all**, and **except all**.

**Multiset Operation Rules:**
Suppose a tuple occurs $m$ times in $r$ and $n$ times in $s$, then it occurs:
*   $m + n$ times in $r$ **union all** $s$.
*   $\min(m, n)$ times in $r$ **intersect all** $s$.
*   $\max(0, m - n)$ times in $r$ **except all** $s$.

---

#### **2. Null Values**

*   It is possible for tuples to have a **null value**, denoted by `null`, for some of their attributes.
*   `null` signifies an **unknown value** or that a **value does not exist**.
*   The result of any **arithmetic expression involving null is null** (e.g., $5 + null$ returns `null`).

**Testing for Nulls:**
*   The predicate **is null** can be used to check for null values.
    *   *Example:* `select name from instructor where salary is null;`
*   The predicate **is not null** succeeds if the value is not null.
*   It is **not possible** to test for null values with comparison operators such as $=, <, >$.

**Three-Valued Logic:**
Any comparison with `null` returns **unknown**. This creates three logical values: **true**, **false**, and **unknown**.

| Operation | Logical Result |
| :--- | :--- |
| **OR** | `(unknown or true) = true`, `(unknown or false) = unknown`, `(unknown or unknown) = unknown` |
| **AND** | `(true and unknown) = unknown`, `(false and unknown) = false`, `(unknown and unknown) = unknown` |
| **NOT** | `(not unknown) = unknown` |

*   **"P is unknown"** evaluates to true if predicate $P$ evaluates to unknown.
*   The result of a **where clause predicate is treated as false** if it evaluates to unknown.

---

#### **3. Aggregate Functions**

Aggregate functions take a collection (a set or multiset) of values as input and return a **single value**.

**Standard Built-in Functions:**
*   `avg`: average value.
*   `min`: minimum value.
*   `max`: maximum value.
*   `sum`: sum of values.
*   `count`: number of values.

**Examples:**
*   **Average salary in Computer Science:**
    ```sql
    select avg(salary)
    from instructor
    where dept_name = 'Comp. Sci';
    ```
*   **Total number of instructors teaching in Spring 2010:**
    ```sql
    select count(distinct ID)
    from teaches
    where semester = 'Spring' and year = 2010;
    ```
*   **Number of tuples in the course relation:**
    ```sql
    select count(*)
    from courses;
    ```

---

#### **4. Aggregation with Grouping and Having**

**The Group By Clause:**
The attribute or attributes given in the `group by` clause are used to **form groups**. Tuples with the same value on all attributes in the clause are placed in one group.

*   *Example:* Find average salary of instructors in each department:
    ```sql
    select dept_name, avg(salary) as avg_salary
    from instructor
    group by dept_name;
    ```

> [!CAUTION]
> **Rule:** Attributes in the `select` clause outside of aggregate functions **must appear in the group by list**.
>
> **Erroneous Query Example:**
> ```sql
> select dept_name, ID, avg(salary)
> from instructor
> group by dept_name;
> ```
> *Explanation:* Each instructor in a group can have a different ID; there is no unique way to choose which ID value to output.


<img width="845" height="467" alt="image" src="https://github.com/user-attachments/assets/f6b6786e-9c31-4275-a3ee-7f7460b69ebd" />




#### **5. The Having Clause**
It is useful to state a condition that **applies to groups** rather than to tuples. SQL applies predicates in the `having` clause **after groups have been formed**.

*   *Example:* Find average salary of instructors in departments where that average is > \$42,000:
    ```sql
    select dept_name, avg(salary) as avg_salary
    from instructor
    group by dept_name
    having avg(salary) > 42000;
    ```

---

#### **Summary**
*   **Familiarized with set operations, null values and aggregation**.
*   The meaning of a query containing aggregation is defined by the sequence: **from** $\rightarrow$ **where** $\rightarrow$ **group by** $\rightarrow$ **having**.




---




