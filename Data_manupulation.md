### Oracle SQL - Manipulating Data (DML) বাংলায়
```
Data Manipulation কী?

Data Manipulation বলতে Database Table-এর ভিতরের Data যোগ করা, পরিবর্তন করা এবং মুছে ফেলা বোঝায়।

Oracle-এ Data Manipulation করার জন্য DML (Data Manipulation Language) ব্যবহার করা হয়।

প্রধান DML Commands:

Command	কাজ
INSERT	নতুন Data যোগ করা
UPDATE	Existing Data পরিবর্তন করা
DELETE	Data মুছে ফেলা
MERGE	Insert + Update একসাথে করা
Example Table

ধরি আমাদের একটি Employee Table আছে।

EMPLOYEE
EMP_ID	EMP_NAME	SALARY	DEPT_ID
101	Rahim	40000	10
102	Karim	60000	20
103	Sakib	35000	10
1. INSERT Statement
কী?

INSERT ব্যবহার করে Table-এ নতুন Row যোগ করা হয়।

Syntax
INSERT INTO table_name
(column1, column2, column3)
VALUES
(value1, value2, value3);
Example

নতুন Employee যোগ করতে:

INSERT INTO employee
(
    emp_id,
    emp_name,
    salary,
    dept_id
)
VALUES
(
    104,
    'Hasan',
    70000,
    30
);
Insert হওয়ার পরে Table
EMP_ID	EMP_NAME	SALARY	DEPT_ID
101	Rahim	40000	10
102	Karim	60000	20
103	Sakib	35000	10
104	Hasan	70000	30
Insert Without Column Name
INSERT INTO employee
VALUES
(
    105,
    'Nabil',
    50000,
    20
);

এখানে Value-এর Order অবশ্যই Table Column Order অনুযায়ী হতে হবে।

Insert Multiple Rows

Oracle-এ:

INSERT ALL

INTO employee VALUES(106,'Rafi',45000,10)
INTO employee VALUES(107,'Jony',55000,20)

SELECT * FROM dual;
2. UPDATE Statement
কী?

UPDATE ব্যবহার করে Existing Data পরিবর্তন করা হয়।

Syntax
UPDATE table_name
SET column_name = value
WHERE condition;
Example 1

Rahim-এর Salary পরিবর্তন:

UPDATE employee
SET salary = 50000
WHERE emp_id = 101;

Before:

EMP_ID	NAME	SALARY
101	Rahim	40000

After:

EMP_ID	NAME	SALARY
101	Rahim	50000
Multiple Column Update
UPDATE employee
SET 
salary = 60000,
dept_id = 20
WHERE emp_id = 101;
Important

যদি WHERE না দেন:

UPDATE employee
SET salary = 50000;

তাহলে সব Employee-এর Salary 50000 হয়ে যাবে।

3. DELETE Statement
কী?

DELETE ব্যবহার করে Table থেকে Row মুছে ফেলা হয়।

Syntax
DELETE FROM table_name
WHERE condition;
Example

Employee 103 Delete:

DELETE FROM employee
WHERE emp_id = 103;
Before
EMP_ID	NAME
101	Rahim
102	Karim
103	Sakib
After
EMP_ID	NAME
101	Rahim
102	Karim
Delete Without WHERE
DELETE FROM employee;

এতে Table-এর সব Row Delete হয়ে যাবে।

DELETE vs TRUNCATE vs DROP
DELETE	TRUNCATE	DROP
Row Delete করে	সব Row Remove করে	পুরো Table Remove করে
WHERE ব্যবহার করা যায়	WHERE নেই	Table Structure-ও Delete
Rollback করা যায়	সাধারণত Rollback করা যায় না	Rollback করা যায় না
4. MERGE Statement
কী?

MERGE হলো একটি Combination:

UPDATE + INSERT

যদি Data থাকে → Update করবে।

যদি Data না থাকে → Insert করবে।

একে বলা হয়:

UPSERT

Example

ধরি Source Table:

NEW_EMPLOYEE
EMP_ID	NAME	SALARY
101	Rahim	55000
108	Hasan	60000

Target Table:

EMPLOYEE
EMP_ID	NAME	SALARY
101	Rahim	40000
102	Karim	60000
MERGE Query
MERGE INTO employee e
USING new_employee n
ON (e.emp_id = n.emp_id)

WHEN MATCHED THEN

UPDATE SET
e.salary = n.salary

WHEN NOT MATCHED THEN

INSERT
(
    emp_id,
    emp_name,
    salary
)
VALUES
(
    n.emp_id,
    n.name,
    n.salary
);
Result

Employee 101:

আগে:

Rahim 40000

Update হবে:

Rahim 55000

Employee 108:

নতুন হওয়ায় Insert হবে।

Transaction Control

DML Command-এর পরে Transaction Control ব্যবহার করা হয়।

COMMIT

Permanent Save করে।

COMMIT;
ROLLBACK

আগের অবস্থায় ফিরিয়ে দেয়।

ROLLBACK;
SAVEPOINT

একটি নির্দিষ্ট Point তৈরি করে।

SAVEPOINT before_update;

Rollback:

ROLLBACK TO before_update;
Example Transaction
UPDATE employee
SET salary = 80000
WHERE emp_id = 101;

SAVEPOINT salary_update;

DELETE FROM employee
WHERE emp_id = 102;

ROLLBACK TO salary_update;

COMMIT;

Flow:

Update Salary
     |
Savepoint
     |
Delete Employee
     |
Rollback
     |
Delete Cancel
     |
Commit Update
Banking Project Example
Account Balance Update
UPDATE ACNTBAL
SET ACNTBAL_BALANCE = ACNTBAL_BALANCE + 5000
WHERE ACNTBAL_ACCOUNT_NO = 1001;

Meaning:

Account 1001-এর Balance 5000 বাড়বে।

New Account Insert
INSERT INTO ACNTS
(
    ACNTS_INTERNAL_ACNUM,
    ACNTS_NAME
)
VALUES
(
    1005,
    'Rahim'
);
DML Execution Flow
INSERT
   |
   ↓
UPDATE
   |
   ↓
DELETE
   |
   ↓
COMMIT / ROLLBACK
Interview Questions
1. DML কী?

Answer:

DML (Data Manipulation Language) হলো SQL-এর অংশ যা Database-এর Data Insert, Update এবং Delete করার জন্য ব্যবহৃত হয়।

2. DML Commands কী কী?
INSERT
UPDATE
DELETE
MERGE
3. DELETE এবং TRUNCATE-এর পার্থক্য?
DELETE	TRUNCATE
DML Command	DDL Command
WHERE ব্যবহার করা যায়	WHERE ব্যবহার করা যায় না
Rollback করা যায়	সাধারণত Rollback করা যায় না
4. MERGE কী?

Answer:

MERGE হলো একটি SQL Statement যা একই সাথে INSERT এবং UPDATE করতে পারে। একে UPSERT বলা হয়।
```
# Oracle SQL - SAVEPOINT

