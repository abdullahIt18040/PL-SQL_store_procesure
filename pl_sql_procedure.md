# PL/SQL – Procedures

PL/SQL-এ **Procedure** হলো একটি named PL/SQL program unit, যেখানে SQL এবং PL/SQL logic রাখা যায়। Procedure database-এ **stored** থাকে এবং প্রয়োজন অনুযায়ী বারবার execute করা যায়।

---

## 1. Basic Procedure

### Syntax

```sql
CREATE OR REPLACE PROCEDURE procedure_name
AS
BEGIN
    -- PL/SQL statements
END;
/
```

### Example

```sql
CREATE OR REPLACE PROCEDURE greet_user
AS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello Abdullah!');
END;
/
```

Execute:

```sql
EXEC greet_user;
```

Output:

```text
Hello Abdullah!
```

---

# 2. Procedure Structure

```sql
CREATE OR REPLACE PROCEDURE procedure_name(
    parameter1 IN datatype,
    parameter2 OUT datatype
)
AS

    -- Declaration section
    v_variable NUMBER;

BEGIN

    -- Executable section
    -- SQL / PL-SQL logic

EXCEPTION

    -- Exception handling

END;
/
```

### Main Sections

```text
CREATE OR REPLACE PROCEDURE
            ↓
       Parameters
            ↓
            AS
            ↓
       Declaration
            ↓
          BEGIN
            ↓
      Business Logic
            ↓
       EXCEPTION
            ↓
           END
```

---

# 3. Procedure with Variable

Procedure-এর ভিতরে local variable declare করা যায়।

```sql
CREATE OR REPLACE PROCEDURE show_salary
AS
    v_salary NUMBER;
BEGIN
    v_salary := 50000;

    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_salary);
END;
/
```

Execute:

```sql
EXEC show_salary;
```

Output:

```text
Salary = 50000
```

---

# 4. Procedure with IN Parameter

`IN` parameter ব্যবহার করে procedure-এ বাইরে থেকে value পাঠানো হয়।

```sql
CREATE OR REPLACE PROCEDURE greet_employee(
    p_name IN VARCHAR2
)
AS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello ' || p_name);
END;
/
```

Execute:

```sql
EXEC greet_employee('Abdullah');
```

Output:

```text
Hello Abdullah
```

### Flow

```text
'Abdullah'
     ↓
   p_name
     ↓
 Procedure
     ↓
Hello Abdullah
```

---

# 5. Multiple IN Parameters

একাধিক parameter ব্যবহার করা যায়।

```sql
CREATE OR REPLACE PROCEDURE calculate_total(
    p_salary IN NUMBER,
    p_bonus  IN NUMBER
)
AS
    v_total NUMBER;
BEGIN
    v_total := p_salary + p_bonus;

    DBMS_OUTPUT.PUT_LINE('Total = ' || v_total);
END;
/
```

Execute:

```sql
EXEC calculate_total(50000, 10000);
```

Output:

```text
Total = 60000
```

---

# 6. Procedure Parameter Modes

PL/SQL Procedure-এ প্রধানত তিন ধরনের parameter mode আছে:

| Mode     | Purpose                                |
| -------- | -------------------------------------- |
| `IN`     | Procedure-এ input পাঠায়                |
| `OUT`    | Procedure থেকে result বাইরে পাঠায়      |
| `IN OUT` | Input নেয় এবং পরিবর্তিত value ফেরত দেয় |

---

# 7. OUT Parameter

`OUT` parameter ব্যবহার করে procedure থেকে result বাইরে পাঠানো যায়।

```sql
CREATE OR REPLACE PROCEDURE calculate_salary(
    p_salary IN NUMBER,
    p_bonus  IN NUMBER,
    p_total  OUT NUMBER
)
AS
BEGIN
    p_total := p_salary + p_bonus;
END;
/
```

Call করার সময় একটি variable ব্যবহার করতে হবে:

```sql
DECLARE
    v_total NUMBER;
BEGIN
    calculate_salary(50000, 10000, v_total);

    DBMS_OUTPUT.PUT_LINE(
        'Total Salary = ' || v_total
    );
END;
/
```

