```
PL/SQL Packages

A PL/SQL Package is a collection of related PL/SQL elements such as:

Procedures
Functions
Variables
Constants
Cursors
Records
Collections
Exceptions
Types

Packages are useful for organizing large PL/SQL applications and providing a clean public API.

1. Package Structure

A package normally has two parts:

Package
   │
   ├── Package Specification
   │       ↓
   │   Public Interface
   │
   └── Package Body
           ↓
       Implementation
Package Specification

Defines what other programs can access.

CREATE OR REPLACE PACKAGE employee_pkg IS

    PROCEDURE add_employee(
        p_empno NUMBER,
        p_name  VARCHAR2
    );

    FUNCTION get_salary(
        p_empno NUMBER
    ) RETURN NUMBER;

END employee_pkg;
/
Package Body

Contains the actual implementation.

CREATE OR REPLACE PACKAGE BODY employee_pkg IS

    PROCEDURE add_employee(
        p_empno NUMBER,
        p_name  VARCHAR2
    ) IS
    BEGIN
        INSERT INTO employee(empno, name)
        VALUES (p_empno, p_name);
    END add_employee;


    FUNCTION get_salary(
        p_empno NUMBER
    ) RETURN NUMBER
    IS
        v_salary NUMBER;
    BEGIN

        SELECT salary
        INTO v_salary
        FROM employee
        WHERE empno = p_empno;

        RETURN v_salary;

    END get_salary;

END employee_pkg;
/
2. Package Specification

Package specification is the public part of the package.

Example:

CREATE OR REPLACE PACKAGE employee_pkg IS

    PROCEDURE add_employee(
        p_empno NUMBER,
        p_name VARCHAR2
    );

    FUNCTION get_salary(
        p_empno NUMBER
    ) RETURN NUMBER;

END employee_pkg;
/

Here other PL/SQL programs can access:

add_employee()
get_salary()

Think of it like a Java interface/API.

Package Specification
        ↓
Public API
3. Package Body

Package body contains the implementation.

CREATE OR REPLACE PACKAGE BODY employee_pkg IS

    PROCEDURE add_employee(
        p_empno NUMBER,
        p_name VARCHAR2
    )
    IS
    BEGIN

        INSERT INTO employee(empno, name)
        VALUES (p_empno, p_name);

    END add_employee;

END employee_pkg;
/

The implementation is hidden from users of the package.

Specification
     ↓
What can be used

Body
     ↓
How it works
4. Calling a Package Procedure

Use:

PACKAGE_NAME.PROCEDURE_NAME

Example:

BEGIN

    employee_pkg.add_employee(
        101,
        'Abdullah'
    );

END;
/
5. Calling a Package Function

Example:

DECLARE
    v_salary NUMBER;
BEGIN

    v_salary := employee_pkg.get_salary(101);

    DBMS_OUTPUT.PUT_LINE(
        'Salary = ' || v_salary
    );

END;
/
6. Package Variables

A package can also contain variables.

Specification
CREATE OR REPLACE PACKAGE employee_pkg IS

    g_company_name VARCHAR2(100);

    PROCEDURE show_company;

END employee_pkg;
/
Body
CREATE OR REPLACE PACKAGE BODY employee_pkg IS

    PROCEDURE show_company IS
    BEGIN

        DBMS_OUTPUT.PUT_LINE(
            'Company = ' || g_company_name
        );

    END show_company;

END employee_pkg;
/

You can access the public package variable:

BEGIN

    employee_pkg.g_company_name := 'SDLC Pro';

    employee_pkg.show_company;

END;
/

Output:

Company = SDLC Pro
7. Private Procedure

A procedure/function can exist only inside the package body.

It will not be accessible from outside.

CREATE OR REPLACE PACKAGE BODY employee_pkg IS

    PROCEDURE log_message IS
    BEGIN
        DBMS_OUTPUT.PUT_LINE('Logging...');
    END log_message;


    PROCEDURE add_employee(
        p_empno NUMBER,
        p_name VARCHAR2
    )
    IS
    BEGIN

        log_message;

        INSERT INTO employee(empno, name)
        VALUES (p_empno, p_name);

    END add_employee;

END employee_pkg;
/

Here:

log_message()

is private.

It can be called inside the package body:

log_message;

But this will not work from outside:

BEGIN
    employee_pkg.log_message;
END;
/

because it is not declared in the specification.

8. Package with Procedure + Function + Exception

Example:

CREATE OR REPLACE PACKAGE employee_pkg IS

    PROCEDURE update_salary(
        p_empno NUMBER,
        p_salary NUMBER
    );

    FUNCTION get_salary(
        p_empno NUMBER
    ) RETURN NUMBER;

END employee_pkg;
/

Body:

CREATE OR REPLACE PACKAGE BODY employee_pkg IS

    e_invalid_salary EXCEPTION;


    PROCEDURE update_salary(
        p_empno NUMBER,
        p_salary NUMBER
    )
    IS
    BEGIN

        IF p_salary < 0 THEN
            RAISE e_invalid_salary;
        END IF;

        UPDATE employee
        SET salary = p_salary
        WHERE empno = p_empno;

    EXCEPTION
        WHEN e_invalid_salary THEN
            DBMS_OUTPUT.PUT_LINE(
                'Salary cannot be negative'
            );

    END update_salary;


    FUNCTION get_salary(
        p_empno NUMBER
    ) RETURN NUMBER
    IS
        v_salary NUMBER;
    BEGIN

        SELECT salary
        INTO v_salary
        FROM employee
        WHERE empno = p_empno;

        RETURN v_salary;

    END get_salary;

END employee_pkg;
/
9. Package Types

Packages can contain custom types.

Example:

CREATE OR REPLACE PACKAGE employee_pkg IS

    TYPE employee_record IS RECORD (
        empno NUMBER,
        name  VARCHAR2(100),
        salary NUMBER
    );

END employee_pkg;
/

Then:

DECLARE

    v_emp employee_pkg.employee_record;

BEGIN

    v_emp.empno := 101;
    v_emp.name := 'Abdullah';
    v_emp.salary := 50000;

    DBMS_OUTPUT.PUT_LINE(
        v_emp.name
    );

END;
/
10. Your Application Package

Your package specification:

CREATE OR REPLACE PACKAGE ICBSPROD_28JAN2025.PKG_ACCR_GLBALTRF IS

    PROCEDURE SP_ACCR_GLBALTRF(
        V_ENTITY_NUM IN NUMBER,
        P_BRN_CODE   IN NUMBER DEFAULT 0,
        P_FLAG1      IN NUMBER DEFAULT 0
    );

END PKG_ACCR_GLBALTRF;
/

This means your package exposes one public procedure:

SP_ACCR_GLBALTRF()

So another program can call:

BEGIN

    ICBSPROD_28JAN2025.PKG_ACCR_GLBALTRF.SP_ACCR_GLBALTRF(
        V_ENTITY_NUM => 1,
        P_BRN_CODE   => 10,
        P_FLAG1      => 0
    );

END;
/
11. Your Package Body

Your package body contains many procedures:

PKG_ACCR_GLBALTRF
│
├── INIT_LOOP_PARA
├── DESTROY_ARRAYS
├── INIT_PARA
├── CHECK_TODAY_PROCESS
├── READ_GLBALTRF_WITH_BRN
├── AUTOPOST_ARRAY_ASSIGN
├── SET_TRAN_KEY_VALUES
├── SET_TRANBAT_VALUES
├── UPDATE_GLBALTRFPOST
├── POST_PARA
├── PROCESS_FOR_GL
├── TRANSFER_ALL_GL_WITH_BRN
└── SP_ACCR_GLBALTRF

But only this one is in the specification:

SP_ACCR_GLBALTRF

Therefore:

Outside Package
      │
      ↓
SP_ACCR_GLBALTRF()
      │
      ├── INIT_PARA()
      ├── READ_GLBALTRF_WITH_BRN()
      ├── TRANSFER_ALL_GL_WITH_BRN()
      │       │
      │       └── PROCESS_FOR_GL()
      │               │
      │               └── AUTOPOST_ARRAY_ASSIGN()
      │
      └── POST_PARA()

The other procedures are private implementation details.

12. Your Package's Main Flow

Your main entry point is:

SP_ACCR_GLBALTRF

Inside it:

INIT_PARA;

initializes variables and arrays.

Then:

READ_GLBALTRF_WITH_BRN;

reads GL transfer configuration.

Then:

TRANSFER_ALL_GL_WITH_BRN;

processes the collected records.

Inside that:

PROCESS_FOR_GL;

gets GL balances.

Then:

AUTOPOST_ARRAY_ASSIGN;

creates accounting/posting entries.

Finally:

POST_PARA;

posts the transaction batch.

So the high-level flow is:

SP_ACCR_GLBALTRF
       ↓
   INIT_PARA
       ↓
READ_GLBALTRF_WITH_BRN
       ↓
TRANSFER_ALL_GL_WITH_BRN
       ↓
  PROCESS_FOR_GL
       ↓
AUTOPOST_ARRAY_ASSIGN
       ↓
    POST_PARA
       ↓
 Database Posting
13. Why Packages Are Useful
Without Package

You may have many unrelated procedures:

SP_EMPLOYEE_INSERT
SP_EMPLOYEE_UPDATE
SP_EMPLOYEE_DELETE
FN_EMPLOYEE_SALARY
SP_EMPLOYEE_SEARCH

Managing them separately becomes difficult.

With Package
EMPLOYEE_PKG
│
├── INSERT_EMPLOYEE
├── UPDATE_EMPLOYEE
├── DELETE_EMPLOYEE
├── GET_SALARY
└── SEARCH_EMPLOYEE

Everything related to employee functionality is grouped together.

14. Advantages of Packages
1. Modularity

Related functionality can be grouped.

EMPLOYEE_PKG
ACCOUNT_PKG
CUSTOMER_PKG
TRANSACTION_PKG
2. Encapsulation

Private procedures and variables can be hidden inside the package body.

3. Reusability

A package procedure can be called from many places.

PKG_ACCR_GLBALTRF.SP_ACCR_GLBALTRF(...);
4. Maintainability

Large applications become easier to manage.

5. Performance

Oracle loads the package into memory when needed, and package state can remain available for the session.

6. Common Data

Package-level variables can be shared among procedures/functions in the same package.

15. Package vs Procedure
Package	Procedure
Container of related PL/SQL elements	Performs one specific operation
Has specification + body	Usually one unit
Can contain procedures	Cannot contain multiple top-level procedures
Can contain functions	Can contain local functions
Can contain variables/types/exceptions	Has local declarations
Supports public/private members	No package-style public/private API
Good for large applications	Good for individual operations
16. Important Concept

Think of a PL/SQL package like a Java class:

Java Class
    │
    ├── Fields
    ├── Methods
    ├── Constants
    └── Inner Types

Similar idea:

PL/SQL Package
    │
    ├── Variables
    ├── Procedures
    ├── Functions
    ├── Constants
    ├── Records
    ├── Collections
    └── Exceptions

And:

Package Specification ≈ Public API
Package Body          ≈ Implementation

```
