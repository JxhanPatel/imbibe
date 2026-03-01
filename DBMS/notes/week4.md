# **4.1: Formal Relational Query Languages/1**

---

#### **1. Recap**
*   **SQL Examples** have been practiced for basic query structures.
*   **Nested Subquery** in SQL.
*   **Data Modification**.
*   **SQL expressions for Join and Views**.
*   **Transactions**.
*   **Integrity Constraints**.
*   **More data types in SQL**.
*   **Authorization in SQL**.
*   **Functions and Procedures in SQL**.
*   **Triggers**.

---

#### **2. Objectives**
*   To understand **formal query language through relational algebra**.

---

#### **3. Formal Relational Query Language**

*   **Relational Algebra**: Procedural and Algebra based.
*   **Tuple Relational Calculus**: Non-Procedural and Predicate Calculus based.
*   **Domain Relational Calculus**: Non-Procedural and Predicate Calculus based.

---

#### **4. Relational Algebra**

*   **Created by Edgar F Codd at IBM in 1970**.
*   It is a **procedural language**.
*   It consists of **six basic operators**:
    *   **select**: $\sigma$
    *   **project**: $\Pi$
    *   **union**: $\cup$
    *   **set difference**: $-$
    *   **Cartesian product**: $\times$
    *   **rename**: $\rho$
*   The operators take **one or two relations as inputs** and produce a **new relation as a result**.

---

#### **5. Select Operation**

*   **Notation**: $\sigma_{p}(r)$.
*   $p$ is called the **selection predicate**.
*   **Formal Definition**: $\sigma_{p}(r) = \{t | t \in r \text{ and } p(t)\}$.
*   The predicate $p$ is a formula in **propositional calculus** consisting of terms connected by:
    *   $\wedge$ (and)
    *   $\vee$ (or)
    *   $\neg$ (not)
*   Each term is one of: $\text{<attribute> op <attribute>}$ or $\text{<attribute> op <constant>}$.
*   **Comparison operators (op)**: $=, \ne, >, \ge, <, \le$.

**Example 1:**
Selection predicate: $\sigma_{A=B \wedge D>5}(r)$.

<img width="285" height="603" alt="image" src="https://github.com/user-attachments/assets/0e7566f2-d0ed-4b6a-8b43-4d26366215c1" />

**Example 2:**
Select those tuples of the instructor relation where the instructor is in the "Physics" department.
$$\sigma_{dept\_name="Physics"}(instructor)$$.

---

#### **6. Project Operation**

*   **Notation**: $\Pi_{A_1, A_2, ..., A_k}(r)$ where $A_1, A_2, ...$ are attribute names and $r$ is a relation.
*   The result is defined as the relation of $k$ columns obtained by **erasing the columns that are not listed**.
*   **Duplicate rows are removed** from the result, since relations are sets.

**Example 1:**
To eliminate the `dept_name` attribute of the `instructor` relation:
$$\Pi_{ID, name, salary}(instructor)$$

<img width="385" height="517" alt="image" src="https://github.com/user-attachments/assets/fbecd74f-5ac0-4157-859b-45172083c652" />

 ---

**Example 2:**
$\Pi_{ID, name, salary/12}(instructor)$ to get the monthly salary of each instructor.

---

#### **7. Union Operation**

*   **Notation**: $r \cup s$.
*   **Formal Definition**: $r \cup s = \{t | t \in r \text{ or } t \in s\}$.
*   **Conditions for validity**:
    1.  $r$ and $s$ must have the **same arity** (same number of attributes).
    2.  The **attribute domains must be compatible** (e.g., 2nd column of $r$ deals with the same type of values as the 2nd column of $s$).

**Example:**
Find all courses taught in the Fall 2009 semester, or in the Spring 2010 semester, or in both:
$$\Pi_{course\_id}(\sigma_{semester="Fall" \wedge year=2009}(section)) \cup \Pi_{course\_id}(\sigma_{semester="Spring" \wedge year=2010}(section))$$.

<img width="346" height="491" alt="image" src="https://github.com/user-attachments/assets/b646348b-8651-40ed-a177-07fbc87bb281" />

---

#### **8. Set Difference Operation**

*   **Notation**: $r - s$.
*   **Formal Definition**: $r - s = \{t | t \in r \text{ and } t \notin s\}$.
*   **Conditions for validity**:
    1.  Relations must be **compatible**.
    2.  $r$ and $s$ must have the **same arity**.
    3.  **Attribute domains** of $r$ and $s$ must be compatible.

