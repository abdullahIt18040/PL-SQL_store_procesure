# Oracle SQL - Creating Other Schema Objects

## What is a Schema?

Oracle-এ **Schema** হলো একটি User-এর অধীনে থাকা সকল Database Object-এর Collection।

উদাহরণ:

```text
BANK Schema
│
├── Tables
├── Views
├── Sequences
├── Indexes
├── Synonyms
├── Constraints
├── Procedures
├── Functions
├── Packages
└── Triggers
```

> **সহজভাবে:** একটি User-এর তৈরি করা সব Database Object মিলে তার **Schema**।

---

# Common Oracle Schema Objects

| Schema Object | কাজ                            |
| ------------- | ------------------------------ |
| Table         | Data সংরক্ষণ করে               |
| View          | Virtual Table                  |
| Sequence      | Auto Increment Number তৈরি করে |
| Index         | Query Performance বাড়ায়      |
| Synonym       | Object-এর Alias Name           |
| Constraint    | Data Validation করে            |

---

# 1. VIEW

## View কী?

View হলো একটি **Virtual Table**।

* নিজে Data Store করে না।
* একটি বা একাধিক Table-এর Data দেখায়।
* Complex Query সহজ করার জন্য ব্যবহৃত হয়।

---

## Syntax

```sql
CREATE VIEW view_name AS
SELECT column1, column2
FROM table_name;
```

---

## Example

### Employee Table

```sql
CREATE TABLE employee
(
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    salary NUMBER
);
```

### Create View

```sql
CREATE VIEW emp_view AS
SELECT emp_id,
       emp_name
FROM employee;
```

### Query View

```sql
SELECT *
FROM emp_view;
```

### Output

| EMP_ID | EMP_NAME |
| ------ | -------- |
| 101    | Rahim    |
| 102    | Karim    |

> এখানে Salary দেখাবে না, কারণ View-এ Salary Include করা হয়নি।

---

## View Diagram

```text
Employee Table
        │
        ▼
   CREATE VIEW
        │
        ▼
    EMP_VIEW
        │
        ▼
 SELECT * FROM EMP_VIEW
```

---

# 2. SEQUENCE

## Sequence কী?

Sequence হলো এমন একটি Database Object যা Automatically Unique Number Generate করে।

Primary Key Generate করার জন্য বেশি ব্যবহার করা হয়।

---

## Syntax

```sql
CREATE SEQUENCE emp_seq
START WITH 1
INCREMENT BY 1;
```

---

## NEXTVAL

```sql
SELECT emp_seq.NEXTVAL
FROM dual;
```

Output

```text
1
```

আবার Execute করলে

```text
2
```

---

## CURRVAL

```sql
SELECT emp_seq.CURRVAL
FROM dual;
```

Output

```text
2
```

> `CURRVAL` সর্বশেষ Generate হওয়া Value Return করে।

---

## Insert Example

```sql
INSERT INTO employee
VALUES
(
emp_seq.NEXTVAL,
'Rahim',
50000
);
```

---

## Sequence Diagram

```text
Sequence
    │
    ▼
NEXTVAL
    │
    ▼
1
    │
    ▼
2
    │
    ▼
3
```

---

# 3. INDEX

## Index কী?

Index হলো এমন একটি Database Object যা Data Search দ্রুত করে।

বইয়ের Index-এর মতো কাজ করে।

---

## Without Index

```text
Employee Table

101
102
103
104
105

↓

Search 104

↓

Full Table Scan
```

---

## With Index

```text
Employee Table
      │
      ▼
    Index
      │
      ▼
 Search 104
      │
      ▼
 Direct Access
```

---

## Syntax

```sql
CREATE INDEX idx_employee_name
ON employee(emp_name);
```

---

## Example

```sql
SELECT *
FROM employee
WHERE emp_name='Rahim';
```

Index থাকলে Query দ্রুত Execute হবে।

---

## কখন Index ব্যবহার করবেন?

* WHERE Clause
* JOIN
* ORDER BY
* GROUP BY
* Frequently Searched Column

---

# 4. SYNONYM

## Synonym কী?

Synonym হলো Database Object-এর **Shortcut Name** বা **Alias Name**।

---

## Example

Original Table

```text
employee_master
```

Create Synonym

```sql
CREATE SYNONYM employee
FOR employee_master;
```

এখন

আগে

```sql
SELECT *
FROM employee_master;
```

এখন

```sql
SELECT *
FROM employee;
```

---

## Diagram

```text
employee_master
        │
        ▼
 CREATE SYNONYM
        │
        ▼
employee
```

---

