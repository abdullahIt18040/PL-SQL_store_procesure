## PL/SQL Cursors

A Cursor in PL/SQL is a mechanism used to process the result of a SQL query row by row.
```
সহজভাবে:

SQL Query
   ↓
Multiple Rows
   ↓
Cursor
   ↓
Process one row at a time
1. Why Cursor?

ধরুন EMPLOYEE table-এ অনেক employee আছে:

EMPNO | MARK1
------+------
21    | 80
22    | 90
23    | 75

যদি প্রতিটি row আলাদাভাবে process করতে চাই, তখন Cursor ব্যবহার করা যায়।

2. Types of Cursor

PL/SQL-এ প্রধানত দুই ধরনের Cursor:

Cursor
├── Implicit Cursor
└── Explicit Cursor
3. Implicit Cursor

Oracle নিজে automatically cursor তৈরি করে যখন আমরা INSERT, UPDATE, DELETE বা SELECT INTO করি।

Example:

DECLARE
    v_mark NUMBER;
BEGIN

    SELECT MARK1
    INTO v_mark
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = 21;

    DBMS_OUTPUT.PUT_LINE('Mark = ' || v_mark);

END;
/

এখানে Oracle automatically cursor manage করছে।

Implicit Cursor Attributes
SQL%FOUND
SQL%NOTFOUND
SQL%ROWCOUNT
SQL%ISOPEN

Example:

BEGIN

    UPDATE SBLTRY_ISLAMIC.EMPLOYEE
    SET MARK1 = 95
    WHERE EMPNO = 21;

    DBMS_OUTPUT.PUT_LINE(
        'Updated Rows = ' || SQL%ROWCOUNT
    );

END;
/

যদি একটি row update হয়:

Updated Rows = 1
4. Explicit Cursor

যখন programmer নিজে cursor declare এবং control করে, সেটাকে Explicit Cursor বলে।

Basic structure:

DECLARE

    CURSOR c_employee IS
        SELECT EMPNO, MARK1
        FROM SBLTRY_ISLAMIC.EMPLOYEE;

BEGIN

    FOR rec IN c_employee LOOP

        DBMS_OUTPUT.PUT_LINE(
            'Employee = ' || rec.EMPNO ||
            ', Mark = ' || rec.MARK1
        );

    END LOOP;

END;
/

Output:

Employee = 21, Mark = 80
Employee = 22, Mark = 90
Employee = 23, Mark = 75
5. Cursor FOR Loop

এটি সবচেয়ে সহজ এবং commonly used approach।

DECLARE

    CURSOR c_employee IS
        SELECT EMPNO, EMPADDRESS
        FROM SBLTRY_ISLAMIC.EMPLOYEE;

BEGIN

    FOR emp IN c_employee LOOP

        DBMS_OUTPUT.PUT_LINE(
            'EMPNO = ' || emp.EMPNO
        );

        DBMS_OUTPUT.PUT_LINE(
            'Address = ' || emp.EMPADDRESS
        );

    END LOOP;

END;
/

এখানে Oracle automatically:

OPEN
  ↓
FETCH
  ↓
Process row
  ↓
FETCH next row
  ↓
Process row
  ↓
CLOSE

করে।

6. Explicit Cursor Manually Control

Cursor manually control করা যায়:

DECLARE
   Cursor
BEGIN
   OPEN
   FETCH
   CLOSE
END

Example:

DECLARE

    CURSOR c_employee IS
        SELECT EMPNO, MARK1
        FROM SBLTRY_ISLAMIC.EMPLOYEE;

    v_empno NUMBER;
    v_mark  NUMBER;

BEGIN

    OPEN c_employee;

    LOOP

        FETCH c_employee
        INTO v_empno, v_mark;

        EXIT WHEN c_employee%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            'Employee = ' || v_empno ||
            ', Mark = ' || v_mark
        );

    END LOOP;

    CLOSE c_employee;

END;
/
7. Cursor Attributes

Explicit cursor-এর গুরুত্বপূর্ণ attributes:

Attribute	Meaning
%FOUND	Last fetch successful কিনা
%NOTFOUND	আর কোনো row পাওয়া যায়নি কিনা
%ROWCOUNT	কতগুলো row fetch হয়েছে
%ISOPEN	Cursor open আছে কিনা

Example:

DECLARE

    CURSOR c_employee IS
        SELECT EMPNO, MARK1
        FROM SBLTRY_ISLAMIC.EMPLOYEE;

    v_empno NUMBER;
    v_mark  NUMBER;

BEGIN

    OPEN c_employee;

    LOOP

        FETCH c_employee
        INTO v_empno, v_mark;

        EXIT WHEN c_employee%NOTFOUND;

        DBMS_OUTPUT.PUT_LINE(
            'Row Number = ' || c_employee%ROWCOUNT
        );

    END LOOP;

    CLOSE c_employee;

END;
/
8. Parameterized Cursor

Cursor-এ parameter পাঠানো যায়।

DECLARE

    CURSOR c_employee(p_empno NUMBER) IS
        SELECT EMPNO, MARK1
        FROM SBLTRY_ISLAMIC.EMPLOYEE
        WHERE EMPNO = p_empno;

BEGIN

    FOR emp IN c_employee(21) LOOP

        DBMS_OUTPUT.PUT_LINE(
            'Employee = ' || emp.EMPNO
        );

        DBMS_OUTPUT.PUT_LINE(
            'Mark = ' || emp.MARK1
        );

    END LOOP;

END;
/

এখানে:

c_employee(21)

মানে cursor-এর parameter:

p_empno = 21
9. Cursor + UPDATE Example

ধরুন যাদের MARK1 < 50, তাদের mark 10 বাড়াতে চাই।

DECLARE

    CURSOR c_employee IS
        SELECT EMPNO, MARK1
        FROM SBLTRY_ISLAMIC.EMPLOYEE
        WHERE MARK1 < 50;

BEGIN

    FOR emp IN c_employee LOOP

        UPDATE SBLTRY_ISLAMIC.EMPLOYEE
        SET MARK1 = emp.MARK1 + 10
        WHERE EMPNO = emp.EMPNO;

        DBMS_OUTPUT.PUT_LINE(
            'Updated Employee = ' || emp.EMPNO
        );

    END LOOP;

END;
/

Flow:

SELECT employees
      ↓
Cursor
      ↓
Employee 1
      ↓
UPDATE
      ↓
Employee 2
      ↓
UPDATE
      ↓
Employee 3
      ↓
UPDATE
10. Cursor in Your Application Package

আপনার আগের application package-এর মতো বড় PL/SQL package-এ এই ধরনের cursor খুব common:

FOR G IN (
    SELECT DISTINCT GLBBAL_CURR_CODE
    FROM GLBBAL
    WHERE GLBBAL_ENTITY_NUM = ...
) LOOP

    W_CURR_CODE := G.GLBBAL_CURR_CODE;

    -- processing

END LOOP;

এখানে:

FOR G IN (...)

একটি Cursor FOR Loop।

Oracle internally:

Execute SELECT
      ↓
Get first row
      ↓
G.GLBBAL_CURR_CODE
      ↓
Process
      ↓
Get next row
      ↓
Process
      ↓
No more rows
      ↓
Loop ends
11. Cursor vs SELECT INTO
SELECT INTO

একটি row পাওয়ার জন্য:

SELECT MARK1
INTO v_mark
FROM EMPLOYEE
WHERE EMPNO = 21;
Cursor

Multiple rows process করার জন্য:

FOR emp IN (
    SELECT EMPNO, MARK1
    FROM EMPLOYEE
) LOOP
```

    DBMS_OUTPUT.PUT_LINE(emp.EMPNO);

END LOOP;
