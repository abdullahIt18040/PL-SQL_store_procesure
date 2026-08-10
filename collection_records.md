# PL/SQL Collections and Records

PL/SQL-এ **Record** এবং **Collection** ব্যবহার করা হয় একাধিক related data এবং অনেকগুলো data একসাথে memory-তে রাখার জন্য।

সহজভাবে:

```text
Record
   ↓
একটি row-এর মতো
   ↓
Multiple fields

Collection
   ↓
অনেকগুলো value/record
   ↓
একসাথে store
```

---

# 1. PL/SQL Record

একটি `RECORD` হলো এমন একটি structure যেখানে একাধিক related field রাখা যায়।

### Example

```sql
DECLARE

    TYPE employee_record IS RECORD (
        empno    NUMBER,
        name     VARCHAR2(50),
        salary   NUMBER,
        address  VARCHAR2(100)
    );

    v_employee employee_record;

BEGIN

    v_employee.empno := 101;
    v_employee.name := 'Abdullah';
    v_employee.salary := 50000;
    v_employee.address := 'Dhaka';

    DBMS_OUTPUT.PUT_LINE('Employee No = ' || v_employee.empno);
    DBMS_OUTPUT.PUT_LINE('Name = ' || v_employee.name);
    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_employee.salary);
    DBMS_OUTPUT.PUT_LINE('Address = ' || v_employee.address);

END;
/
```

Output:

```text
Employee No = 101
Name = Abdullah
Salary = 50000
Address = Dhaka
```

### Structure

```text
employee_record
│
├── empno
├── name
├── salary
└── address
```

একটি Record-কে conceptually Java class-এর সাথে তুলনা করা যায়:

```java
class Employee {
    int empno;
    String name;
    double salary;
    String address;
}
```

---

# 2. `%ROWTYPE`

Database table-এর একটি complete row রাখার জন্য `%ROWTYPE` ব্যবহার করা যায়।

ধরা যাক table:

```text
EMPLOYEE
----------------
EMPNO
MARK1
MARK2
MARK3
EMPADDRESS
```

Example:

```sql
DECLARE

    v_employee SBLTRY_ISLAMIC.EMPLOYEE%ROWTYPE;

BEGIN

    SELECT *
    INTO v_employee
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = 21;

    DBMS_OUTPUT.PUT_LINE('EMPNO = ' || v_employee.EMPNO);
    DBMS_OUTPUT.PUT_LINE('MARK1 = ' || v_employee.MARK1);
    DBMS_OUTPUT.PUT_LINE('MARK2 = ' || v_employee.MARK2);
    DBMS_OUTPUT.PUT_LINE('ADDRESS = ' || v_employee.EMPADDRESS);

END;
/
```

এখানে:

```sql
v_employee SBLTRY_ISLAMIC.EMPLOYEE%ROWTYPE;
```

এর অর্থ হলো `EMPLOYEE` table-এর একটি complete row রাখার জন্য `v_employee` variable তৈরি করা হয়েছে।

---

# 3. PL/SQL Collection

Collection হলো একই ধরনের একাধিক value অথবা record রাখার structure।

Example:

```text
Index    Value
-----    --------
1        Abdullah
2        Rahim
3        Karim
4        Hasan
```

PL/SQL-এ প্রধানত ৩ ধরনের Collection আছে:

```text
PL/SQL Collections
│
├── Associative Array
├── Nested Table
└── VARRAY
```

---

# 4. Associative Array

Associative Array-এর basic syntax:

```sql
TYPE collection_name IS TABLE OF datatype
INDEX BY index_type;
```

### Example

```sql
DECLARE

    TYPE name_table IS TABLE OF VARCHAR2(50)
        INDEX BY PLS_INTEGER;

    v_names name_table;

BEGIN

    v_names(1) := 'Abdullah';
    v_names(2) := 'Rahim';
    v_names(3) := 'Karim';

    DBMS_OUTPUT.PUT_LINE(v_names(1));
    DBMS_OUTPUT.PUT_LINE(v_names(2));
    DBMS_OUTPUT.PUT_LINE(v_names(3));

END;
/
```

Output:

```text
Abdullah
Rahim
Karim
```

এখানে:

```sql
TYPE name_table IS TABLE OF VARCHAR2(50)
INDEX BY PLS_INTEGER;
```

Collection type তৈরি করেছে।

