# PL/SQL Control Statements

PL/SQL **Control Statements** ব্যবহার করা হয় program-এর execution flow control করার জন্য।

সহজভাবে:

```text
PL/SQL Program
      ↓
Control Statement
      ↓
কোন code কখন execute হবে
      ↓
Decision / Repetition / Sequential Execution
```

## Types of PL/SQL Control Statements

প্রধানত ৩ ধরনের:

```text
1. Conditional Control
   ├── IF
   ├── IF ... ELSE
   ├── IF ... ELSIF ... ELSE
   └── CASE

2. Iterative Control
   ├── LOOP
   ├── WHILE LOOP
   └── FOR LOOP

3. Sequential Control
   ├── EXIT
   ├── CONTINUE
   ├── GOTO
   └── NULL
```

---

# 1. Conditional Control Statements

Conditional statements কোনো **condition-এর উপর ভিত্তি করে decision** নেয়।

```text
Condition
   ↓
TRUE  → এক ধরনের code execute
FALSE → অন্য code execute
```

---

## 1.1 IF Statement

একটি condition check করার জন্য `IF` ব্যবহার করা হয়।

### Syntax

```sql
IF condition THEN
    statements;
END IF;
```

### Example

```sql
DECLARE
    v_salary NUMBER := 50000;
BEGIN

    IF v_salary > 40000 THEN
        DBMS_OUTPUT.PUT_LINE('Salary is high');
    END IF;

END;
/
```

Output:

```text
Salary is high
```

### How it works

```text
v_salary > 40000?
       |
   ┌───┴───┐
  TRUE    FALSE
   ↓        ↓
Execute   Nothing
```

---

# 2. IF ... ELSE

Condition `TRUE` হলে এক code এবং `FALSE` হলে অন্য code execute হয়।

### Syntax

```sql
IF condition THEN
    statements;
ELSE
    statements;
END IF;
```

### Example

```sql
DECLARE
    v_age NUMBER := 20;
BEGIN

    IF v_age >= 18 THEN
        DBMS_OUTPUT.PUT_LINE('Adult');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Minor');
    END IF;

END;
/
```

Output:

```text
Adult
```

### Flow

```text
        v_age >= 18?
             |
       ┌─────┴─────┐
      TRUE        FALSE
       ↓             ↓
    Adult          Minor
```

---

# 3. IF ... ELSIF ... ELSE

একাধিক condition check করার জন্য ব্যবহার করা হয়।

### Syntax

```sql
IF condition1 THEN
    statements;

ELSIF condition2 THEN
    statements;

ELSIF condition3 THEN
    statements;

ELSE
    statements;
END IF;
```

### Example

```sql
DECLARE
    v_marks NUMBER := 75;
BEGIN

    IF v_marks >= 80 THEN
        DBMS_OUTPUT.PUT_LINE('Grade A+');

    ELSIF v_marks >= 70 THEN
        DBMS_OUTPUT.PUT_LINE('Grade A');

    ELSIF v_marks >= 60 THEN
        DBMS_OUTPUT.PUT_LINE('Grade B');

    ELSE
        DBMS_OUTPUT.PUT_LINE('Grade C');

    END IF;

END;
/
```

Output:

```text
Grade A
```

কারণ:

```text
75 >= 80 → FALSE
75 >= 70 → TRUE
```

তাই `Grade A` execute হবে।

---

# 4. CASE Statement

একাধিক value বা condition check করার জন্য `CASE` ব্যবহার করা হয়।

দুই ধরনের CASE:

```text
1. Simple CASE
2. Searched CASE
```

---

## 4.1 Simple CASE

একটি expression-এর value বিভিন্ন value-এর সাথে compare করে।

### Example

```sql
DECLARE
    v_day NUMBER := 2;
BEGIN

    CASE v_day

        WHEN 1 THEN
            DBMS_OUTPUT.PUT_LINE('Sunday');

        WHEN 2 THEN
            DBMS_OUTPUT.PUT_LINE('Monday');

        WHEN 3 THEN
            DBMS_OUTPUT.PUT_LINE('Tuesday');

        ELSE
            DBMS_OUTPUT.PUT_LINE('Invalid day');

    END CASE;

END;
/
```

