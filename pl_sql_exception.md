# PL/SQL Error Handling

PL/SQL-এ program চলার সময় কোনো **runtime error** হলে তাকে **Exception** বলা হয়।

**Error Handling** ব্যবহার করে আমরা error detect করতে এবং সেই অনুযায়ী proper action নিতে পারি।

---

## 1. Basic Structure

PL/SQL block-এর error handling অংশ হলো `EXCEPTION`.

```sql
DECLARE
    -- Variable declaration
BEGIN
    -- Main program
EXCEPTION
    -- Error handling
END;
/
```

Flow:

```text
DECLARE
   ↓
BEGIN
   ↓
Execute Program
   ↓
Error?
   ↓
EXCEPTION
   ↓
Handle Error
   ↓
END
```

---

# 2. NO_DATA_FOUND

যখন `SELECT INTO` কোনো row return করে না, তখন `NO_DATA_FOUND` exception হয়।

### Example

```sql
DECLARE
    v_name VARCHAR2(100);
BEGIN

    SELECT EMPADDRESS
    INTO v_name
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = 999;

    DBMS_OUTPUT.PUT_LINE('Address = ' || v_name);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employee found.');

END;
/
```

যদি `EMPNO = 999` না থাকে:

```text
No employee found.
```

---

# 3. TOO_MANY_ROWS

যখন `SELECT INTO` একাধিক row return করে, তখন `TOO_MANY_ROWS` exception হয়।

```sql
DECLARE
    v_address VARCHAR2(100);
BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE;

    DBMS_OUTPUT.PUT_LINE('Address = ' || v_address);

EXCEPTION
    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('Multiple rows found.');

END;
/
```

কারণ `SELECT INTO` সাধারণত একটি row-এর জন্য ব্যবহৃত হয়।

---

# 4. ZERO_DIVIDE

কোনো number-কে `0` দিয়ে divide করলে `ZERO_DIVIDE` exception হয়।

```sql
DECLARE
    v_result NUMBER;
BEGIN

    v_result := 100 / 0;

    DBMS_OUTPUT.PUT_LINE('Result = ' || v_result);

EXCEPTION
    WHEN ZERO_DIVIDE THEN
        DBMS_OUTPUT.PUT_LINE('Cannot divide by zero.');

END;
/
```

Output:

```text
Cannot divide by zero.
```

---

# 5. VALUE_ERROR

Variable-এর datatype বা size-এর সাথে value match না করলে `VALUE_ERROR` হতে পারে।

```sql
DECLARE
    v_name VARCHAR2(5);
BEGIN

    v_name := 'Abdullah';

    DBMS_OUTPUT.PUT_LINE(v_name);

EXCEPTION
    WHEN VALUE_ERROR THEN
        DBMS_OUTPUT.PUT_LINE('Value or size error.');

END;
/
```

`VARCHAR2(5)`-এ `Abdullah` রাখা সম্ভব নয়।

---

# 6. OTHERS

যে exception-এর জন্য আলাদা handler লেখা হয়নি, সেগুলো ধরার জন্য `WHEN OTHERS` ব্যবহার করা হয়।

```sql
DECLARE
    v_result NUMBER;
BEGIN

    v_result := 100 / 0;

EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error occurred.');

END;
/
```

`OTHERS` হলো **catch-all exception handler**।

---

# 7. SQLCODE এবং SQLERRM

Error সম্পর্কে বিস্তারিত information পাওয়ার জন্য:

### SQLCODE

Error-এর numeric code return করে।

### SQLERRM

Error-এর message return করে।

```sql
DECLARE
    v_result NUMBER;
BEGIN

    v_result := 100 / 0;

EXCEPTION
    WHEN OTHERS THEN

        DBMS_OUTPUT.PUT_LINE('Error Code = ' || SQLCODE);
        DBMS_OUTPUT.PUT_LINE('Error Message = ' || SQLERRM);

END;
/
```

Example output:

```text
Error Code = -1476
Error Message = ORA-01476: divisor is equal to zero
```

---

# 8. Multiple Exception Handling

একটি block-এ একাধিক exception handle করা যায়।

```sql
DECLARE
    v_address VARCHAR2(100);
BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = 999;

    DBMS_OUTPUT.PUT_LINE('Address = ' || v_address);

EXCEPTION

    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Employee not found.');

    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('Multiple employees found.');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error = ' || SQLERRM);

END;
/
```

---

# 9. Nested Exception Handling

একটি PL/SQL block-এর ভিতরে আরেকটি block থাকতে পারে।

```sql
DECLARE
    v_result NUMBER;
BEGIN

    BEGIN

        v_result := 100 / 0;

    EXCEPTION
        WHEN ZERO_DIVIDE THEN
            DBMS_OUTPUT.PUT_LINE('Inner block: Cannot divide by zero.');
    END;

    DBMS_OUTPUT.PUT_LINE('Main block continues.');

END;
/
```

Output:

```text
Inner block: Cannot divide by zero.
Main block continues.
```

এখানে inner block error handle করেছে, তাই outer block execution continue করেছে।

---

# 10. RAISE

নিজে থেকে কোনো exception trigger করার জন্য `RAISE` ব্যবহার করা যায়।

```sql
DECLARE
    e_invalid_salary EXCEPTION;
    v_salary NUMBER := -5000;
BEGIN

    IF v_salary < 0 THEN
        RAISE e_invalid_salary;
    END IF;

EXCEPTION
    WHEN e_invalid_salary THEN
        DBMS_OUTPUT.PUT_LINE('Salary cannot be negative.');

END;
/
```

Output:

```text
Salary cannot be negative.
```

---

# 11. RAISE_APPLICATION_ERROR

