# Anonymous PL/SQL Block

## 1. What is an Anonymous PL/SQL Block?

An **Anonymous PL/SQL Block** is a PL/SQL block that **does not have a name** and is **not permanently stored in the Oracle database**.

It is mainly used when we want to execute some PL/SQL code **temporarily or only once**.

### Basic Structure

```sql
DECLARE
    -- Variable / Constant / Cursor declarations

BEGIN
    -- Executable statements

EXCEPTION
    -- Exception handling

END;
/
```

---

## 2. Structure of an Anonymous Block

An anonymous PL/SQL block has three main sections:

```text
DECLARE
   ↓
Declaration Section
   ↓
BEGIN
   ↓
Executable Section
   ↓
EXCEPTION
   ↓
Exception Handling Section
   ↓
END;
```

### `DECLARE`

Used to declare:

* Variables
* Constants
* Cursors
* User-defined types

Example:

```sql
DECLARE
    v_name   VARCHAR2(50);
    v_salary NUMBER;
```

---

### `BEGIN`

Contains the executable statements.

```sql
BEGIN
    v_name := 'Abdullah';
    v_salary := 50000;

    DBMS_OUTPUT.PUT_LINE(v_name);
END;
/
```

The `BEGIN` section is **mandatory**.

---

### `EXCEPTION`

Used for error handling.

```sql
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE(SQLERRM);
```

The `EXCEPTION` section is **optional**.

---

### `END`

Marks the end of the PL/SQL block.

```sql
END;
/
```

The `/` tells tools such as SQL*Plus, SQL Developer, or TOAD to execute the PL/SQL block.

---

# 3. Simple Example

```sql
DECLARE
    v_name   VARCHAR2(50);
    v_salary NUMBER;
BEGIN

    v_name := 'Abdullah';
    v_salary := 50000;

    DBMS_OUTPUT.PUT_LINE('Name   = ' || v_name);
    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_salary);

END;
/
```

### Output

```text
Name   = Abdullah
Salary = 50000
```

### Explanation

```sql
v_name VARCHAR2(50);
```

Declares a variable.

```sql
v_name := 'Abdullah';
```

Assigns a value.

```sql
DBMS_OUTPUT.PUT_LINE(...)
```

Prints output.

---

# 4. Anonymous Block with SELECT INTO

We can retrieve data from a table using `SELECT INTO`.

Suppose we have:

```text
EMPLOYEE
-------------------------
ID    NAME      SALARY
1     Rahim     30000
2     Karim     40000
```

Example:

```sql
DECLARE
    v_name   employee.name%TYPE;
    v_salary employee.salary%TYPE;
BEGIN

    SELECT name, salary
    INTO v_name, v_salary
    FROM employee
    WHERE id = 1;

    DBMS_OUTPUT.PUT_LINE('Name   = ' || v_name);
    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_salary);

END;
/
```

Output:

```text
Name   = Rahim
Salary = 30000
```

### Important

Inside PL/SQL, when retrieving a single row into variables, we commonly use:

```sql
SELECT column1, column2
INTO variable1, variable2
FROM table_name
WHERE condition;
```

---

# 5. Using `%TYPE`

Instead of manually specifying the datatype:

```sql
v_name VARCHAR2(50);
```

we can use `%TYPE`:

```sql
v_name employee.name%TYPE;
```

This means:

> `v_name` will use the same datatype as `employee.name`.

Example:

```sql
DECLARE
    v_salary employee.salary%TYPE;
BEGIN

    SELECT salary
    INTO v_salary
    FROM employee
    WHERE id = 1;

    DBMS_OUTPUT.PUT_LINE('Salary = ' || v_salary);

END;
/
```

### Advantage

If the datatype of `employee.salary` changes, the PL/SQL variable automatically follows that datatype.

---

# 6. Anonymous Block with IF

PL/SQL supports conditional logic.

```sql
DECLARE
    v_salary NUMBER := 50000;
BEGIN

    IF v_salary >= 40000 THEN
        DBMS_OUTPUT.PUT_LINE('Good Salary');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Low Salary');
    END IF;

END;
/
```

Output:

```text
Good Salary
```

---

# 7. Anonymous Block with Exception Handling

Example:

```sql
DECLARE
    v_name employee.name%TYPE;
BEGIN

    SELECT name
    INTO v_name
    FROM employee
    WHERE id = 9999;

    DBMS_OUTPUT.PUT_LINE('Name = ' || v_name);

EXCEPTION

    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Employee not found');

    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('More than one employee found');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error = ' || SQLERRM);

END;
/
```

## Common Exceptions

### `NO_DATA_FOUND`

Occurs when `SELECT INTO` does not return any row.

```sql
WHEN NO_DATA_FOUND THEN
```

### `TOO_MANY_ROWS`

Occurs when `SELECT INTO` returns more than one row.

```sql
WHEN TOO_MANY_ROWS THEN
```

### `OTHERS`

Handles other unexpected errors.

