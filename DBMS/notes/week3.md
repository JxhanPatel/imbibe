# 3.1 SQL Examples 

### Overview

This module provides a quick recap and roundup of major SQL features through practical examples. It is essential to practice coding in SQL because, as a declarative language, its style of programming is significantly different from procedural languages. Internalizing these mechanisms is necessary to effectively extract queries and manipulate database structures.

---

### 1. Basic Select Clause

The simplest form of a query involves selecting information from a single relation based on a specific predicate.

**Example:** From the `classroom` relation, find the names of buildings in which individual classrooms have a capacity less than 100.

```sql
select distinct building
from classroom
where capacity < 100;

```

- **From Clause:** Specifies the relation, which is `classroom`.
- **Where Clause:** Sets the predicate `capacity < 100`. In the classroom schema, room number is the key, as multiple classrooms can exist in one building and share capacities.
- **Select Clause:** Specifies the attribute `building`.
- **Distinct Qualifier:** By default, SQL `select-from-where` returns a multiset (duplicates are not removed). The `distinct` keyword is used here to remove duplicate building names.

<img width="511" height="276" alt="image" src="https://github.com/user-attachments/assets/187be6fc-1eaa-4495-9c06-6a91028af6bf" />


If the `distinct` qualifier is omitted, or if the `all` keyword is used, the duplicates remain in the output. The `all` keyword is optional as multiset behavior is the default.

---

### 2. Cartesian Product and Renaming

When a query involves two or more relations, SQL performs a Cartesian product (all combinations of tuples) which is then filtered by a qualifier.

**Example:** Find the names and budgets of students where the student's department matches the department's name and the budget is less than $100,000.

```sql
select name, budget
from student, department
where student.dept_name = department.dept_name
      and budget < 100000;

```

#### The Renaming Feature (as)

Any attribute or relation can be renamed using the `as` clause. This is optional but useful for clarity and essential for self-joins.

```sql
select s.name as student_name, budget as dept_budget
from student as s, department as d
where s.dept_name = d.dept_name;

```

- **Aliasing:** `student` is renamed to `s` and `department` to `d`.
- **Ambiguity:** If an attribute like `dept_name` occurs in both relations, it must be distinguished (e.g., `s.dept_name`). Attributes unique to one table (like `budget`) do not strictly require the relation prefix.

<img width="399" height="620" alt="image" src="https://github.com/user-attachments/assets/baf7fdfe-55cd-4ac5-af78-eda8cce8aa5f" />


---

### 3. Predicates and Boolean Algebra

The `where` clause can utilize `and`, `or`, and `not` operators.

**Example:** Find names of all instructors whose department is 'Finance' or whose department building is 'Watson' or 'Taylor'.

```sql
select i.name
from instructor as i, department as d
where d.dept_name = i.dept_name
      and (i.dept_name = 'Finance' or d.building in ('Watson', 'Taylor'));

```

- **Join Condition:** `d.dept_name = i.dept_name` ensures the instructor and department are properly correlated.
- **In Clause:** `building in ('Watson', 'Taylor')` is used to simplify the predicate instead of using multiple `or` statements.
- **Parentheses:** Correct use of parentheses is required to ensure the join condition and the selection criteria are evaluated with the correct semantics.

---

### 4. String Operations

SQL uses the `like` operator for string matching with two special characters:

- **Percent (%):** Matches any substring.
- **Underscore (_):** Matches any character.

**Example:** Find titles of all courses whose course ID has three alphabets followed by a hyphen and any other characters.

```sql
select title
from course
where course_id like '___-%';

```

<img width="515" height="347" alt="image" src="https://github.com/user-attachments/assets/9448f23b-81bf-484a-83fd-50aa108d0b3b" />


This pattern ensures that IDs with only two characters before the hyphen (like 'CS-101') are excluded.

---

### 5. Ordering the Output

The `order by` clause is used to sort the result. By default, it sorts in ascending order (`asc`), but descending (`desc`) can be specified.

**Example:** Sort by department name (ascending) and then by total credits (descending).

```sql
select *
from instructor
order by dept_name asc, salary desc;

```

<img width="614" height="373" alt="image" src="https://github.com/user-attachments/assets/396c9faf-4c3a-4c3b-bd80-b9dae72f98f0" />


When multiple attributes are provided, the system sorts by the first; for equal values of the first attribute, it then sorts by the second.

---

### 6. Set Operations

SQL provides three primary set operations: `union`, `intersect`, and `except`. Unlike the `select` clause, these are pure set operations and remove duplicates automatically unless the `all` qualifier is added (e.g., `union all`).


- **Union:** Combines the results of two queries.
<img width="445" height="378" alt="image" src="https://github.com/user-attachments/assets/e01118c1-7e0d-4d69-86bb-40ee48220ed9" />

