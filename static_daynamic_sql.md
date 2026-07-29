# Static & Dynamic SQL in Oracle (বাংলা)

## Static SQL

### কী?

| BLOB        | CLOB           |
| ----------- | -------------- |
| Binary Data | Character Data |
| Image       | Text           |
| Video       | Article        |
| PDF         | XML            |
| Audio       | JSON           |


**Static SQL** হলো এমন SQL Statement যা **Compile Time**-এ নির্ধারিত থাকে। অর্থাৎ, SQL Query আগে থেকেই কোডে লেখা থাকে এবং Program চলার সময় পরিবর্তন হয় না।

### বৈশিষ্ট্য

* SQL আগে থেকেই লেখা থাকে।
* Compile Time-এ Syntax Check হয়।
* Execute হতে দ্রুত।
* Performance ভালো।
* Syntax Error আগে থেকেই ধরা পড়ে।

### Example

```sql
DECLARE
    v_salary NUMBER;
BEGIN
    SELECT salary
    INTO v_salary
    FROM employee
    WHERE empno = 1001;

    DBMS_OUTPUT.PUT_LINE(v_salary);
END;
/
```

এখানে Query সবসময় একই থাকবে, তাই এটি **Static SQL**।

---

## Dynamic SQL

### কী?

**Dynamic SQL** হলো এমন SQL Statement যা **Runtime**-এ String আকারে তৈরি হয় এবং পরে Execute করা হয়।

### বৈশিষ্ট্য

* SQL Runtime-এ তৈরি হয়।
* User Input অনুযায়ী Query পরিবর্তন করা যায়।
* Flexible Report তৈরি করা যায়।
* Compile Time-এ SQL জানা থাকে না।
* Runtime-এ Parse ও Execute হয়।

### Example

```sql
DECLARE
    v_sql VARCHAR2(200);
BEGIN
    v_sql := 'UPDATE employee
              SET salary = 50000
              WHERE empno = 1001';

    EXECUTE IMMEDIATE v_sql;
END;
/
```

এখানে SQL Runtime-এ তৈরি হয়েছে, তাই এটি **Dynamic SQL**।

---

# Dynamic SQL Example (User Input অনুযায়ী)

```sql
DECLARE
    v_table VARCHAR2(30) := 'employee';
    v_sql   VARCHAR2(200);
BEGIN
    v_sql := 'SELECT COUNT(*) FROM ' || v_table;

    DBMS_OUTPUT.PUT_LINE(v_sql);
END;
/
```

Output

```text
SELECT COUNT(*) FROM employee
```

---

# আপনার Project-এর Example

```sql
w_sql_extgl := 'SELECT EXTGL_ACCESS_CODE,
                       EXTGL_GL_HEAD,
                       EXTGL_SUB_GL_HEAD,
                       EXTGL_BRK_GL_HEAD
                FROM EXTGL';

IF NVL(P_GL_HEAD,0) <> 0 THEN
    w_sql_extgl :=
        w_sql_extgl ||
        ' WHERE EXTGL_GL_HEAD = ' || P_GL_HEAD;
END IF;

OPEN rc_extgl FOR w_sql_extgl;
```

ধরুন,

```text
P_GL_HEAD = 101
```

Final Query হবে

```sql
SELECT EXTGL_ACCESS_CODE,
       EXTGL_GL_HEAD,
       EXTGL_SUB_GL_HEAD,
       EXTGL_BRK_GL_HEAD
FROM EXTGL
WHERE EXTGL_GL_HEAD = 101;
```

এটি **Dynamic SQL**, কারণ Query Runtime-এ তৈরি হয়েছে।

---

# Static SQL vs Dynamic SQL

| বিষয়           | Static SQL   | Dynamic SQL                |
| -------------- | ------------ | -------------------------- |
| SQL তৈরি       | Compile Time | Runtime                    |
| Query পরিবর্তন | ❌ না         | ✅ হ্যাঁ                    |
| Performance    | দ্রুত        | তুলনামূলক ধীর              |
| Syntax Check   | Compile Time | Runtime                    |
| Flexibility    | কম           | বেশি                       |
| ব্যবহার        | Fixed Query  | User Input, Dynamic Report |

---

# কখন Static SQL ব্যবহার করবেন?

* Query আগে থেকেই নির্ধারিত।
* Table ও Column পরিবর্তন হবে না।
* Performance গুরুত্বপূর্ণ।
* সাধারণ CRUD Operation।

উদাহরণ:

```sql
SELECT *
FROM employee
WHERE empno = 1001;
```

---

# কখন Dynamic SQL ব্যবহার করবেন?

* Table Name Runtime-এ পরিবর্তন হবে।
* WHERE Clause Parameter অনুযায়ী তৈরি হবে।
* Flexible Report তৈরি করতে হবে।
* User Input অনুযায়ী Query পরিবর্তন করতে হবে।

উদাহরণ:

```sql
IF p_branch <> 0 THEN
    v_sql := v_sql || ' AND branch_code = ' || p_branch;
END IF;

IF p_curr IS NOT NULL THEN
    v_sql := v_sql || ' AND curr_code = ''' || p_curr || '''';
END IF;
```

---

# Dynamic SQL Execute করার দুটি উপায়

### 1. EXECUTE IMMEDIATE

```sql
EXECUTE IMMEDIATE v_sql;
```

ব্যবহার করা হয়:

* INSERT
* UPDATE
* DELETE
* DDL (CREATE, DROP, ALTER)

---

### 2. OPEN Cursor FOR

```sql
OPEN rc FOR v_sql;
```

ব্যবহার করা হয়:

* SELECT Query
* REF CURSOR

---

# Interview Note

### Static SQL

* Fixed Query
* Compile Time Parse
* Fast Execution
* Better Performance

### Dynamic SQL

* Runtime Query Generation
* Flexible
* User Input Support
* Uses `EXECUTE IMMEDIATE` or `OPEN ... FOR`

---

# মনে রাখার সহজ উপায়

```text
Static SQL  = Fixed SQL

Dynamic SQL = Runtime Generated SQL
```

### Shortcut

* **Static = আগে থেকেই লেখা Query**
* **Dynamic = Program চলার সময় তৈরি হওয়া Query**

আপনি চাইলে আমি একইভাবে **Procedure, Function, Package, Cursor এবং NVL**-এরও GitHub Notes তৈরি করে দিতে পারি।
