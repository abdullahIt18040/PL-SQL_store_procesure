# PL/SQL Functions

## What is a Function?

PL/SQL-এর **Function** হলো একটি named PL/SQL program unit, যেটি নির্দিষ্ট কাজ সম্পন্ন করে এবং **অবশ্যই একটি value return করে**।

সহজভাবে:

```text
Input
  ↓
Function
  ↓
Process / Calculation
  ↓
RETURN Value
```

---

## 1. Basic Function Syntax

```sql
CREATE OR REPLACE FUNCTION function_name
(
    parameter_name datatype
)
RETURN return_datatype
IS
    -- Variable declaration
BEGIN
    -- Logic

    RETURN value;
END;
/
```

### Example

```sql
CREATE OR REPLACE FUNCTION get_sum
(
    p_num1 NUMBER,
    p_num2 NUMBER
)
RETURN NUMBER
IS
    v_result NUMBER;
BEGIN
    v_result := p_num1 + p_num2;

    RETURN v_result;
END;
/
```

এখানে:

```text
Function Name = get_sum
Input         = p_num1, p_num2
Return Type   = NUMBER
Return Value  = v_result
```

---

# 2. Calling a Function

Function তৈরি করার পরে `SELECT` statement দিয়ে call করা যায়।

```sql
SELECT get_sum(10, 20)
FROM dual;
```

Output:

```text
30
```

PL/SQL block থেকেও call করা যায়:

```sql
DECLARE
    v_result NUMBER;
BEGIN
    v_result := get_sum(10, 20);

    DBMS_OUTPUT.PUT_LINE('Result = ' || v_result);
END;
/
```

Output:

```text
Result = 30
```

---

# 3. Function Without Parameter

Function-এ কোনো parameter না থাকলেও function তৈরি করা যায়।

```sql
CREATE OR REPLACE FUNCTION get_message
RETURN VARCHAR2
IS
BEGIN
    RETURN 'Hello Abdullah';
END;
/
```

Call:

```sql
SELECT get_message
FROM dual;
```

Output:

```text
Hello Abdullah
```

---

# 4. Function with One Parameter

```sql
CREATE OR REPLACE FUNCTION get_square
(
    p_number NUMBER
)
RETURN NUMBER
IS
BEGIN
    RETURN p_number * p_number;
END;
/
```

Call:

```sql
SELECT get_square(5)
FROM dual;
```

Output:

```text
25
```

Flow:

```text
get_square(5)
      ↓
5 × 5
      ↓
25
```

---

# 5. Function Using a Table

Function-এর ভিতরে database table থেকে data নিয়ে return করা যায়।

ধরা যাক আমাদের table:

```text
SBLTRY_ISLAMIC.EMPLOYEE
```

Columns:

```text
EMPNO
MARK1
MARK2
MARK3
EMPADDRESS
```

### Example

Employee-এর address return করার function:

```sql
CREATE OR REPLACE FUNCTION get_employee_address
(
    p_empno NUMBER
)
RETURN VARCHAR2
IS
    v_address SBLTRY_ISLAMIC.EMPLOYEE.EMPADDRESS%TYPE;
BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = p_empno;

    RETURN v_address;

END;
/
```

Call:

```sql
SELECT get_employee_address(21)
FROM dual;
```

যদি employee `21`-এর address `Dhaka` হয়:

```text
Dhaka
```

### Flow

```text
Employee Number
       ↓
Function
       ↓
SELECT EMPADDRESS
       ↓
v_address
       ↓
RETURN v_address
```

---

# 6. Using `%TYPE`

PL/SQL Function-এর variable declaration-এ `%TYPE` ব্যবহার করা যায়।

```sql
v_address SBLTRY_ISLAMIC.EMPLOYEE.EMPADDRESS%TYPE;
```

এর মাধ্যমে variable-এর datatype সরাসরি database column থেকে নেওয়া হয়।

যেমন:

```text
EMPADDRESS → VARCHAR2(19)
```

তাহলে:

```sql
v_address SBLTRY_ISLAMIC.EMPLOYEE.EMPADDRESS%TYPE;
```

automatically `EMPADDRESS` column-এর datatype follow করবে।

### সুবিধা

Database column-এর datatype পরিবর্তন হলে variable declaration আলাদাভাবে পরিবর্তন করার প্রয়োজন হয় না।

---

# 7. Function with Calculation

Employee-এর তিনটি mark-এর total বের করার function:

```sql
CREATE OR REPLACE FUNCTION get_total_mark
(
    p_mark1 NUMBER,
    p_mark2 NUMBER,
    p_mark3 NUMBER
)
RETURN NUMBER
IS
BEGIN
    RETURN p_mark1 + p_mark2 + p_mark3;
END;
/
```

Call:

```sql
SELECT get_total_mark(80, 75, 90)
FROM dual;
```

Output:

```text
245
```

Flow:

```text
80 + 75 + 90
     ↓
    245
     ↓
 RETURN 245
```

---

# 8. Function with IF-ELSE

Function-এর ভিতরে conditional statement ব্যবহার করা যায়।