**Example:**
Find all courses taught in the Fall 2009 semester, but not in the Spring 2010 semester:
$$\Pi_{course\_id}(\sigma_{semester="Fall" \wedge year=2009}(section)) - \Pi_{course\_id}(\sigma_{semester="Spring" \wedge year=2010}(section))$$.

<img width="395" height="403" alt="image" src="https://github.com/user-attachments/assets/a5f05c9e-25d8-494b-8d6f-1feca1ca9a04" />

---

#### **9. Set Intersection Operation**

*   **Notation**: $r \cap s$.
*   **Formal Definition**: $r \cap s = \{t | t \in r \text{ and } t \in s\}$.
*   **Assumptions**:
    1.  $r$ and $s$ have the **same arity**.
    2.  Attributes are **compatible**.
*   **Derived Nature**: Set intersection is not independent; it can be expressed as:
    $$r \cap s = r - (r - s)$$.
<img width="380" height="337" alt="image" src="https://github.com/user-attachments/assets/9329be41-b97f-4238-b0e6-b4bb80859dd3" />

<img width="373" height="433" alt="image" src="https://github.com/user-attachments/assets/ffe50b31-733c-4261-bf93-141b8b5098a3" />

---

#### **10. Cartesian-Product Operation**

*   **Notation**: $r \times s$.
*   **Formal Definition**: $r \times s = \{t \cdot q | t \in r \text{ and } q \in s\}$.
*   **Assumption**: Attributes of $r(R)$ and $s(S)$ are **disjoint** (i.e., $R \cap S = \emptyset$).
*   If attributes are not disjoint, **renaming must be used**.

<img width="250" height="486" alt="image" src="https://github.com/user-attachments/assets/14cc1780-096d-48cd-abd0-ffffc57b7b0d" />

---

#### **11. Rename Operation**

*   Allows naming results of relational-algebra expressions and referring to a relation by **more than one name**.
*   **Expression 1**: $\rho_{x}(E)$ returns the result of expression $E$ under the name $x$.
*   **Expression 2**: For an expression $E$ of arity $n$:
    $$\rho_{x(A_1, A_2, ..., A_n)}(E)$$
    Returns the result of expression $E$ under name $x$, with attributes renamed to $A_1, A_2, ..., A_n$.

---

#### **12. Division Operation**

*   The division operation is applied to two relations.
*   Let $R(Z) \div S(X)$, where $X \subset Z$.
*   Let $Y = Z - X$ (hence $Z = X \cup Y$); $Y$ is the set of attributes of $R$ that are **not** attributes of $S$.
*   The result is a relation $T(Y)$ that includes tuple $t$ if tuples $t_R$ appear in $R$ with $t_R[Y] = t$ and $t_R[X] = t_s$ for **every tuple $t_s$ in $S$**.
*   **Derived Nature**: Division can be expressed as:
    $$r \div s \equiv \Pi_{R-S}(r) - \Pi_{R-S}((\Pi_{R-S}(r) \times s) - \Pi_{R-S, S}(r))$$.

**Division Example 1:**
Relations $R$ (Lecturer, Module) and $S$ (Subject: Prolog).
Result: (Green, Lewis).
<img width="1289" height="534" alt="image" src="https://github.com/user-attachments/assets/55188292-35ae-44f7-beee-34573131a1cb" />



**Division Example 2:**
Dividing $R$ (Lecturer, Module) by $S$ (Subject: Databases, Prolog).
Result: (Green).
<img width="1338" height="776" alt="image" src="https://github.com/user-attachments/assets/75275b09-91e5-4ba7-b721-a68b630207e9" />

**Division Example 3:**
<img width="1221" height="670" alt="image" src="https://github.com/user-attachments/assets/fd1c9d16-40a7-4146-98b5-566eadb43655" />


**Division Example 4:**
Finding customers who have an account in all branches of the bank.
$A$ is customer name; $B$ is branch name. If $S$ contains branches $\{1, 2\}$, the result includes customers associated with both branches in relation $r$.
<img width="1341" height="690" alt="image" src="https://github.com/user-attachments/assets/e0c9bee0-ab1f-4642-bb1e-197863bbbab1" />

---

#### **13. Derived Operation: Join**

*   The join operation (theta join) $\bowtie_{\theta}$ is defined as a Cartesian product followed by a selection:
    $$r \bowtie_{\theta} s = \sigma_{\theta}(r \times s)$$.
