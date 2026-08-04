# Oracle SQL - Regular Expression (REGEXP)

## What is Regular Expression?

Regular Expression (Regex) হলো একটি **Pattern Matching Technique**, যা Oracle-এ Data Search, Validation এবং Manipulation-এর জন্য ব্যবহার করা হয়।

`LIKE` শুধুমাত্র Simple Pattern Match করতে পারে, কিন্তু `REGEXP` Complex Pattern Match করতে পারে।

### কোথায় ব্যবহার হয়?

* Email Validation
* Mobile Number Validation
* Password Validation
* Account Number Validation
* Data Cleaning
* Text Search
* Text Replace

---

# Oracle REGEXP Functions

| Function           | Description                       |
| ------------------ | --------------------------------- |
| `REGEXP_LIKE()`    | Pattern Match করে                 |
| `REGEXP_SUBSTR()`  | Pattern অনুযায়ী Text Extract করে |
| `REGEXP_REPLACE()` | Pattern Replace করে               |
| `REGEXP_INSTR()`   | Pattern-এর Position বের করে       |
| `REGEXP_COUNT()`   | Pattern কতবার আছে Count করে       |

---

# Sample Table

```sql
CREATE TABLE employee
(
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    email VARCHAR2(100),
    phone VARCHAR2(20)
);

INSERT INTO employee VALUES
(1,'Rahim','rahim@gmail.com','01712345678');

INSERT INTO employee VALUES
(2,'Karim','karim@yahoo.com','01898765432');

INSERT INTO employee VALUES
(3,'Hasan','hasan123@gmail.com','01955555555');

COMMIT;
```

---

# 1. REGEXP_LIKE()

Pattern Match করার জন্য ব্যবহৃত হয়।

## Syntax

```sql
REGEXP_LIKE(column_name,'pattern')
```

---

### Example 1: Name 'R' দিয়ে শুরু

```sql
SELECT *
FROM employee
WHERE REGEXP_LIKE(emp_name,'^R');
```

**Output**

```text
Rahim
```

---

### Example 2: Name 'n' দিয়ে শেষ

```sql
SELECT *
FROM employee
WHERE REGEXP_LIKE(emp_name,'n$');
```

**Output**

```text
Hasan
```

---

### Example 3: শুধুমাত্র Number

```sql
SELECT *
FROM employee
WHERE REGEXP_LIKE(phone,'^[0-9]+$');
```

---

### Example 4: Name-এ "im" আছে

```sql
SELECT *
FROM employee
WHERE REGEXP_LIKE(emp_name,'im');
```

**Output**

```text
Rahim
```

---

# 2. REGEXP_SUBSTR()

Pattern অনুযায়ী String থেকে Data Extract করে।

## Syntax

```sql
REGEXP_SUBSTR(source_string, pattern)
```

### Example

```sql
SELECT REGEXP_SUBSTR
(
'ABC123XYZ',
'[0-9]+'
)
FROM dual;
```

**Output**

```text
123
```

---

আরেকটি Example

```sql
SELECT REGEXP_SUBSTR
(
'Oracle SQL Tutorial',
'[A-Za-z]+'
)
FROM dual;
```

**Output**

```text
Oracle
```

---

# 3. REGEXP_REPLACE()

Pattern Replace করার জন্য ব্যবহৃত হয়।

## Syntax

```sql
REGEXP_REPLACE(source, pattern, replace_text)
```

### Example

Number Remove

```sql
SELECT REGEXP_REPLACE
(
'Oracle123',
'[0-9]',
''
)
FROM dual;
```

**Output**

```text
Oracle
```

---

Space Replace

```sql
SELECT REGEXP_REPLACE
(
'Rahim Karim Hasan',
' ',
'-'
)
FROM dual;
```

**Output**

```text
Rahim-Karim-Hasan
```

---

# 4. REGEXP_INSTR()

Pattern কোন Position থেকে শুরু হয়েছে তা Return করে।

## Syntax

```sql
REGEXP_INSTR(source, pattern)
```

### Example

```sql
SELECT REGEXP_INSTR
(
'Oracle Tutorial',
'Tutorial'
)
FROM dual;
```

**Output**

```text
8
```

---

# 5. REGEXP_COUNT()

Pattern কতবার আছে Count করে।

## Syntax

```sql
REGEXP_COUNT(source, pattern)
```

### Example

```sql
SELECT REGEXP_COUNT
(
'abc123abc456abc',
'abc'
)
FROM dual;
```

**Output**

```text
3
```

---

# Most Common Regex Symbols

