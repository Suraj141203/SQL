# SQL
🚀 SQL Practice Repository This repository contains structured SQL problems and solutions ranging from basic queries to advanced database concepts.  Topics Covered:  CRUD Operations  Joins (INNER, LEFT, RIGHT, FULL)  Subqueries &amp; Nested Queries  Aggregate Functions  Group By &amp; Having  Indexing ...

# **Section 1: Fundamental SQL Concepts**

---

## **Q1. What is the difference between WHERE and HAVING?**

| Aspect | WHERE | HAVING |
| --- | --- | --- |
| Filters on | Raw data (before GROUP BY) | Aggregated data (after GROUP BY) |
| Example usage | `WHERE status = 'active'` | `HAVING COUNT(*) > 10` |

**Example:**

```sql

SELECT department, COUNT(*)
FROM employees
WHERE status = 'active'
GROUP BY department
HAVING COUNT(*) > 5;

```
**Follow-up:**

*Can you use HAVING without GROUP BY?*

✅ Yes, HAVING works even without GROUP BY to filter aggregated results.

## **Q2. What is a Primary Key vs Foreign Key?**

- **Primary Key:** Uniquely identifies each row (e.g., `customer_id`)
- **Foreign Key:** Links to another table’s primary key (e.g., `order.customer_id`)

✅:** Why do we need Foreign Keys?

➡️ They **enforce referential integrity.**

## **Q3. Difference Between UNION and UNION ALL**

- **UNION:** Removes duplicates
- **UNION ALL:** Keeps duplicates

✅ **Understand:** Always prefer UNION ALL if you don’t need deduplication (faster).

## **Q4. Types of JOINs and When to Use Them**

| Type | What It Does | Example Use Case |
| --- | --- | --- |
| INNER JOIN | Only matching rows from both tables | Customers who placed orders |
| LEFT JOIN | All from left + matched rows from right | All products, even if unsold |
| RIGHT JOIN | All from right + matched rows from left | All sales reps, even if no sales |
| FULL JOIN | All rows from both (match if possible) | Audit of users vs purchases |


*When do you use EXISTS instead of JOIN?*

✅ EXISTS is faster for **checking existence** without returning full data.

## **Q5. NULL Handling: How Do You Deal with NULLs in SQL?**

- Use `COALESCE()`, `IFNULL()`
- Filtering: `WHERE col IS NULL` (not `= NULL`)

✅ **Example:**

```sql

SELECT COALESCE(phone_number, 'Not Provided') FROM customers;

```

✅ ** What’s the difference between NULLIF and COALESCE?

- NULLIF returns NULL if **two inputs are equal**
- COALESCE returns the **first non-NULL**

## **Q6. What is Normalization? Why is it Important?**

- Process of **breaking data into smaller, related tables** to avoid redundancy.
- Ensures **data integrity + easier updates.**

✅ **Example:**

Split `customer_name` + `customer_orders` into 2 tables.