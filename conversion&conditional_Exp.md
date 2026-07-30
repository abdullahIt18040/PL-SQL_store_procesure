### Using Conversion Functions and Conditional Expressions (Oracle SQL)
```
1. Conversion Functions in Oracle
What are Conversion Functions?

Conversion Functions are Oracle functions used to convert data from one data type to another.

Commonly used conversion functions:

TO_CHAR()
TO_DATE()
TO_NUMBER()
1. TO_CHAR()

TO_CHAR() converts:

Number → Character
Date → Character
Syntax
TO_CHAR(value, format_model)
Convert Date to Character

Example:

SELECT TO_CHAR(SYSDATE,'DD-MM-YYYY')
FROM DUAL;

Output:

30-07-2026
Date Format Examples
Format	Meaning	Example
DD	Day	30
MM	Month	07
YYYY	Year	2026
MON	Month Name	JUL
DAY	Day Name	THURSDAY

Example:

SELECT TO_CHAR(
SYSDATE,
'DD-MON-YYYY'
)
FROM DUAL;

Output:

30-JUL-2026
Convert Number to Character

Example:

SELECT TO_CHAR(25000,'99,999')
FROM DUAL;

Output:

25,000
Currency Format

Example:

SELECT TO_CHAR(
5000,
'$9,999'
)
FROM DUAL;

Output:

$5,000
2. TO_DATE()

TO_DATE() converts character values into DATE data type.

Syntax
TO_DATE(character_value, format_model)

Example:

SELECT TO_DATE(
'30-07-2026',
'DD-MM-YYYY'
)
FROM DUAL;

Output:

30-JUL-26
Insert Date Using TO_DATE()
INSERT INTO EMPLOYEEPRO
VALUES
(
104,
'Rahim',
30000,
NULL,
TO_DATE('15-01-2025','DD-MM-YYYY')
);
3. TO_NUMBER()

TO_NUMBER() converts character values into NUMBER data type.

Syntax
TO_NUMBER(character_value)

Example:

SELECT TO_NUMBER('5000')
FROM DUAL;

Output:

5000

Example with calculation:

SELECT TO_NUMBER('1000') + 500
FROM DUAL;

Output:

1500
Implicit Conversion vs Explicit Conversion
Implicit Conversion

Oracle automatically converts data types.

Example:

SELECT '100' + 200
FROM DUAL;

Output:

300

Oracle converts '100' into number automatically.

Explicit Conversion

Developer manually converts data types.

Example:

SELECT TO_NUMBER('100') + 200
FROM DUAL;

Output:

300
Conditional Expressions in Oracle

Conditional expressions allow SQL statements to perform IF-THEN-ELSE logic.

Oracle provides:

CASE Expression
DECODE Function
1. CASE Expression

CASE works like IF-ELSE statement.

Simple CASE

Syntax:

CASE expression
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ELSE result
END

Example:

SELECT EMP_NAME,
       SALARY,
       CASE SALARY
           WHEN 25000 THEN 'LOW'
           WHEN 50000 THEN 'HIGH'
           ELSE 'MEDIUM'
       END AS SALARY_STATUS
FROM EMPLOYEEPRO;

Output:

EMP_NAME	SALARY	SALARY_STATUS
abdullah	25000	LOW
mamun	35678	MEDIUM
Searched CASE Expression

Used with conditions.

Syntax:

CASE
    WHEN condition THEN result
    WHEN condition THEN result
    ELSE result
END

Example:

SELECT EMP_NAME,
       SALARY,
       CASE
           WHEN SALARY >= 50000 THEN 'HIGH'
           WHEN SALARY >= 30000 THEN 'MEDIUM'
           ELSE 'LOW'
       END AS SALARY_LEVEL
FROM EMPLOYEEPRO;

Output:

EMP_NAME	SALARY	LEVEL
abdullah	25000	LOW
mamun	35678	MEDIUM
CASE with Date

Example:

SELECT EMP_NAME,
       CASE
          WHEN JOIN_DATE < DATE '2020-01-01'
          THEN 'Old Employee'
          ELSE 'New Employee'
       END AS EMP_TYPE
FROM EMPLOYEEPRO;
2. DECODE Function

DECODE() works like IF-ELSE or switch-case.

Syntax
DECODE(
expression,
search_value,
result,
default_value
)

Example:

SELECT EMP_NAME,
       DECODE(
          SALARY,
          25000,'LOW',
          50000,'HIGH',
          'MEDIUM'
       ) AS SALARY_STATUS
FROM EMPLOYEEPRO;
CASE vs DECODE
CASE	DECODE
ANSI SQL Standard	Oracle specific
Supports conditions (>, <, >=)	Only equality check
More powerful	Simple comparison
Recommended for new development	Legacy Oracle code
Using Conversion + Conditional Expression Together

Example:

SELECT 
EMP_NAME,
TO_CHAR(JOIN_DATE,'DD-MON-YYYY') JOINING_DATE,

CASE
    WHEN SALARY >= 40000 THEN 'HIGH SALARY'
    ELSE 'NORMAL SALARY'
END AS STATUS

FROM EMPLOYEEPRO;

Output:

EMP_NAME	JOINING_DATE	STATUS
abdullah	15-JAN-2024	NORMAL SALARY
mamun	20-MAY-2023	NORMAL SALARY
PL/SQL Example
SET SERVEROUTPUT ON;

DECLARE

    V_SALARY NUMBER := 50000;
    V_STATUS VARCHAR2(20);

BEGIN

    V_STATUS :=
    CASE
        WHEN V_SALARY >= 50000 THEN 'HIGH'
        ELSE 'LOW'
    END;

    DBMS_OUTPUT.PUT_LINE(V_STATUS);

END;
/

Output:

HIGH
Interview Questions
1. What are Conversion Functions?

Functions used to convert data from one datatype to another.

Examples:

TO_CHAR()
TO_DATE()
TO_NUMBER()
2. Difference between TO_CHAR() and TO_DATE()
TO_CHAR()	TO_DATE()
Date/Number → Character	Character → Date
Used for formatting output	Used for storing date values
3. Difference between CASE and DECODE
CASE supports complex conditions.
DECODE supports only equality comparison.
CASE is ANSI standard.
CASE is preferred in modern Oracle SQL.
GitHub Short Note
# Conversion Functions and Conditional Expressions (Oracle SQL)

## Conversion Functions

Used to convert one datatype into another.

### TO_CHAR()
- Converts Number/Date into Character.

Example:
```sql
SELECT TO_CHAR(SYSDATE,'DD-MM-YYYY')
FROM DUAL;
TO_DATE()
Converts Character into Date.

Example:

SELECT TO_DATE('30-07-2026','DD-MM-YYYY')
FROM DUAL;
TO_NUMBER()
Converts Character into Number.

Example:

SELECT TO_NUMBER('5000')
FROM DUAL;
Conditional Expressions

Used for IF-THEN-ELSE logic in SQL.

CASE Expression

Supports conditions.

Example:

CASE
 WHEN SALARY >= 50000 THEN 'HIGH'
 ELSE 'LOW'
END
DECODE()

Oracle-specific conditional function.

Example:

DECODE(
SALARY,
50000,'HIGH',
'LOW'
)
CASE vs DECODE
CASE	DECODE
ANSI Standard	Oracle Specific
Supports conditions	Equality only
More flexible	Less flexible

```