# SAVEPOINT কী?

**SAVEPOINT** হলো একটি **Transaction-এর ভিতরে তৈরি করা Checkpoint বা Bookmark**।

যদি Transaction চলাকালীন কোনো Error হয়, তাহলে পুরো Transaction Rollback না করে **শুধুমাত্র নির্দিষ্ট SAVEPOINT পর্যন্ত Rollback** করা যায়।

> **সহজভাবে মনে রাখুন:**
>
> **SAVEPOINT = Transaction-এর Bookmark**

---

# কেন SAVEPOINT ব্যবহার করা হয়?

ধরুন আপনি একটি Transaction-এ একাধিক কাজ করছেন।

* Employee-এর Salary Update
* Customer-এর Balance Update
* Account Delete
* Report Insert

যদি শেষের কাজটিতে Error হয়, তাহলে পুরো Transaction Rollback করতে না চাইলে **SAVEPOINT** ব্যবহার করা হয়।

---

# Syntax

## SAVEPOINT তৈরি

```sql
SAVEPOINT savepoint_name;
```

## SAVEPOINT পর্যন্ত Rollback

```sql
ROLLBACK TO savepoint_name;
```

## পুরো Transaction Rollback

```sql
ROLLBACK;
```

---

# Example 1: SAVEPOINT ছাড়া