*   **Equi-join**: If $\theta$ is an equality condition.
*   **Natural Join**: If $\theta$ enforces values on identical columns to be equal and then removes the redundant columns.

---

#### **14. Summary**
*   **Relational Algebra** discussed with examples.
*   Six basic operators: **select, project, union, set difference, Cartesian product, and rename**.
*   Relational Algebra is **not Turing complete**.



---



# **4.2: Formal Relational Query Languages/2**

---

#### **1. Recap**
*   **Relational Algebras and its Operations**.

---

#### **2. Objectives**
*   To understand **formal calculus-based query language through relational algebra**.

---

#### **3. Outline**
*   **Tuple Relational Calculus (Overview only)**.
*   **Domain Relational Calculus (Overview only)**.
*   **Equivalence of Algebra and Calculus**.

---

#### **4. Formal Relational Query Language**

*   **Relational Algebra**: Procedural and Algebra based.
*   **Tuple Relational Calculus**: Non-Procedural and Predicate Calculus based.
*   **Domain Relational Calculus**: Non-Procedural and Predicate Calculus based.

---

#### **5. Predicate Logic**

*   **Predicate Logic or Predicate Calculus** is an extension of Propositional Logic or Boolean Algebra.
*   It adds the concept of **predicates and quantifiers** to better capture the meaning of statements that cannot be adequately expressed by propositional logic.
*   Tuple Relational Calculus and Domain Relational Calculus are based on **Predicate Calculus**.

**Predicate:**
*   Consider the statement, "x is greater than 3". It has two parts. The first part, the **variable x**, is the subject of the statement. The second part, **"is greater than 3"**, is the predicate. It refers to a property that the subject of the statement can have.
*   The statement "x is greater than 3" can be denoted by $P(x)$ where $P$ is the predicate "is greater than 3" and $x$ is the variable.

**Quantifiers:**
In predicate logic, predicates are used alongside quantifiers to express the extent to which a predicate is true over a range of elements. Using quantifiers to create such propositions is called **quantification**.

*   **Universal Quantifier ($\forall$):** Mathematical statements sometimes assert that a property is true for all the values of a variable in a particular domain, called the **domain of discourse**.
    *   The notation $\forall x P(x)$ denotes the universal quantification of $P(x)$.
    *   It is read as **"for all x P(x)"**.
    *   *Example:* Let $P(x)$ be the statement "$x + 2 > x$". The truth value of $\forall x P(x)$ is **True (T)** as it is true for any real number.
*   **Existential Quantifier ($\exists$):** Some mathematical statements assert that there is an element with a certain property.
    *   The notation $\exists x P(x)$ denotes the existential quantification of $P(x)$.
    *   It is read as **"there exists an x such that P(x)"**.
    *   *Example:* Let $P(x)$ be the statement "$x > 5$". $\exists x P(x) = T$ because it is true for all real numbers greater than 5.

---

#### **6. Tuple Relational Calculus (TRC)**

**Definition:**
*   TRC is a **non-procedural query language**, where each query is of the form:
    $$\{t | P(t)\}$$
    *   $t =$ resulting tuples.
    *   $P(t) =$ known as **predicate**; these are the conditions that are used to fetch $t$.
*   $P(t)$ may have various conditions logically combined with **OR ($\vee$)**, **AND ($\wedge$)**, **NOT ($\neg$)**.
*   It uses quantifiers:
    *   $\exists t \in r (Q(t))$: "there exists" a tuple $t$ in relation $r$ such that predicate $Q(t)$ is true.
    *   $\forall t \in r (Q(t))$: $Q(t)$ is true "for all" tuples in relation $r$.

**Predicate Calculus Formula Components:**
1.  **Set of attributes and constants**.
2.  **Set of comparison operators**: $<, \le, =, \ne, >, \ge)$.
3.  **Set of connectives**: and $(\wedge)$, or $(\vee)$, not $(\neg)$.
4.  **Implication ($\Rightarrow$)**: $x \Rightarrow y$; if $x$ is true, then $y$ is true ($x \Rightarrow y \equiv \neg x \vee y$).
5.  **Set of quantifiers**: $\exists, \forall$.

**TRC Examples:**

<img width="327" height="173" alt="image" src="https://github.com/user-attachments/assets/20fdd4cd-b9b0-4e9a-a4ba-1a3e3810eddf" />


