# Using Single-Row Functions to Customize Output (Oracle SQL)

## What is a Single-Row Function?

A **Single-Row Function** is an Oracle function that:

- Executes once for each row.
- Returns one result for each row.
- Used to customize, format, and manipulate query output.

Example:

```sql
SELECT UPPER('oracle')
FROM DUAL;

Output:

ORACLE
Types of Single-Row Functions

Oracle Single-Row Functions are divided into 5 categories:

Character Functions
Number Functions
Date Functions
Conversion Functions
General Functions
1. Character Functions

Used to manipulate character/string data.

UPPER()

Convert lowercase letters into uppercase.

SELECT UPPER('oracle database')
FROM DUAL;

Output:

ORACLE DATABASE
LOWER()

Convert uppercase letters into lowercase.

SELECT LOWER('ORACLE')
FROM DUAL;

Output:

oracle
INITCAP()

Convert first letter of each word into uppercase.

SELECT INITCAP('abdullah al mamun')
FROM DUAL;

Output:

Abdullah Al Mamun
LENGTH()

Returns string length.

SELECT LENGTH('Oracle')
FROM DUAL;

Output:

6
SUBSTR()

Extract part of a string.

Syntax:

SUBSTR(string, start_position, length)

Example:

SELECT SUBSTR('BANGLADESH',1,5)
FROM DUAL;

Output:

BANGL
INSTR()

Find the position of a character.

SELECT INSTR('BANGLADESH','L')
FROM DUAL;

Output:

4
REPLACE()

Replace characters.

SELECT REPLACE('JAVA','J','K')
FROM DUAL;

Output:

KAVA
TRIM()

Remove extra spaces.

SELECT TRIM('   Oracle   ')
FROM DUAL;

Output:

Oracle
2. Number Functions

Used for mathematical operations.

ROUND()

Rounds a number.

SELECT ROUND(45.678,2)
FROM DUAL;

Output:

45.68
TRUNC()

Removes decimal portion.

SELECT TRUNC(45.678,2)
FROM DUAL;

Output:

45.67
MOD()

Returns remainder.

SELECT MOD(10,3)
FROM DUAL;

Output:

1
CEIL()

Returns next highest integer.

SELECT CEIL(12.3)
FROM DUAL;

Output:

13
FLOOR()

Returns lowest integer.

SELECT FLOOR(12.9)
FROM DUAL;

Output:

12
3. Date Functions

Used to manipulate DATE values.

SYSDATE

Returns current date and time.

SELECT SYSDATE
FROM DUAL;
ADD_MONTHS()

Add months to a date.

SELECT ADD_MONTHS(SYSDATE,6)
FROM DUAL;
MONTHS_BETWEEN()

Find difference between two dates.

SELECT MONTHS_BETWEEN(
DATE '2026-12-01',
DATE '2026-01-01'
)
FROM DUAL;

Output:

11
LAST_DAY()

Returns last day of month.

SELECT LAST_DAY(SYSDATE)
FROM DUAL;
4. Conversion Functions

Used to convert data types.

TO_CHAR()

Convert Date/Number into String.

Example:

SELECT TO_CHAR(
SYSDATE,
'DD-MM-YYYY'
)
FROM DUAL;

Output:

30-07-2026
TO_DATE()

Convert String into Date.

SELECT TO_DATE(
'30-07-2026',
'DD-MM-YYYY'
)
FROM DUAL;
TO_NUMBER()

Convert String into Number.

SELECT TO_NUMBER('5000')
FROM DUAL;

Output:

5000
5. General Functions

Used for handling NULL values.

NVL()

Replace NULL value.

Syntax:

NVL(expression, replacement_value)

Example:

SELECT EMP_NAME,
NVL(COMMISSION,0)
FROM EMPLOYEEPRO;

Output:

abdullah  0
mamun     5000
NVL2()

Returns different values based on NULL condition.

Syntax:

NVL2(expression,
value_if_not_null,
value_if_null)

Example:

SELECT EMP_NAME,
NVL2(COMMISSION,
'Commission Available',
'No Commission')
FROM EMPLOYEEPRO;
COALESCE()

Returns the first non-null value.

Example:

SELECT COALESCE(
NULL,
NULL,
'Oracle',
'Java'
)
FROM DUAL;

Output:

Oracle
NULLIF()

Returns NULL if two values are equal.

Example:

SELECT NULLIF(100,100)
FROM DUAL;

Output:

NULL
Using Single-Row Functions with Table

Example:

SELECT 
EMP_NAME,
UPPER(EMP_NAME),
LENGTH(EMP_NAME),
NVL(COMMISSION,0)
FROM EMPLOYEEPRO;

Output:

EMP_NAME	UPPER	LENGTH	COMMISSION
abdullah	ABDULLAH	8	0
mamun	MAMUN	5	5000
Where Can We Use Single-Row Functions?
SELECT
SELECT UPPER(EMP_NAME)
FROM EMPLOYEEPRO;
WHERE
SELECT *
FROM EMPLOYEEPRO
WHERE UPPER(EMP_NAME)='ABDULLAH';
ORDER BY
SELECT *
FROM EMPLOYEEPRO
ORDER BY LOWER(EMP_NAME);
PL/SQL
DECLARE
    V_NAME VARCHAR2(50) := 'abdullah';
BEGIN
    DBMS_OUTPUT.PUT_LINE(UPPER(V_NAME));
END;
/

Output:

ABDULLAH
