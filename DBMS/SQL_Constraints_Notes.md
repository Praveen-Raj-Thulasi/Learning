# SQL Constraints — Complete Notes

SQL constraints are rules enforced on columns (or tables) to ensure the accuracy, integrity, and reliability of data in a database. They are defined at the time of table creation or can be added later using `ALTER TABLE`.

---

## 1. NOT NULL

Ensures a column **cannot store NULL values**. Every row must provide a value for this column.

```sql
CREATE TABLE employees (
    emp_id   INT          NOT NULL,
    emp_name VARCHAR(100) NOT NULL,
    email    VARCHAR(100)
);
```

> `emp_id` and `emp_name` must always have values; `email` is optional.

---

## 2. UNIQUE

Ensures all values in a column (or combination of columns) are **distinct** across rows. Unlike PRIMARY KEY, a table can have multiple UNIQUE constraints, and UNIQUE columns can hold NULL (unless also NOT NULL).

```sql
CREATE TABLE users (
    user_id  INT         NOT NULL,
    username VARCHAR(50) UNIQUE,
    email    VARCHAR(100) UNIQUE NOT NULL
);
```

**Composite UNIQUE constraint:**

```sql
CREATE TABLE enrollments (
    student_id INT,
    course_id  INT,
    CONSTRAINT uq_enrollment UNIQUE (student_id, course_id)
);
```

> The same student cannot enroll in the same course twice, but can enroll in different courses.

---

## 3. PRIMARY KEY

A combination of `NOT NULL` and `UNIQUE`. **Uniquely identifies each row** in a table. A table can have only **one** primary key, which can span multiple columns (composite key).

```sql
CREATE TABLE products (
    product_id   INT          PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price        DECIMAL(10, 2)
);
```

**Composite Primary Key:**

```sql
CREATE TABLE order_items (
    order_id   INT,
    product_id INT,
    quantity   INT,
    PRIMARY KEY (order_id, product_id)
);
```

---

## 4. FOREIGN KEY

Establishes a **link between two tables** by referencing the primary key (or unique key) of another table. Enforces **referential integrity** — you cannot insert a value in the foreign key column that doesn't exist in the referenced table.

```sql
CREATE TABLE orders (
    order_id    INT PRIMARY KEY,
    customer_id INT,
    order_date  DATE,
    CONSTRAINT fk_customer
        FOREIGN KEY (customer_id)
        REFERENCES customers(customer_id)
);
```

**ON DELETE / ON UPDATE actions:**

| Action      | Behaviour |
|-------------|-----------|
| `CASCADE`   | Automatically deletes/updates child rows when the parent row is deleted/updated |
| `SET NULL`  | Sets foreign key column to NULL when parent row is deleted |
| `RESTRICT`  | Prevents deletion/update of parent row if child rows exist |
| `NO ACTION` | Similar to RESTRICT (default in most DBs) |

```sql
CONSTRAINT fk_customer
    FOREIGN KEY (customer_id)
    REFERENCES customers(customer_id)
    ON DELETE CASCADE
    ON UPDATE SET NULL
```

---

## 5. CHECK

Ensures that values in a column satisfy a **specific condition or expression**. Rejects any row where the condition evaluates to FALSE.

```sql
CREATE TABLE employees (
    emp_id  INT  PRIMARY KEY,
    salary  DECIMAL(10,2),
    age     INT,
    gender  CHAR(1),
    CONSTRAINT chk_salary CHECK (salary > 0),
    CONSTRAINT chk_age    CHECK (age BETWEEN 18 AND 65),
    CONSTRAINT chk_gender CHECK (gender IN ('M', 'F', 'O'))
);
```

**Multi-column CHECK:**

```sql
CREATE TABLE bookings (
    booking_id  INT PRIMARY KEY,
    check_in    DATE,
    check_out   DATE,
    CONSTRAINT chk_dates CHECK (check_out > check_in)
);
```

---

## 6. DEFAULT

Assigns an **automatic value** to a column when no value is provided during an INSERT.

```sql
CREATE TABLE orders (
    order_id    INT PRIMARY KEY,
    order_date  DATE          DEFAULT CURRENT_DATE,
    status      VARCHAR(20)   DEFAULT 'Pending',
    quantity    INT           DEFAULT 1
);

-- Inserting without specifying status or order_date
INSERT INTO orders (order_id) VALUES (101);
-- Result: order_date = today, status = 'Pending', quantity = 1
```

---

## 7. INDEX (Implicit Constraint)

While not a constraint in the strict sense, indexes are closely related — PRIMARY KEY and UNIQUE constraints automatically create indexes. You can also create explicit indexes to speed up queries.

```sql
CREATE INDEX idx_emp_name ON employees(emp_name);
```

---

## Adding Constraints to Existing Tables

Use `ALTER TABLE` to add constraints after table creation:

```sql
-- Add NOT NULL
ALTER TABLE employees MODIFY emp_name VARCHAR(100) NOT NULL;

-- Add UNIQUE
ALTER TABLE users ADD CONSTRAINT uq_email UNIQUE (email);

-- Add CHECK
ALTER TABLE products ADD CONSTRAINT chk_price CHECK (price >= 0);

-- Add FOREIGN KEY
ALTER TABLE orders
    ADD CONSTRAINT fk_customer
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id);
```

---

## Dropping Constraints

```sql
-- Drop a named constraint
ALTER TABLE orders DROP CONSTRAINT fk_customer;

-- Drop PRIMARY KEY (MySQL)
ALTER TABLE products DROP PRIMARY KEY;

-- Drop UNIQUE (MySQL)
ALTER TABLE users DROP INDEX uq_email;
```

---

## Summary Table

| Constraint    | Purpose                                          | NULL Allowed? | Multiple per Table? |
|---------------|--------------------------------------------------|:-------------:|:-------------------:|
| `NOT NULL`    | Column must have a value                         | No            | Yes                 |
| `UNIQUE`      | All values must be distinct                      | Yes (once)    | Yes                 |
| `PRIMARY KEY` | Unique row identifier (NOT NULL + UNIQUE)        | No            | No (one only)       |
| `FOREIGN KEY` | Links to another table's primary/unique key      | Yes           | Yes                 |
| `CHECK`       | Value must satisfy a condition                   | Yes           | Yes                 |
| `DEFAULT`     | Provides a fallback value when none is given     | Yes           | Yes                 |

---

## Best Practices

- Always define a `PRIMARY KEY` for every table.
- Use `NOT NULL` wherever a column is logically required.
- Name your constraints explicitly (e.g., `CONSTRAINT fk_customer ...`) for easier management.
- Use `FOREIGN KEY` constraints to maintain referential integrity rather than relying on application logic alone.
- Use `CHECK` constraints to enforce domain rules at the database level.
- Avoid over-indexing — too many indexes slow down INSERT/UPDATE operations.