Employee Table

| EMP_ID | EMP_NAME | SALARY |
| ------ | -------- | -----: |
| 101    | Rahim    |  40000 |
| 102    | Karim    |  50000 |

### Step 1: Salary Update

```sql
UPDATE employee
SET salary = 45000
WHERE emp_id = 101;
```

### Step 2: Employee Delete

```sql
DELETE FROM employee
WHERE emp_id = 102;
```

ভুল হয়ে গেল।

এখন যদি লিখি:

```sql
ROLLBACK;
```

### Result

সব পরিবর্তন বাতিল হবে।

* ❌ Salary Update বাতিল
* ❌ Delete বাতিল

---

# Example 2: SAVEPOINT ব্যবহার করে

### Step 1: Salary Update

```sql
UPDATE employee
SET salary = 45000
WHERE emp_id = 101;
```

### Step 2: SAVEPOINT তৈরি

```sql
SAVEPOINT sp_salary;
```

### Step 3: Employee Delete

```sql
DELETE FROM employee
WHERE emp_id = 102;
```

### Step 4: Delete ভুল হয়েছে

```sql
ROLLBACK TO sp_salary;
```

### Final Result

| Operation     | Status      |
| ------------- | ----------- |
| Salary Update | ✅ থাকবে     |
| Delete        | ❌ বাতিল হবে |

অর্থাৎ,

* Rahim-এর Salary Update থাকবে।
* Karim Delete হবে না।

---

# Execution Flow

```text
START TRANSACTION
        │
        ▼
UPDATE Salary
        │
        ▼
SAVEPOINT sp_salary
        │
        ▼
DELETE Employee
        │
        ▼
ROLLBACK TO sp_salary
        │
        ▼
DELETE Cancel
        │
        ▼
UPDATE Remains
```

---

# Multiple SAVEPOINT

একটি Transaction-এ একাধিক SAVEPOINT তৈরি করা যায়।

```sql
UPDATE employee
SET salary = 50000
WHERE emp_id = 101;

SAVEPOINT sp1;

UPDATE employee
SET salary = 60000
WHERE emp_id = 102;

SAVEPOINT sp2;

DELETE FROM employee
WHERE emp_id = 103;
```

## Rollback to SP2

```sql
ROLLBACK TO sp2;
```

**Result:**

* DELETE বাতিল হবে।
* দ্বিতীয় UPDATE থাকবে।

---

## Rollback to SP1

```sql
ROLLBACK TO sp1;
```

**Result:**

* দ্বিতীয় UPDATE বাতিল হবে।
* DELETE বাতিল হবে।
* প্রথম UPDATE থাকবে।

---

# COMMIT-এর পরে কী হয়?

```sql
UPDATE employee
SET salary = 45000
WHERE emp_id = 101;

SAVEPOINT sp1;

COMMIT;
```

COMMIT করার পরে

* Transaction শেষ হয়ে যায়।
* সব SAVEPOINT মুছে যায়।

এখন

```sql
ROLLBACK TO sp1;
```

Error হবে, কারণ `sp1` আর নেই।

---

# Banking Project Example

ধরি একটি টাকা Transfer করার Transaction চলছে।

### Step 1: Sender-এর Account থেকে টাকা কাটা

```sql
UPDATE account
SET balance = balance - 5000
WHERE account_no = 1001;
```

### Step 2: SAVEPOINT