- **Intersect:** Returns only the tuples common to both queries.
<img width="666" height="291" alt="image" src="https://github.com/user-attachments/assets/95be0969-3f0d-48cd-a931-b4f0deaae0ae" />


- **Except:** Returns tuples present in the first result but not the second.
<img width="672" height="289" alt="image" src="https://github.com/user-attachments/assets/e6ac7611-49a5-41d5-a9e8-78979c5723fe" />

---

### 7. Aggregate Functions

Aggregation is performed on a collection of values using functions like `avg`, `min`, `max`, `sum`, and `count`.

**Example:** Find the names and average capacity of each building where the average is greater than 25.

```sql
select building, avg(capacity)
from classroom
group by building
having avg(capacity) > 25;

```

- **Group By:** Used to group the tuples by a specific entity (e.g., `building`) so the function can be applied to each group.
- **Having Clause:** Used to provide a predicate on the aggregate results. While `where` filters individual tuples, `having` filters the groups formed by the `group by` clause.

#### Count and Sum Examples

- **Count:** `select count(course_id) from section where building = 'Taylor';` counts the number of rows/instances.
<img width="1256" height="506" alt="image" src="https://github.com/user-attachments/assets/d0d8d662-eea2-4a53-9d3b-2b2059968d35" />



- **Sum:** `select dept_name, sum(credits) from course group by dept_name;` calculates the total credits offered by each department.

<img width="1216" height="518" alt="image" src="https://github.com/user-attachments/assets/fb336c4c-78b7-4de0-b4a7-c3b02cf0b627" />


**Summary:** These foundational SQL features form the basis for data manipulation. Practice is highly encouraged to gain confidence before moving toward advanced features like nested subqueries, joins, and triggers.


# Practise Questions

<img width="1020" height="328" alt="image" src="https://github.com/user-attachments/assets/3283324b-02bc-405d-8076-4a4b444d7983" />
<img width="310" height="250" alt="image" src="https://github.com/user-attachments/assets/1a0b7e93-c274-4639-ab6c-1d6854ad8254" />
<img width="532" height="269" alt="image" src="https://github.com/user-attachments/assets/9e468d52-e7e6-400a-b683-f1d0c34214b8" />
<img width="1372" height="398" alt="image" src="https://github.com/user-attachments/assets/caa561d4-b45e-4ff1-9cb6-e120f03fb747" />
<img width="491" height="288" alt="image" src="https://github.com/user-attachments/assets/17005790-5d49-40d5-8b44-83b9fabc11f7" />



---


# 3.2: Intermediate SQL/1

---

## 1. Introduction to Intermediate SQL

In the previous modules, we discussed basic query structures, including `select`, `from`, and `where` clauses. In this module, we transition to **Intermediate SQL**, which extends these capabilities to include nested subqueries, data modification, and more complex predicates.

Intermediate SQL allows us to:

- Understand nested subqueries.
- Perform data manipulation (Insert, Delete, Update).
- Use advanced predicates like `in`, `exists`, and `unique`.

---

## 2. Nested Subqueries

A subquery is a `select-from-where` expression that is nested within another query. A common use for subqueries is to perform tests for:

- **Set Membership** (`in`, `not in`)
- **Set Comparison** (`some`, `all`)
- **Set Cardinality** (`exists`, `not exists`, `unique`)

### 2.1 Set Membership

The `in` connective tests for set membership, where the set is a collection of values produced by a `select` clause. The `not in` connective tests for the absence of membership.

**Example 1: Find courses offered in Fall 2017 and in Spring 2018**

```sql
select distinct course_id
from section
where semester = 'Fall' and year = 2017 and
      course_id in (select course_id
                    from section
                    where semester = 'Spring' and year = 2018);

```


**Example 2: Find courses offered in Fall 2017 but not in Spring 2018**

```sql
select distinct course_id
from section
where semester = 'Fall' and year = 2017 and
      course_id not in (select course_id
                        from section
                        where semester = 'Spring' and year = 2018);

```

---

### 2.2 Set Comparison

Set comparisons allow comparing a single value to a set of values using `some` or `all`.

#### 2.2.1 The some Clause

The `some` clause returns true if the condition holds for **at least one** member in the set.

**Example: Find names of instructors with salary greater than that of some (at least one) instructor in the Biology department**

```sql
select name
from instructor
where salary > some (select salary
                     from instructor
                     where dept_name = 'Biology');

```
<img width="916" height="688" alt="image" src="https://github.com/user-attachments/assets/1a44775a-c788-4fb6-bb26-4092676c633d" />

*Note: > some is equivalent to "greater than the minimum". Similarly, < some is "less than the maximum", and = some is equivalent to in.*

#### 2.2.2 The all Clause

The `all` clause returns true if the condition holds for **all** members in the set.

**Example: Find the names of all instructors whose salary is greater than the salary of all instructors in the Biology department**