*   **Example 1: Obtain the first name of students whose age is greater than 21.**
    *   $\{t.Fname | Student(t) \wedge t.age > 21\}$
    *   $\{t.Fname | t \in Student \wedge t.age > 21\}$
    *   $\{t | \exists s \in Student(s.age > 21 \wedge t.Fname = s.Fname)\}$
*   **Example 2: Find the names of all students who have taken the course name 'DBMS'.**
    *   Schema: `student(rollNo, name, year, courseId)`, `course(courseId, cname, teacher)`
    *   $\{s.name | s \in student \wedge \exists c \in course(s.courseId = c.courseId \wedge c.cname = 'DBMS')\}$
 
<img width="661" height="308" alt="image" src="https://github.com/user-attachments/assets/b69ac70d-0d0e-418b-b24a-92d52b5a1e07" />

*   **Example 3: Find the eids of pilots certified for Boeing aircraft.**
    *   Relations: `Aircraft(aid, aname, cruisingrange)`, `Certified(eid, aid)`
    *   $\{C.eid | C \in Certified \wedge \exists A \in Aircraft(A.aid = C.aid \wedge A.aname = 'Boeing')\}$
*   **Example 4: Identify the flights that can be piloted by every pilot whose salary is more than \$100,000.**
    *   $\{F.flno | F \in Flights \wedge \forall C \in Certified \exists E \in Employees(A.cruisingrange > F.distance \wedge A.aid = C.aid \wedge E.salary > 100,000 \wedge E.eid = C.eid)\}$

**Safety of Expressions:**
*   It is possible to write tuple calculus expressions that generate **infinite relations**.
*   *Example:* $\{t | \neg t \in r\}$ results in an infinite relation if the domain of any attribute is infinite.
*   To guard against this, we restrict to **safe expressions**.
*   **Definition:** An expression $\{t | P(t)\}$ in the TRC is **safe** if every component of $t$ appears in one of the relations, tuples, or constants that appear in $P$.

---

#### **7. Domain Relational Calculus (DRC)**

*   A **non-procedural query language** equivalent in power to the tuple relational calculus.
*   In TRC, you have a variable for the entire tuple; in DRC, every attribute is a different variable.
*   Each query is an expression of the form:
    $$\{<x_1, x_2, ..., x_n> | P(x_1, x_2, ..., x_n)\}$$
    *   $x_1, x_2, ..., x_n$ represent **domain variables**.
    *   $P$ represents a formula similar to that of the predicate calculus.

---

#### **8. Equivalence of RA, TRC, and DRC**

The three models are equivalent in power. For every relational algebra operator, there is an equivalent calculus expression, and vice versa.

| Operation | Relational Algebra | Tuple Relational Calculus | Domain Relational Calculus |
| :--- | :--- | :--- | :--- |
| **Select** | $\sigma_{B=17}(r)$ | ${t∣t∈R∧B=17}$ | ${⟨a,b⟩∣⟨a,b⟩∈r∧b=17}$ | $\{<a,b> | <a,b> \in r \wedge b=17\}$ |
| **Project** | $\Pi_{A}(r)$ | $\{t \| \exists p \in r(t[A]=p[A])\}$ | $\{<a> \| \exists b(<a,b> \in r)\}$ |
| **Union** | $r \cup s$ | $\{t \| t \in r \vee t \in s\}$ | $\{<a,b,c> \| <a,b,c> \in r \vee <a,b,c> \in s\}$ |
| **Set Difference** | $r - s$ | $\{t \| t \in r \wedge t \notin s\}$ | $\{<a,b,c> \| <a,b,c> \in r \wedge <a,b,c> \notin s\}$ |
| **Intersection** | $r \cap s$ | $\{t \| t \in r \wedge t \in s\}$ | $\{<a,b,c> \| <a,b,c> \in r \wedge <a,b,c> \in s\}$ |
| **Cartesian Product** | $r \times s$ | $\{t \| \exists p \in r \exists q \in s (t[A]=p[A] \wedge t[B]=p[B] \wedge t[C]=q[C] \wedge t[D]=q[D])\}$ | $\{<a,b,c,d> \| <a,b> \in r \wedge <c,d> \in s\}$ |
| **Natural Join** | $r \bowtie s$ | $\{t \| \exists p \in r \exists q \in s (... \text{matching conditions} ...)\}$ | $\{<a,b,c,d,e> \| <a,b,c,d> \in r \wedge <b,d,e> \in s\}$ |
| **Division** | $r \div s$ | $\{t \| \exists p \in r \forall q \in s (p[B]=q[B] \Rightarrow t[A]=p[A])\}$ | $\{<a> \ \forall <b> (<b> \in s \Rightarrow <a>, <b> \in r)\}$ |

