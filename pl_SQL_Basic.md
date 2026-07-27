# PL/SQL Notes

A concise and interview-oriented PL/SQL guide for Oracle Database developers.

---

# Table of Contents

1. Introduction
2. PL/SQL Block Structure
3. Data Types
4. Variables
5. Operators
6. Conditional Statements
7. Loops
8. Procedures
9. Functions
10. Cursors
11. Records
12. Exception Handling
13. Triggers
14. Packages
15. Collections
16. Transactions
17. Interview Questions
18. Learning Roadmap

---

# 1. What is PL/SQL?

**PL/SQL (Procedural Language/SQL)** is Oracle's procedural extension of SQL.

It combines SQL with programming features such as:

- Variables
- Loops
- Conditions
- Procedures
- Functions
- Exception Handling
- Packages
- Triggers

### Why PL/SQL?

- Executes business logic inside Oracle Database
- Improves performance
- Reduces network traffic
- Supports modular programming
- Provides robust exception handling

---

# 2. PL/SQL Block Structure

Every PL/SQL program is written as a block.

```sql
DECLARE
    -- Variable Declaration
BEGIN
    -- Executable Statements
EXCEPTION
    -- Exception Handling
END;
/
```

### Sections

### DECLARE (Optional)

Used to declare variables, cursors, records, etc.

### BEGIN (Required)

Contains executable statements.

### EXCEPTION (Optional)

Handles runtime errors.

---

# 3. Data Types

## Numeric

- NUMBER
- INTEGER
- PLS_INTEGER
- BINARY_INTEGER
- FLOAT

```sql
v_salary NUMBER;
```

---

## Character

- CHAR
- VARCHAR2

```sql
v_name VARCHAR2(100);
```

---

## Boolean

```sql
v_found BOOLEAN;
```

---

## Date

```sql
v_date DATE;
```

---

## Large Objects (LOB)

- BLOB
- CLOB
- BFILE

---

# 4. Variables

## Declaration

```sql
DECLARE
    v_name VARCHAR2(50);
```

## Initialization

```sql
v_name := 'Abdullah';
```

## Assigning SQL Result

```sql
SELECT name
INTO v_name
FROM employee
WHERE id = 1;
```

---

# 5. Operators

## Arithmetic

```
+
-
*
/
```

## Relational

```
=
<>
!=
<
>
<=
>=
```

## Comparison

- LIKE
- BETWEEN
- IN
- IS NULL
- IS NOT NULL

## Logical

- AND
- OR
- NOT

---

# 6. Conditional Statements

## IF

```sql
IF salary > 50000 THEN
    ...
END IF;
```

---

## IF ELSE

```sql
IF salary > 50000 THEN
    ...
ELSE
    ...
END IF;
```

---

## IF ELSIF

```sql
IF salary > 50000 THEN
    ...
ELSIF salary > 30000 THEN
    ...
ELSE
    ...
END IF;
```

---

## CASE

```sql
CASE
    WHEN salary > 50000 THEN
        ...
    ELSE
        ...
END CASE;
```

---

# 7. Loops

## Basic LOOP

```sql
LOOP
    EXIT WHEN condition;
END LOOP;
```

---

## WHILE LOOP

```sql
WHILE condition LOOP
    ...
END LOOP;
```

---

## FOR LOOP

```sql
FOR i IN 1..10 LOOP
    ...
END LOOP;
```

---

## Loop Control Statements

- EXIT
- CONTINUE
- GOTO

---

# 8. Procedures

A Procedure is a stored PL/SQL program that performs a specific task.

## Syntax

```sql
CREATE OR REPLACE PROCEDURE procedure_name
AS
BEGIN
    ...
END;
/
```

## Execute

```sql
BEGIN
    procedure_name;
END;
/
```

---

## Parameter Modes

### IN

Input Parameter

### OUT

Output Parameter

### IN OUT

Input + Output Parameter

---

# 9. Functions

A Function always returns a value.

## Syntax

```sql
CREATE OR REPLACE FUNCTION get_salary
RETURN NUMBER
AS
BEGIN
    RETURN 1000;
END;
/
```

## Call

```sql
SELECT get_salary
FROM dual;
```

---

# Procedure vs Function

| Procedure | Function |
|------------|----------|
| May or may not return value | Must return value |
| Called using BEGIN...END | Can be called from SQL |
| Used for business operations | Used for calculations |

---

# 10. Cursors

A Cursor processes multiple rows.

## Implicit Cursor

Automatically created by Oracle.

Useful Attributes

```
SQL%FOUND
SQL%NOTFOUND
SQL%ROWCOUNT
```

