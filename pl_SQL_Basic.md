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
