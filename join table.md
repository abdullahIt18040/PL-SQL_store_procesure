# Oracle SQL - Self Join

## What is Self Join?

**Self Join** হলো এমন একটি **JOIN** যেখানে **একই Table-কে নিজের সাথেই Join করা হয়।**

অর্থাৎ,

> **একটি Table-কে দুইটি আলাদা Alias ব্যবহার করে Join করা হয়।**

> **Note:** নতুন কোনো Table তৈরি হয় না। একই Table-কে দুইবার Refer করা হয়।

---

# কেন Self Join ব্যবহার করা হয়?

যখন একই Table-এর Row-গুলোর মধ্যে Relationship থাকে, তখন Self Join ব্যবহার করা হয়।

**Common Examples:**

* Employee → Manager
* Branch → Parent Branch
* Category → Parent Category
* Comment → Parent Comment

---

# Example 1: Employee & Manager

## EMPLOYEE Table

| EMP_ID | EMP_NAME | MANAGER_ID |
| ------ | -------- | ---------- |
| 101    | Rahim    | 103        |
| 102    | Karim    | 103        |
| 103    | Sakib    | 104        |
| 104    | Hasan    | NULL       |

এখানে,

* Rahim-এর Manager = Sakib
* Karim-এর Manager = Sakib
* Sakib-এর Manager = Hasan
* Hasan-এর Manager নেই (CEO)

---

## Required Output

| Employee | Manager |
| -------- | ------- |
| Rahim    | Sakib   |
| Karim    | Sakib   |
| Sakib    | Hasan   |
| Hasan    | NULL    |

---

# SQL Query

```sql id="2k5vmt"
SELECT
    e.emp_name AS employee,
    m.emp_name AS manager
FROM employee e
LEFT JOIN employee m
ON e.manager_id = m.emp_id;
```

---

# Query Explanation

## Step 1

```sql id="4tztu6"
FROM employee e
```

* `employee` Table-কে `e` নামে ব্যবহার করা হয়েছে।
* `e` = Employee Information

---

## Step 2

```sql id="eh6zdm"
LEFT JOIN employee m
```

* একই `employee` Table-কে আবার `m` নামে ব্যবহার করা হয়েছে।
* `m` = Manager Information

---

## Step 3

```sql id="mbqarf"
ON e.manager_id = m.emp_id;
```

Oracle Compare করবে:

```text id="b8o1pd"
Employee.Manager_ID = Manager.EMP_ID
```

যেখানে Match হবে, সেখানে Manager-এর নাম দেখাবে।

---

# Oracle কীভাবে Match করে?

### Employee (Alias = e)

| EMP_ID | EMP_NAME | MANAGER_ID |
| ------ | -------- | ---------- |
| 101    | Rahim    | 103        |
| 102    | Karim    | 103        |
| 103    | Sakib    | 104        |
| 104    | Hasan    | NULL       |

↓

### Manager (Alias = m)

| EMP_ID | EMP_NAME |
| ------ | -------- |
| 101    | Rahim    |
| 102    | Karim    |
| 103    | Sakib    |
| 104    | Hasan    |

↓

Oracle Match করে

```text id="lmp4ra"
101 → Manager_ID = 103 → Sakib

102 → Manager_ID = 103 → Sakib

103 → Manager_ID = 104 → Hasan

104 → Manager_ID = NULL → NULL
```

---

# Final Output

| Employee | Manager |
| -------- | ------- |
| Rahim    | Sakib   |
| Karim    | Sakib   |
| Sakib    | Hasan   |
| Hasan    | NULL    |

---

# কেন LEFT JOIN ব্যবহার করা হয়েছে?

```sql id="ry1ffp"
LEFT JOIN
```

ব্যবহার করার কারণে সব Employee Output-এ এসেছে।

যদি `INNER JOIN` ব্যবহার করা হতো, তাহলে Hasan-এর `MANAGER_ID = NULL` হওয়ায় Hasan Output-এ আসত না।

---

# Example 2: Branch & Parent Branch

## BRANCH Table

| BRANCH_ID | BRANCH_NAME | PARENT_BRANCH |
| --------- | ----------- | ------------- |
| 101       | Dhaka       | NULL          |
| 102       | Mirpur      | 101           |
| 103       | Uttara      | 101           |
| 104       | Pallabi     | 102           |

---

## SQL Query

```sql id="3phmqr"
SELECT
    b.branch_name,
    p.branch_name AS parent_branch
FROM branch b
LEFT JOIN branch p
ON b.parent_branch = p.branch_id;
```

---

## Output

| Branch  | Parent Branch |
| ------- | ------------- |
| Dhaka   | NULL          |
| Mirpur  | Dhaka         |
| Uttara  | Dhaka         |
| Pallabi | Mirpur        |

---

# Self Join Diagram

```text id="fjlwm7"
              EMPLOYEE
                 │
      ┌──────────┴──────────┐
      │                     │
      ▼                     ▼
Employee (e)          Manager (m)

e.manager_id = m.emp_id
```

---

# Self Join Execution

```text id="uzywte"
FROM employee e
        │
        ▼
LEFT JOIN employee m
        │
        ▼
ON e.manager_id = m.emp_id
        │
        ▼
Rows Match
        │
        ▼
SELECT Employee Name + Manager Name
```

---

# INNER JOIN vs LEFT JOIN in Self Join

| INNER JOIN                               | LEFT JOIN                    |
| ---------------------------------------- | ---------------------------- |
| শুধুমাত্র যাদের Manager আছে তাদের দেখাবে | সব Employee দেখাবে           |
| Manager না থাকলে Row বাদ যাবে            | Manager না থাকলে NULL দেখাবে |

---

# Interview Questions

### Q1. What is Self Join?

**Answer:**
Self Join হলো এমন একটি JOIN যেখানে একই Table-কে দুই বা ততোধিক Alias ব্যবহার করে নিজের সাথেই Join করা হয়।

---

### Q2. কেন Alias ব্যবহার করা হয়?

**Answer:**
কারণ একই Table দুইবার ব্যবহার করা হয়। Alias Oracle-কে বুঝতে সাহায্য করে কোনটি Employee এবং কোনটি Manager।

---

### Q3. Self Join কোথায় ব্যবহার করা হয়?

* Employee → Manager
* Branch → Parent Branch
* Category → Parent Category
* Comment → Parent Comment
* Organization Hierarchy

---

# Quick Revision

```text id="cm90di"
Self Join

Same Table
     │
     ├──────────────┐
     ▼              ▼
Employee (e)    Manager (m)

Join Condition

e.manager_id = m.emp_id
```

---

# Key Points

* Self Join = **Same Table + Different Alias**
* Alias ব্যবহার করা বাধ্যতামূলক।
* Employee-Manager Relationship দেখাতে সবচেয়ে বেশি ব্যবহৃত হয়।
* Hierarchical Data (Parent-Child Relationship) দেখানোর জন্য Self Join খুবই গুরুত্বপূর্ণ।
* `LEFT JOIN` ব্যবহার করলে Manager না থাকলেও Employee Output-এ থাকবে।

আপনি Oracle SQL শিখছেন, তাই পরবর্তী GitHub Note হিসেবে **INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN, CROSS JOIN এবং SELF JOIN-এর Complete Comparison** একসাথে করলে Interview-এর জন্য খুবই কাজে লাগবে।