---

## Explicit Cursor

```sql
CURSOR emp_cur IS
SELECT *
FROM employee;
```

### Steps

1. OPEN
2. FETCH
3. CLOSE

---

# 11. Records

## %ROWTYPE

Stores one complete row.

```sql
emp_rec employee%ROWTYPE;
```

---

## User Defined Record

```sql
TYPE emp_rec IS RECORD(
    id NUMBER,
    name VARCHAR2(50)
);
```

---

# 12. Exception Handling

```sql
BEGIN
    ...
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        ...
    WHEN OTHERS THEN
        ...
END;
/
```

## Common Exceptions

- NO_DATA_FOUND
- TOO_MANY_ROWS
- ZERO_DIVIDE
- DUP_VAL_ON_INDEX
- INVALID_NUMBER
- INVALID_CURSOR
- OTHERS

---

# 13. Triggers

A Trigger executes automatically when an event occurs.

## Types

### BEFORE Trigger

Runs before INSERT/UPDATE/DELETE

### AFTER Trigger

Runs after INSERT/UPDATE/DELETE

### Row-Level Trigger

Runs for each row.

### Statement-Level Trigger

Runs once per statement.

---

# 14. Packages

A Package is a collection of related:

- Procedures
- Functions
- Variables
- Cursors

Components

- Package Specification
- Package Body

Advantages

- Code Reusability
- Better Organization
- Performance
- Security

---

# 15. Collections

## Associative Array

```
INDEX BY PLS_INTEGER
```

---

## Nested Table

```
TABLE OF datatype
```

---

## VARRAY

Fixed-size array.

### Collection Methods

- COUNT
- EXISTS
- FIRST
- LAST
- NEXT
- PRIOR
- DELETE
- EXTEND

---

# 16. Transactions

## COMMIT

Permanently saves changes.

```sql
COMMIT;
```

---

## ROLLBACK

Undo uncommitted changes.

```sql
ROLLBACK;
```

---

# Important Keywords

```
DECLARE
BEGIN
END
EXCEPTION
IF
CASE
LOOP
FOR
WHILE
CURSOR
PROCEDURE
FUNCTION
PACKAGE
TRIGGER
COMMIT
ROLLBACK
```

---

# PL/SQL Interview Questions

1. What is PL/SQL?
2. Difference between SQL and PL/SQL?
3. Procedure vs Function?
4. Implicit vs Explicit Cursor?
5. What is %TYPE?
6. What is %ROWTYPE?
7. What are IN, OUT, and IN OUT parameters?
8. What is Exception Handling?
9. What is Trigger?
10. What is Package?
11. Trigger vs Procedure?
12. What are Collections?
13. Nested Table vs VARRAY?
14. What is COMMIT?
15. What is ROLLBACK?
16. What is Dynamic SQL?

---

# Learning Roadmap

```
PL/SQL Basics
      │
      ▼
Variables
      │
      ▼
Data Types
      │
      ▼
Operators
      │
      ▼
IF / CASE
      │
      ▼
Loops
      │
      ▼
Procedures
      │
      ▼
Functions
      │
      ▼
Cursors
      │
      ▼
Records
      │
      ▼
Exception Handling
      │
      ▼
Triggers
      │
      ▼
Packages
      │
      ▼
Collections
      │
      ▼
Transactions
      │
      ▼
Dynamic SQL
```

---

# References

- Oracle PL/SQL Documentation
- Oracle Database Concepts
- Oracle SQL Language Reference

  # Oracle SQL & PL/SQL Notes

> A complete Oracle SQL & PL/SQL learning guide for beginners, interview preparation, and enterprise application development.

---

# Table of Contents

## SQL

1. SQL Statement Types
2. SELECT Statement
3. WHERE Clause
4. ORDER BY Clause
5. HAVING Clause
6. SQL Functions
7. GROUP BY
8. Joins
9. Subqueries
10. Set Operators
11. DML
12. Transactions
13. DDL
14. Constraints
15. Views
16. Sequences
17. Indexes
18. Synonyms
19. Data Dictionary Views
20. Important Intellect Tables

---

## PL/SQL

1. Introduction
2. Data Types
3. Variables
4. Operators
5. Conditional Statements
6. Loops
7. Procedures
8. Functions
9. Cursors
10. Records
11. Exceptions
12. Triggers
13. Packages
14. Collections
15. Transactions

---

# SQL

## 1. Types of SQL Statements

### DDL (Data Definition Language)

Used to define database objects.

- CREATE
- ALTER
- DROP
- TRUNCATE
- RENAME

---

