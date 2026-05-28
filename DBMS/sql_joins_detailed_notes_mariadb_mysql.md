# SQL Joins – Detailed Notes (MariaDB/MySQL)

## What is a JOIN in SQL?

A JOIN is used to combine rows from two or more tables based on a related column.

Joins are mainly used when:
- Data is stored across multiple tables
- We want to retrieve combined information
- We want to establish relationships between tables

---

# Tables Used in Examples

## Table: e1

```sql
create table e1(
    eid int primary key,
    ename varchar(20)
);
```

### Insert Data

```sql
insert into e1 values
    (101, "a"),
    (102, "b"),
    (103, "c");

insert into e1 values (105, "d");
```

### Data in e1

| eid | ename |
|-----|--------|
| 101 | a |
| 102 | b |
| 103 | c |
| 105 | d |

---

## Table: e2

```sql
create table e2(
    eid int primary key,
    esal int
);
```

### Insert Data

```sql
insert into e2 values
    (101, 5000),
    (102, 10000),
    (103, 2000);

insert into e2 values (104, 50000);
```

### Data in e2

| eid | esal |
|-----|-------|
| 101 | 5000 |
| 102 | 10000 |
| 103 | 2000 |
| 104 | 50000 |

---

# Types of SQL Joins

1. INNER JOIN
2. LEFT JOIN
3. RIGHT JOIN
4. FULL JOIN
5. CROSS JOIN
6. SELF JOIN

---

# 1. INNER JOIN

## Definition

INNER JOIN returns only the matching rows from both tables.

Only rows satisfying the join condition are displayed.

---

## Syntax

```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

---

## Your Example

```sql
select e1.eid, e1.ename, e2.eid, e2.esal
from e1 inner join e2
on e1.eid = e2.eid;
```

---

## Output

| eid | ename | eid | esal |
|-----|--------|-----|-------|
| 101 | a | 101 | 5000 |
| 102 | b | 102 | 10000 |
| 103 | c | 103 | 2000 |

---

## Explanation

Matching eid values in both tables:
- 101
- 102
- 103

These rows exist in both tables, so they are returned.

Rows:
- 105 exists only in e1
- 104 exists only in e2

These are ignored.

---

# 2. LEFT JOIN

## Definition

LEFT JOIN returns:
- all rows from the left table
- matching rows from the right table

If no match exists, NULL values are shown for the right table.

---

## Syntax

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```

---

## Your Example

```sql
select e1.eid, e1.ename, e2.eid, e2.esal
from e1 left join e2
on e1.eid = e2.eid;
```

---

## Output

| eid | ename | eid | esal |
|-----|--------|-----|-------|
| 101 | a | 101 | 5000 |
| 102 | b | 102 | 10000 |
| 103 | c | 103 | 2000 |
| 105 | d | NULL | NULL |

---

## Explanation

All rows from e1 are returned.

For eid = 105:
- no matching row exists in e2
- therefore NULL values appear.

---

# 3. RIGHT JOIN

## Definition

RIGHT JOIN returns:
- all rows from the right table
- matching rows from the left table

If no match exists, NULL values are shown for the left table.

---

## Syntax

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```

---

## Your Example

```sql
select e1.eid, e1.ename, e2.eid, e2.esal
from e1 right join e2
on e1.eid = e2.eid;
```

---

## Output

| eid | ename | eid | esal |
|-----|--------|-----|-------|
| 101 | a | 101 | 5000 |
| 102 | b | 102 | 10000 |
| 103 | c | 103 | 2000 |
| NULL | NULL | 104 | 50000 |

---

## Explanation

All rows from e2 are returned.

For eid = 104:
- no matching row exists in e1
- therefore NULL values appear.

---

# 4. FULL JOIN / FULL OUTER JOIN

## Definition

FULL JOIN returns:
- all matching rows
- unmatched rows from left table
- unmatched rows from right table

---

## Standard SQL Syntax

```sql
SELECT columns
FROM table1
FULL JOIN table2
ON table1.column = table2.column;
```

---

# Important Note About MariaDB/MySQL

MariaDB and MySQL do NOT support:

```sql
FULL JOIN
```

or

```sql
FULL OUTER JOIN
```

Trying this query:

```sql
select e1.eid, e1.ename, e2.eid, e2.esal
from e1 full join e2
on e1.eid = e2.eid;
```

will produce a syntax error.

---

## Why?

MariaDB/MySQL never implemented native FULL JOIN support.

Databases supporting FULL JOIN:
- PostgreSQL
- SQL Server
- Oracle

Databases not supporting FULL JOIN:
- MySQL
- MariaDB
- SQLite

---

# FULL JOIN Simulation in MariaDB/MySQL

We simulate FULL JOIN using:
- LEFT JOIN
- RIGHT JOIN
- UNION

---

## Query

```sql
select e1.eid, e1.ename, e2.eid, e2.esal
from e1 left join e2
on e1.eid = e2.eid

