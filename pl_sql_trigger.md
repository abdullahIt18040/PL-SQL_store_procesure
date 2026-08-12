
```
PL/SQL Triggers

A Trigger হলো একটি special PL/SQL program, যেটা কোনো নির্দিষ্ট database event ঘটলে automatically execute হয়।

Trigger-কে সাধারণ procedure-এর মতো manually call করতে হয় না।

Basic Flow
Database Event
      ↓
   Trigger
      ↓
PL/SQL Code Execute
1. Trigger কী?

Trigger সাধারণত নিচের event-এর উপর কাজ করে:

INSERT
UPDATE
DELETE
CREATE
ALTER
DROP
Database events

Example:

CREATE OR REPLACE TRIGGER trg_employee_insert
AFTER INSERT ON SBLTRY_ISLAMIC.EMPLOYEE
BEGIN
    DBMS_OUTPUT.PUT_LINE('New employee inserted');
END;
/

এখন EMPLOYEE table-এ INSERT হলে trigger automatically execute হবে।

INSERT INTO SBLTRY_ISLAMIC.EMPLOYEE
    (EMPNO, MARK1, MARK2, MARK3, EMPADDRESS)
VALUES
    (21, 80, 75, 90, 'Dhaka');

Trigger automatically execute করবে:

INSERT
  ↓
trg_employee_insert
  ↓
"New employee inserted"
2. Trigger-এর Basic Syntax
CREATE OR REPLACE TRIGGER trigger_name
BEFORE | AFTER
INSERT | UPDATE | DELETE
ON table_name
BEGIN
    -- PL/SQL code
END;
/

Example:

CREATE OR REPLACE TRIGGER trg_employee
BEFORE INSERT ON SBLTRY_ISLAMIC.EMPLOYEE
BEGIN
    DBMS_OUTPUT.PUT_LINE('Before insert trigger executed');
END;
/
3. BEFORE Trigger

BEFORE trigger মূল database operation-এর আগে execute হয়।

CREATE OR REPLACE TRIGGER trg_employee_before
BEFORE INSERT ON SBLTRY_ISLAMIC.EMPLOYEE
BEGIN
    DBMS_OUTPUT.PUT_LINE('Before INSERT');
END;
/

Flow:

INSERT
  ↓
BEFORE Trigger
  ↓
Row Insert
4. AFTER Trigger

AFTER trigger database operation complete হওয়ার পরে execute হয়।

CREATE OR REPLACE TRIGGER trg_employee_after
AFTER INSERT ON SBLTRY_ISLAMIC.EMPLOYEE
BEGIN
    DBMS_OUTPUT.PUT_LINE('After INSERT');
END;
/

Flow:

INSERT
  ↓
Row Insert
  ↓
AFTER Trigger
5. INSERT Trigger

শুধু INSERT event-এর জন্য trigger:

CREATE OR REPLACE TRIGGER trg_employee_insert
AFTER INSERT ON SBLTRY_ISLAMIC.EMPLOYEE
BEGIN
    DBMS_OUTPUT.PUT_LINE('Employee inserted');
END;
/
6. UPDATE Trigger

UPDATE হলে trigger execute হবে।

CREATE OR REPLACE TRIGGER trg_employee_update
AFTER UPDATE ON SBLTRY_ISLAMIC.EMPLOYEE
BEGIN
    DBMS_OUTPUT.PUT_LINE('Employee updated');
END;
/

Test:

UPDATE SBLTRY_ISLAMIC.EMPLOYEE
SET MARK1 = 90
WHERE EMPNO = 21;

Output:

Employee updated
7. DELETE Trigger

DELETE হলে trigger execute হবে।

CREATE OR REPLACE TRIGGER trg_employee_delete
AFTER DELETE ON SBLTRY_ISLAMIC.EMPLOYEE
BEGIN
    DBMS_OUTPUT.PUT_LINE('Employee deleted');
END;
/
8. Row-Level Trigger

Row-level trigger-এর জন্য FOR EACH ROW ব্যবহার করা হয়।

CREATE OR REPLACE TRIGGER trg_employee_row
AFTER INSERT ON SBLTRY_ISLAMIC.EMPLOYEE
FOR EACH ROW
BEGIN
    DBMS_OUTPUT.PUT_LINE('Employee No = ' || :NEW.EMPNO);
END;
/

এখানে:

:NEW.EMPNO

নতুন inserted row-এর EMPNO value নির্দেশ করে।

যদি insert করি:

INSERT INTO SBLTRY_ISLAMIC.EMPLOYEE
    (EMPNO, MARK1, MARK2, MARK3, EMPADDRESS)
VALUES
    (25, 80, 70, 90, 'Dhaka');

তাহলে:

Employee No = 25
9. :NEW এবং :OLD

Row-level trigger-এ সবচেয়ে গুরুত্বপূর্ণ বিষয় হলো :NEW এবং :OLD।

Event	:OLD	:NEW
INSERT	❌	✅
UPDATE	✅	✅
DELETE	✅	❌
INSERT
:NEW.EMPNO

নতুন value পাওয়া যাবে।

UPDATE
:OLD.MARK1
:NEW.MARK1

আগের এবং নতুন value পাওয়া যাবে।

DELETE
:OLD.EMPNO

delete হওয়ার আগের value পাওয়া যাবে।

10. UPDATE Trigger Example

ধরি MARK1 update করার আগে এবং পরে value দেখতে চাই:

CREATE OR REPLACE TRIGGER trg_employee_mark_update
BEFORE UPDATE OF MARK1
ON SBLTRY_ISLAMIC.EMPLOYEE
FOR EACH ROW
BEGIN
    DBMS_OUTPUT.PUT_LINE(
        'Old Mark = ' || :OLD.MARK1
    );

    DBMS_OUTPUT.PUT_LINE(
        'New Mark = ' || :NEW.MARK1
    );
END;
/

তারপর:

UPDATE SBLTRY_ISLAMIC.EMPLOYEE
SET MARK1 = 95
WHERE EMPNO = 21;

Output:

Old Mark = 80
New Mark = 95
11. Trigger দিয়ে Validation

Trigger ব্যবহার করে data validation করা যায়।

Example: Salary negative হতে দেওয়া হবে না।

ধরি table:

EMPLOYEE
---------
EMPNO
SALARY

Trigger:

CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT OR UPDATE OF SALARY
ON EMPLOYEE
FOR EACH ROW
BEGIN
    IF :NEW.SALARY < 0 THEN
        RAISE_APPLICATION_ERROR(
            -20001,
            'Salary cannot be negative.'
        );
    END IF;
END;
/

এখন:

INSERT INTO EMPLOYEE (EMPNO, SALARY)
VALUES (101, -5000);

Error:

ORA-20001: Salary cannot be negative.

Flow:

INSERT
   ↓
Trigger
   ↓
Check Salary
   ↓
Salary < 0 ?
   ↓
YES
   ↓
RAISE_APPLICATION_ERROR
   ↓
INSERT Failed
12. Audit Trigger

Trigger-এর একটি common use হলো audit করা।

ধরি employee update হলে old এবং new value একটি audit table-এ রাখতে চাই।

Audit table:

CREATE TABLE employee_audit (
    empno        NUMBER,
    old_mark1    NUMBER,
    new_mark1    NUMBER,
    changed_date DATE
);

Trigger:

CREATE OR REPLACE TRIGGER trg_employee_audit
AFTER UPDATE OF MARK1
ON SBLTRY_ISLAMIC.EMPLOYEE
FOR EACH ROW
BEGIN
    INSERT INTO employee_audit
    (
        empno,
        old_mark1,
        new_mark1,
        changed_date
    )
    VALUES
    (
        :OLD.EMPNO,
        :OLD.MARK1,
        :NEW.MARK1,
        SYSDATE
    );
END;
/

তারপর:

UPDATE SBLTRY_ISLAMIC.EMPLOYEE
SET MARK1 = 95
WHERE EMPNO = 21;

Audit table-এ automatically record তৈরি হবে।

EMPNO | OLD_MARK1 | NEW_MARK1 | CHANGED_DATE
------------------------------------------------
21    | 80        | 95        | 12-AUG-2026
13. Trigger বনাম Procedure
Trigger	Procedure
Automatically execute হয়	Manually call করতে হয়
Database event-এর সাথে যুক্ত	সাধারণ business logic
INSERT/UPDATE/DELETE event-এ কাজ করে	যেকোনো logic execute করতে পারে
সাধারণত explicitly call করা হয় না	EXEC / application থেকে call করা যায়

Example Procedure:

EXEC my_procedure();

কিন্তু Trigger:

INSERT INTO employee (...);

করলেই automatically execute হয়।

14. Trigger-এর প্রধান Types
Trigger
│
├── DML Trigger
│   ├── INSERT
│   ├── UPDATE
│   └── DELETE
│
├── DDL Trigger
│   ├── CREATE
│   ├── ALTER
│   └── DROP
│
├── Database Event Trigger
│   ├── LOGON
│   └── LOGOFF
│
└── Compound Trigger
15. Important Keywords
BEFORE

Operation-এর আগে execute হয়।

BEFORE INSERT
AFTER

Operation-এর পরে execute হয়।

AFTER INSERT
FOR EACH ROW

প্রতিটি affected row-এর জন্য execute হয়।

FOR EACH ROW
:NEW

নতুন value।

:NEW.MARK1
:OLD

পুরোনো value।

:OLD.MARK1
RAISE_APPLICATION_ERROR

Custom error তৈরি করতে ব্যবহার করা হয়।

RAISE_APPLICATION_ERROR(
    -20001,
    'Salary cannot be negative.'
);
16. Complete Practical Example
CREATE OR REPLACE TRIGGER trg_employee_validation
BEFORE INSERT OR UPDATE
ON SBLTRY_ISLAMIC.EMPLOYEE
FOR EACH ROW
BEGIN

    IF :NEW.MARK1 < 0 THEN

        RAISE_APPLICATION_ERROR(
            -20001,
            'MARK1 cannot be negative.'
        );

    END IF;

END;
/

Test:

INSERT INTO SBLTRY_ISLAMIC.EMPLOYEE
(
    EMPNO,
    MARK1,
    MARK2,
    MARK3,
    EMPADDRESS
)
VALUES
(
    30,
    -10,
    80,
    90,
    'Dhaka'
);

Result:

ORA-20001: MARK1 cannot be negative.
Key Points
Trigger = Automatically executed PL/SQL program

BEFORE
    ↓
Event-এর আগে execute

AFTER
    ↓
Event-এর পরে execute

FOR EACH ROW
    ↓
প্রতিটি row-এর জন্য execute

:NEW
    ↓
New value

:OLD
    ↓
Old value
Quick Example
CREATE OR REPLACE TRIGGER trg_test
BEFORE UPDATE ON EMPLOYEE
FOR EACH ROW
BEGIN
    DBMS_OUTPUT.PUT_LINE(
        'Old = ' || :OLD.MARK1
    );

    DBMS_OUTPUT.PUT_LINE(
        'New = ' || :NEW.MARK1
    );
END;
/

মনে রাখার সহজ formula:

Trigger
  =
Database Event
  +
Automatically Execute
  +
PL/SQL Logic
```
