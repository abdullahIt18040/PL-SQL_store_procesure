# Oracle SQL - Using Subqueries to Solve Queries (বাংলা)

# Subquery কী?

**Subquery** হলো এমন একটি SQL Query, যা **অন্য একটি SQL Query-এর ভিতরে লেখা হয়।**

অর্থাৎ,

> **একটি Query-এর Result ব্যবহার করে আরেকটি Query Execute করাই Subquery।**

Subquery-কে আরও বলা হয়:

* **Inner Query**
* **Nested Query**

---

# কেন Subquery ব্যবহার করা হয়?

যখন একটি Query-এর Result জানা না থাকায় আগে সেটি বের করতে হয়, তারপর সেই Result ব্যবহার করে মূল Query চালাতে হয়, তখন Subquery ব্যবহার করা হয়।

**উদাহরণ:**

* সর্বোচ্চ Salary-এর Employee খুঁজে বের করা।
* Average Salary-এর চেয়ে বেশি Salary যাদের আছে তাদের বের করা।
* নির্দিষ্ট Department-এর Employee বের করা।
* সর্বোচ্চ Balance-এর Account বের করা।

---

# General Syntax

```sql
SELECT column_name
FROM table_name
WHERE column_name operator
(
    SELECT column_name
    FROM table_name
);
```

---

# Example Table

## EMPLOYEE

| EMP_ID | EMP_NAME | SALARY | DEPT_ID |
| ------ | -------- | ------ | ------- |
| 101    | Rahim    | 40000  | 10      |
| 102    | Karim    | 60000  | 20      |
| 103    | Sakib    | 35000  | 10      |
| 104    | Hasan    | 70000  | 30      |

---

# Example 1: সর্বোচ্চ Salary-এর Employee বের করা

## Query

```sql
SELECT *
FROM employee
WHERE salary =
(
    SELECT MAX(salary)
    FROM employee
);
```

### Step 1: Inner Query Execute হবে

```sql
SELECT MAX(salary)
FROM employee;
```

Result:

```text
70000
```

### Step 2: Outer Query Execute হবে

Oracle ভিতরে Query-টি এভাবে Execute করবে:

```sql
SELECT *
FROM employee
WHERE salary = 70000;
```

### Output

| EMP_ID | EMP_NAME | SALARY |
| ------ | -------- | ------ |
| 104    | Hasan    | 70000  |

---

# Example 2: Average Salary-এর বেশি Salary যাদের আছে

## Query

```sql
SELECT
    emp_name,
    salary
FROM employee
WHERE salary >
(
    SELECT AVG(salary)
    FROM employee
);
```

### Step 1: Average Salary বের হবে

```sql
SELECT AVG(salary)
FROM employee;
```

Result:

```text
51250
```

### Step 2: Oracle ভিতরে Execute করবে

```sql
WHERE salary > 51250
```

### Output

| EMP_NAME | SALARY |
| -------- | ------ |
| Karim    | 60000  |
| Hasan    | 70000  |

---

# Example 3: IT Department-এর Employee বের করা

## DEPARTMENT

| DEPT_ID | DEPT_NAME |
| ------- | --------- |
| 10      | IT        |
| 20      | HR        |
| 30      | Finance   |

## Query

```sql
SELECT *
FROM employee
WHERE dept_id =
(
    SELECT dept_id
    FROM department
    WHERE dept_name = 'IT'
);
```

### Step 1

```sql
SELECT dept_id
FROM department
WHERE dept_name = 'IT';
```

Result:

```text
10
```

### Step 2

Oracle Execute করবে

```sql
SELECT *
FROM employee
WHERE dept_id = 10;
```

### Output

| EMP_NAME |
| -------- |
| Rahim    |
| Sakib    |

---

# Example 4: Second Highest Salary

## Query

```sql
SELECT MAX(salary)
FROM employee
WHERE salary <
(
    SELECT MAX(salary)
    FROM employee
);
```

### Step 1

```sql
SELECT MAX(salary)
FROM employee;
```

Result:

```text
70000
```

### Step 2

Oracle Execute করবে

```sql
SELECT MAX(salary)
FROM employee
WHERE salary < 70000;
```

Result:

```text
60000
```

---

# Example 5: Banking Project Example

