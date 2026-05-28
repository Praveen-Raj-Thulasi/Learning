# SQL Subqueries – Detailed Notes

## 1. Introduction to Subqueries

A **Subquery** is a query written inside another SQL query.

It is also called:
- Inner Query
- Nested Query

The outer query uses the result returned by the inner query.

## Basic Syntax

```sql
SELECT column_name
FROM table_name
WHERE column_name OPERATOR (
    SELECT column_name
    FROM table_name
);
```

---

# 2. Why Use Subqueries?

Subqueries are useful when:

- You need the result of one query inside another query.
- You want dynamic filtering.
- You want to compare values with aggregated data.
- You want to avoid hardcoding values.
- You want row-wise comparison.

---

# 3. Types of Subqueries

SQL subqueries are mainly classified into:

1. Single-row subquery
2. Multi-row subquery
3. Correlated subquery

---

# 4. Sample Tables Used in Examples

## Employee Table

```sql
CREATE TABLE employee (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(50),
    department VARCHAR(50),
    salary INT,
    manager_id INT
);
```

## Insert Sample Data

```sql
INSERT INTO employee VALUES
(101, 'Arun', 'HR', 40000, 105),
(102, 'Bala', 'IT', 70000, 106),
(103, 'Charan', 'IT', 65000, 106),
(104, 'Deepak', 'Sales', 50000, 107),
(105, 'Eshan', 'HR', 90000, NULL),
(106, 'Farhan', 'IT', 120000, NULL),
(107, 'Gokul', 'Sales', 95000, NULL);
```

---

# 5. Single-Row Subquery

## Definition

A **Single-row subquery** returns only ONE row and ONE value.

These subqueries are commonly used with operators like:

- =
- >
- <
- >=
- <=
- <>

---

# Example 1 – Employee With Maximum Salary

## Query

```sql
SELECT emp_name, salary
FROM employee
WHERE salary = (
    SELECT MAX(salary)
    FROM employee
);
```

## Explanation

### Step 1: Inner Query Executes First

```sql
SELECT MAX(salary)
FROM employee;
```

Result:

```text
120000
```

### Step 2: Outer Query Executes

```sql
SELECT emp_name, salary
FROM employee
WHERE salary = 120000;
```

## Output

| emp_name | salary |
|----------|---------|
| Farhan   | 120000  |

---

# Example 2 – Employees Earning More Than Average Salary

## Query

```sql
SELECT emp_name, salary
FROM employee
WHERE salary > (
    SELECT AVG(salary)
    FROM employee
);
```

## Explanation

Inner query calculates average salary.

Suppose average salary is:

```text
71666
```

Outer query finds employees whose salary is greater than that value.

## Output

| emp_name | salary |
|----------|---------|
| Eshan    | 90000   |
| Farhan   | 120000  |
| Gokul    | 95000   |

---

# Example 3 – Employees Working in HR Department

```sql
SELECT emp_name
FROM employee
WHERE department = (
    SELECT department
    FROM employee
    WHERE emp_name = 'Arun'
);
```

## Explanation

Inner query:

```sql
SELECT department
FROM employee
WHERE emp_name = 'Arun';
```

Returns:

```text
HR
```

Outer query:

```sql
SELECT emp_name
FROM employee
WHERE department = 'HR';
```

## Output

| emp_name |
|----------|
| Arun     |
| Eshan    |

---

# Characteristics of Single-Row Subqueries

| Feature | Description |
|---|---|
| Returns | One row |
| Common Operators | =, >, <, >=, <= |
| Often Used With | Aggregate functions |
| Error Condition | Error if multiple rows returned |

---

# Common Error in Single-Row Subqueries

## Wrong Query

```sql
SELECT emp_name
FROM employee
WHERE salary = (
    SELECT salary
    FROM employee
    WHERE department = 'IT'
);
```

## Why Error Occurs?

IT department has multiple employees.

Inner query returns:

```text
70000
65000
120000
```

But operator `=` expects only ONE value.

## Error

```text
Subquery returns more than 1 row
```

---

