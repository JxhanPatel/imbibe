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
