# Oracle SQL - Mostly Used Analytical Functions

## Analytical Function কী?

**Analytical Function (Window Function)** হলো এমন Function যা **একাধিক Row-এর উপর Calculation করে**, কিন্তু **Result Set-এর প্রতিটি Row আলাদা রাখে**।

অর্থাৎ,

* `GROUP BY` → অনেক Row-কে একটি Row-তে পরিণত করে।
* **Analytical Function** → Row Merge না করে প্রতিটি Row-এর সাথে Calculation Result দেখায়।

---

# Analytical Function Syntax

```sql
Function_Name(expression)
OVER
(
    PARTITION BY column_name
    ORDER BY column_name
)
```

### Keywords

* **OVER()** → Analytical Function-এর Window নির্ধারণ করে।
* **PARTITION BY** → Data-কে Group করে, কিন্তু Row Merge করে না।
* **ORDER BY** → Window-এর ভিতরে Row-এর Order নির্ধারণ করে।

---

# Sample Table

```sql
CREATE TABLE employee
(
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    department VARCHAR2(20),
    salary NUMBER
);
```

```sql
INSERT INTO employee VALUES (1,'Rahim','IT',50000);
INSERT INTO employee VALUES (2,'Karim','IT',70000);
INSERT INTO employee VALUES (3,'Hasan','HR',40000);
INSERT INTO employee VALUES (4,'Sakib','HR',60000);
INSERT INTO employee VALUES (5,'Jamal','IT',80000);

COMMIT;
```

### Sample Data

| EMP_ID | EMP_NAME | DEPARTMENT | SALARY |
| ------ | -------- | ---------- | ------ |
| 1      | Rahim    | IT         | 50000  |
| 2      | Karim    | IT         | 70000  |
| 3      | Hasan    | HR         | 40000  |
| 4      | Sakib    | HR         | 60000  |
| 5      | Jamal    | IT         | 80000  |

---

# 1. ROW_NUMBER()

প্রতিটি Row-কে Unique Serial Number দেয়।

```sql
SELECT emp_name,
       salary,
       ROW_NUMBER() OVER(ORDER BY salary DESC) AS row_no
FROM employee;
```

### Output

| Employee | Salary | Row Number |
| -------- | ------ | ---------- |
| Jamal    | 80000  | 1          |
| Karim    | 70000  | 2          |
| Sakib    | 60000  | 3          |
| Rahim    | 50000  | 4          |
| Hasan    | 40000  | 5          |

### ব্যবহার

* Pagination
* Top-N Query
* Duplicate Remove

---

# 2. RANK()

একই Value হলে একই Rank দেয় এবং পরবর্তী Rank Skip করে।

```sql
SELECT emp_name,
       salary,
       RANK() OVER(ORDER BY salary DESC) AS rank_no
FROM employee;
```

ধরি Salary

| Salary |
| ------ |
| 80000  |
| 70000  |
| 70000  |
| 50000  |

### Output

| Salary | Rank |
| ------ | ---- |
| 80000  | 1    |
| 70000  | 2    |
| 70000  | 2    |
| 50000  | 4    |

> এখানে **Rank 3 Skip হয়েছে।**

---

# 3. DENSE_RANK()

একই Value হলে একই Rank দেয়, কিন্তু Rank Skip করে না।

```sql
SELECT emp_name,
       salary,
       DENSE_RANK() OVER(ORDER BY salary DESC) AS dense_rank
FROM employee;
```

### Output

| Salary | Dense Rank |
| ------ | ---------- |
| 80000  | 1          |
| 70000  | 2          |
| 70000  | 2          |
| 50000  | 3          |

---

# RANK() vs DENSE_RANK()

| Salary | RANK | DENSE_RANK |
| ------ | ---- | ---------- |
| 80000  | 1    | 1          |
| 70000  | 2    | 2          |
| 70000  | 2    | 2          |
| 50000  | 4    | 3          |

---

# 4. SUM() OVER()

সব Employee-এর Total Salary বের করে।