Output:

```text
Monday
```

কারণ:

```text
v_day = 2
   ↓
WHEN 2
   ↓
Monday
```

---

## 4.2 Searched CASE

এখানে সরাসরি condition ব্যবহার করা যায়।

### Example

```sql
DECLARE
    v_salary NUMBER := 60000;
BEGIN

    CASE

        WHEN v_salary >= 80000 THEN
            DBMS_OUTPUT.PUT_LINE('High Salary');

        WHEN v_salary >= 50000 THEN
            DBMS_OUTPUT.PUT_LINE('Medium Salary');

        ELSE
            DBMS_OUTPUT.PUT_LINE('Low Salary');

    END CASE;

END;
/
```

Output:

```text
Medium Salary
```

---

# 5. Iterative Control Statements

একই code বারবার execute করার জন্য **loop** ব্যবহার করা হয়।

```text
Iteration
    ↓
Same code repeatedly execute
    ↓
Condition false হলে stop
```

প্রধান loop:

```text
1. LOOP
2. WHILE LOOP
3. FOR LOOP
```

---

# 6. Basic LOOP

`LOOP` একটি block বারবার execute করে।

সাধারণত `EXIT` বা `EXIT WHEN` ব্যবহার করে loop থেকে বের হতে হয়।

### Syntax

```sql
LOOP

    statements;

    EXIT WHEN condition;

END LOOP;
```

### Example

```sql
DECLARE
    v_count NUMBER := 1;
BEGIN

    LOOP

        DBMS_OUTPUT.PUT_LINE('Count = ' || v_count);

        v_count := v_count + 1;

        EXIT WHEN v_count > 5;

    END LOOP;

END;
/
```

Output:

```text
Count = 1
Count = 2
Count = 3
Count = 4
Count = 5
```

### Flow

```text
Start
  ↓
Print count
  ↓
count + 1
  ↓
count > 5?
  ↓
 NO ──────────→ LOOP আবার চলবে
  ↓
 YES
  ↓
 EXIT
```

---

# 7. WHILE LOOP

Condition `TRUE` থাকা পর্যন্ত loop চলবে।

### Syntax

```sql
WHILE condition LOOP

    statements;

END LOOP;
```

### Example

```sql
DECLARE
    v_count NUMBER := 1;
BEGIN

    WHILE v_count <= 5 LOOP

        DBMS_OUTPUT.PUT_LINE('Count = ' || v_count);

        v_count := v_count + 1;

    END LOOP;

END;
/
```

Output:

```text
Count = 1
Count = 2
Count = 3
Count = 4
Count = 5
```

### Flow

```text
        Condition
           ↓
     v_count <= 5?
       /        \
     YES         NO
      ↓           ↓
   Execute       Stop
      ↓
  count + 1
      ↓
  Condition
```

---

# 8. FOR LOOP

নির্দিষ্ট range-এর মধ্যে loop চালানোর জন্য `FOR LOOP` ব্যবহার করা হয়।

### Syntax

```sql
FOR variable IN start_value..end_value LOOP

    statements;

END LOOP;
```

### Example

```sql
BEGIN

    FOR i IN 1..5 LOOP

        DBMS_OUTPUT.PUT_LINE('Count = ' || i);

    END LOOP;

END;
/
```

Output:

```text
Count = 1
Count = 2
Count = 3
Count = 4
Count = 5
```

`i` automatically:

```text
1 → 2 → 3 → 4 → 5
```

হবে।

### Important

`FOR LOOP` counter variable আলাদাভাবে declare করতে হয় না।

```sql
FOR i IN 1..5 LOOP
```

এখানে `i` automatically তৈরি হয়।

---

# 9. REVERSE FOR LOOP

`FOR LOOP`-কে reverse direction-এ চালানো যায়।

### Example