আর:

```sql
v_names name_table;
```

Collection variable তৈরি করেছে।

---

# 5. Record + Collection

একটি Collection-এর মধ্যে অনেকগুলো Record রাখা যায়।

প্রথমে Record:

```sql
TYPE employee_record IS RECORD (
    empno   NUMBER,
    name    VARCHAR2(50),
    salary  NUMBER
);
```

তারপর Record-এর Collection:

```sql
TYPE employee_table IS TABLE OF employee_record
INDEX BY PLS_INTEGER;
```

তারপর Collection variable:

```sql
v_employees employee_table;
```

### Complete Example

```sql
DECLARE

    TYPE employee_record IS RECORD (
        empno   NUMBER,
        name    VARCHAR2(50),
        salary  NUMBER
    );

    TYPE employee_table IS TABLE OF employee_record
        INDEX BY PLS_INTEGER;

    v_employees employee_table;

BEGIN

    v_employees(1).empno := 101;
    v_employees(1).name := 'Abdullah';
    v_employees(1).salary := 50000;

    v_employees(2).empno := 102;
    v_employees(2).name := 'Rahim';
    v_employees(2).salary := 60000;

    DBMS_OUTPUT.PUT_LINE(
        'Name = ' || v_employees(1).name
    );

    DBMS_OUTPUT.PUT_LINE(
        'Salary = ' || v_employees(2).salary
    );

END;
/
```

### Structure

```text
v_employees
│
├── [1]
│    ├── empno   = 101
│    ├── name    = Abdullah
│    └── salary  = 50000
│
└── [2]
     ├── empno   = 102
     ├── name    = Rahim
     └── salary  = 60000
```

---

# 6. BULK COLLECT

`BULK COLLECT` ব্যবহার করে database থেকে একসাথে অনেকগুলো row Collection-এর মধ্যে নেওয়া যায়।

Normally:

```sql
SELECT ...
INTO ...
```

একটি row নিয়ে কাজ করে।

কিন্তু:

```sql
BULK COLLECT INTO
```

একসাথে multiple rows collection-এর মধ্যে নিয়ে আসে।

### Example

```sql
DECLARE

    TYPE emp_record IS RECORD (
        empno    NUMBER,
        address  VARCHAR2(100)
    );

    TYPE emp_table IS TABLE OF emp_record
        INDEX BY PLS_INTEGER;

    v_employees emp_table;

BEGIN

    SELECT EMPNO, EMPADDRESS
    BULK COLLECT INTO v_employees
    FROM SBLTRY_ISLAMIC.EMPLOYEE;

    FOR i IN 1 .. v_employees.COUNT LOOP

        DBMS_OUTPUT.PUT_LINE(
            'EMP = ' || v_employees(i).empno ||
            ', Address = ' || v_employees(i).address
        );

    END LOOP;

END;
/
```

### Flow

```text
EMPLOYEE TABLE
      ↓
   SELECT
      ↓
BULK COLLECT
      ↓
Collection
      ↓
[1] Record
[2] Record
[3] Record
[4] Record
```

---

# 7. Collection `.COUNT`

Collection-এর মধ্যে কয়টি element আছে তা জানতে:

```sql
v_employees.COUNT
```

Example:

```sql
FOR i IN 1 .. v_employees.COUNT LOOP

    DBMS_OUTPUT.PUT_LINE(
        v_employees(i).empno
    );

END LOOP;
```

যদি Collection-এ 5টি element থাকে:

```text
v_employees.COUNT = 5
```

তাহলে loop হবে:

```text
1
2
3
4
5
```

---

# 8. Collection `.FIRST`

Collection-এর প্রথম index বের করতে:

```sql
v_collection.FIRST
```

Example:

```sql
idx := v_numbers.FIRST;
```

---

# 9. Collection `.NEXT`

বর্তমান index-এর পরের index বের করতে:

```sql
v_collection.NEXT(index)
```

Example:

```sql
idx := v_numbers.FIRST;

WHILE idx IS NOT NULL LOOP

    DBMS_OUTPUT.PUT_LINE(
        'Index = ' || idx ||
        ', Value = ' || v_numbers(idx)
    );

    idx := v_numbers.NEXT(idx);

END LOOP;
```

---

# 10. FIRST + NEXT Example