# 6. Multi-Row Subquery

## Definition

A **Multi-row subquery** returns multiple rows.

Since multiple values are returned, we use operators like:

- IN
- ANY
- ALL
- EXISTS

---

# Example 1 – Employees in IT or HR Departments

## Query

```sql
SELECT emp_name, department
FROM employee
WHERE department IN (
    SELECT department
    FROM employee
    WHERE department IN ('IT', 'HR')
);
```

## Explanation

Inner query returns:

```text
IT
HR
```

Outer query retrieves all employees belonging to those departments.

## Output

| emp_name | department |
|----------|-------------|
| Arun     | HR          |
| Bala     | IT          |
| Charan   | IT          |
| Eshan    | HR          |
| Farhan   | IT          |

---

# Example 2 – Employees Earning More Than ANY IT Employee

## Query

```sql
SELECT emp_name, salary
FROM employee
WHERE salary > ANY (
    SELECT salary
    FROM employee
    WHERE department = 'IT'
);
```

## Inner Query Returns

```text
70000
65000
120000
```

## Meaning of ANY

Condition becomes:

```text
salary > at least one value
```

So:

```text
salary > 65000
```

## Output

| emp_name | salary |
|----------|---------|
| Bala     | 70000   |
| Eshan    | 90000   |
| Farhan   | 120000  |
| Gokul    | 95000   |

---

# Example 3 – Employees Earning More Than ALL IT Employees

## Query

```sql
SELECT emp_name, salary
FROM employee
WHERE salary > ALL (
    SELECT salary
    FROM employee
    WHERE department = 'IT'
);
```

## Meaning of ALL

Condition becomes:

```text
salary > every value
```

Must be greater than:

```text
120000
```

## Output

No rows returned because no employee earns more than 120000.

---

# Example 4 – Using EXISTS

## Query

```sql
SELECT emp_name
FROM employee e
WHERE EXISTS (
    SELECT *
    FROM employee
    WHERE manager_id = e.emp_id
);
```

## Explanation

Checks whether an employee manages at least one employee.

## Output

| emp_name |
|----------|
| Eshan    |
| Farhan   |
| Gokul    |

---

# Difference Between IN, ANY, and ALL

| Operator | Meaning |
|---|---|
| IN | Matches any value in list |
| ANY | Condition true for at least one value |
| ALL | Condition true for every value |

---

# 7. Correlated Subquery

## Definition

A **Correlated Subquery** is a subquery that depends on the outer query.

The inner query executes repeatedly for each row processed by the outer query.

---

# Important Concept

Unlike normal subqueries:

- Normal subquery executes first once.
- Correlated subquery executes row-by-row.

---

# Example 1 – Employees Earning More Than Department Average

## Query

```sql
SELECT emp_name, department, salary
FROM employee e1
WHERE salary > (
    SELECT AVG(salary)
    FROM employee e2
    WHERE e1.department = e2.department
);
```

---

# Step-by-Step Explanation

For each employee:

1. Inner query calculates average salary of that employee's department.
2. Outer query compares employee salary with department average.

---

## Department-wise Average

| Department | Average Salary |
|---|---|
| HR | 65000 |
| IT | 85000 |
| Sales | 72500 |

---

## Final Output

| emp_name | department | salary |
|----------|-------------|---------|
| Eshan    | HR          | 90000   |
| Farhan   | IT          | 120000  |
| Gokul    | Sales       | 95000   |

---

# Example 2 – Highest Paid Employee in Each Department

## Query

```sql
SELECT emp_name, department, salary
FROM employee e1
WHERE salary = (
    SELECT MAX(salary)
    FROM employee e2
    WHERE e1.department = e2.department
);
```

---

# Explanation

For each department:

- Inner query finds maximum salary.
- Outer query selects employee matching that salary.

## Output

| emp_name | department | salary |
|----------|-------------|---------|
| Eshan    | HR          | 90000   |
| Farhan   | IT          | 120000  |
| Gokul    | Sales       | 95000   |

---

# Example 3 – Employees Who Are Managers

## Query