Output:

```text
Total Salary = 60000
```

### Flow

```text
p_salary = 50000
p_bonus  = 10000
       ↓
   PROCEDURE
       ↓
p_total = 60000
       ↓
v_total = 60000
```

---

# 8. Procedure with SELECT INTO

Database থেকে data নিয়ে variable-এ রাখা যায় `SELECT INTO` ব্যবহার করে।

Example:

```sql
CREATE OR REPLACE PROCEDURE get_employee_address(
    p_empno IN NUMBER
)
AS
    v_address SBLTRY_ISLAMIC.EMPLOYEE.EMPADDRESS%TYPE;
BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = p_empno;

    DBMS_OUTPUT.PUT_LINE(
        'Employee Address = ' || v_address
    );

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Employee not found');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(
            'Error = ' || SQLERRM
        );
END;
/
```

Execute:

```sql
EXEC get_employee_address(21);
```

যদি employee পাওয়া যায়:

```text
Employee Address = Dhaka
```

যদি employee না পাওয়া যায়:

```text
Employee not found
```

---

# 9. Procedure with INSERT

Procedure ব্যবহার করে database-এ data insert করা যায়।

```sql
CREATE OR REPLACE PROCEDURE add_employee(
    p_empno   IN NUMBER,
    p_mark1   IN NUMBER,
    p_mark2   IN NUMBER,
    p_mark3   IN NUMBER,
    p_address IN VARCHAR2
)
AS
BEGIN

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
        p_empno,
        p_mark1,
        p_mark2,
        p_mark3,
        p_address
    );

    DBMS_OUTPUT.PUT_LINE(
        'Employee inserted successfully'
    );

END;
/
```

Execute:

```sql
EXEC add_employee(25, 80, 85, 90, 'Dhaka');
```

তারপর transaction save করতে:

```sql
COMMIT;
```

---

# 10. Procedure with UPDATE

```sql
CREATE OR REPLACE PROCEDURE update_address(
    p_empno    IN NUMBER,
    p_address  IN VARCHAR2
)
AS
BEGIN

    UPDATE SBLTRY_ISLAMIC.EMPLOYEE
    SET EMPADDRESS = p_address
    WHERE EMPNO = p_empno;

    DBMS_OUTPUT.PUT_LINE(
        'Address updated'
    );

END;
/
```

Execute:

```sql
EXEC update_address(21, 'Mirpur, Dhaka');
```

---

# 11. Procedure with DELETE

```sql
CREATE OR REPLACE PROCEDURE delete_employee(
    p_empno IN NUMBER
)
AS
BEGIN

    DELETE FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = p_empno;

    DBMS_OUTPUT.PUT_LINE(
        'Employee deleted'
    );

END;
/
```

Execute:

```sql
EXEC delete_employee(21);
```

---

# 12. Procedure with Exception Handling

Procedure-এর ভিতরে error handle করার জন্য `EXCEPTION` section ব্যবহার করা যায়।

```sql
CREATE OR REPLACE PROCEDURE find_employee(
    p_empno IN NUMBER
)
AS
    v_address VARCHAR2(100);
BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = p_empno;

    DBMS_OUTPUT.PUT_LINE(
        'Address = ' || v_address
    );

EXCEPTION

    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE(
            'No employee found'
        );

    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE(
            'Multiple employees found'
        );

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(
            'Error = ' || SQLERRM
        );

END;
/
```

---

# 13. Procedure vs Anonymous Block

## Anonymous PL/SQL Block

```sql
DECLARE
    v_name VARCHAR2(50);
BEGIN
    v_name := 'Abdullah';

    DBMS_OUTPUT.PUT_LINE(v_name);
END;
/
```

Anonymous block-এর কোনো নাম নেই এবং এটি সাধারণত reusable database object হিসেবে stored থাকে না।

## Procedure

```sql
CREATE OR REPLACE PROCEDURE show_name
AS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Abdullah');
END;
/
```

Procedure-এর একটি নাম থাকে এবং database-এ stored থাকে।

