### Using PL/SQL Variables
```

PL/SQL variable হলো এমন একটি memory location, যেখানে program চলাকালীন কোনো value temporarily রাখা যায়।

সহজভাবে:

Variable
   ↓
Value store করে
   ↓
Program-এর মধ্যে সেই value ব্যবহার করি
1. Basic Variable Declaration

PL/SQL-এ variable সাধারণত DECLARE section-এ declare করা হয়।

DECLARE
    v_name VARCHAR2(50);
    v_age  NUMBER;
BEGIN
    v_name := 'Abdullah';
    v_age := 25;

    DBMS_OUTPUT.PUT_LINE('Name = ' || v_name);
    DBMS_OUTPUT.PUT_LINE('Age = ' || v_age);
END;
/

Output:

Name = Abdullah
Age = 25

এখানে:

v_name VARCHAR2(50);

মানে v_name একটি variable, যার datatype VARCHAR2.

আর:

v_age NUMBER;

মানে v_age-এ numeric value রাখা যাবে।

2. Variable-এর Syntax

সাধারণ syntax:

variable_name datatype;

Example:

v_name     VARCHAR2(100);
v_salary   NUMBER(10,2);
v_joinDate DATE;
v_status   CHAR(1);
3. Variable-এ Value Assign করা

PL/SQL-এ value assign করার জন্য := ব্যবহার করা হয়।

DECLARE
    v_salary NUMBER;
BEGIN
    v_salary := 50000;

    DBMS_OUTPUT.PUT_LINE(v_salary);
END;
/
Important

PL/SQL-এ:

v_salary := 50000;

এটা assignment।

কিন্তু:

v_salary = 50000;

এভাবে assignment করা হয় না।

4. Variable Declaration + Initialization

Declaration-এর সময় সরাসরি initial value দেওয়া যায়।

DECLARE
    v_name VARCHAR2(50) := 'Abdullah';
    v_salary NUMBER := 50000;
BEGIN
    DBMS_OUTPUT.PUT_LINE(v_name);
    DBMS_OUTPUT.PUT_LINE(v_salary);
END;
/

এখানে:

v_name VARCHAR2(50) := 'Abdullah';

মানে variable তৈরি করার সময়ই value দেওয়া হয়েছে।

5. Constant Variable

যদি এমন value থাকে যেটা পরিবর্তন করা যাবে না, তাহলে CONSTANT ব্যবহার করা যায়।

DECLARE
    v_pi CONSTANT NUMBER := 3.14159;
BEGIN
    DBMS_OUTPUT.PUT_LINE(v_pi);
END;
/

এখন:

v_pi := 10;

করলে error হবে, কারণ v_pi একটি constant।

6. Variable দিয়ে Calculation

Variable ব্যবহার করে calculation করা যায়।

DECLARE
    v_salary NUMBER := 50000;
    v_bonus  NUMBER := 10000;
    v_total  NUMBER;
BEGIN
    v_total := v_salary + v_bonus;

    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_salary);
    DBMS_OUTPUT.PUT_LINE('Bonus  = ' || v_bonus);
    DBMS_OUTPUT.PUT_LINE('Total  = ' || v_total);
END;
/

Output:

Salary = 50000
Bonus  = 10000
Total  = 60000

Flow:

v_salary = 50000
v_bonus  = 10000

        ↓

v_total = v_salary + v_bonus

        ↓

v_total = 60000
7. Variable থেকে Variable-এ Value

একটি variable-এর value অন্য variable-এ assign করা যায়।

DECLARE
    v_salary NUMBER := 50000;
    v_total  NUMBER;
BEGIN
    v_total := v_salary;

    DBMS_OUTPUT.PUT_LINE('Total = ' || v_total);
END;
/

এখানে:

v_total := v_salary;

এর অর্থ হলো v_salary-এর current value v_total-এ copy করা।

8. SELECT INTO দিয়ে Variable-এ Database Value রাখা

এটা খুব গুরুত্বপূর্ণ, বিশেষ করে আপনার মতো PL/SQL procedure কাজ করার সময়।

ধরা যাক table:

EMPLOYEE
--------------------------
ID
NAME
SALARY

আমরা database থেকে salary নিয়ে variable-এ রাখতে পারি:

DECLARE
    v_salary NUMBER;
BEGIN
    SELECT salary
    INTO v_salary
    FROM employee
    WHERE id = 101;

    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_salary);
END;
/

Flow:

EMPLOYEE TABLE
      ↓
SELECT salary
      ↓
INTO v_salary
      ↓
PL/SQL Variable
গুরুত্বপূর্ণ Rule

SELECT INTO-তে সাধারণত query-টি একটি row return করবে।

যদি কোনো row না পাওয়া যায়:

NO_DATA_FOUND

আর একাধিক row পাওয়া গেলে:

TOO_MANY_ROWS

exception হতে পারে।

9. %TYPE

PL/SQL-এ খুব useful feature হলো %TYPE.

ধরা যাক:

EMPLOYEE.SALARY

এর datatype বর্তমানে:

NUMBER(10,2)

আপনি variable declare করতে পারেন:

v_salary employee.salary%TYPE;

এতে variable-এর datatype automatically employee.salary column-এর datatype অনুসরণ করবে।

Example:

DECLARE
    v_salary employee.salary%TYPE;
BEGIN
    SELECT salary
    INTO v_salary
    FROM employee
    WHERE id = 101;

    DBMS_OUTPUT.PUT_LINE(v_salary);
END;
/
কেন %TYPE ব্যবহার করব?

ধরুন database-এ পরে salary column-এর datatype পরিবর্তন হলো।

NUMBER(10,2)
        ↓
NUMBER(15,2)

আপনার variable:

v_salary employee.salary%TYPE;

হলে আলাদাভাবে datatype change করতে হবে না।

10. %ROWTYPE

একটি table-এর পুরো row variable-এর মধ্যে রাখতে %ROWTYPE ব্যবহার করা যায়।

DECLARE
    v_employee employee%ROWTYPE;
BEGIN
    SELECT *
    INTO v_employee
    FROM employee
    WHERE id = 101;

    DBMS_OUTPUT.PUT_LINE('ID = ' || v_employee.id);
    DBMS_OUTPUT.PUT_LINE('Name = ' || v_employee.name);
    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_employee.salary);
END;
/

এখানে:

v_employee employee%ROWTYPE;

মানে employee table-এর একটি complete row রাখার জন্য v_employee তৈরি হয়েছে।

Flow:

EMPLOYEE TABLE

ID | NAME | SALARY
------------------
101| Rahim| 50000
       ↓
SELECT *
       ↓
v_employee
       ↓
v_employee.id
v_employee.name
v_employee.salary
11. Variable Scope

Variable যেখানে declare করা হয়, সাধারণত সেই block-এর ভিতরেই ব্যবহার করা যায়।

DECLARE
    v_name VARCHAR2(50) := 'Abdullah';
BEGIN
    DBMS_OUTPUT.PUT_LINE(v_name);
END;
/

কিন্তু END হওয়ার পরে:

DBMS_OUTPUT.PUT_LINE(v_name);

করলে variable আর available থাকবে না।

12. আপনার Procedure-এর সাথে মিলিয়ে দেখা

আপনার SP_TRIALBAL procedure-এ অনেক variable আছে।

যেমন:

W_FIN_YEAR1 NUMBER(4);
W_FIN_YEAR2 NUMBER(4);
W_YR_END_DATE DATE;
W_ASON_DATE DATE;
W_COUNT PLS_INTEGER := 0;
V_ERR_MSG VARCHAR2(100);
V_TEMP_SER NUMBER(6);
VSQL VARCHAR2(4300);
W_BRNNAME VARCHAR2(50) := '';
P_BRNCODE NUMBER(6);
P_ASON_DATE DATE;

এগুলো সব PL/SQL variables।

যেমন:

W_COUNT PLS_INTEGER := 0;

মানে:

Variable name = W_COUNT
Datatype      = PLS_INTEGER
Initial value = 0

পরে:

W_COUNT := W_COUNT + 1;

করলে:

0 → 1 → 2 → 3 → ...
13. আপনার Procedure-এর সবচেয়ে গুরুত্বপূর্ণ Example

আপনার code-এ:

P_ASON_DATE := TO_DATE(
    PKG_SPILIT_TOKEN.V_LIST_REC(2),
    'DD-MM-YYYY'
);

এখানে P_ASON_DATE একটি variable।

Flow:

P_IN_MSG
   ↓
PKG_SPILIT_TOKEN
   ↓
V_LIST_REC(2)
   ↓
TO_DATE(...)
   ↓
P_ASON_DATE

আর:

W_FIN_YEAR1 := SP_GETFINYEAR(
    PKG_ENTITY.FN_GET_ENTITY_CODE,
    W_FROM_DATE
);

এখানে function থেকে পাওয়া result W_FIN_YEAR1 variable-এ রাখা হচ্ছে।

14. Variable-এর Main Types

আপনার PL/SQL শেখার জন্য এগুলো আগে ভালোভাবে বুঝুন:

Type	Example	Use
NUMBER	v_salary NUMBER	Number
VARCHAR2	v_name VARCHAR2(50)	Text
DATE	v_date DATE	Date
CHAR	v_status CHAR(1)	Fixed character
BOOLEAN	v_found BOOLEAN	TRUE/FALSE
%TYPE	v_sal emp.salary%TYPE	Column datatype follow
%ROWTYPE	v_emp emp%ROWTYPE	Complete row
15. সবচেয়ে গুরুত্বপূর্ণ বিষয়

PL/SQL শেখার সময় Variable-কে এইভাবে চিন্তা করুন:

DECLARE
    ↓
Variable তৈরি
    ↓
Value assign
    ↓
Calculation / SQL
    ↓
Result ব্যবহার
    ↓
END

একটি simple real example:

DECLARE
    v_name   VARCHAR2(50);
    v_salary NUMBER;
    v_bonus  NUMBER;
    v_total  NUMBER;
BEGIN

    v_name := 'Abdullah';
    v_salary := 50000;
    v_bonus := 10000;

    v_total := v_salary + v_bonus;

    DBMS_OUTPUT.PUT_LINE('Employee = ' || v_name);
    DBMS_OUTPUT.PUT_LINE('Total Salary = ' || v_total);

END;
/
```

Output:

Employee = Abdullah
Total Salary = 60000

আপনার SP_TRIALBAL বুঝতে হলে এখন সবচেয়ে গুরুত্বপূর্ণ next topic হবে PL/SQL Records → Collections → BULK COLLECT → FORALL, কারণ আপনার procedure-এ এগুলো প্রচুর ব্যবহার হয়েছে।
