# Oracle vs PostgreSQL - Interview Quick Notes

## Oracle vs PostgreSQL (Most Important Differences)

| Oracle                        | PostgreSQL                         |
| ----------------------------- | ---------------------------------- |
| `ROWNUM` / `FETCH FIRST`      | `LIMIT`                            |
| `NVL()`                       | `COALESCE()`                       |
| `DUAL` Table ব্যবহার করতে হয় | `DUAL` Table লাগে না               |
| `VARCHAR2`                    | `VARCHAR`                          |
| `NUMBER`                      | `NUMERIC` / `INTEGER`              |
| `CLOB`                        | `TEXT`                             |
| `BLOB`                        | `BYTEA`                            |
| `PACKAGE` Supported           | `PACKAGE` নেই                      |
| `SEQUENCE` বেশি ব্যবহৃত       | `SERIAL` / `IDENTITY` বেশি ব্যবহৃত |

---

# Example Comparison

## 1. Limit Rows

### Oracle

```sql
SELECT *
FROM employee
FETCH FIRST 10 ROWS ONLY;
```

### PostgreSQL

```sql
SELECT *
FROM employee
LIMIT 10;
```

---

## 2. NULL Handling

### Oracle

```sql
SELECT NVL(salary, 0)
FROM employee;
```

### PostgreSQL

```sql
SELECT COALESCE(salary, 0)
FROM employee;
```

---

## 3. Dummy Table

### Oracle

```sql
SELECT SYSDATE
FROM dual;
```

### PostgreSQL

```sql
SELECT CURRENT_DATE;
```

> PostgreSQL-এ `DUAL` Table ব্যবহার করতে হয় না।

---

## 4. Data Types

| Oracle          | PostgreSQL            |
| --------------- | --------------------- |
| `VARCHAR2(100)` | `VARCHAR(100)`        |
| `NUMBER`        | `NUMERIC` / `INTEGER` |
| `DATE`          | `DATE`                |
| `TIMESTAMP`     | `TIMESTAMP`           |
| `CLOB`          | `TEXT`                |
| `BLOB`          | `BYTEA`               |

---

## 5. Auto Increment

### Oracle

```sql
CREATE SEQUENCE emp_seq;

INSERT INTO employee(id, name)
VALUES(emp_seq.NEXTVAL, 'Rahim');
```

### PostgreSQL

```sql
CREATE TABLE employee
(
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);
```

---

# Quick Revision

```text
Oracle                  PostgreSQL
--------------------------------------------
ROWNUM/FETCH FIRST   →  LIMIT
NVL()                →  COALESCE()
DUAL                 →  No DUAL
VARCHAR2             →  VARCHAR
NUMBER               →  NUMERIC / INTEGER
CLOB                 →  TEXT
BLOB                 →  BYTEA
PACKAGE              →  Not Supported
SEQUENCE             →  SERIAL / IDENTITY
```

---

# Interview Questions

### Q1. Oracle-এ `LIMIT` এর পরিবর্তে কী ব্যবহার করা হয়?

**Answer:**

* Oracle 12c+: `FETCH FIRST n ROWS ONLY`
* Oracle 11g: `ROWNUM`

---

### Q2. PostgreSQL-এ `NVL()` এর পরিবর্তে কী ব্যবহার করা হয়?

**Answer:**
`COALESCE()`

---

### Q3. Oracle-এর `VARCHAR2`-এর সমতুল্য PostgreSQL Data Type কী?

**Answer:**
`VARCHAR`

---

### Q4. Oracle-এর `CLOB` এবং `BLOB`-এর সমতুল্য PostgreSQL Data Type কী?

**Answer:**

* `CLOB` → `TEXT`
* `BLOB` → `BYTEA`

---

### Q5. PostgreSQL-এ কি `PACKAGE` আছে?

**Answer:**
না। PostgreSQL-এ Oracle-এর মতো `PACKAGE` নেই। এর পরিবর্তে `Schema`, `Function` এবং `Procedure` ব্যবহার করা হয়।

---

## 💡 মনে রাখার সহজ উপায়

```text
Oracle = Enterprise + PL/SQL + PACKAGE

PostgreSQL = Open Source + PL/pgSQL + No PACKAGE
```