| Anonymous Block              | Procedure                       |
| ---------------------------- | ------------------------------- |
| নাম নেই                      | নাম আছে                         |
| সাধারণত একবার execute করা হয় | বারবার execute করা যায়          |
| Stored database object নয়    | Stored database object          |
| Reuse করা কঠিন               | সহজে reuse করা যায়              |
| `DECLARE` দিয়ে শুরু হতে পারে | `CREATE PROCEDURE` দিয়ে তৈরি হয় |

---

# 14. Nested Procedure

একটি procedure-এর ভিতরে আরেকটি local procedure তৈরি করা যায়।

```sql
CREATE OR REPLACE PROCEDURE main_procedure
AS

    PROCEDURE print_message
    AS
    BEGIN
        DBMS_OUTPUT.PUT_LINE(
            'Hello from nested procedure'
        );
    END;

BEGIN

    print_message;

END;
/
```

এখানে `print_message` হলো একটি **nested/local procedure**।

এটি শুধু `main_procedure`-এর scope-এর মধ্যে ব্যবহার করা যায়।

---

# 15. Your `SP_TRIALBAL` Example

আপনার বড় procedure:

```sql
CREATE OR REPLACE PROCEDURE ICBSPROD_28JAN2025.SP_TRIALBAL(
    V_ENTITY_NUM IN NUMBER,
    P_IN_MSG     IN VARCHAR2,
    P_TEMPSER    OUT NUMBER,
    P_ERR_MSG    OUT VARCHAR2
)
AS
```

এটিও একটি PL/SQL Procedure।

এখানে:

### IN Parameters

```text
V_ENTITY_NUM
P_IN_MSG
```

এগুলো procedure-এ input হিসেবে আসে।

### OUT Parameters

```text
P_TEMPSER
P_ERR_MSG
```

এগুলো procedure থেকে result বাইরে পাঠায়।

Flow:

```text
Input
  ↓
V_ENTITY_NUM
P_IN_MSG
  ↓
SP_TRIALBAL
  ↓
Business Logic
  ↓
P_TEMPSER
P_ERR_MSG
  ↓
Output
```

---

# 16. Nested Procedures in `SP_TRIALBAL`

আপনার `SP_TRIALBAL`-এর ভিতরে:

```sql
PROCEDURE UPDATE_RTMPTRIALBAL AS
BEGIN
    ...
END;
```

এবং:

```sql
PROCEDURE GET_GLSUM_DTLS_SAMEFINYR AS
BEGIN
    ...
END;
```

এগুলো হলো **nested/local procedures**।

অর্থাৎ:

```text
SP_TRIALBAL
    │
    ├── UPDATE_RTMPTRIALBAL
    │
    ├── GET_GLSUM_DTLS_SAMEFINYR
    │
    └── GET_GLSUM_DTLS_DIFFFINYR
```

এগুলো `SP_TRIALBAL`-এর internal logic আলাদা আলাদা অংশে ভাগ করতে সাহায্য করে।

---

# 17. Important Points

PL/SQL Procedure সম্পর্কে মনে রাখুন:

```text
1. Procedure একটি named PL/SQL program unit
2. Database-এ stored থাকে
3. বারবার execute করা যায়
4. IN parameter দিয়ে input নেওয়া যায়
5. OUT parameter দিয়ে result ফেরত দেওয়া যায়
6. IN OUT parameter দিয়ে input নিয়ে পরিবর্তিত value ফেরত দেওয়া যায়
7. SQL এবং PL/SQL একসাথে ব্যবহার করা যায়
8. Exception handling করা যায়
9. Local variable declare করা যায়
10. Procedure-এর ভিতরে nested procedure থাকতে পারে
```

## Quick Revision

```text
             PROCEDURE
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
       IN                  OUT
     Input                Output
        │                   ↑
        └──────→ LOGIC ─────┘
                   │
             SQL + PL/SQL
                   │
              EXCEPTION
```

### One-Line Definition

> **A PL/SQL Procedure is a named, stored PL/SQL program unit that performs a specific task and can accept parameters and return values through OUT/IN OUT parameters.**
