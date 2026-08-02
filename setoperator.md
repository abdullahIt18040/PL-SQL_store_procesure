# Oracle SQL - Using the Set Operators

## Set Operator কী?

**Set Operator** ব্যবহার করে দুই বা তার বেশি `SELECT` Query-এর Result একসাথে Combine করা হয়।

অর্থাৎ,

> একাধিক Query-এর Result Set-এর মধ্যে Operation করার জন্য Set Operator ব্যবহার করা হয়।

---

# Oracle Set Operators

Oracle-এ প্রধান ৪টি Set Operator আছে:

| Operator    | কাজ                                                                 |
| ----------- | ------------------------------------------------------------------- |
| `UNION`     | দুই Query-এর Result Combine করে এবং Duplicate Remove করে            |
| `UNION ALL` | দুই Query-এর Result Combine করে এবং Duplicate রাখে                  |
| `INTERSECT` | দুই Query-এর Common Data Return করে                                 |
| `MINUS`     | প্রথম Query-তে আছে কিন্তু দ্বিতীয় Query-তে নেই এমন Data Return করে |

---

# Set Operator Rules

Set Operator ব্যবহার করার সময় কিছু Rule মানতে হয়:

## 1. Column সংখ্যা একই হতে হবে

Correct:

```sql
SELECT emp_id, emp_name
FROM employee

UNION

SELECT cust_id, cust_name
FROM customer;
```

Wrong:

```sql
SELECT emp_id, emp_name
FROM employee

UNION

SELECT cust_id
FROM customer;
```

---

## 2. Column Data Type Compatible হতে হবে

Correct:

```text
NUMBER  ↔ NUMBER

VARCHAR2 ↔ VARCHAR2
```

Wrong:

```text
NUMBER ↔ VARCHAR2
```

---

## 3. Column Order একই হতে হবে

Example:

```sql
SELECT emp_id, emp_name
FROM employee

UNION

SELECT cust_id, cust_name
FROM customer;
```

প্রথম Column-এর সাথে প্রথম Column এবং দ্বিতীয় Column-এর সাথে দ্বিতীয় Column Match হবে।

---

# Example Tables

## EMPLOYEE_2025

| EMP_ID | NAME  |
| ------ | ----- |
| 101    | Rahim |
| 102    | Karim |
| 103    | Sakib |

---

## EMPLOYEE_2026

| EMP_ID | NAME  |
| ------ | ----- |
| 103    | Sakib |
| 104    | Hasan |
| 105    | Nabil |

---

# 1. UNION Operator

## কী?

`UNION` দুইটি Query-এর Result Combine করে এবং Duplicate Row Remove করে।

---

## Syntax

```sql
SELECT column_name
FROM table1

UNION

SELECT column_name
FROM table2;
```

---

## Example

```sql
SELECT name
FROM employee_2025

UNION

SELECT name
FROM employee_2026;
```

---

## Result

| NAME  |
| ----- |
| Rahim |
| Karim |
| Sakib |
| Hasan |
| Nabil |

এখানে `Sakib` দুই Table-এ থাকলেও একবার দেখাবে।

---

# 2. UNION ALL Operator

## কী?

`UNION ALL` দুই Query-এর Result Combine করে কিন্তু Duplicate Remove করে না।

---

## Example

```sql
SELECT name
FROM employee_2025

UNION ALL

SELECT name
FROM employee_2026;
```

---

## Result

| NAME  |
| ----- |
| Rahim |
| Karim |
| Sakib |
| Sakib |
| Hasan |
| Nabil |

এখানে Duplicate রাখা হয়েছে।

---

# UNION vs UNION ALL

| UNION                | UNION ALL      |
| -------------------- | -------------- |
| Duplicate Remove করে | Duplicate রাখে |
| তুলনামূলক Slow       | তুলনামূলক Fast |
| Sorting করে          | Sorting করে না |

---

# 3. INTERSECT Operator

## কী?

`INTERSECT` দুই Query-এর Common Data Return করে।

---

## Example

