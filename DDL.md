# Oracle SQL - Using DDL Statements to Create and Manage Tables

# DDL (Data Definition Language) কী?

**DDL (Data Definition Language)** হলো SQL-এর এমন একটি অংশ, যা **Database Object (Table, View, Index, Sequence, Synonym ইত্যাদি)** তৈরি, পরিবর্তন এবং মুছে ফেলার জন্য ব্যবহৃত হয়।

সহজভাবে,

> **DDL = Database Structure তৈরি ও পরিচালনা করার Language।**

---

# DDL Commands

| Command    | কাজ                                    |
| ---------- | -------------------------------------- |
| `CREATE`   | নতুন Object তৈরি করে                   |
| `ALTER`    | Existing Object পরিবর্তন করে           |
| `DROP`     | Object Permanently Delete করে          |
| `TRUNCATE` | Table-এর সব Data Remove করে            |
| `RENAME`   | Object-এর নাম পরিবর্তন করে             |
| `COMMENT`  | Table বা Column-এর Description যোগ করে |

---

# DDL Execution Flow

```text
DDL Statement
      │
      ▼
Database Structure Change
      │
      ▼
Oracle Auto COMMIT
```

> **Note:** Oracle-এ DDL Statement Execute করার পরে **Automatic COMMIT** হয়ে যায়।

---

# 1. CREATE TABLE

## কী?

নতুন Table তৈরি করার জন্য `CREATE TABLE` ব্যবহার করা হয়।

## Syntax

```sql
CREATE TABLE table_name
(
    column_name datatype(size),
    column_name datatype(size)
);
```

---

## Example

```sql
CREATE TABLE employee
(
    emp_id      NUMBER(5),
    emp_name    VARCHAR2(50),
    salary      NUMBER(10,2),
    department  VARCHAR2(30),
    join_date   DATE
);
```

---

## Table Structure

| Column     | Data Type    |
| ---------- | ------------ |
| EMP_ID     | NUMBER(5)    |
| EMP_NAME   | VARCHAR2(50) |
| SALARY     | NUMBER(10,2) |
| DEPARTMENT | VARCHAR2(30) |
| JOIN_DATE  | DATE         |

---

# Describe Table

Table-এর Structure দেখার জন্য

```sql
DESC employee;
```

Output

```text
EMP_ID       NUMBER(5)
EMP_NAME     VARCHAR2(50)
SALARY       NUMBER(10,2)
DEPARTMENT   VARCHAR2(30)
JOIN_DATE    DATE
```

TOAD-এ:

**Right Click Table → Describe**

---

# Oracle Common Data Types

| Data Type  | ব্যবহার                  |
| ---------- | ------------------------ |
| `NUMBER`   | Integer / Decimal Number |
| `VARCHAR2` | Variable Length String   |
| `CHAR`     | Fixed Length String      |
| `DATE`     | Date & Time              |
| `CLOB`     | Large Character Data     |
| `BLOB`     | Binary Data              |

---

# 2. Table Constraints

## Primary Key

```sql
CREATE TABLE employee
(
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50)
);
```

**Features**

* Unique
* NULL হবে না

---

## NOT NULL

```sql
emp_name VARCHAR2(50) NOT NULL
```

Employee Name অবশ্যই দিতে হবে।

---

## UNIQUE

```sql
email VARCHAR2(100) UNIQUE
```

একই Email দুইবার রাখা যাবে না।

---

## CHECK

```sql
salary NUMBER CHECK(salary > 0)
```

Salary অবশ্যই 0-এর বেশি হতে হবে।

---

## DEFAULT

```sql
status CHAR(1) DEFAULT 'A'
```

Value না দিলে Automatically `'A'` হবে।

---

# Complete Example

```sql
CREATE TABLE employee
(
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(100) NOT NULL,
    salary NUMBER(10,2) CHECK(salary>0),
    email VARCHAR2(100) UNIQUE,
    status CHAR(1) DEFAULT 'A',
    join_date DATE
);
```

---

# 3. ALTER TABLE

## কী?

Existing Table-এর Structure পরিবর্তন করার জন্য ব্যবহার করা হয়।

---

## Add Column

```sql
ALTER TABLE employee
ADD phone VARCHAR2(20);
```

---

## Modify Column

```sql
ALTER TABLE employee
MODIFY phone VARCHAR2(30);
```

---

## Rename Column

```sql
ALTER TABLE employee
RENAME COLUMN phone TO mobile;
```

---

## Drop Column

```sql
ALTER TABLE employee
DROP COLUMN mobile;
```

---

# Add Constraint Using ALTER

## Primary Key

```sql
ALTER TABLE employee
ADD CONSTRAINT pk_employee
PRIMARY KEY(emp_id);
```

---

## Unique

```sql
ALTER TABLE employee
ADD CONSTRAINT uk_employee_email
UNIQUE(email);
```

---

## Check

```sql
ALTER TABLE employee
ADD CONSTRAINT chk_salary
CHECK(salary>0);
```

---

# ALTER TABLE Flow

```text
Existing Table
       │
       ▼
ALTER TABLE
       │
       ▼
New Structure
```

---

# 4. RENAME TABLE

```sql
RENAME employee TO employee_master;
```

Before

```text
employee
```

After

```text
employee_master
```

---

# 5. COMMENT

## Table Comment

```sql
COMMENT ON TABLE employee_master
IS 'Employee Information';
```

---

## Column Comment

