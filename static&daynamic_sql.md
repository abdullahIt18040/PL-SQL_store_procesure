
# Static SQL & Dynamic SQL in PL/SQL

PL/SQL-এ SQL statement প্রধানত দুইভাবে ব্যবহার করা যায়:

1. **Static SQL**
2. **Dynamic SQL**

সহজভাবে:

```text
Static SQL
    ↓
SQL structure আগে থেকেই fixed
    ↓
PL/SQL-এর মধ্যে সরাসরি লেখা হয়
```

```text
Dynamic SQL
    ↓
SQL runtime-এ তৈরি হয়
    ↓
EXECUTE IMMEDIATE দিয়ে execute করা হয়
```

---

# 1. Static SQL

যে SQL statement-এর **structure আগে থেকেই নির্দিষ্ট থাকে**, তাকে Static SQL বলে।

## Example

```sql
DECLARE
    v_address VARCHAR2(100);
BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = 1;

    DBMS_OUTPUT.PUT_LINE(
        'Address = ' || v_address
    );

END;
/
```

এখানে SQL statement fixed:

```sql
SELECT EMPADDRESS
FROM SBLTRY_ISLAMIC.EMPLOYEE
WHERE EMPNO = 1;
```

Runtime-এ SQL-এর structure পরিবর্তন হচ্ছে না।

---

# 2. Static SQL with Variable

Static SQL-এর মধ্যে PL/SQL variable ব্যবহার করা যায়।

```sql
DECLARE
    v_empno   NUMBER := 1;
    v_address VARCHAR2(100);
BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = v_empno;

    DBMS_OUTPUT.PUT_LINE(
        'Address = ' || v_address
    );

END;
/
```

এখানে:

```sql
WHERE EMPNO = v_empno;
```

`v_empno`-এর value পরিবর্তন হতে পারে, কিন্তু SQL-এর structure fixed থাকে।

---

# 3. Static SQL Flow

```text
PL/SQL Program
      ↓
SELECT Statement
      ↓
Oracle SQL Engine
      ↓
Execute
      ↓
Result
```

---

# 4. Dynamic SQL

যখন SQL statement-এর **structure runtime-এ তৈরি বা পরিবর্তন করতে হয়**, তখন Dynamic SQL ব্যবহার করা হয়।

Dynamic SQL execute করার জন্য সবচেয়ে common command:

```sql
EXECUTE IMMEDIATE
```

---

# 5. Simple Dynamic SQL Example

```sql
DECLARE
    v_sql     VARCHAR2(500);
    v_address VARCHAR2(100);
BEGIN

    v_sql := '
        SELECT EMPADDRESS
        FROM SBLTRY_ISLAMIC.EMPLOYEE
        WHERE EMPNO = 1';

    EXECUTE IMMEDIATE v_sql
    INTO v_address;

    DBMS_OUTPUT.PUT_LINE(
        'Address = ' || v_address
    );

END;
/
```

এখানে প্রথমে SQL একটি variable-এর মধ্যে রাখা হয়েছে:

```sql
v_sql := 'SELECT ...';
```

তারপর:

```sql
EXECUTE IMMEDIATE v_sql;
```

দিয়ে SQL execute করা হয়েছে।

---

# 6. Dynamic SQL কেন ব্যবহার করব?

ধরুন table name runtime-এ পরিবর্তন হবে।

```text
EMPLOYEE
STUDENT
CUSTOMER
```

Static SQL:

```sql
SELECT * FROM EMPLOYEE;
```

এখানে table fixed।

কিন্তু Dynamic SQL:

```sql
DECLARE
    v_table_name VARCHAR2(50) := 'EMPLOYEE';
    v_sql        VARCHAR2(500);
BEGIN

    v_sql := 'SELECT COUNT(*) FROM ' || v_table_name;

    EXECUTE IMMEDIATE v_sql;

END;
/
```

যদি:

```text
v_table_name = EMPLOYEE
```

তাহলে SQL হবে:

```sql
SELECT COUNT(*) FROM EMPLOYEE;
```

আবার:

```text
v_table_name = CUSTOMER
```

হলে SQL হবে:

```sql
SELECT COUNT(*) FROM CUSTOMER;
```

অর্থাৎ table name runtime-এ পরিবর্তন হচ্ছে।

---

# 7. Dynamic SQL with Bind Variable

Dynamic SQL-এ value দেওয়ার জন্য **bind variable** ব্যবহার করা ভালো।