```sql
SAVEPOINT after_debit;
```

### Step 3: Receiver-এর Account-এ টাকা যোগ করা

```sql
UPDATE account
SET balance = balance + 5000
WHERE account_no = 2001;
```

যদি Step 3-এ কোনো Error হয়:

```sql
ROLLBACK TO after_debit;
```

> **নোট:** বাস্তব Banking Application-এ সাধারণত পুরো Transfer সফল না হলে সম্পূর্ণ Transaction `ROLLBACK` করা হয়, যাতে Sender-এর টাকা কাটা থাকলেও Receiver-এর কাছে না পৌঁছানোর মতো সমস্যা না হয়। SAVEPOINT সাধারণত জটিল Transaction-এর নির্দিষ্ট অংশ নিয়ন্ত্রণে ব্যবহৃত হয়।

---

# SAVEPOINT vs ROLLBACK vs COMMIT

| Command                 | কাজ                                      |
| ----------------------- | ---------------------------------------- |
| `SAVEPOINT`             | Transaction-এর মধ্যে Checkpoint তৈরি করে |
| `ROLLBACK`              | পুরো Transaction বাতিল করে               |
| `ROLLBACK TO SAVEPOINT` | নির্দিষ্ট SAVEPOINT পর্যন্ত ফিরে যায়    |
| `COMMIT`                | সব পরিবর্তন স্থায়ীভাবে সংরক্ষণ করে      |

---

# SAVEPOINT ব্যবহারের সুবিধা

* পুরো Transaction Rollback করতে হয় না।
* নির্দিষ্ট Point পর্যন্ত ফিরে যাওয়া যায়।
* বড় Transaction নিয়ন্ত্রণ করা সহজ হয়।
* Error Recovery সহজ হয়।
* Complex Business Logic বাস্তবায়নে সাহায্য করে।

---

# Interview Questions

### 1. SAVEPOINT কী?

**Answer:**

SAVEPOINT হলো Transaction-এর মধ্যে একটি Checkpoint, যেখানে প্রয়োজনে ফিরে যাওয়া যায়।

---

### 2. `ROLLBACK` এবং `ROLLBACK TO SAVEPOINT`-এর পার্থক্য কী?

| ROLLBACK                   | ROLLBACK TO SAVEPOINT                          |
| -------------------------- | ---------------------------------------------- |
| পুরো Transaction বাতিল করে | নির্দিষ্ট SAVEPOINT-এর পরের পরিবর্তন বাতিল করে |

---

### 3. COMMIT-এর পরে SAVEPOINT ব্যবহার করা যায়?

**Answer:**

না। COMMIT করার পরে সব SAVEPOINT মুছে যায়।

---

### 4. একটি Transaction-এ একাধিক SAVEPOINT তৈরি করা যায়?

**Answer:**

হ্যাঁ। একটি Transaction-এ যত প্রয়োজন তত SAVEPOINT তৈরি করা যায়।

---

# Quick Revision

```text
SAVEPOINT
      │
      ▼
Transaction-এর Bookmark

ROLLBACK
      │
      ▼
সব পরিবর্তন বাতিল

ROLLBACK TO SAVEPOINT
      │
      ▼
শুধু SAVEPOINT-এর পরের পরিবর্তন বাতিল

COMMIT
      │
      ▼
সব পরিবর্তন স্থায়ীভাবে Save
```

---

# Key Points

* `SAVEPOINT` হলো Transaction-এর মধ্যে একটি **Checkpoint**।
* `ROLLBACK TO SAVEPOINT` করলে শুধু নির্দিষ্ট SAVEPOINT-এর পরের পরিবর্তন বাতিল হয়।
* `ROLLBACK` করলে পুরো Transaction বাতিল হয়।
* `COMMIT` করার পরে সব SAVEPOINT মুছে যায়।
* বড় Transaction এবং Banking Project-এ SAVEPOINT খুবই গুরুত্বপূর্ণ।