| Symbol   | Meaning                | Example      |
| -------- | ---------------------- | ------------ |
| `^`      | String-এর শুরু         | `^A`         |
| `$`      | String-এর শেষ          | `Z$`         |
| `.`      | যেকোনো একটি Character  | `A.C`        |
| `*`      | Zero or More           | `ab*`        |
| `+`      | One or More            | `ab+`        |
| `?`      | Zero or One            | `ab?`        |
| `[abc]`  | a, b, c-এর যেকোনো একটি | `[ABC]`      |
| `[^abc]` | a, b, c ছাড়া          | `[^0-9]`     |
| `[A-Z]`  | Uppercase Letter       | `[A-Z]+`     |
| `[a-z]`  | Lowercase Letter       | `[a-z]+`     |
| `[0-9]`  | Digit                  | `[0-9]+`     |
| `{n}`    | ঠিক n বার              | `[0-9]{5}`   |
| `{n,m}`  | n থেকে m বার           | `[A-Z]{2,5}` |

---

# Email Validation

```sql
SELECT email
FROM employee
WHERE REGEXP_LIKE
(
email,
'^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$'
);
```

---

# Bangladesh Mobile Number Validation

```sql
SELECT phone
FROM employee
WHERE REGEXP_LIKE
(
phone,
'^01[3-9][0-9]{8}$'
);
```

### Valid

```text
01712345678
01898765432
01955555555
```

### Invalid

```text
12712345678
01912345
018ABC12345
```

---

# Banking Project Example

Customer Table

| Account No | Customer | Phone       |
| ---------- | -------- | ----------- |
| 1001       | Rahim    | 01712345678 |
| 1002       | Karim    | ABC123      |
| 1003       | Hasan    | 01987654321 |

শুধুমাত্র Valid Mobile Number বের করা

```sql
SELECT account_no,
       customer_name
FROM accounts
WHERE REGEXP_LIKE
(
phone,
'^01[3-9][0-9]{8}$'
);
```

**Output**

| Account No | Customer |
| ---------- | -------- |
| 1001       | Rahim    |
| 1003       | Hasan    |

---

# LIKE vs REGEXP

| LIKE                    | REGEXP                          |
| ----------------------- | ------------------------------- |
| Simple Pattern Search   | Advanced Pattern Matching       |
| `%` এবং `_` ব্যবহার করে | Full Regular Expression Support |
| Fast                    | তুলনামূলক Slow                  |
| Basic Search            | Validation + Advanced Search    |

---

# Interview Questions

### 1. Regular Expression কী?

Pattern Matching এবং Data Validation-এর জন্য ব্যবহৃত একটি Powerful Technique।

---

### 2. Oracle-এর পাঁচটি REGEXP Function কী?

* REGEXP_LIKE()
* REGEXP_SUBSTR()
* REGEXP_REPLACE()
* REGEXP_INSTR()
* REGEXP_COUNT()

---

### 3. `^` এবং `$` কী বোঝায়?

* `^` → String-এর শুরু (Start)
* `$` → String-এর শেষ (End)

---

### 4. `+` এবং `*` এর পার্থক্য কী?

| Symbol | Meaning                     |
| ------ | --------------------------- |
| `+`    | কমপক্ষে ১ বার (One or More) |
| `*`    | ০ বা অনেকবার (Zero or More) |

---

# Quick Revision

```text
Oracle REGEXP Functions

REGEXP_LIKE()      → Pattern Match
REGEXP_SUBSTR()    → Extract Text
REGEXP_REPLACE()   → Replace Text
REGEXP_INSTR()     → Find Position
REGEXP_COUNT()     → Count Pattern

Common Symbols

^        Start of String
$        End of String
.        Any Character
*        Zero or More
+        One or More
?        Optional
[]       Character Set
[^]      Exclude Character Set
[0-9]    Digit
[A-Z]    Uppercase
[a-z]    Lowercase
{n}      Exactly n Times
{n,m}    Between n and m Times
```

---

# Key Points

* Regular Expression (`REGEXP`) হলো Advanced Pattern Matching Technique।
* `REGEXP_LIKE()` সবচেয়ে বেশি ব্যবহার হয় Validation-এর জন্য।
* `REGEXP_SUBSTR()` Text Extract করে।
* `REGEXP_REPLACE()` Data Clean বা Replace করতে ব্যবহৃত হয়।
* `REGEXP_INSTR()` Pattern-এর Position বের করে।
* `REGEXP_COUNT()` Pattern কতবার আছে Count করে।
* Banking Project-এ Email, Mobile Number, Account Number এবং Customer ID Validation-এ REGEXP ব্যাপকভাবে ব্যবহৃত হয়।