(Note: Schemas for equivalence examples: $R=(A,B)$ for Select/Project/Cartesian; $R,S=(A,B,C)$ for set operations; $R=(A,B,C,D)$ and $S=(B,D,E)$ for Natural Join; $R=(A,B)$ and $S=(B)$ for Division.)

---

#### **9. Summary**
*   **Introduced tuple relational and domain relational calculus**.
*   **Illustrated equivalence of algebra and calculus**.






---




# **4.3: Entity-Relationship Model/1**

---

#### **1. Recap**
*   **Predicate Calculus**.
*   **Tuple Relational and Domain Relational Calculus**.
*   **Equivalence of Relational Algebra and Relational Calculus**.

---

#### **2. Objectives**
*   To understand the **Design Process for Database Systems**.
*   To study the **E-R Model for real world representation**.

---

#### **3. Outline**
*   **Design Process**.
*   **E-R Model**:
    *   **Entity and Entity Set**.
    *   **Relationship** (including **Cardinality**).
    *   **Attributes**.
    *   **Weak Entity Sets**.

---

#### **4. Design Process**

**What is Design?**
A Design:
*   Satisfies a given (perhaps informal) **functional specification**.
*   Conforms to **limitations of the target medium**.
*   Meets implicit or explicit requirements on **performance and resource usage**.
*   Satisfies implicit or explicit design criteria on the form of the artifact.
*   Satisfies restrictions on the design process itself, such as its length or cost, or the tools available for doing the design.

**Role of Abstraction**
*   **Disorganized Complexity** results from Storage (STM) limitations of the human brain; an individual can simultaneously comprehend of the order of **seven, plus or minus two chunks of information**.
*   **Abstraction** allows a person to use a complex device or system without having to know the details of how that device or system is constructed.
*   **Example of Abstraction Levels**:
    *   $1100011010111001_2$ (Raw Data).
    *   $6251_8$ (Octal Abstraction).
    *   $CA9_{16}$ (Hexadecimal Abstraction).

**Model Building**
*   Models are common in all engineering disciplines.
*   Model building follows principles of **decomposition, abstraction, and hierarchy**.
*   Each model describes a specific aspect of the system.
*   New models are built upon old proven models.
*   **Examples in other fields**:
    *   **Physics**: Time-Distance Equation, Quantum Mechanics.
    *   **Geography**: Maps, Projections.
    *   **Building & Bridges**: Drawings - Plan, Elevation, Side view.

**Design Approach**
1.  **Requirement Analysis**: Analyze the data needs of the prospective database users.
    *   Planning.
    *   System Definition.
2.  **Database Designing**: Use a modeling framework to create abstraction of the real world.
    *   **Logical Model**: Deciding on a good database schema.
        *   *Business Decision*: What attributes should we record in the database?.
        *   *Computer Science Decision*: What relation schema should we have and how should the attributes be distributed among the various relation schema?.
    *   **Physical Model**: Deciding on the physical layout of the database.
3.  **Implementation**:
    *   Data Conversion and Loading.
    *   Testing.




<img width="1424" height="636" alt="image" src="https://github.com/user-attachments/assets/8c9fc408-ce7c-4eb9-ad5c-193e54b733fe" />

---

#### **5. Entity Relationship (ER) Model**

**Database Modeling Concepts**
*   The ER data model was developed to facilitate database design by allowing specification of an **enterprise schema** that represents the overall logical structure of a database.
*   It is useful in mapping the meanings and interactions of real-world enterprises onto a conceptual schema.
*   **Three Basic Concepts**: Attributes, Entity sets, and Relationship sets.
*   **ER Diagram**: A diagrammatic representation that can express the overall logical structure of a database graphically.

**Attributes**
*   An **Attribute** is a property associated with an entity / entity set.
*   Based on values of certain attributes, an entity can be identified uniquely.
*   **Attribute Types**:
    *   **Simple and Composite**: Simple attributes are not divided into subparts. Composite attributes can be divided into subparts (e.g., `name` divided into `first_name`, `middle_initial`, `last_name`).
    *   **Single-valued and Multivalued**: Single-valued attributes have one value for a particular entity. Multivalued attributes may have a set of values for a specific entity (e.g., `phone_numbers`, `dependent_name`).
    *   **Derived**: The value can be computed from other attributes (e.g., `age` derived from `date_of_birth` and current date).