```sql
DECLARE

    TYPE number_table IS TABLE OF NUMBER
        INDEX BY PLS_INTEGER;

    v_numbers number_table;

    idx PLS_INTEGER;

BEGIN

    v_numbers(10) := 100;
    v_numbers(20) := 200;
    v_numbers(30) := 300;

    idx := v_numbers.FIRST;

    WHILE idx IS NOT NULL LOOP

        DBMS_OUTPUT.PUT_LINE(
            'Index = ' || idx ||
            ', Value = ' || v_numbers(idx)
        );

        idx := v_numbers.NEXT(idx);

    END LOOP;

END;
/
```

Output:

```text
Index = 10, Value = 100
Index = 20, Value = 200
Index = 30, Value = 300
```

---

# 11. Collection `.EXISTS`

কোনো index Collection-এর মধ্যে আছে কিনা check করতে:

```sql
v_collection.EXISTS(index)
```

Example:

```sql
IF v_numbers.EXISTS(10) THEN
    DBMS_OUTPUT.PUT_LINE('10 exists');
END IF;
```

### আপনার `SP_TRIALBAL`-এর Example

আপনার procedure-এ:

```sql
IF V_TRIALOPBAL.EXISTS(
       V_TEMPTRIALBAL(IDX).TRIALBAL_GLACCESS_CODE
   )
THEN
```

এর অর্থ:

> `TRIALBAL_GLACCESS_CODE` value-টি `V_TRIALOPBAL` Collection-এর মধ্যে আছে কিনা check করা হচ্ছে।

---

# 12. FORALL

`FORALL` Collection-এর data ব্যবহার করে bulk DML operation করার জন্য ব্যবহৃত হয়।

যেমন:

```sql
FORALL IDX IN 1 .. V_TRIALBAL.COUNT
    INSERT INTO RTMPTRIALBAL
    VALUES V_TRIALBAL(IDX);
```

এখানে Collection-এর অনেকগুলো record একসাথে database table-এ insert করা হচ্ছে।

### Flow

```text
V_TRIALBAL
     ↓
Many Records
     ↓
FORALL
     ↓
INSERT
     ↓
RTMPTRIALBAL
```

---

# 13. আপনার SP_TRIALBAL-এর Record

আপনার procedure-এ:

```sql
TYPE TRIALBAL IS RECORD(
    TRIALBAL_TMP_SER       NUMBER(6),
    TRIALBAL_BRN_CODE      NUMBER(6),
    TRIALBAL_AST_LIAB      CHAR(1),
    TRIALBAL_GLCAT_CODE    VARCHAR2(3),
    TRIALBAL_GLACCESS_CODE VARCHAR2(15),
    TRIALBAL_EXTGL_DESC    VARCHAR2(50),
    TRIALBAL_GL_HEAD       NUMBER(6),
    TRIALBAL_GL_NAME       VARCHAR2(50),
    TRIALBAL_GL_TYPE       CHAR(1),
    TRIALBAL_BRN_NAME      VARCHAR2(50),
    TRIALBAL_OPNG_BAL      NUMBER(18,3),
    TRIALBAL_TODAY_DBS     NUMBER(18,3),
    TRIALBAL_TODAY_CRS     NUMBER(18,3)
);
```

এটি একটি **Record Type**।

একটি Trial Balance record-এর মধ্যে 13টি field রয়েছে।

---

# 14. আপনার SP_TRIALBAL-এর Collection

এরপর:

```sql
TYPE TABTRIALBAL IS TABLE OF TRIALBAL
INDEX BY PLS_INTEGER;
```

এটি হলো:

```text
Collection of TRIALBAL Records
```

তারপর:

```sql
V_TRIALBAL TABTRIALBAL;
```

এটি হলো actual Collection variable।

### Concept

```text
TRIALBAL
    ↓
One Trial Balance Record
    ↓
TABTRIALBAL
    ↓
Collection of Records
    ↓
V_TRIALBAL
```

---

# 15. `V_TRIALBAL(1)` কী?

আপনার code:

```sql
V_TRIALBAL(1)
```

একটি Record নির্দেশ করে।

যেমন:

```sql
V_TRIALBAL(1).TRIALBAL_GLACCESS_CODE
```

মানে প্রথম Record-এর `TRIALBAL_GLACCESS_CODE` field।

Conceptually:

```text
V_TRIALBAL

[1]
 ├── GLACCESS_CODE = '1001001'
 ├── GL_NAME       = 'Cash'
 ├── OPNG_BAL      = 50000
 ├── TODAY_DBS     = 10000
 └── TODAY_CRS     = 5000

[2]
 ├── GLACCESS_CODE = '1001002'
 ├── GL_NAME       = 'Bank'
 ├── OPNG_BAL      = 80000
 ├── TODAY_DBS     = 20000
 └── TODAY_CRS     = 10000
```

---

# 16. `INDEX BY PLS_INTEGER`

আপনার code:

```sql
TYPE TABTRIALBAL IS TABLE OF TRIALBAL
INDEX BY PLS_INTEGER;
```

এখানে:

```text
TABLE OF TRIALBAL
```

মানে Collection-এর প্রতিটি element হবে `TRIALBAL` Record।

আর:

```text
INDEX BY PLS_INTEGER
```

মানে Collection-এর index হবে integer type।

Example:

```sql
V_TRIALBAL(1)
V_TRIALBAL(2)
V_TRIALBAL(3)
```

---

# 17. `INDEX BY VARCHAR2`

আপনার procedure-এ আরও গুরুত্বপূর্ণ Collection আছে:

```sql
TYPE TBLOPBALANCE IS TABLE OF RECOPBALANCE
INDEX BY VARCHAR2(15);
```

এখানে Collection-এর index number নয়, **VARCHAR2 key**।

Example:

```sql
V_TRIALOPBAL('1001001')
V_TRIALOPBAL('1001002')
V_TRIALOPBAL('1001003')
```

এটি অনেকটা Java-এর:

```java
Map<String, Employee>
```

এর মতো concept।

---

# 18. Record vs Collection

| Feature      | Record             | Collection          |
| ------------ | ------------------ | ------------------- |
| কী রাখে      | Multiple fields    | Multiple elements   |
| Example      | একটি Employee      | অনেক Employee       |
| Access       | `v_emp.name`       | `v_emp(1)`          |
| Main purpose | Related data group | Multiple data store |

সহজভাবে:

```text
RECORD
= একটি structured data

COLLECTION
= অনেক data

COLLECTION OF RECORD
= অনেকগুলো structured data
```

---

# 19. BULK COLLECT vs FORALL

এই দুইটি একসাথে মনে রাখুন:

```text
BULK COLLECT

Database
   ↓
Collection
```

আর:

```text
FORALL

Collection
   ↓
Database
```

অর্থাৎ:

```text
             BULK COLLECT
Database ─────────────────→ Collection
                              │
                              │ Processing
                              ↓
                           FORALL
                              │
                              ↓
Database ←────────────────────┘
```

---

# 20. Important Collection Methods

| Method   | কাজ                       |
| -------- | ------------------------- |
| `COUNT`  | কতগুলো element আছে        |
| `FIRST`  | প্রথম index               |
| `LAST`   | শেষ index                 |
| `NEXT`   | পরবর্তী index             |
| `PRIOR`  | আগের index                |
| `EXISTS` | index আছে কিনা            |
| `DELETE` | element/collection delete |

Example:

```sql
v_collection.COUNT
v_collection.FIRST
v_collection.LAST
v_collection.NEXT(index)
v_collection.PRIOR(index)
v_collection.EXISTS(index)
v_collection.DELETE
```

---

# 21. Quick Revision

```text
RECORD
↓
একাধিক related field
```

```text
COLLECTION
↓
একাধিক value/record
```

```text
RECORD + COLLECTION
↓
অনেকগুলো structured record
```

```text
BULK COLLECT
↓
Database → Collection
```

```text
FORALL
↓
Collection → Database
```

### আপনার `SP_TRIALBAL`-এর ক্ষেত্রে

```text
TRIALBAL
    ↓
RECORD
    ↓
TABTRIALBAL
    ↓
COLLECTION
    ↓
V_TRIALBAL
    ↓
Many Trial Balance Records
    ↓
FORALL
    ↓
RTMPTRIALBAL
```

### সবচেয়ে গুরুত্বপূর্ণ চারটি Topic

```text
1. RECORD
2. ASSOCIATIVE ARRAY
3. BULK COLLECT
4. FORALL
```

এই চারটি ভালোভাবে বুঝলে আপনার `SP_TRIALBAL`-এর Collection-related code অনেক সহজে বুঝতে পারবেন।