ধরুন `GLBBAL` Table-এ সর্বোচ্চ Balance-এর Account বের করতে হবে।

```sql
SELECT *
FROM glbbal
WHERE balance =
(
    SELECT MAX(balance)
    FROM glbbal
);
```

---

# Single Row Subquery

যখন Subquery **শুধুমাত্র একটি Value Return করে**, তখন তাকে **Single Row Subquery** বলে।

উদাহরণ:

```sql
SELECT MAX(salary)
FROM employee;
```

Result:

```text
70000
```

এক্ষেত্রে নিচের Operator ব্যবহার করা যায়:

* `=`
* `>`
* `<`
* `>=`
* `<=`

---

# Multiple Row Subquery

যখন Subquery **একাধিক Row Return করে**, তখন তাকে **Multiple Row Subquery** বলে।

উদাহরণ:

```sql
SELECT dept_id
FROM department
WHERE location = 'Dhaka';
```

Result:

```text
10
20
30
```

এক্ষেত্রে নিচের Operator ব্যবহার করতে হবে:

* `IN`
* `ANY`
* `ALL`
* `EXISTS`

উদাহরণ:

```sql
SELECT *
FROM employee
WHERE dept_id IN
(
    SELECT dept_id
    FROM department
    WHERE location = 'Dhaka'
);
```

---

# Oracle কীভাবে Subquery Execute করে?

Oracle Query Parse করার সময় Outer Query থেকে শুরু করলেও **Execution-এর সময় প্রথমে Inner Query Execute করে**।

Execution Flow:

```text
Outer Query
      │
      ▼
Inner Query Execute
      │
      ▼
Result পাওয়া
      │
      ▼
Outer Query Execute
      │
      ▼
Final Result
```

---

# Subquery vs Join

| Subquery                                    | Join                               |
| ------------------------------------------- | ---------------------------------- |
| একটি Query-এর ভিতরে আরেকটি Query            | একাধিক Table-এর Data একসাথে দেখায় |
| একটি Query-এর Result অন্য Query ব্যবহার করে | Table-এর Relationship ব্যবহার করে  |
| Complex Condition-এর জন্য উপযোগী            | Related Data দেখানোর জন্য উপযোগী   |

---

# Interview Questions

### ১. Subquery কী?

**উত্তর:**
Subquery হলো এমন একটি SQL Query যা অন্য একটি SQL Query-এর ভিতরে লেখা হয় এবং যার Result Outer Query ব্যবহার করে।

---

### ২. Subquery-এর অন্য নাম কী?

* Inner Query
* Nested Query

---

### ৩. Single Row Subquery এবং Multiple Row Subquery-এর পার্থক্য কী?

| Single Row                    | Multiple Row                                 |
| ----------------------------- | -------------------------------------------- |
| একটি Value Return করে         | একাধিক Value Return করে                      |
| `=`, `>`, `<` ব্যবহার করা হয় | `IN`, `ANY`, `ALL`, `EXISTS` ব্যবহার করা হয় |

---

### ৪. Oracle কোন Query আগে Execute করে?

**উত্তর:**
Oracle প্রথমে **Inner Query (Subquery)** Execute করে। এরপর সেই Result ব্যবহার করে **Outer Query** Execute করে।

---

# Quick Revision

```text
Subquery = Query-এর ভিতরে Query

Execution Steps

১. Inner Query Execute
২. Result পাওয়া
৩. Result Outer Query-তে পাঠানো
৪. Outer Query Execute
৫. Final Result Return

Single Row
↓
=, >, <

Multiple Row
↓
IN, ANY, ALL, EXISTS
```

---

# গুরুত্বপূর্ণ বিষয়

* Subquery-কে **Inner Query** বা **Nested Query** বলা হয়।
* Oracle সবসময় **Inner Query আগে Execute করে**।
* একটি Value Return করলে **Single Row Subquery** ব্যবহার হয়।
* একাধিক Value Return করলে **Multiple Row Subquery** ব্যবহার হয়।
* Subquery `WHERE`, `FROM` এবং `SELECT` Clause-এ ব্যবহার করা যায়।
* Complex Query সহজভাবে লেখার জন্য Subquery খুবই গুরুত্বপূর্ণ।
