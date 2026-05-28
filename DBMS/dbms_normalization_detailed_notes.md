# Database Normalization in DBMS

## Introduction to Normalization

Normalization is a database design technique used to organize data in relational databases to:

- Reduce data redundancy (duplicate data)
- Eliminate unwanted characteristics like insertion, update, and deletion anomalies
- Improve data consistency
- Maintain data integrity
- Make the database structure more efficient

Normalization divides large tables into smaller related tables and establishes relationships between them using keys.

---

# Problems Without Normalization

Before learning normal forms, understand the problems caused by poor database design.

## Example: Unnormalized Student Course Table

| StudentID | StudentName | CourseID | CourseName | Faculty | FacultyPhone |
|---|---|---|---|---|---|
| 101 | Arun | C101 | DBMS | Ravi | 9876543210 |
| 101 | Arun | C102 | Java | Kumar | 9999999999 |
| 102 | Priya | C101 | DBMS | Ravi | 9876543210 |

### Problems

### 1. Data Redundancy
- Faculty details are repeated multiple times.
- Course information is duplicated.

### 2. Update Anomaly
- If Ravi's phone number changes, it must be updated in multiple rows.
- Missing one row creates inconsistent data.

### 3. Insertion Anomaly
- Cannot insert a new course unless at least one student enrolls.

### 4. Deletion Anomaly
- If the last student leaves a course, course details may also get deleted.

Normalization solves these problems.

---

# Types of Normal Forms

Normalization is performed in stages called Normal Forms.

1. First Normal Form (1NF)
2. Second Normal Form (2NF)
3. Third Normal Form (3NF)
4. Boyce-Codd Normal Form (BCNF)
5. Fourth Normal Form (4NF)
6. Fifth Normal Form (5NF)

The most commonly used normal forms are:
- 1NF
- 2NF
- 3NF
- BCNF

---

# First Normal Form (1NF)

## Definition

A table is in First Normal Form if:

- Each column contains atomic (single) values
- No repeating groups or arrays exist
- Each row is unique

### Atomic Value
Atomic means indivisible.

For example:
- "Java, Python" in one column is NOT atomic.
- Separate rows should be used.

---

# Example Before 1NF

| StudentID | StudentName | Subjects |
|---|---|---|
| 101 | Arun | DBMS, Java |
| 102 | Priya | OS |

### Problem
- Subjects column contains multiple values.
- Violates 1NF.

---

# Converting to 1NF

| StudentID | StudentName | Subject |
|---|---|---|
| 101 | Arun | DBMS |
| 101 | Arun | Java |
| 102 | Priya | OS |

Now:
- Each cell contains a single value.
- Table satisfies 1NF.

---

# Important Points About 1NF

## Rules

- No multi-valued attributes
- No repeating columns
- Unique rows
- Atomic values only

## Advantages

- Easier querying
- Better organization
- Removes repeating groups

## Still Existing Problems

Even after 1NF:
- Redundancy may still exist
- Update anomalies may still occur

This leads to 2NF.

---

# Second Normal Form (2NF)

## Definition

A table is in Second Normal Form if:

1. It is already in 1NF
2. No partial dependency exists

---

# What is Partial Dependency?

Partial dependency occurs when:

- A non-prime attribute depends only on part of a composite primary key.

---

# Example of 2NF Violation

## Student Course Table

| StudentID | CourseID | StudentName | CourseName | Marks |
|---|---|---|---|---|
| 101 | C101 | Arun | DBMS | 90 |
| 101 | C102 | Arun | Java | 88 |
| 102 | C101 | Priya | DBMS | 91 |

### Composite Primary Key

Primary Key = (StudentID, CourseID)

---

# Dependency Analysis

| Attribute | Depends On |
|---|---|
| StudentName | StudentID |
| CourseName | CourseID |
| Marks | StudentID + CourseID |

### Problem

StudentName depends only on StudentID.
CourseName depends only on CourseID.

They do NOT depend on the entire composite key.

This is partial dependency.

Therefore, table is NOT in 2NF.

---

# Converting to 2NF

## Student Table