### DML (Data Manipulation Language)

Used to manipulate data.

- SELECT
- INSERT
- UPDATE
- DELETE
- MERGE

---

### DCL (Data Control Language)

Used for permissions.

- GRANT
- REVOKE

---

### TCL (Transaction Control Language)

Used to control transactions.

- COMMIT
- ROLLBACK
- SAVEPOINT

---

# 2. SELECT Statement

Retrieve data from a table.

```sql
SELECT *
FROM EMPLOYEE;
```

---

## Column Alias

```sql
SELECT EMP_NAME AS NAME
FROM EMPLOYEE;
```

---

## Concatenation Operator

```sql
SELECT FIRST_NAME || ' ' || LAST_NAME
FROM EMPLOYEE;
```

---

# 3. WHERE Clause

Filters rows.

```sql
SELECT *
FROM EMPLOYEE
WHERE SALARY > 50000;
```

---

## Comparison Operators

- =
- <>
- !=
- >
- <
- >=
- <=

---

## Logical Operators

- AND
- OR
- NOT

---

## Rules of Precedence

```
()
NOT
AND
OR
```

---

# 4. ORDER BY

Sorts result.

```sql
SELECT *
FROM EMPLOYEE
ORDER BY SALARY DESC;
```

---

# 5. HAVING Clause

Filters grouped data.

```sql
SELECT DEPT_ID,
COUNT(*)
FROM EMPLOYEE
GROUP BY DEPT_ID
HAVING COUNT(*) > 5;
```

---

# 6. Substitution Variables

```
&
:
```

Example

```sql
SELECT *
FROM EMPLOYEE
WHERE EMP_ID=&ID;
```

---

# 7. SQL Functions

## Single Row Functions

- UPPER
- LOWER
- INITCAP
- LENGTH
- SUBSTR
- ROUND
- ABS

---

## Date Functions

- SYSDATE
- ADD_MONTHS
- MONTHS_BETWEEN
- NEXT_DAY
- LAST_DAY

---

## Conversion Functions

- TO_CHAR
- TO_DATE
- TO_NUMBER

---

## General Functions

- NVL
- NVL2
- NULLIF
- COALESCE

---

## Conditional Expressions

- CASE
- DECODE

---

## Group Functions

- COUNT
- SUM
- AVG
- MIN
- MAX

---

# 8. GROUP BY

```sql
SELECT DEPT_ID,
AVG(SALARY)
FROM EMPLOYEE
GROUP BY DEPT_ID;
```

---

# 9. Joins

## Types

- INNER JOIN
- LEFT OUTER JOIN
- RIGHT OUTER JOIN
- FULL OUTER JOIN
- CROSS JOIN
- SELF JOIN

---

# 10. Subqueries

## Single Row

```
=
<
>
```

---

## Multiple Row

```
IN
ANY
ALL
EXISTS
```

---

# Guidelines

- Execute inner query first.
- Outer query uses inner query result.

---

# 11. Set Operators

## UNION

Removes duplicates.

## UNION ALL

Keeps duplicates.

## INTERSECT

Common rows.

## MINUS

Rows from first query not found in second query.

---

# 12. DML Statements

## INSERT

```sql
INSERT INTO EMPLOYEE
VALUES(...);
```

---

## UPDATE

```sql
UPDATE EMPLOYEE
SET SALARY=60000;
```

---

## DELETE

```sql
DELETE FROM EMPLOYEE;
```

---

## TRUNCATE

Deletes all rows.

Cannot rollback.

---

## MERGE

Insert or Update.

---

## INSERT Using Subquery

```sql
INSERT INTO EMPLOYEE_BACKUP
SELECT *
FROM EMPLOYEE;
```

---

# 13. Transactions

## COMMIT

Save changes.

---

## ROLLBACK

Undo changes.

---

## SAVEPOINT

Partial rollback.

---

# 14. DDL

## CREATE TABLE

```sql
CREATE TABLE EMPLOYEE(
ID NUMBER PRIMARY KEY,
NAME VARCHAR2(100)
);
```

---

## ALTER TABLE

Add, Modify or Drop columns.

---

## DROP TABLE

Delete table permanently.

---

# 15. Constraints

- NOT NULL
- UNIQUE
- PRIMARY KEY
- FOREIGN KEY
- CHECK

---

## Constraint Levels

- Column Level
- Table Level

---

# 16. Views

## Simple View

Based on one table.

---

## Complex View

Based on multiple tables.

---

## Operations

- CREATE VIEW
- UPDATE VIEW
- DROP VIEW

---

# 17. Sequence

```sql
CREATE SEQUENCE EMP_SEQ;
```