```sql
select name
from instructor
where salary > all (select salary
                    from instructor
                    where dept_name = 'Biology');

```
<img width="880" height="699" alt="image" src="https://github.com/user-attachments/assets/6cf624c3-f344-49df-a919-9441b44ac216" />

*Note: > all is equivalent to "greater than the maximum". Similarly, < all is "less than the minimum", and <> all is equivalent to not in.*

---

### 2.3 Test for Empty Relations

The `exists` construct returns the value **true** if the argument subquery is nonempty.

#### 2.3.1 The exists Construct

**Example: Find all courses taught in both the Fall 2017 semester and in the Spring 2018 semester**

```sql
select course_id
from section as S
where semester = 'Fall' and year = 2017 and
      exists (select *
              from section as T
              where semester = 'Spring' and year = 2018 and
                    S.course_id = T.course_id);

```

- **Correlated Subquery:** The subquery uses the correlation variable `S` from the outer query. This is called a **correlated subquery** 

#### 2.3.2 The not exists Construct

`not exists` tests whether a subquery is empty. This is frequently used to implement the **Set Difference** or **Division** operation.

---

### 2.4 Test for Absence of Duplicate Tuples

The `unique` construct tests whether a subquery has any duplicate tuples in its result. It returns **true** if the subquery contains no duplicates.
<img width="1333" height="444" alt="image" src="https://github.com/user-attachments/assets/a84be3d1-70f0-48a9-8c7f-0221275c0c00" />

---

## 3. Subqueries in the from Clause

SQL allows subqueries to be used in the `from` clause. The result of the subquery is treated as a temporary relation.

**Example: Find the average instructors' salaries of those departments where the average salary is greater than $42,000**

```sql
select dept_name, avg_salary
from (select dept_name, avg(salary) as avg_salary
      from instructor
      group by dept_name)
where avg_salary > 42000;

```

---

## 4. Subqueries in the select Clause

SQL also allows **scalar subqueries**—subqueries that return a single value—to appear in the `select` clause.

**Example: List all departments and the number of instructors in each department**

```sql
select dept_name, 
       (select count(*)
        from instructor
        where department.dept_name = instructor.dept_name)
       as num_instructors
from department;

```

---

## 5. Modification of the Database

Intermediate SQL provides three primary ways to modify data: `delete`, `insert`, and `update`.

### 5.1 Deletion

The `delete` request is expressed in much the same way as a query.

**Example 1: Delete all tuples in the instructor relation**

```sql
delete from instructor;

```

**Example 2: Delete all instructors from the Finance department**

```sql
delete from instructor
where dept_name = 'Finance';

```
*   **Example:** Delete all instructors whose salary is less than the average salary of instructors.
    ```sql
    delete from instructor
    where salary < (select avg (salary)
                    from instructor);
    ```
---

### 5.2 Insertion

To insert data into a relation, we either specify a tuple to be inserted or write a query whose result is a set of tuples to be inserted.
*   **Standard Insertion:** 
    ```sql
    insert into course
    values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
    ```
*   **Explicit Attribute List:**
    ```sql
    insert into course (course_id, title, dept_name, credits)
    values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
    ```
*   **Insertion of Query Results:** Add all instructors to the student relation with `tot_creds` set to 0.
    ```sql
    insert into student
    select ID, name, dept_name, 0
    from instructor;
    ```
    > [!CAUTION]
    > **Infinite Insertion Prevention:** The `select from where` statement is evaluated fully before any results are inserted.


---

### 5.3 Updates

The `update` statement allows us to change values in some tuples without changing all values in the tuple.

**Example 1: Give a 5% raise to all instructors**

```sql
update instructor
set salary = salary * 1.05;

```

**Example 2: Conditional Update**
Give a 5% raise to instructors whose salary is less than 70,000:

```sql
update instructor
set salary = salary * 1.05
where salary < 70000;

```

**Example 3: Case Statement in Update**
Increase salaries of instructors whose salary is over 100,000 by 3%, and all others by 5%:

```sql
update instructor
set salary = case
               when salary <= 100000 then salary * 1.05
               else salary * 1.03
             end;

```
*   **Updates with Scalar Subqueries:** Recompute and update `tot_creds` value for all students.
    ```sql
    update student S
    set tot_creds = (select sum(credits)
                     from takes, course
                     where takes.course_id = course.course_id and
                           S.ID = takes.ID and
                           takes.grade <> 'F' and
                           takes.grade is not null);
    ```
---

## Summary

- **Nested subqueries** provide a powerful way to structure complex queries.
- **Set membership (in)**, **set comparison (some, all)**, and **cardinality (exists, unique)** are core components of intermediate SQL.
- **Data modification** is handled via `insert`, `delete`, and `update` commands, which can also incorporate subqueries.