| StudentID | StudentName |
|---|---|
| 101 | Arun |
| 102 | Priya |

---

## Course Table

| CourseID | CourseName |
|---|---|
| C101 | DBMS |
| C102 | Java |

---

## Enrollment Table

| StudentID | CourseID | Marks |
|---|---|---|
| 101 | C101 | 90 |
| 101 | C102 | 88 |
| 102 | C101 | 91 |

Now:
- No partial dependency exists.
- Table structure satisfies 2NF.

---

# Advantages of 2NF

- Reduces redundancy
- Eliminates partial dependency
- Improves consistency
- Better data organization

---

# Third Normal Form (3NF)

## Definition

A table is in Third Normal Form if:

1. It is already in 2NF
2. No transitive dependency exists

---

# What is Transitive Dependency?

A transitive dependency occurs when:

- A non-key attribute depends on another non-key attribute.

Example:

A → B
B → C

Then:
A → C (transitive dependency)

---

# Example of 3NF Violation

| EmpID | EmpName | DeptID | DeptName |
|---|---|---|---|
| E1 | Arun | D1 | HR |
| E2 | Priya | D2 | Finance |
| E3 | Rahul | D1 | HR |

---

# Dependency Analysis

Primary Key = EmpID

| Attribute | Depends On |
|---|---|
| EmpName | EmpID |
| DeptID | EmpID |
| DeptName | DeptID |

### Problem

DeptName depends on DeptID.
DeptID depends on EmpID.

So:
EmpID → DeptID → DeptName

This is transitive dependency.

Table is NOT in 3NF.

---

# Converting to 3NF

## Employee Table

| EmpID | EmpName | DeptID |
|---|---|---|
| E1 | Arun | D1 |
| E2 | Priya | D2 |
| E3 | Rahul | D1 |

---

## Department Table

| DeptID | DeptName |
|---|---|
| D1 | HR |
| D2 | Finance |

Now:
- No transitive dependency exists.
- Table satisfies 3NF.

---

# Advantages of 3NF

- Removes transitive dependency
- Reduces duplication
- Prevents anomalies
- Improves maintainability

---

# Boyce-Codd Normal Form (BCNF)

## Definition

A table is in BCNF if:

- It is in 3NF
- For every functional dependency:

X → Y

X must be a super key.

BCNF is a stricter version of 3NF.

---

# Example of BCNF Violation

| Student | Subject | Teacher |
|---|---|---|
| Arun | DBMS | Ravi |
| Priya | Java | Kumar |
| Arun | Java | Kumar |

Assumptions:
- One teacher teaches only one subject.
- A student can take multiple subjects.

---

# Functional Dependencies

1. (Student, Subject) → Teacher
2. Teacher → Subject

### Candidate Keys
- (Student, Subject)
- (Student, Teacher)

### Problem

Teacher is not a super key.
But:
Teacher → Subject

This violates BCNF.

---

# Converting to BCNF

## Teacher Table

| Teacher | Subject |
|---|---|
| Ravi | DBMS |
| Kumar | Java |

---

## Student Teacher Table

| Student | Teacher |
|---|---|
| Arun | Ravi |
| Priya | Kumar |
| Arun | Kumar |

Now the tables satisfy BCNF.

---

# Fourth Normal Form (4NF)

## Definition

A table is in 4NF if:

- It is in BCNF
- No multi-valued dependency exists

---

# Multi-Valued Dependency

Occurs when:

One attribute independently determines multiple values of another attribute.

---

# Example of 4NF Violation

| Student | Hobby | Language |
|---|---|---|
| Arun | Cricket | English |
| Arun | Music | English |
| Arun | Cricket | Tamil |
| Arun | Music | Tamil |

Here:
- Hobbies and languages are independent.
- Causes unnecessary combinations.

---

# Converting to 4NF

## Student Hobby Table

| Student | Hobby |
|---|---|
| Arun | Cricket |
| Arun | Music |

---

## Student Language Table

| Student | Language |
|---|---|
| Arun | English |
| Arun | Tamil |

Now table satisfies 4NF.

---

# Fifth Normal Form (5NF)

## Definition

A table is in 5NF if:

- It is in 4NF
- It cannot be decomposed further without losing information.

5NF deals with join dependencies.

---

# Example of 5NF

Suppose a company maintains:

- Supplier
- Product
- Project

Complex many-to-many relationships may require decomposition into smaller tables.

5NF ensures:
- No redundant joins
- No loss of information after decomposition

5NF is rarely used in practical applications.

---

# Summary of Normal Forms

| Normal Form | Removes |
|---|---|
| 1NF | Repeating groups and multi-valued attributes |
| 2NF | Partial dependency |
| 3NF | Transitive dependency |
| BCNF | Dependency where determinant is not a super key |
| 4NF | Multi-valued dependency |
| 5NF | Join dependency |

---

# Comparison of 1NF, 2NF, and 3NF

| Feature | 1NF | 2NF | 3NF |
|---|---|---|---|
| Atomic values | Yes | Yes | Yes |
| No partial dependency | No | Yes | Yes |
| No transitive dependency | No | No | Yes |
| Removes redundancy | Partially | Better | More effectively |

---

# Functional Dependency

## Definition

Functional dependency describes relationships between attributes.

If:

A → B

Then:
- Attribute A determines attribute B.

---

# Types of Functional Dependency

## 1. Full Functional Dependency

Attribute depends on the entire primary key.

Example:
(StudentID, CourseID) → Marks

---

## 2. Partial Dependency

Attribute depends on part of the key.

Example:
StudentID → StudentName

---

## 3. Transitive Dependency

Indirect dependency.

Example:
EmpID → DeptID
DeptID → DeptName

---

# Candidate Key

A candidate key is:

- Minimal attribute set
- Uniquely identifies each row

A table can have multiple candidate keys.

---

# Primary Key

A primary key is:

- Selected candidate key
- Used for unique identification

Example:
StudentID

---

# Super Key

A super key is:

- Any attribute combination that uniquely identifies rows.

Example:
(StudentID, Name)

---

# Why Normalization is Important

## Advantages

### 1. Reduces Redundancy
Duplicate data is minimized.

### 2. Improves Data Integrity
Consistent and accurate data.

### 3. Avoids Anomalies
Prevents insertion, update, and deletion issues.

### 4. Saves Storage
Less repeated information.

### 5. Easier Maintenance
Simpler updates and modifications.

---

# Disadvantages of Normalization

## 1. More Tables
Database structure becomes complex.

## 2. More Joins
Queries may become slower due to joins.

## 3. Complex Design
Requires proper understanding of dependencies.

---

# Denormalization

## Definition

Denormalization is the process of intentionally adding redundancy to improve read performance.

Used when:
- Faster querying is needed
- Read operations are more frequent

---

# Real-World Example

## E-Commerce Database

### Tables

### Customer Table

| CustomerID | CustomerName |
|---|---|
| 1 | Arun |

---

### Product Table

| ProductID | ProductName |
|---|---|
| P1 | Laptop |

---

### Order Table

| OrderID | CustomerID | ProductID |
|---|---|---|
| O1 | 1 | P1 |

This design:
- Avoids redundancy
- Maintains consistency
- Follows normalization principles

---

# Important Interview Questions

## 1. What is normalization?
Normalization is the process of organizing database tables to reduce redundancy and improve integrity.

---

## 2. What are anomalies?

- Insertion anomaly
- Update anomaly
- Deletion anomaly

---

## 3. Difference Between 2NF and 3NF

| 2NF | 3NF |
|---|---|
| Removes partial dependency | Removes transitive dependency |
| Based on composite keys | Based on non-key attributes |

---

## 4. Is BCNF stronger than 3NF?
Yes. BCNF is a stricter version of 3NF.

---

## 5. Which normal form is most commonly used?
3NF is most commonly used in practical database systems.

---

# Final Conclusion

Normalization is one of the most important concepts in DBMS.

It helps:
- Organize data efficiently
- Reduce redundancy
- Prevent anomalies
- Improve database consistency

Most real-world systems generally use normalization up to:
- 3NF
- BCNF

Higher normal forms like 4NF and 5NF are used in specialized situations.