```sql
BEGIN

    FOR i IN REVERSE 1..5 LOOP

        DBMS_OUTPUT.PUT_LINE('Count = ' || i);

    END LOOP;

END;
/
```

Output:

```text
Count = 5
Count = 4
Count = 3
Count = 2
Count = 1
```

---

# 10. EXIT Statement

Loop থেকে বের হওয়ার জন্য `EXIT` ব্যবহার করা হয়।

### Example

```sql
DECLARE
    v_count NUMBER := 1;
BEGIN

    LOOP

        DBMS_OUTPUT.PUT_LINE(v_count);

        EXIT WHEN v_count = 3;

        v_count := v_count + 1;

    END LOOP;

END;
/
```

Output:

```text
1
2
3
```

`v_count = 3` হলে loop terminate করবে।

---

# 11. CONTINUE Statement

Current iteration skip করে next iteration-এ যাওয়ার জন্য `CONTINUE` ব্যবহার করা হয়।

### Example

```sql
BEGIN

    FOR i IN 1..5 LOOP

        IF i = 3 THEN
            CONTINUE;
        END IF;

        DBMS_OUTPUT.PUT_LINE('Number = ' || i);

    END LOOP;

END;
/
```

Output:

```text
Number = 1
Number = 2
Number = 4
Number = 5
```

এখানে `i = 3` হলে:

```text
CONTINUE
    ↓
Current iteration skip
    ↓
Next iteration
```

---

# 12. CONTINUE WHEN

`CONTINUE WHEN` হলো `CONTINUE` ব্যবহারের একটি short form।

### Example

```sql
BEGIN

    FOR i IN 1..5 LOOP

        CONTINUE WHEN i = 3;

        DBMS_OUTPUT.PUT_LINE('Number = ' || i);

    END LOOP;

END;
/
```

Output:

```text
Number = 1
Number = 2
Number = 4
Number = 5
```

---

# 13. NULL Statement

`NULL` statement কোনো action perform করে না।

কোনো condition-এর জন্য intentionally কিছু না করতে চাইলে `NULL` ব্যবহার করা যায়।

### Example

```sql
DECLARE
    v_salary NUMBER := 50000;
BEGIN

    IF v_salary > 100000 THEN

        DBMS_OUTPUT.PUT_LINE('High salary');

    ELSE

        NULL;

    END IF;

END;
/
```

এখানে `ELSE` block-এ কোনো action প্রয়োজন নেই।

তাই:

```sql
NULL;
```

ব্যবহার করা হয়েছে।

---

# 14. GOTO Statement

`GOTO` ব্যবহার করে program-এর control একটি নির্দিষ্ট label-এ পাঠানো যায়।

### Example

```sql
BEGIN

    GOTO my_label;

    DBMS_OUTPUT.PUT_LINE('This will not execute');

    <<my_label>>

    DBMS_OUTPUT.PUT_LINE('Control reached here');

END;
/
```

Output:

```text
Control reached here
```

এখানে:

```sql
GOTO my_label;
```

control সরাসরি:

```sql
<<my_label>>
```

এ চলে যায়।

> **Best Practice:** Modern PL/SQL code-এ `GOTO` সাধারণত avoid করা ভালো, কারণ এতে program flow unnecessarily complex হতে পারে।

---

# 15. Control Statements in `SP_TRIALBAL`

আপনার `SP_TRIALBAL` procedure-এ Control Statements-এর real-world usage আছে।

## IF Example

```sql
IF W_FIN_YEAR1 = W_FIN_YEAR2 THEN

    GET_GLSUM_DTLS_SAMEFINYR;

ELSE

    GET_GLSUM_DTLS_DIFFFINYR;

END IF;
```

এখানে:

```text
W_FIN_YEAR1 = W_FIN_YEAR2
        ↓
      TRUE
        ↓
GET_GLSUM_DTLS_SAMEFINYR

        OR

      FALSE
        ↓
GET_GLSUM_DTLS_DIFFFINYR
```

অর্থাৎ financial year একই না আলাদা তার উপর ভিত্তি করে আলাদা procedure call হচ্ছে।