```sql
DECLARE
    v_sql     VARCHAR2(500);
    v_empno   NUMBER := 21;
    v_address VARCHAR2(100);
BEGIN

    v_sql := '
        SELECT EMPADDRESS
        FROM SBLTRY_ISLAMIC.EMPLOYEE
        WHERE EMPNO = :1';

    EXECUTE IMMEDIATE v_sql
    INTO v_address
    USING v_empno;

    DBMS_OUTPUT.PUT_LINE(
        'Address = ' || v_address
    );

END;
/
```

এখানে:

```sql
:1
```

হলো bind placeholder।

আর:

```sql
USING v_empno
```

এর মাধ্যমে value দেওয়া হয়েছে।

Flow:

```text
v_empno = 21
     ↓
    :1
     ↓
EXECUTE IMMEDIATE
     ↓
WHERE EMPNO = 21
```

---

# 8. Dynamic SQL with INSERT

`EXECUTE IMMEDIATE` শুধু `SELECT`-এর জন্য নয়। `INSERT`, `UPDATE`, `DELETE`-এর জন্যও ব্যবহার করা যায়।

```sql
DECLARE
    v_sql VARCHAR2(500);
BEGIN

    v_sql := '
        INSERT INTO SBLTRY_ISLAMIC.EMPLOYEE
        (EMPNO, MARK1, MARK2, MARK3, EMPADDRESS)
        VALUES (:1, :2, :3, :4, :5)';

    EXECUTE IMMEDIATE v_sql
    USING 25, 80, 85, 90, 'Dhaka';

    COMMIT;

END;
/
```

---

# 9. Dynamic SQL with UPDATE

```sql
DECLARE
    v_sql VARCHAR2(500);
BEGIN

    v_sql := '
        UPDATE SBLTRY_ISLAMIC.EMPLOYEE
        SET MARK1 = :1
        WHERE EMPNO = :2';

    EXECUTE IMMEDIATE v_sql
    USING 95, 25;

    COMMIT;

END;
/
```

---

# 10. Dynamic SQL with DELETE

```sql
DECLARE
    v_sql VARCHAR2(500);
BEGIN

    v_sql := '
        DELETE FROM SBLTRY_ISLAMIC.EMPLOYEE
        WHERE EMPNO = :1';

    EXECUTE IMMEDIATE v_sql
    USING 25;

    COMMIT;

END;
/
```

---

# 11. Dynamic SQL in `SP_TRIALBAL`

আপনার `SP_TRIALBAL` procedure-এ Dynamic SQL-এর গুরুত্বপূর্ণ example আছে:

```sql
VSQL := 'SELECT GLSUM_GLACC_CODE GLACC_CODE,
                Nvl(Sum(GLSUM_BC_CR_SUM), 0)
              - Nvl(Sum(GLSUM_BC_DB_SUM), 0)
         FROM GLSUM' || W_FIN_YEAR1 ||
        ' WHERE GLSUM_ENTITY_NUM = PKG_ENTITY.FN_GET_ENTITY_CODE';
```

তারপর:

```sql
EXECUTE IMMEDIATE VSQL
BULK COLLECT INTO V_OPBAL;
```

এখানে গুরুত্বপূর্ণ অংশ:

```sql
'GLSUM' || W_FIN_YEAR1
```

ধরুন:

```text
W_FIN_YEAR1 = 2025
```

তাহলে SQL-এর table হবে:

```sql
FROM GLSUM2025
```

আবার:

```text
W_FIN_YEAR1 = 2026
```

হলে:

```sql
FROM GLSUM2026
```

অর্থাৎ **table name runtime-এ পরিবর্তন হচ্ছে**।

এই কারণেই এখানে Dynamic SQL ব্যবহার করা হয়েছে।

---

# 12. Dynamic SQL with Conditions

আপনার procedure-এ আরও একটি Dynamic SQL pattern আছে:

```sql
IF P_BRNCODE <> 0 THEN

    VSQL := VSQL ||
            ' AND GLSUM_BRANCH_CODE = ' ||
            P_BRNCODE;

END IF;
```

ধরুন:

```text
P_BRNCODE = 101
```

তাহলে SQL-এর মধ্যে যোগ হবে:

```sql
AND GLSUM_BRANCH_CODE = 101
```

এরপর:

```sql
EXECUTE IMMEDIATE VSQL;
```

দিয়ে query execute হবে।

---

# 13. `BULK COLLECT` with Dynamic SQL

আপনার `SP_TRIALBAL`-এ:

```sql
EXECUTE IMMEDIATE VSQL
BULK COLLECT INTO V_OPBAL;
```

এখানে Dynamic SQL-এর result একসাথে collection-এর মধ্যে নেওয়া হচ্ছে।

Flow:

```text
Dynamic SQL
     ↓
EXECUTE IMMEDIATE
     ↓
Query Result
     ↓
BULK COLLECT
     ↓
V_OPBAL
```