```sql
SELECT emp_name,
       salary,
       SUM(salary) OVER() AS total_salary
FROM employee;
```

### Output

| Employee | Salary | Total Salary |
| -------- | ------ | ------------ |
| Rahim    | 50000  | 300000       |
| Karim    | 70000  | 300000       |
| Hasan    | 40000  | 300000       |
| Sakib    | 60000  | 300000       |
| Jamal    | 80000  | 300000       |

---

# Running Total

```sql
SELECT emp_name,
       salary,
       SUM(salary)
       OVER(ORDER BY salary) AS running_total
FROM employee;
```

### Output

| Salary | Running Total |
| ------ | ------------- |
| 40000  | 40000         |
| 50000  | 90000         |
| 60000  | 150000        |
| 70000  | 220000        |
| 80000  | 300000        |

---

# 5. AVG()

Department অনুযায়ী Average Salary বের করে।

```sql
SELECT emp_name,
       department,
       salary,
       AVG(salary)
       OVER(PARTITION BY department) AS avg_salary
FROM employee;
```

### Output

| Department | Salary | Average Salary |
| ---------- | ------ | -------------- |
| IT         | 50000  | 66666          |
| IT         | 70000  | 66666          |
| IT         | 80000  | 66666          |
| HR         | 40000  | 50000          |
| HR         | 60000  | 50000          |

---

# 6. COUNT()

Department অনুযায়ী Employee সংখ্যা বের করে।

```sql
SELECT emp_name,
       department,
       COUNT(*)
       OVER(PARTITION BY department) AS total_employee
FROM employee;
```

### Output

| Department | Employee Count |
| ---------- | -------------- |
| IT         | 3              |
| HR         | 2              |

---

# 7. MAX()

Department অনুযায়ী সর্বোচ্চ Salary বের করে।

```sql
SELECT emp_name,
       department,
       salary,
       MAX(salary)
       OVER(PARTITION BY department) AS max_salary
FROM employee;
```

---

# 8. MIN()

Department অনুযায়ী সর্বনিম্ন Salary বের করে।

```sql
SELECT emp_name,
       department,
       salary,
       MIN(salary)
       OVER(PARTITION BY department) AS min_salary
FROM employee;
```

---

# 9. LAG()

আগের Row-এর Value দেখায়।

```sql
SELECT emp_name,
       salary,
       LAG(salary)
       OVER(ORDER BY salary) AS previous_salary
FROM employee;
```

### Output

| Salary | Previous Salary |
| ------ | --------------- |
| 40000  | NULL            |
| 50000  | 40000           |
| 60000  | 50000           |
| 70000  | 60000           |
| 80000  | 70000           |

---

# 10. LEAD()

পরের Row-এর Value দেখায়।

```sql
SELECT emp_name,
       salary,
       LEAD(salary)
       OVER(ORDER BY salary) AS next_salary
FROM employee;
```

### Output

| Salary | Next Salary |
| ------ | ----------- |
| 40000  | 50000       |
| 50000  | 60000       |
| 60000  | 70000       |
| 70000  | 80000       |
| 80000  | NULL        |

---

# 11. FIRST_VALUE()

প্রথম Row-এর Value দেখায়।

```sql
SELECT emp_name,
       salary,
       FIRST_VALUE(salary)
       OVER(ORDER BY salary DESC) AS highest_salary
FROM employee;
```

সব Row-এর জন্য Highest Salary Return করবে।

---

# 12. LAST_VALUE()

শেষ Row-এর Value দেখায়।

```sql
SELECT emp_name,
       salary,
       LAST_VALUE(salary)
       OVER
       (
           ORDER BY salary
           ROWS BETWEEN UNBOUNDED PRECEDING
           AND UNBOUNDED FOLLOWING
       ) AS last_salary
FROM employee;
```

> **Note:** `LAST_VALUE()` ব্যবহার করার সময় Window Frame (`ROWS BETWEEN ...`) ঠিকভাবে উল্লেখ করা গুরুত্বপূর্ণ।

---

# PARTITION BY Example

Department অনুযায়ী Ranking