---

## FOR LOOP Example

আপনার procedure-এ:

```sql
FOR IDX IN 1 .. V_TODAYSBAL.COUNT LOOP

    IF V_TODAYSBAL(IDX).GL_TODAY_DB <> 0
       OR V_TODAYSBAL(IDX).GL_TODAY_CR <> 0 THEN

        -- Processing

    END IF;

END LOOP;
```

এখানে `V_TODAYSBAL` collection-এর প্রতিটি element process করা হচ্ছে।

Flow:

```text
V_TODAYSBAL
     ↓
COUNT
     ↓
1 → 2 → 3 → 4 → ...
     ↓
প্রতিটি record check
     ↓
GL_TODAY_DB / GL_TODAY_CR
     ↓
IF condition
     ↓
Processing
```

---

## WHILE LOOP Example

আপনার procedure-এ:

```sql
WHILE W_FIN_YEAR1 <= W_FIN_YEAR2 LOOP

    -- Financial year processing

    W_FIN_YEAR1 := W_FIN_YEAR1 + 1;

END LOOP;
```

যেমন:

```text
W_FIN_YEAR1 = 2023
W_FIN_YEAR2 = 2025
```

তাহলে:

```text
2023
 ↓
2024
 ↓
2025
 ↓
2026 <= 2025 ? FALSE
 ↓
STOP
```

---

# 16. Control Statements Summary

| Category    | Statement     | Purpose                    |
| ----------- | ------------- | -------------------------- |
| Conditional | `IF`          | Single condition check     |
| Conditional | `IF ... ELSE` | TRUE/FALSE decision        |
| Conditional | `ELSIF`       | Multiple conditions        |
| Conditional | `CASE`        | Multiple values/conditions |
| Iterative   | `LOOP`        | Repeated execution         |
| Iterative   | `WHILE LOOP`  | Condition-based repetition |
| Iterative   | `FOR LOOP`    | Range/collection iteration |
| Sequential  | `EXIT`        | Exit from loop             |
| Sequential  | `CONTINUE`    | Skip current iteration     |
| Sequential  | `GOTO`        | Jump to a label            |
| Sequential  | `NULL`        | Perform no action          |

---

# 17. Easy Way to Remember

```text
              PL/SQL CONTROL
                    |
          ┌─────────┼─────────┐
          ↓         ↓         ↓
       Decision    Repeat   Sequential
          |         |         |
          ↓         ↓         ↓
       IF/CASE     LOOP      EXIT
                   WHILE     CONTINUE
                   FOR       GOTO
                             NULL
```

---

# 18. Recommended Learning Order

PL/SQL শেখার জন্য এই order অনুসরণ করা ভালো:

```text
1. Variables
      ↓
2. IF / ELSIF / ELSE
      ↓
3. CASE
      ↓
4. Basic LOOP
      ↓
5. WHILE LOOP
      ↓
6. FOR LOOP
      ↓
7. EXIT / CONTINUE
      ↓
8. Records
      ↓
9. Collections
      ↓
10. BULK COLLECT
      ↓
11. FORALL
      ↓
12. Exception Handling
      ↓
13. Dynamic SQL
      ↓
14. Procedures / Functions
```

## Key Takeaways

* `IF` → condition check করে।
* `CASE` → multiple conditions/values handle করে।
* `LOOP` → code repeatedly execute করে।
* `WHILE` → condition `TRUE` থাকা পর্যন্ত চলে।
* `FOR` → নির্দিষ্ট range বা collection iterate করে।
* `EXIT` → loop থেকে বের হয়।
* `CONTINUE` → current iteration skip করে।
* `NULL` → intentionally কোনো action করে না।
* `GOTO` → control নির্দিষ্ট label-এ পাঠায়, তবে সাধারণত avoid করা ভালো।
* Real-world PL/SQL procedures-এ `IF`, `FOR`, `WHILE`, `EXIT` এবং `CONTINUE` সবচেয়ে বেশি গুরুত্বপূর্ণ।