```sql
SELECT name
FROM employee_2025

INTERSECT

SELECT name
FROM employee_2026;
```

---

## Result

| NAME  |
| ----- |
| Sakib |

কারণ দুই Table-এ শুধুমাত্র Sakib Common।

---

# 4. MINUS Operator

## কী?

`MINUS` প্রথম Query-এর Result থেকে দ্বিতীয় Query-এর Matching Data বাদ দেয়।

---

## Example

```sql
SELECT name
FROM employee_2025

MINUS

SELECT name
FROM employee_2026;
```

---

## Result

| NAME  |
| ----- |
| Rahim |
| Karim |

কারণ:

Employee_2025:

```text
Rahim
Karim
Sakib
```

Employee_2026:

```text
Sakib
Hasan
Nabil
```

বাদ দেওয়ার পর:

```text
Rahim
Karim
```

---

# Set Operator Diagram

```text
             Query 1
                |
                |
        Set Operator
                |
                |
             Query 2


UNION
 |
 Combine + Remove Duplicate


UNION ALL
 |
 Combine + Keep Duplicate


INTERSECT
 |
 Common Data


MINUS
 |
 Query 1 - Query 2
```

---

# ORDER BY with Set Operator

`ORDER BY` সবসময় Set Operator-এর শেষে ব্যবহার করতে হয়।

Correct:

```sql
SELECT name
FROM employee_2025

UNION

SELECT name
FROM employee_2026

ORDER BY name;
```

---

# Banking Project Example

## CURRENT_ACCOUNT

| ACCOUNT_NO |
| ---------- |
| 1001       |
| 1002       |
| 1003       |

---

## SAVING_ACCOUNT

| ACCOUNT_NO |
| ---------- |
| 1003       |
| 1004       |
| 1005       |

---

# সব Account বের করা

```sql
SELECT account_no
FROM current_account

UNION

SELECT account_no
FROM saving_account;
```

Output:

```text
1001
1002
1003
1004
1005
```

---

# Common Account বের করা

```sql
SELECT account_no
FROM current_account

INTERSECT

SELECT account_no
FROM saving_account;
```

Output:

```text
1003
```

---

# Current Account-এ আছে কিন্তু Saving Account-এ নেই

```sql
SELECT account_no
FROM current_account

MINUS

SELECT account_no
FROM saving_account;
```

Output:

```text
1001
1002
```

---

# Set Operator Execution Flow

```text
SELECT Query 1

        +

Set Operator

        +

SELECT Query 2

        ↓

Final Result Set
```

---

# Interview Questions

## 1. UNION কী?

**Answer:**

UNION দুই বা তার বেশি SELECT Query-এর Result Combine করে এবং Duplicate Remove করে।

---

## 2. UNION ALL এবং UNION-এর পার্থক্য কী?

| UNION                | UNION ALL      |
| -------------------- | -------------- |
| Duplicate Remove করে | Duplicate রাখে |
| Slow                 | Fast           |

---

## 3. INTERSECT কী করে?

**Answer:**

দুই Query-এর Common Rows Return করে।

---

## 4. MINUS কী করে?

**Answer:**

প্রথম Query-এর Rows থেকে দ্বিতীয় Query-এর Matching Rows বাদ দেয়।

---

## 5. কোন Set Operator Fast?

**Answer:**

`UNION ALL` Fast কারণ এটি Duplicate Check এবং Sorting করে না।

---

# Quick Revision

```text
UNION
 ↓
Combine + Remove Duplicate


UNION ALL
 ↓
Combine + Keep Duplicate


INTERSECT
 ↓
Common Data


MINUS
 ↓
First Query - Second Query
```

---

# Key Points

* Set Operator একাধিক SELECT Query-এর Result Combine করে।
* সব Query-তে Column সংখ্যা এবং Data Type Compatible হতে হবে।
* `UNION` Duplicate Remove করে।
* `UNION ALL` Duplicate রাখে।
* `INTERSECT` Common Data Return করে।
* `MINUS` Difference Data Return করে।
* `ORDER BY` সবসময় শেষে ব্যবহার করতে হয়।