```sql
WHEN OTHERS THEN
```

---

# 8. Anonymous Block vs Stored Procedure

## Anonymous Block

```sql
DECLARE
    v_name VARCHAR2(50);
BEGIN

    v_name := 'Abdullah';

    DBMS_OUTPUT.PUT_LINE(v_name);

END;
/
```

Characteristics:

* No name
* Not permanently stored
* Usually executed temporarily
* Useful for testing and small tasks

---

## Stored Procedure

```sql
CREATE OR REPLACE PROCEDURE test_employee AS

    v_name VARCHAR2(50);

BEGIN

    v_name := 'Abdullah';

    DBMS_OUTPUT.PUT_LINE(v_name);

END;
/
```

Characteristics:

* Has a name
* Stored in the database
* Can be executed multiple times
* Can have `IN`, `OUT`, and `IN OUT` parameters

Call the procedure:

```sql
BEGIN
    test_employee;
END;
/
```

---

# 9. Anonymous Block Calling a Procedure

An anonymous block can be used to test a stored procedure.

Example:

```sql
DECLARE
    v_temp NUMBER;
    v_err  VARCHAR2(500);
BEGIN

    ICBSPROD_28JAN2025.SP_TRIALBAL(
        V_ENTITY_NUM => 1,
        P_IN_MSG     => '...',
        P_TEMPSER    => v_temp,
        P_ERR_MSG    => v_err
    );

    DBMS_OUTPUT.PUT_LINE('TEMP SER = ' || v_temp);
    DBMS_OUTPUT.PUT_LINE('ERROR    = ' || v_err);

END;
/
```

The flow is:

```text
Anonymous Block
       |
       | CALL
       ↓
SP_TRIALBAL
       |
       | OUT parameters
       ↓
v_temp / v_err
```

---

# 10. Anonymous Block with Loop

PL/SQL loops can also be used.

```sql
DECLARE
    v_counter NUMBER := 1;
BEGIN

    WHILE v_counter <= 5 LOOP

        DBMS_OUTPUT.PUT_LINE('Counter = ' || v_counter);

        v_counter := v_counter + 1;

    END LOOP;

END;
/
```

Output:

```text
Counter = 1
Counter = 2
Counter = 3
Counter = 4
Counter = 5
```

---

# 11. Nested Anonymous Block

One PL/SQL block can exist inside another block.

```sql
DECLARE
    v_name VARCHAR2(50) := 'Abdullah';

BEGIN

    DBMS_OUTPUT.PUT_LINE('Outer Block: ' || v_name);

    DECLARE
        v_salary NUMBER := 50000;
    BEGIN

        DBMS_OUTPUT.PUT_LINE('Inner Block: ' || v_salary);

    END;

END;
/
```

Structure:

```text
Outer Block
│
├── DECLARE
│
├── BEGIN
│   │
│   └── Inner Block
│       ├── DECLARE
│       ├── BEGIN
│       └── END
│
└── END
```

---

# 12. When to Use Anonymous Blocks?

Anonymous blocks are useful for:

* Testing SQL/PLSQL logic
* Testing stored procedures
* Debugging
* Running one-time operations
* Assigning variables
* Testing functions
* Running loops
* Exception testing
* Performing temporary data processing

Example:

```sql
DECLARE
    v_count NUMBER;
BEGIN

    SELECT COUNT(*)
    INTO v_count
    FROM employee;

    DBMS_OUTPUT.PUT_LINE('Total Employee = ' || v_count);

END;
/
```

---

# 13. Key Points to Remember

| Concept                | Anonymous PL/SQL Block        |
| ---------------------- | ----------------------------- |
| Has name?              | ❌ No                          |
| Stored permanently?    | ❌ No                          |
| `DECLARE`              | Optional                      |
| `BEGIN`                | Mandatory                     |
| `EXCEPTION`            | Optional                      |
| `END`                  | Mandatory                     |
| Can use variables?     | ✅ Yes                         |
| Can use SQL?           | ✅ Yes                         |
| Can use loops?         | ✅ Yes                         |
| Can handle exceptions? | ✅ Yes                         |
| Can call procedure?    | ✅ Yes                         |
| Best use               | Testing / temporary execution |

---

# 14. Simple Mental Model

Think of an anonymous block like a **temporary Java `main()` method**:

```text
Java

public static void main(String[] args) {
    // temporary execution
}
```

PL/SQL:

```sql
DECLARE
    -- variables
BEGIN
    -- temporary execution
EXCEPTION
    -- error handling
END;
/
```

The main difference is that an anonymous PL/SQL block has **no stored name**.

---

## Summary

```text
Anonymous PL/SQL Block
        |
        ├── DECLARE     → Variables
        |
        ├── BEGIN       → Main logic
        |
        ├── EXCEPTION   → Error handling
        |
        └── END         → End of block
```

**Definition:**

> An Anonymous PL/SQL Block is an unnamed PL/SQL program unit that is compiled and executed when submitted, but is not stored permanently in the database.