```sql
COMMENT ON COLUMN employee_master.salary
IS 'Monthly Salary';
```

---

# 6. TRUNCATE TABLE

## কী?

Table-এর Structure রেখে সব Data Delete করে।

```sql
TRUNCATE TABLE employee_master;
```

Before

| EMP_ID | EMP_NAME |
| ------ | -------- |
| 101    | Rahim    |
| 102    | Karim    |

After

| EMP_ID      | EMP_NAME |
| ----------- | -------- |
| *(No Data)* |          |

---

# 7. DROP TABLE

## কী?

Table এবং Table-এর সব Data Permanently Delete করে।

```sql
DROP TABLE employee_master;
```

Result

* Table থাকবে না।
* Data থাকবে না।

---

# DDL Life Cycle

```text
CREATE TABLE
      │
      ▼
INSERT DATA
      │
      ▼
SELECT DATA
      │
      ▼
ALTER TABLE
      │
      ▼
ADD / MODIFY / DROP COLUMN
      │
      ▼
RENAME TABLE
      │
      ▼
TRUNCATE TABLE
      │
      ▼
DROP TABLE
```

---

# Banking Project Example

## Step 1: Loan Table তৈরি

```sql
CREATE TABLE loan_account
(
    loan_id NUMBER PRIMARY KEY,
    customer_id NUMBER NOT NULL,
    loan_amount NUMBER(15,2),
    loan_date DATE,
    status CHAR(1) DEFAULT 'A'
);
```

---

## Step 2: Interest Rate Column যোগ

```sql
ALTER TABLE loan_account
ADD interest_rate NUMBER(5,2);
```

---

## Step 3: Test Data Remove

```sql
TRUNCATE TABLE loan_account;
```

---

## Step 4: Loan Module Remove

```sql
DROP TABLE loan_account;
```

---

# CREATE vs ALTER vs TRUNCATE vs DROP

| Command    | কাজ                                     |
| ---------- | --------------------------------------- |
| `CREATE`   | নতুন Table/Object তৈরি করে              |
| `ALTER`    | Existing Structure পরিবর্তন করে         |
| `TRUNCATE` | সব Data Remove করে, Structure রেখে দেয় |
| `DROP`     | Table এবং Data দুটোই Remove করে         |

---

# DDL vs DML

| DDL                     | DML                        |
| ----------------------- | -------------------------- |
| Structure নিয়ে কাজ করে | Data নিয়ে কাজ করে         |
| CREATE                  | INSERT                     |
| ALTER                   | UPDATE                     |
| DROP                    | DELETE                     |
| TRUNCATE                | MERGE                      |
| Auto COMMIT             | COMMIT / ROLLBACK প্রয়োজন |

---

# Real Banking Scenario

ধরি একটি Bank-এ নতুন **Fixed Deposit (FDR)** Module চালু হয়েছে।

### নতুন Table তৈরি

```sql
CREATE TABLE fdr_account
(
    fdr_id NUMBER PRIMARY KEY,
    customer_id NUMBER,
    amount NUMBER(15,2),
    open_date DATE
);
```

### পরে নতুন Requirement এলো

Maturity Date যোগ করতে হবে।

```sql
ALTER TABLE fdr_account
ADD maturity_date DATE;
```

### Test Data Remove

```sql
TRUNCATE TABLE fdr_account;
```

### Module বাতিল

```sql
DROP TABLE fdr_account;
```

---

# Interview Questions

## 1. DDL কী?

**Answer:**

DDL (Data Definition Language) Database Object-এর Structure তৈরি, পরিবর্তন এবং মুছে ফেলার জন্য ব্যবহৃত হয়।

---

## 2. DDL Commands কী কী?

* CREATE
* ALTER
* DROP
* TRUNCATE
* RENAME
* COMMENT

---

## 3. CREATE এবং ALTER-এর পার্থক্য কী?

| CREATE               | ALTER                        |
| -------------------- | ---------------------------- |
| নতুন Object তৈরি করে | Existing Object পরিবর্তন করে |

---

## 4. DROP এবং TRUNCATE-এর পার্থক্য কী?

| DROP              | TRUNCATE             |
| ----------------- | -------------------- |
| Table Delete করে  | শুধু Data Delete করে |
| Structure থাকে না | Structure থাকে       |

---

## 5. DDL Command-এর পরে COMMIT দিতে হয়?

**Answer:**

না। Oracle DDL Statement Execute হলে **Automatic COMMIT** হয়ে যায়।

---

# Quick Revision

```text
DDL
│
├── CREATE      → নতুন Object তৈরি
├── ALTER       → Structure পরিবর্তন
├── RENAME      → নাম পরিবর্তন
├── COMMENT     → Description যোগ
├── TRUNCATE    → সব Data Remove
└── DROP        → Table Remove

DDL → Auto COMMIT
```

---

# Key Points

* DDL = **Data Definition Language**
* Database-এর **Structure** তৈরি ও পরিচালনার জন্য ব্যবহৃত হয়।
* `CREATE` নতুন Object তৈরি করে।
* `ALTER` Existing Structure পরিবর্তন করে।
* `TRUNCATE` Data Remove করে কিন্তু Structure রেখে দেয়।
* `DROP` Table এবং Data দুটোই Permanently Delete করে।
* Oracle-এ DDL Execute করলে **Automatic COMMIT** হয়।
* DDL মূলত **Table, View, Index, Sequence, Synonym** ইত্যাদি Object পরিচালনার জন্য ব্যবহৃত হয়।