```sql
SELECT emp_name,
       department,
       salary,
       ROW_NUMBER()
       OVER
       (
           PARTITION BY department
           ORDER BY salary DESC
       ) AS dept_rank
FROM employee;
```

### Output

| Department | Employee | Rank |
| ---------- | -------- | ---- |
| IT         | Jamal    | 1    |
| IT         | Karim    | 2    |
| IT         | Rahim    | 3    |
| HR         | Sakib    | 1    |
| HR         | Hasan    | 2    |

---

# Banking Project Example

ধরি Transaction Table

| Account | Date   | Amount |
| ------- | ------ | ------ |
| 1001    | 01-Jan | 1000   |
| 1001    | 02-Jan | 500    |
| 1001    | 03-Jan | 700    |

Running Balance বের করা

```sql
SELECT account_no,
       tran_date,
       amount,
       SUM(amount)
       OVER
       (
           PARTITION BY account_no
           ORDER BY tran_date
       ) AS running_balance
FROM transactions;
```

### Output

| Date   | Amount | Running Balance |
| ------ | ------ | --------------- |
| 01-Jan | 1000   | 1000            |
| 02-Jan | 500    | 1500            |
| 03-Jan | 700    | 2200            |

---

# GROUP BY vs Analytical Function

| GROUP BY             | Analytical Function             |
| -------------------- | ------------------------------- |
| Row Merge করে        | Row Merge করে না                |
| Summary Result দেয়  | প্রতিটি Row-এর সাথে Result দেয় |
| GROUP BY ব্যবহার করে | OVER() ব্যবহার করে              |
| Detail Row থাকে না   | Detail Row থাকে                 |

---

# Interview Questions

### 1. Analytical Function কী?

Multiple Row-এর উপর Calculation করে, কিন্তু Result Set-এর Row Merge করে না।

---

### 2. OVER() কেন ব্যবহার করা হয়?

Analytical Function-এর Calculation Window নির্ধারণ করার জন্য।

---

### 3. PARTITION BY কী?

Data-কে Group করে Analytical Calculation চালায়, কিন্তু Row Merge করে না।

---

### 4. ROW_NUMBER() এবং RANK() এর পার্থক্য কী?

* **ROW_NUMBER()** → প্রতিটি Row-কে Unique Number দেয়।
* **RANK()** → একই Value হলে একই Rank দেয় এবং পরবর্তী Rank Skip করে।

---

### 5. RANK() এবং DENSE_RANK() এর পার্থক্য কী?

* **RANK()** → Rank Skip করে।
* **DENSE_RANK()** → Rank Skip করে না।

---

# Quick Revision

```text
Analytical Functions

ROW_NUMBER()   → Unique Row Number
RANK()         → Rank (Skip)
DENSE_RANK()   → Rank (No Skip)
SUM() OVER()   → Total / Running Total
AVG() OVER()   → Average
COUNT() OVER() → Count
MAX() OVER()   → Maximum
MIN() OVER()   → Minimum
LAG()          → Previous Row Value
LEAD()         → Next Row Value
FIRST_VALUE()  → First Value
LAST_VALUE()   → Last Value

Important Keywords

OVER()         → Window Definition
PARTITION BY   → Group Without Merging Rows
ORDER BY       → Sorting Within Window
```

# Key Points

* Analytical Function-কে **Window Function**-ও বলা হয়।
* `OVER()` ছাড়া Analytical Function ব্যবহার করা যায় না।
* `PARTITION BY` Data-কে Group করে কিন্তু Row Merge করে না।
* `ROW_NUMBER()` Pagination ও Top-N Query-তে খুব বেশি ব্যবহৃত হয়।
* `RANK()` এবং `DENSE_RANK()` Interview-এর সবচেয়ে গুরুত্বপূর্ণ Topic।
* Banking Project-এ Running Balance, Top Customer, Salary Analysis, Statement, Ledger Report, MIS Report এবং Ranking Report তৈরিতে Analytical Function ব্যাপকভাবে ব্যবহৃত হয়।