---

# 18. Index

Improve query performance.

```sql
CREATE INDEX IDX_EMP
ON EMPLOYEE(NAME);
```

---

# 19. Synonyms

Provide another name for database objects.

---

# 20. Data Dictionary Views

- USER_TABLES
- USER_TAB_COLUMNS
- USER_OBJECTS
- USER_CONSTRAINTS
- USER_CONS_COLUMNS
- USER_VIEWS
- USER_SEQUENCES
- USER_TAB_SYNONYMS
- ALL_OBJECTS

---

# Important Intellect Tables

> Add project-specific tables here.

Example

- ACNTS
- CLIENTS
- TRAN2020
- PRODUCTS
- SMSALERTQ
- GLMAST
- GLBALASONHIST

---

# PL/SQL

## PL/SQL Topics

### Basic Syntax

- Block Structure
- DECLARE
- BEGIN
- EXCEPTION
- END

---

### Data Types

- Numeric
- Character
- Boolean
- Date
- LOB

---

### Variables

- Declaration
- Initialization
- Scope
- Local Variables
- Global Variables

---

### Operators

- Arithmetic
- Relational
- Logical
- Comparison

---

### Conditional Statements

- IF
- IF ELSE
- IF ELSIF
- CASE

---

### Loops

- LOOP
- WHILE
- FOR
- Nested LOOP

Control Statements

- EXIT
- CONTINUE
- GOTO

---

### Procedures

- Create Procedure
- Execute Procedure
- Drop Procedure

Parameter Modes

- IN
- OUT
- IN OUT

Passing Parameters

- Positional
- Named
- Mixed

---

### Functions

- Create Function
- Call Function
- Recursive Function

---

### Cursors

## Implicit Cursor

- SQL%FOUND
- SQL%NOTFOUND
- SQL%ROWCOUNT

---

## Explicit Cursor

- OPEN
- FETCH
- CLOSE

Attributes

- %ISOPEN

---

### Records

- %ROWTYPE
- Cursor Records
- User Defined Records

---

### Exception Handling

Common Exceptions

- NO_DATA_FOUND
- TOO_MANY_ROWS
- DUP_VAL_ON_INDEX
- ZERO_DIVIDE
- INVALID_CURSOR
- INVALID_NUMBER
- CURSOR_ALREADY_OPEN
- OTHERS

---

### Triggers

Types

- Table Trigger
- Schema Trigger

Benefits

- Auditing
- Validation
- Logging
- Business Rules

---

### Packages

- Specification
- Body
- Package Variables
- Package Functions
- Package Procedures

---

### Collections

## Associative Array

```
INDEX BY PLS_INTEGER
```

---

## Nested Table

---

## VARRAY

---

Collection Methods

- EXISTS
- COUNT
- LIMIT
- FIRST
- LAST
- PRIOR
- NEXT
- EXTEND
- DELETE

Collection Exceptions

- COLLECTION_IS_NULL
- NO_DATA_FOUND
- SUBSCRIPT_BEYOND_COUNT
- SUBSCRIPT_OUTSIDE_LIMIT
- VALUE_ERROR

---

### Transactions

- COMMIT
- ROLLBACK
- SAVEPOINT
- AUTOCOMMIT

```sql
SET AUTOCOMMIT OFF;
```

---

# Oracle SQL Learning Roadmap

```
SQL Basics
      │
      ▼
SELECT
      │
      ▼
WHERE
      │
      ▼
ORDER BY
      │
      ▼
Functions
      │
      ▼
GROUP BY
      │
      ▼
HAVING
      │
      ▼
JOINS
      │
      ▼
SUBQUERY
      │
      ▼
SET OPERATORS
      │
      ▼
DML
      │
      ▼
DDL
      │
      ▼
CONSTRAINTS
      │
      ▼
VIEWS
      │
      ▼
SEQUENCES
      │
      ▼
INDEXES
      │
      ▼
PL/SQL
```

---

# Oracle PL/SQL Learning Roadmap

```
PL/SQL Block
      │
      ▼
Variables
      │
      ▼
Operators
      │
      ▼
IF / CASE
      │
      ▼
Loops
      │
      ▼
Procedures
      │
      ▼
Functions
      │
      ▼
Cursors
      │
      ▼
Records
      │
      ▼
Exceptions
      │
      ▼
Triggers
      │
      ▼
Packages
      │
      ▼
Collections
      │
      ▼
Transactions
```

---

# References

- Oracle Database Documentation
- Oracle SQL Language Reference
- Oracle PL/SQL User's Guide
- Oracle Database Concepts