# 5. CONSTRAINT

Constraint Data Integrity নিশ্চিত করে।

---

## PRIMARY KEY

```sql
emp_id NUMBER PRIMARY KEY
```

* Unique
* NULL হবে না

---

## NOT NULL

```sql
emp_name VARCHAR2(100) NOT NULL
```

Mandatory Value

---

## UNIQUE

```sql
email VARCHAR2(100) UNIQUE
```

Duplicate Value Allow করবে না।

---

## CHECK

```sql
salary NUMBER CHECK(salary > 0)
```

Invalid Salary Insert করা যাবে না।

---

## DEFAULT

```sql
status CHAR(1) DEFAULT 'A'
```

Value না দিলে Automatically `'A'` হবে।

---

# Banking Project Example

ধরি একটি Bank নতুন **Loan Module** তৈরি করেছে।

---

## Step 1: Loan Table

```sql
CREATE TABLE loan_account
(
    loan_id NUMBER PRIMARY KEY,
    customer_name VARCHAR2(100),
    amount NUMBER(15,2)
);
```

---

## Step 2: Sequence

```sql
CREATE SEQUENCE loan_seq
START WITH 1001
INCREMENT BY 1;
```

---

## Step 3: Insert

```sql
INSERT INTO loan_account
VALUES
(
loan_seq.NEXTVAL,
'Rahim',
500000
);
```

---

## Step 4: View

```sql
CREATE VIEW loan_view AS
SELECT loan_id,
       customer_name
FROM loan_account;
```

---

## Step 5: Index

```sql
CREATE INDEX idx_customer
ON loan_account(customer_name);
```

---

## Step 6: Synonym

```sql
CREATE SYNONYM loan
FOR loan_account;
```

এখন

```sql
SELECT *
FROM loan;
```

---

# Complete Flow

```text
Table
 │
 ├────────────┐
 │            │
 ▼            ▼
View      Sequence
 │            │
 ▼            ▼
Read      Auto ID
 │
 ▼
Index
 │
 ▼
Fast Search
 │
 ▼
Synonym
 │
 ▼
Shortcut Name
```

---

# Oracle Schema Objects Summary

| Object     | Purpose                   |
| ---------- | ------------------------- |
| Table      | Store Data                |
| View       | Virtual Table             |
| Sequence   | Auto Increment Number     |
| Index      | Improve Query Performance |
| Synonym    | Alias Name                |
| Constraint | Validate Data             |

---

# Interview Questions

### 1. Schema কী?

একজন User-এর অধীনে থাকা সব Database Object-এর Collection-কে Schema বলে।

---

### 2. View কী?

Virtual Table যা Data Store করে না, বরং Table থেকে Data দেখায়।

---

### 3. Sequence কেন ব্যবহার করা হয়?

Automatically Unique Number Generate করার জন্য।

---

### 4. NEXTVAL এবং CURRVAL-এর পার্থক্য কী?

| NEXTVAL                 | CURRVAL                                 |
| ----------------------- | --------------------------------------- |
| নতুন Value Generate করে | সর্বশেষ Generate হওয়া Value Return করে |

---

### 5. Index কেন ব্যবহার করা হয়?

Query Performance Improve করার জন্য।

---

### 6. Synonym কী?

Database Object-এর Shortcut Name বা Alias।

---

### 7. Constraint কেন ব্যবহার করা হয়?

Data Integrity এবং Validation নিশ্চিত করার জন্য।

---

# Oracle vs Java

| Oracle   | Java                   |
| -------- | ---------------------- |
| Table    | Class/Object Data      |
| View     | DTO / Projection       |
| Sequence | Auto Increment Counter |
| Index    | HashMap Lookup-এর মতো  |
| Synonym  | Alias Variable         |

---

# Quick Revision

```text
Schema Objects
│
├── Table
├── View
├── Sequence
├── Index
├── Synonym
├── Constraint
├── Procedure
├── Function
├── Package
└── Trigger
```

---

# Key Points

* **Schema** = একটি User-এর সকল Database Object-এর Collection।
* **View** = Virtual Table, Data Store করে না।
* **Sequence** = Auto Increment Number Generate করে (`NEXTVAL`, `CURRVAL`)।
* **Index** = Query Performance Improve করে।
* **Synonym** = Database Object-এর Alias বা Shortcut Name।
* **Constraint** = Data Integrity নিশ্চিত করে (`PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `CHECK`, `NOT NULL`, `DEFAULT`)।
* Banking Project-এ এই Schema Objects প্রায় প্রতিটি Module-এ ব্যবহৃত হয়।