Custom error message এবং error number তৈরি করার জন্য `RAISE_APPLICATION_ERROR` ব্যবহার করা হয়।
```text
RAISE_APPLICATION_ERROR-এর syntax:

RAISE_APPLICATION_ERROR(
    error_number,
    error_message
);
Error number-এর rule

Custom error-এর জন্য error number অবশ্যই:

-20000 থেকে -20999

এর মধ্যে হতে হবে।
```
Error number সাধারণত:

```text
-20000 থেকে -20999
```

এর মধ্যে দেওয়া হয়।

### Example

```sql
DECLARE
    v_salary NUMBER := -5000;
BEGIN

    IF v_salary < 0 THEN

        RAISE_APPLICATION_ERROR(
            -20001,
            'Salary cannot be negative.'
        );

    END IF;

END;
/
```

Output:

```text
ORA-20001: Salary cannot be negative.
```

---

# 12. Practical Example

ধরা যাক employee-এর address database থেকে বের করতে চাই।

```sql
DECLARE

    v_empno    NUMBER := 21;
    v_address  VARCHAR2(100);

BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = v_empno;

    DBMS_OUTPUT.PUT_LINE(
        'Employee Address = ' || v_address
    );

EXCEPTION

    WHEN NO_DATA_FOUND THEN

        DBMS_OUTPUT.PUT_LINE(
            'Employee ' || v_empno || ' not found.'
        );

    WHEN TOO_MANY_ROWS THEN

        DBMS_OUTPUT.PUT_LINE(
            'Multiple employees found.'
        );

    WHEN OTHERS THEN

        DBMS_OUTPUT.PUT_LINE(
            'Error Code = ' || SQLCODE
        );

        DBMS_OUTPUT.PUT_LINE(
            'Error Message = ' || SQLERRM
        );

END;
/
```

---

# 13. Exception Handling Flow

```text
              BEGIN
                |
                v
          Execute SQL/Logic
                |
           Error occurs?
             /       \
           No         Yes
           |           |
           v           v
       Continue    EXCEPTION
                       |
          +------------+------------+
          |            |            |
          v            v            v
   NO_DATA_FOUND  TOO_MANY_ROWS  OTHERS
          |            |            |
          +------------+------------+
                       |
                       v
                      END
```

---

# 14. Important Predefined Exceptions

| Exception          | Meaning                    |
| ------------------ | -------------------------- |
| `NO_DATA_FOUND`    | কোনো row পাওয়া যায়নি       |
| `TOO_MANY_ROWS`    | একাধিক row পাওয়া গেছে      |
| `ZERO_DIVIDE`      | 0 দিয়ে division            |
| `VALUE_ERROR`      | Invalid value/size         |
| `DUP_VAL_ON_INDEX` | Duplicate unique value     |
| `INVALID_NUMBER`   | Invalid numeric conversion |
| `OTHERS`           | অন্যান্য exceptions        |

---

# 15. Important Rules

### Rule 1: `SELECT INTO`-তে NO_DATA_FOUND

```sql
SELECT name
INTO v_name
FROM employee
WHERE empno = 999;
```

Row না থাকলে:

```text
NO_DATA_FOUND
```

---

### Rule 2: Multiple rows হলে TOO_MANY_ROWS

```sql
SELECT name
INTO v_name
FROM employee;
```

একাধিক row হলে:

```text
TOO_MANY_ROWS
```

---

### Rule 3: Error hide করবেন না

এভাবে লেখা উচিত নয়:

```sql
EXCEPTION
    WHEN OTHERS THEN
        NULL;
```

কারণ এতে error silently ignore হয়ে যায়।

Better:

```sql
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(
            'Error = ' || SQLERRM
        );
END;
/
```

---

# 16. Best Practice

Production code-এ সাধারণত:

```sql
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        -- specific handling

    WHEN TOO_MANY_ROWS THEN
        -- specific handling

    WHEN OTHERS THEN
        -- log error
        DBMS_OUTPUT.PUT_LINE(
            'Error Code = ' || SQLCODE
        );

        DBMS_OUTPUT.PUT_LINE(
            'Error Message = ' || SQLERRM
        );

        RAISE;
END;
/
```

`RAISE` ব্যবহার করলে original error আবার caller-এর কাছে চলে যায়।

---

# Summary

PL/SQL Error Handling-এর মূল বিষয়:

```text
Exception
   |
   +-- Predefined Exception
   |      |
   |      +-- NO_DATA_FOUND
   |      +-- TOO_MANY_ROWS
   |      +-- ZERO_DIVIDE
   |      +-- VALUE_ERROR
   |
   +-- User Defined Exception
   |      |
   |      +-- RAISE
   |
   +-- Custom Error
          |
          +-- RAISE_APPLICATION_ERROR
```

সবচেয়ে বেশি ব্যবহৃত pattern:

```sql
BEGIN

    -- Main logic

EXCEPTION

    WHEN NO_DATA_FOUND THEN
        -- Handle no data

    WHEN TOO_MANY_ROWS THEN
        -- Handle multiple rows

    WHEN OTHERS THEN
        -- Handle unexpected error
        DBMS_OUTPUT.PUT_LINE(SQLERRM);

END;
/
```

### Key Points

* `EXCEPTION` section error handle করে।
* `NO_DATA_FOUND` → কোনো row পাওয়া যায়নি।
* `TOO_MANY_ROWS` → একাধিক row পাওয়া গেছে।
* `ZERO_DIVIDE` → zero দিয়ে division।
* `WHEN OTHERS` → অন্যান্য unexpected error।
* `SQLCODE` → error number।
* `SQLERRM` → error message।
* `RAISE` → exception manually raise করা।
* `RAISE_APPLICATION_ERROR` → custom Oracle error তৈরি করা।
* `WHEN OTHERS THEN NULL` সাধারণত avoid করা উচিত।
