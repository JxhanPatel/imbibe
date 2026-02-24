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