```sql
SELECT emp_name
FROM employee e1
WHERE emp_id IN (
    SELECT manager_id
    FROM employee e2
    WHERE e1.emp_id = e2.manager_id
);
```

## Output

| emp_name |
|----------|
| Eshan    |
| Farhan   |
| Gokul    |

---

# Characteristics of Correlated Subqueries

| Feature | Description |
|---|---|
| Depends on outer query | Yes |
| Executes | Row-by-row |
| Performance | Slower for large tables |
| Usage | Advanced filtering and comparisons |

---

# Normal Subquery vs Correlated Subquery

| Feature | Normal Subquery | Correlated Subquery |
|---|---|---|
| Execution Count | Once | Repeated for each row |
| Dependency | Independent | Depends on outer query |
| Performance | Faster | Slower |
| Complexity | Simple | Advanced |

---

# 8. Nested Subqueries

Subqueries can also be nested multiple levels deep.

## Example

```sql
SELECT emp_name
FROM employee
WHERE department = (
    SELECT department
    FROM employee
    WHERE emp_id = (
        SELECT manager_id
        FROM employee
        WHERE emp_name = 'Arun'
    )
);
```

---

# 9. Subqueries in Different SQL Clauses

Subqueries can be used in:

| Clause | Example |
|---|---|
| SELECT | Calculated values |
| FROM | Derived tables |
| WHERE | Filtering |
| HAVING | Aggregate filtering |
| INSERT | Insert from another table |
| UPDATE | Update using query result |
| DELETE | Delete based on conditions |

---

# Example – Subquery in FROM Clause

```sql
SELECT department, avg_salary
FROM (
    SELECT department, AVG(salary) AS avg_salary
    FROM employee
    GROUP BY department
) AS dept_avg;
```

---

# Example – Subquery in UPDATE

```sql
UPDATE employee
SET salary = salary + 5000
WHERE salary < (
    SELECT AVG(salary)
    FROM employee
);
```

---

# Example – Subquery in DELETE

```sql
DELETE FROM employee
WHERE salary < (
    SELECT AVG(salary)
    FROM employee
);
```

---

# 10. Advantages of Subqueries

- Makes queries dynamic
- Reduces hardcoding
- Improves readability
- Useful with aggregate functions
- Helps in complex filtering
- Supports advanced reporting

---

# 11. Disadvantages of Subqueries

- Can reduce performance
- Correlated subqueries are slower
- Harder to debug when deeply nested
- Complex queries become difficult to maintain

---

# 12. Best Practices

## Use Aliases

```sql
SELECT *
FROM employee e;
```

Aliases improve readability.

---

## Avoid Deep Nesting

Too many nested queries reduce readability.

---

## Use JOINs When Appropriate

Sometimes JOINs perform better than subqueries.

---

## Use EXISTS for Large Data

`EXISTS` is often faster than `IN` for large datasets.

---

# 13. Frequently Used Operators in Subqueries

| Operator | Purpose |
|---|---|
| = | Equal comparison |
| > | Greater than |
| < | Less than |
| IN | Match list of values |
| ANY | Match at least one value |
| ALL | Match all values |
| EXISTS | Checks existence |
| NOT EXISTS | Checks absence |

---

# 14. Real-World Use Cases

## Banking Systems

Find customers with balance above average.

## E-Commerce

Find products priced above category average.

## HR Systems

Find employees earning highest salary per department.

## Education Systems

Find students scoring above class average.

---

# 15. Summary

| Type | Returns | Common Operators |
|---|---|---|
| Single-row Subquery | One row | =, >, < |
| Multi-row Subquery | Multiple rows | IN, ANY, ALL |
| Correlated Subquery | Depends on outer query | EXISTS, comparison operators |

---

# Final Notes

Subqueries are one of the most powerful features in SQL.

Understanding:

- how inner queries execute,
- how outer queries use results,
- and when to use correlated subqueries

is essential for writing advanced SQL queries.

Mastering subqueries will help in:

- interviews,
- competitive coding,
- backend development,
- database administration,
- and real-world application development.