---

# 14. SQL Injection

Dynamic SQL-এ সরাসরি string concatenation করলে security risk থাকতে পারে।

### ❌ Risky

```sql
v_sql := 'SELECT *
          FROM EMPLOYEE
          WHERE EMPNO = ' || v_empno;
```

বিশেষ করে `v_empno` যদি user input থেকে আসে।

### ✅ Better

Bind variable ব্যবহার করুন:

```sql
v_sql := 'SELECT EMPADDRESS
          FROM EMPLOYEE
          WHERE EMPNO = :1';

EXECUTE IMMEDIATE v_sql
INTO v_address
USING v_empno;
```

তাই:

```text
String Concatenation
        ↓
SQL Injection Risk
```

এর পরিবর্তে:

```text
Bind Variable
        ↓
Safer SQL
```

---

# 15. Static SQL vs Dynamic SQL

| Feature            | Static SQL     | Dynamic SQL                  |
| ------------------ | -------------- | ---------------------------- |
| SQL Structure      | Fixed          | Runtime-এ তৈরি হতে পারে      |
| SQL জানা থাকে      | Compile time   | Runtime                      |
| Common Syntax      | `SELECT INTO`  | `EXECUTE IMMEDIATE`          |
| Table Name         | Usually fixed  | Dynamic হতে পারে             |
| Column Name        | Usually fixed  | Dynamic হতে পারে             |
| Complexity         | সহজ            | তুলনামূলক complex            |
| Performance        | সাধারণত ভালো   | অতিরিক্ত parsing হতে পারে    |
| SQL Injection Risk | কম             | বেশি যদি ভুলভাবে তৈরি করা হয় |
| Use Case           | Normal queries | Dynamic queries              |

---

# 16. Static SQL Example

```sql
DECLARE
    v_address VARCHAR2(100);
BEGIN

    SELECT EMPADDRESS
    INTO v_address
    FROM SBLTRY_ISLAMIC.EMPLOYEE
    WHERE EMPNO = 21;

    DBMS_OUTPUT.PUT_LINE(
        'Address = ' || v_address
    );

END;
/
```

SQL structure fixed:

```text
EMPLOYEE
   ↓
SELECT
   ↓
WHERE EMPNO
   ↓
Execute
```

---

# 17. Dynamic SQL Example

```sql
DECLARE
    v_sql     VARCHAR2(500);
    v_address VARCHAR2(100);
    v_empno   NUMBER := 21;
BEGIN

    v_sql := '
        SELECT EMPADDRESS
        FROM SBLTRY_ISLAMIC.EMPLOYEE
        WHERE EMPNO = :1';

    EXECUTE IMMEDIATE v_sql
    INTO v_address
    USING v_empno;

    DBMS_OUTPUT.PUT_LINE(
        'Address = ' || v_address
    );

END;
/
```

Flow:

```text
v_sql
  ↓
SQL String
  ↓
EXECUTE IMMEDIATE
  ↓
Oracle SQL Engine
  ↓
Result
```

---

# 18. Key Points

### Static SQL

```text
SQL fixed
    ↓
Easy to write
    ↓
Compile-time validation
    ↓
Usually preferred when SQL structure does not change
```

### Dynamic SQL

```text
SQL runtime-এ তৈরি
    ↓
EXECUTE IMMEDIATE
    ↓
Dynamic table/column/condition
    ↓
Useful for flexible queries
```

### Remember

```text
STATIC SQL
= SQL structure fixed

DYNAMIC SQL
= SQL structure runtime-এ তৈরি/পরিবর্তন

EXECUTE IMMEDIATE
= Dynamic SQL execute করার common method

USING
= Dynamic SQL-এ bind value পাঠানোর জন্য

BULK COLLECT
= Multiple rows একসাথে collection-এ নেওয়ার জন্য
```

---

## `SP_TRIALBAL`-এর ক্ষেত্রে

আপনার procedure-এ এই অংশটি বিশেষভাবে মনে রাখুন:

```sql
VSQL := 'SELECT ... FROM GLSUM' || W_FIN_YEAR1 || ' ...';

EXECUTE IMMEDIATE VSQL
BULK COLLECT INTO V_OPBAL;
```

এখানে:

```text
W_FIN_YEAR1
     ↓
Dynamic Table Name
     ↓
VSQL তৈরি
     ↓
EXECUTE IMMEDIATE
     ↓
BULK COLLECT
     ↓
V_OPBAL Collection
```

এটাই আপনার `SP_TRIALBAL` বুঝতে **Static SQL → Dynamic SQL → EXECUTE IMMEDIATE → BULK COLLECT** শেখার গুরুত্বপূর্ণ chain।