UNION

select e1.eid, e1.ename, e2.eid, e2.esal
from e1 right join e2
on e1.eid = e2.eid;
```

---

## Output

| eid | ename | eid | esal |
|-----|--------|-----|-------|
| 101 | a | 101 | 5000 |
| 102 | b | 102 | 10000 |
| 103 | c | 103 | 2000 |
| 105 | d | NULL | NULL |
| NULL | NULL | 104 | 50000 |

---

## Explanation

This combines:
- unmatched rows from e1
- unmatched rows from e2
- matching rows from both

Thus behaving like FULL JOIN.

---

# UNION vs UNION ALL

## UNION

- Removes duplicate rows
- Used commonly in FULL JOIN simulation

Example:

```sql
SELECT * FROM t1
UNION
SELECT * FROM t2;
```

---

## UNION ALL

- Keeps duplicate rows
- Faster than UNION

Example:

```sql
SELECT * FROM t1
UNION ALL
SELECT * FROM t2;
```

---

# 5. CROSS JOIN

## Definition

CROSS JOIN returns the Cartesian product of two tables.

Every row from table1 combines with every row from table2.

---

## Syntax

```sql
SELECT columns
FROM table1
CROSS JOIN table2;
```

---

## Example

```sql
select *
from e1 cross join e2;
```

---

## Explanation

If:
- e1 has 4 rows
- e2 has 4 rows

Result will contain:

4 × 4 = 16 rows

---

# 6. SELF JOIN

## Definition

A SELF JOIN joins a table with itself.

Used when rows within the same table are related.

---

## Syntax

```sql
SELECT a.column, b.column
FROM table_name a
JOIN table_name b
ON a.column = b.column;
```

---

## Example

Employee-manager relationship.

```sql
CREATE TABLE employee(
    empid INT,
    empname VARCHAR(20),
    managerid INT
);
```

---

# Difference Between Joins

| Join Type | Returns |
|-----------|----------|
| INNER JOIN | Only matching rows |
| LEFT JOIN | All left rows + matching right rows |
| RIGHT JOIN | All right rows + matching left rows |
| FULL JOIN | All rows from both tables |
| CROSS JOIN | Cartesian product |
| SELF JOIN | Table joined with itself |

---

# Visual Understanding

## INNER JOIN

Common data only.

## LEFT JOIN

Everything from left table.

## RIGHT JOIN

Everything from right table.

## FULL JOIN

Everything from both tables.

---

# Best Practice

Instead of:

```sql
select e1.eid, e1.ename, e2.eid, e2.esal
```

prefer:

```sql
select e1.eid, e1.ename, e2.esal
```

because duplicate eid columns are unnecessary.

---

# Clean Query Example

```sql
select e1.eid, e1.ename, e2.esal
from e1 inner join e2
on e1.eid = e2.eid;
```

---

# Important Interview Questions

## Q1. Does MySQL support FULL JOIN?

Answer:
No. FULL JOIN must be simulated using LEFT JOIN + RIGHT JOIN with UNION.

---

## Q2. Difference between WHERE and ON?

- ON specifies join condition
- WHERE filters rows after joining

---

## Q3. Which join keeps unmatched rows?

- LEFT JOIN → unmatched left rows
- RIGHT JOIN → unmatched right rows
- FULL JOIN → unmatched rows from both tables

---

# Conclusion

Joins are one of the most important SQL concepts.

They are heavily used in:
- Database applications
- Backend development
- Reports
- Data analytics
- Enterprise systems

Understanding joins properly is essential for writing efficient SQL queries.

