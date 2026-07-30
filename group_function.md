# Reporting Aggregated Data Using Group Functions (Oracle SQL)

## What are Group Functions?

**Group Functions (Aggregate Functions)** operate on **multiple rows** and return **a single result**.

### Common Group Functions

- `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`

> **Key Point:** Single-Row Functions work on **one row**, whereas Group Functions work on **multiple rows**.

---

# Sample Table

| EMP_ID | EMP_NAME | DEPT | SALARY | COMMISSION |
|--------|----------|------|--------|------------|
| 101 | Abdullah | IT | 25000 | NULL |
| 102 | Mamun | IT | 35000 | 5000 |
| 103 | Rahim | HR | 40000 | NULL |
| 104 | Karim | HR | 30000 | 3000 |
| 105 | Jony | Sales | 45000 | 4000 |

---

# 1. COUNT()

Returns the number of rows.

### Count all rows

```sql
SELECT COUNT(*)
FROM EMPLOYEEPRO;
```

Output:

```
5
```

### Count non-NULL values

```sql
SELECT COUNT(COMMISSION)
FROM EMPLOYEEPRO;
```

Output:

```
3
```

> **Note:** `COUNT(column)` ignores **NULL** values.

---

# 2. SUM()

Returns the total value.

```sql
SELECT SUM(SALARY)
FROM EMPLOYEEPRO;
```

Output:

```
175000
```

---

# 3. AVG()

Returns the average value.

```sql
SELECT AVG(SALARY)
FROM EMPLOYEEPRO;
```

Output:

```
35000
```

---

# 4. MIN()

Returns the smallest value.

```sql
SELECT MIN(SALARY)
FROM EMPLOYEEPRO;
```

Output:

```
25000
```

---

# 5. MAX()

Returns the largest value.

```sql
SELECT MAX(SALARY)
FROM EMPLOYEEPRO;
```

Output:

```
45000
```

---

# Using Multiple Group Functions

```sql
SELECT
    COUNT(*) AS TOTAL_EMPLOYEE,
    SUM(SALARY) AS TOTAL_SALARY,
    AVG(SALARY) AS AVG_SALARY,
    MIN(SALARY) AS MIN_SALARY,
    MAX(SALARY) AS MAX_SALARY
FROM EMPLOYEEPRO;
```

---

# GROUP BY Clause

Used to divide rows into groups before applying Group Functions.

### Syntax

```sql
SELECT column_name,
       GROUP_FUNCTION(column_name)
FROM table_name
GROUP BY column_name;
```

### Example

```sql
SELECT DEPT,
       COUNT(*) AS TOTAL_EMPLOYEE,
       AVG(SALARY) AS AVG_SALARY
FROM EMPLOYEEPRO
GROUP BY DEPT;
```

Output:

| DEPT | TOTAL_EMPLOYEE | AVG_SALARY |
|------|---------------:|-----------:|
| IT | 2 | 30000 |
| HR | 2 | 35000 |
| Sales | 1 | 45000 |

---

# HAVING Clause

Filters groups after `GROUP BY`.

### Syntax

```sql
SELECT column_name,
       GROUP_FUNCTION(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

### Example

```sql
SELECT DEPT,
       AVG(SALARY)
FROM EMPLOYEEPRO
GROUP BY DEPT
HAVING AVG(SALARY) > 32000;
```

Output:

| DEPT | AVG(SALARY) |
|------|------------:|
| HR | 35000 |
| Sales | 45000 |

---

# WHERE vs HAVING

| WHERE | HAVING |
|--------|---------|
| Filters rows | Filters groups |
| Executes before `GROUP BY` | Executes after `GROUP BY` |
| Cannot use Group Functions | Can use Group Functions |

### WHERE Example

```sql
SELECT *
FROM EMPLOYEEPRO
WHERE SALARY > 30000;
```

### HAVING Example

```sql
SELECT DEPT,
       AVG(SALARY)
FROM EMPLOYEEPRO
GROUP BY DEPT
HAVING AVG(SALARY) > 32000;
```

---

# NULL Values in Group Functions

- `COUNT(*)` → Counts **all rows**
- `COUNT(column)` → Counts **only non-NULL values**
- `SUM()`, `AVG()`, `MIN()`, `MAX()` → Ignore **NULL** values

Example:

```sql
SELECT COUNT(*),
       COUNT(COMMISSION)
FROM EMPLOYEEPRO;
```

Output:

| COUNT(*) | COUNT(COMMISSION) |
|----------|------------------:|
| 5 | 3 |

---

# Nested Group Function

Example:

```sql
SELECT MAX(AVG(SALARY))
FROM EMPLOYEE
GROUP BY DEPT;
```

> Finds the highest average salary among all departments.

---

# Interview Questions

## 1. What is a Group Function?

A function that operates on multiple rows and returns a single result.

---

## 2. Name the Group Functions.

- `COUNT()`
- `SUM()`
- `AVG()`
- `MIN()`
- `MAX()`

---

## 3. Difference between `COUNT(*)` and `COUNT(column)`?

| COUNT(*) | COUNT(column) |
|-----------|---------------|
| Counts all rows | Counts only non-NULL values |

---

## 4. Why is GROUP BY used?

To divide rows into groups before applying Group Functions.

---

## 5. Difference between WHERE and HAVING?

| WHERE | HAVING |
|--------|---------|
| Filters rows | Filters grouped data |
| Before `GROUP BY` | After `GROUP BY` |

---

# Summary

## Group Functions

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

## Clauses

```text
GROUP BY
HAVING
```

## Key Points

- Group Functions return **one result** for **multiple rows**.
- `COUNT(*)` counts all rows.
- `COUNT(column)` ignores `NULL` values.
- `SUM()`, `AVG()`, `MIN()`, and `MAX()` ignore `NULL` values.
- `GROUP BY` groups rows before aggregation.
- `HAVING` filters grouped results.
- `WHERE` filters rows before grouping.