*   **Domain**: The set of permitted values for each attribute.

<img width="654" height="237" alt="image" src="https://github.com/user-attachments/assets/d7bccfe2-6063-489c-aa12-89f72e2fda6d" />

**Entity Sets**
*   An **Entity** is an object that exists and is distinguishable from other objects (e.g., a specific person, company, event).
*   An **Entity Set** is a set of entities of the same type that share the same properties (e.g., the set of all persons, companies).
*   An entity is represented by a set of attributes.
    *   *Example*: $instructor = (ID, name, street, city, salary)$.
*   A subset of attributes forms a **primary key** of the entity set to uniquely identify each member. Primary keys are represented by **underlining** them.

**Relationship Sets**
*   A **Relationship** is an association among several entities.
*   A **Relationship Set** is a mathematical relation among $n \ge 2$ entities, each taken from entity sets.
    *   $\{(e_1, e_2, ..., e_n) \mid e_1 \in E_1, e_2 \in E_2, ..., e_n \in E_n\}$.
 <img width="531" height="258" alt="image" src="https://github.com/user-attachments/assets/8e83213c-44c8-438a-97f4-d017d0915d1d" />

*   **Degree of Relationship Set**: The number of entity sets that participate in a relationship set.
    *   **Binary Relationship**: Involves two entity sets (degree two); most common in database systems.
    *   **Ternary Relationship**: Involves three entity sets (e.g., `proj_guide` between `instructor`, `student`, and `project`).
*   Attributes can be associated with a relationship set (e.g., `date` attribute for the `advisor` relationship tracking when the association started).

**Redundant Attributes**
*   Attributes may be redundant if the information they replicate is present in a relationship.
*   *Example*: If `instructor` has `dept_name` and there is an `inst_dept` relationship with the `department` entity set, the `dept_name` attribute is redundant in the `instructor` entity set and needs to be removed.

---

#### **6. Mapping Cardinality Constraints**

*   Express the number of entities to which another entity can be associated via a relationship set.
*   **Types for Binary Relationship Sets**:
    1.  **One to one**: An entity in A is associated with at most one entity in B, and an entity in B is associated with at most one entity in A.
    2.  **One to many**: An entity in A is associated with any number (zero or more) of entities in B. An entity in B can be associated with at most one entity in A.
    3.  **Many to one**: An entity in A is associated with at most one entity in B. An entity in B can be associated with any number (zero or more) of entities in A.
    4.  **Many to many**: An entity in A is associated with any number of entities in B, and an entity in B is associated with any number of entities in A.

<img width="535" height="327" alt="image" src="https://github.com/user-attachments/assets/b1ea943c-6408-4f84-90ff-124a4190580c" />
<img width="540" height="319" alt="image" src="https://github.com/user-attachments/assets/4bddc6e7-d934-4bea-acfc-b8186a8f5c25" />

---

#### **7. Weak Entity Sets**

**Definition**
*   **Strong Entity Set**: An entity set that contains sufficient attributes to uniquely identify all its entities; a primary key exists.
*   **Weak Entity Set**: An entity set that does not contain sufficient attributes to uniquely identify its entities.
*   **Discriminator**: A partial key in a weak entity set that can identify a group of entities within that set. It is represented by underlining with a **dashed line**.

**Existence Dependency**
*   A weak entity set is **existence dependent** on another entity set, called its **identifying entity set** (or owner).
*   **Identifying Relationship**: The relationship associating the weak entity set with the identifying entity set. It is depicted by a **double diamond**.
*   The weak entity set must have **total participation** in the identifying relationship.

**Primary Key of Weak Entity Set**
*   Formed by the combination of the **discriminator** of the weak entity set and the **primary key of the identifying strong entity set**.
*   **Formula**: $\text{Primary Key of Weak Entity Set} = \text{Discriminator} + \text{Primary Key of Strong Entity Set}$.

**Example**
*   **Strong Entity Set**: `Building(building_no, building_name, address)`.
*   **Weak Entity Set**: `Apartment(door_no, floor)`. `door_no` is the discriminator.
*   **Relationship**: `BA` between `Building` and `Apartment`.
*   **Primary Key of Apartment**: `building_no` + `door_no`.

---