```sql
CREATE OR REPLACE FUNCTION get_result
(
    p_mark NUMBER
)
RETURN VARCHAR2
IS
BEGIN

    IF p_mark >= 40 THEN
        RETURN 'PASS';
    ELSE
        RETURN 'FAIL';
    END IF;

END;
/
```

Call:

```sql
SELECT get_result(75)
FROM dual;
```

Output:

```text
PASS
```

আর:

```sql
SELECT get_result(30)
FROM dual;
```

Output:

```text
FAIL
```

---

# 9. Function with Exception Handling

Function-এর ভিতরে exception handling করা যায়।

```sql
CREATE OR REPLACE FUNCTION get_employee_address
(
    p_empno NUMBER
)
RETURN VARCHAR2
IS
    v_address SBLTRY_ISLAMIC.EMPLOYEE.EMPADDRESS%TYPE;
BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = p_empno;

    RETURN v_address;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RETURN 'Employee Not Found';

    WHEN OTHERS THEN
        RETURN 'Error: ' || SQLERRM;

END;
/
```

যদি employee `999` না থাকে:

```sql
SELECT get_employee_address(999)
FROM dual;
```

Output:

```text
Employee Not Found
```

---

# 10. Function vs Procedure

| Function                                   | Procedure                                        |
| ------------------------------------------ | ------------------------------------------------ |
| অবশ্যই value return করে                    | Value return করা বাধ্যতামূলক নয়                  |
| `RETURN datatype` থাকে                     | সাধারণত `RETURN datatype` থাকে না                |
| `RETURN value;` ব্যবহার করে                | `RETURN;` ব্যবহার করা যেতে পারে                  |
| SQL statement-এর মধ্যে ব্যবহার করা যায়     | সাধারণত SQL-এর মধ্যে সরাসরি ব্যবহার করা যায় না   |
| Calculation / value retrieval-এর জন্য ভালো | Business operation / task execution-এর জন্য ভালো |

### Function

```sql
CREATE OR REPLACE FUNCTION add_number
(
    p_a NUMBER,
    p_b NUMBER
)
RETURN NUMBER
IS
BEGIN
    RETURN p_a + p_b;
END;
/
```

Call:

```sql
SELECT add_number(10, 20)
FROM dual;
```

Result:

```text
30
```

### Procedure

```sql
CREATE OR REPLACE PROCEDURE print_sum
(
    p_a NUMBER,
    p_b NUMBER
)
IS
BEGIN
    DBMS_OUTPUT.PUT_LINE(p_a + p_b);
END;
/
```

Call:

```sql
BEGIN
    print_sum(10, 20);
END;
/
```

Output:

```text
30
```

---

# 11. Function Structure

একটি complete function সাধারণত এই structure follow করে:

```sql
CREATE OR REPLACE FUNCTION function_name
(
    parameters
)
RETURN datatype
IS
    -- Variables
BEGIN
    -- Business Logic

    RETURN value;

EXCEPTION
    -- Error Handling

END;
/
```

### Real Example

```sql
CREATE OR REPLACE FUNCTION calculate_salary
(
    p_salary NUMBER,
    p_bonus  NUMBER
)
RETURN NUMBER
IS
    v_total NUMBER;
BEGIN

    v_total := p_salary + p_bonus;

    RETURN v_total;

EXCEPTION
    WHEN OTHERS THEN
        RETURN 0;

END;
/
```

Call:

```sql
SELECT calculate_salary(50000, 10000)
FROM dual;
```

Output:

```text
60000
```

---

# 12. Function Flow

```text
             FUNCTION
                 │
                 ▼
        Receive Parameters
                 │
                 ▼
          Execute Logic
                 │
                 ▼
        Calculate / Query
                 │
                 ▼
              RETURN
                 │
                 ▼
            Result Value
```

---

# 13. Important Points

### 1. Function অবশ্যই একটি value return করবে

```sql
RETURN NUMBER
```

এবং:

```sql
RETURN v_result;
```

দুটিই গুরুত্বপূর্ণ।

### 2. Function parameter নিতে পারে

```sql
get_sum(10, 20)
```

### 3. Function parameter ছাড়াও হতে পারে

```sql
get_message()
```

### 4. Function database query করতে পারে

```sql
SELECT ...
INTO ...
FROM ...
```

### 5. `%TYPE` ব্যবহার করা যায়

```sql
v_salary EMPLOYEE.MARK1%TYPE;
```

### 6. Exception handling করা যায়

```sql
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        ...
```

### 7. SQL-এর মধ্যে Function ব্যবহার করা যায়

```sql
SELECT get_sum(10, 20)
FROM dual;
```

---

# 14. Function vs Normal SQL

Normal SQL:

```sql
SELECT MARK1
FROM SBLTRY_ISLAMIC.EMPLOYEE
WHERE EMPNO = 21;
```

Function:

```sql
SELECT get_employee_mark(21)
FROM dual;
```

Function ব্যবহার করলে একই ধরনের business logic বারবার reuse করা যায়।

---

# 15. Key Concept

PL/SQL Function-কে এভাবে মনে রাখুন:

```text
       INPUT
         ↓
     FUNCTION
         ↓
      PROCESS
         ↓
      RETURN
         ↓
       OUTPUT
```

### One-Line Definition

> **A PL/SQL Function is a named PL/SQL program unit that performs a specific task and must return a value.**