#### **8. Summary**
*   Introduced the **Design Process for Database Systems**.
*   Elucidated the **E-R Model** for real-world representation with entities, entity sets, attributes, and relationships.





---




# **4.4: Entity-Relationship Model/2**

---

#### **1. Recap**
*   **Design Process for Database Systems**.
*   **ER Model for real-world representation** with entities, entity sets, attributes, and relationships.

---

#### **2. Objectives**
*   **To illustrate ER Diagram notation** for ER Models.
*   **To explore translation of ER Models to Relational Schemas**.

---

#### **3. ER Diagram: Graphical Representations**

**Entity Sets:**
*   **Rectangles** represent entity sets.
*   **Attributes** are listed inside the entity rectangle.
*   **Underline** indicates primary key attributes.

<img width="378" height="163" alt="image" src="https://github.com/user-attachments/assets/831b23d7-5eb4-49ea-b000-c617699f0a94" />

**Relationship Sets:**
*   **Diamonds** represent relationship sets.
*   The diamond is linked via lines to a number of different entity sets (rectangles).
*   Unless otherwise specified, a relationship represents an association between the keys of the connected entity sets.

**Relationship Sets with Attributes:**
*   An attribute of a relationship set is represented by an **undivided rectangle**.
*   It is linked with a **dashed line** to the diamond representing that relationship set.
*   *Example:* The `advisor` relationship may have an attribute `date` tracking when the association started.

<img width="606" height="208" alt="image" src="https://github.com/user-attachments/assets/9bfbaff5-1806-4e33-a335-9a2c1c20e96d" />

**Roles:**
*   Entity sets of a relationship need not be distinct; each occurrence of an entity set plays a **"role"** in the relationship.
*   In a **recursive relationship set**, explicit role names are necessary to specify how an entity participates.
*   *Example:* In the `prereq` relationship between `course` and `course`, labels `course_id` and `prereq_id` are role indicators.

<img width="517" height="169" alt="image" src="https://github.com/user-attachments/assets/697441fe-1c2c-4176-a360-58e414a18cb3" />

---

#### **4. Cardinality Constraints**

Cardinality is expressed by drawing either a **directed line** ($\rightarrow$) signifying "one," or an **undirected line** (—) signifying "many," between the relationship set and the entity set.
<img width="500" height="157" alt="image" src="https://github.com/user-attachments/assets/c86e5233-42bd-4706-8d79-32bca0cfa1a4" />
<img width="568" height="163" alt="image" src="https://github.com/user-attachments/assets/acb5179b-b0b8-4ce5-9cb8-fd8364fbedf5" />

**Binary Relationship Types:**
1.  **One-to-one:** A directed line points from the relationship set to both entity sets. An entity in A is associated with at most one entity in B, and vice-versa.
2.  **One-to-many:** A directed line points to the "one" side. An instructor is associated with several students, but a student is associated with at most one instructor.
3.  **Many-to-one:** A directed line points from the relationship set to the "one" side (e.g., student to instructor). An instructor is associated with at most one student, while a student can have many instructors.
4.  **Many-to-many:** Undirected lines point to both entity sets. An instructor is associated with several students, and a student is associated with several instructors.

---

#### **5. Participation Constraints**

*   **Total Participation:** Indicated by a **double line**. Every entity in the entity set must participate in at least one relationship in the relationship set.
*   **Partial Participation:** Some entities may not participate in any relationship in the set.
*   *Example:* Participation of `student` in `advisor` is total (every student must have an instructor), but participation of `instructor` in `advisor` is partial.

<img width="494" height="126" alt="image" src="https://github.com/user-attachments/assets/bc9b66c5-06eb-4b82-a815-4ecdcdfcfd29" />

---

#### **6. Complex Constraints: Cardinality Limits**

*   A line may have an associated **minimum and maximum cardinality** shown as $l..h$.
*   **Minimum value ($l$):** A value of 1 indicates total participation.
*   **Maximum value ($h$):** A value of 1 indicates the entity participates in at most one relationship; a value of $*$ indicates no limit.
*   *Example:* `1..1` on the `student` side of `advisor` means each student must have exactly one advisor; `0..*` on the `instructor` side means an instructor can have zero or more students.

<img width="549" height="128" alt="image" src="https://github.com/user-attachments/assets/0d86dc27-0a77-4849-a8b7-11c7ff9be49c" />

---

#### **7. Notation for Complex Attributes**

*   **Composite Attributes:** Represented by the main attribute name with component attributes listed below it (often indented or branching).
*   **Multivalued Attributes:** Indicated by **curly braces** around the attribute name, e.g., `{phone_number}`.
*   **Derived Attributes:** Indicated by a **pair of parenthesis** following the attribute name, e.g., `age()`.

<img width="216" height="417" alt="image" src="https://github.com/user-attachments/assets/75d83825-b514-4cb0-9462-340cd302d85a" />

---

#### **8. Weak Entity Sets**

*   **Representation:** Depicted via a **double rectangle**.
*   **Discriminator:** The partial key is underlined with a **dashed line**.
*   **Identifying Relationship:** The link to the strong entity set is depicted by a **double diamond**.
*   **Primary Key:** Formed by the discriminator of the weak entity set plus the primary key of the identifying strong entity set.
*   *Example:* `section` is a weak entity set dependent on `course`. Its primary key is `(course_id, sec_id, semester, year)`.

<img width="502" height="118" alt="image" src="https://github.com/user-attachments/assets/4509969d-7b2d-46df-953e-3547caed2403" />

---

#### **9. Full University Enterprise ER Diagram**

The complete ER diagram includes entity sets like `classroom`, `department`, `course`, `instructor`, `section`, `student`, and `time_slot`, along with their complex relationship interconnections.

<img width="956" height="874" alt="image" src="https://github.com/user-attachments/assets/4db18f2b-5457-4ebe-ac74-4194a4a7aceb" />

---

#### **10. Reduction to Relational Schema**

Entity sets and relationship sets can be expressed uniformly as **relation schemas**.

**Representing Entity Sets:**
*   **Strong Entity Set:** Reduces to a schema with the same attributes.
    *   *Example:* `student (ID, name, tot_cred)`.
*   **Weak Entity Set:** Becomes a table that includes a column for the primary key of the identifying strong entity set.
    *   *Example:* `section (course_id, sec_id, semester, year)`.

**Representing Relationship Sets:**
*   A relationship set $R$ is represented by a schema with attributes formed by the union of the **primary keys** of each participating entity set, plus any **descriptive attributes** of $R$.
*   **Many-to-Many:** The primary key for the schema consists of the union of the primary keys of the participating entity sets.
    *   *Example:* `advisor (s_ID, i_ID)`.

**Handling Complex Attributes:**
*   **Composite Attributes:** Flattened out by creating a separate attribute for each component attribute.
    *   *Example:* `name` becomes `name_first_name` and `name_last_name`. Prefixes are omitted if there is no ambiguity.
*   **Multivalued Attributes:** A multivalued attribute $M$ of entity $E$ is represented by a **separate schema** $EM$.
    *   Schema $EM$ contains attributes for the primary key of $E$ and an attribute for $M$.
    *   *Example:* `{phone_number}` of `instructor` becomes `inst_phone (ID, phone_number)`.
*   **Derived Attributes:** Not explicitly represented in the relational model; they may be represented as stored procedures or functions.

---

#### **11. Schema Redundancy and Optimization**

**Combining Schemas:**
*   **Many-to-One/One-to-Many:** If participation is **total** on the "many" side, the relationship schema can be combined with the "many" side entity schema by adding the primary key of the "one" side as an extra attribute.
    *   *Example:* Instead of a separate `inst_dept` table, add `dept_name` to the `instructor` table.
*   **One-to-One:** The relationship schema can be merged into either of the two entity tables.
*   **Partial Participation Warning:** If participation is partial on the "many" side, merging could result in null values.

**Redundancy of Weak Entity Relationships:**
*   The schema for a relationship set linking a weak entity set to its strong identifying entity set is **redundant**.
*   The attributes of the relationship are already present in the schema generated for the weak entity set.

---

#### **12. Summary**
*   **Illustrated ER Diagram notation** for entities, relationships, cardinalities, and complex attributes.
*   **Discussed translation of ER Models to Relational Schemas**, including rules for handling composite/multivalued attributes and schema optimization.



----



# Practise Question
<img width="581" height="459" alt="image" src="https://github.com/user-attachments/assets/7c47da68-f4d8-4d88-aca8-a3fe9e91eef3" />
<img width="583" height="454" alt="image" src="https://github.com/user-attachments/assets/c7c1b9ea-1268-47a4-bd91-25807067d11c" />
<img width="613" height="391" alt="image" src="https://github.com/user-attachments/assets/e2e90079-60af-43a7-83ef-007683f17c43" />


---
