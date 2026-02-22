---

# 🚀 Tricky Advanced Level SQL Questions

### **Q01. What is an Index? Why Do Queries Get Faster?**

* **The Core:** An Index is a **pointer structure** (typically a B-Tree) that lives alongside your data.
* **The Benefit:** It speeds up row retrieval by allowing the database engine to locate data without performing a **Full Table Scan**.
* **Analogy:** It’s like the index at the back of a textbook; you find the page number first rather than reading every page.

---

### **Q02. How Do You Optimize a Slow Query?**

To troubleshoot performance bottlenecks, follow this checklist:

1. ✅ **Analyze:** Use `EXPLAIN` or `EXPLAIN ANALYZE` to see the execution plan.
2. ✅ **Indexing:** Add proper indexes on columns used in `WHERE`, `JOIN`, and `ORDER BY` clauses.
3. ✅ **Selectivity:** Avoid `SELECT *`; only fetch the columns you actually need.
4. ✅ **Refactor:** Rewrite inefficient subqueries as `JOINs` where possible.
5. ✅ **Partitioning:** Utilize partition filters to narrow the data scan range.

---

### **Q03. How Do You Pivot Data in SQL?**

Pivoting transforms rows into columns. This is commonly achieved using **Conditional Aggregation**:

```sql
SELECT 
    user_id,
    SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END) AS jan_sales,
    SUM(CASE WHEN month = 'Feb' THEN sales ELSE 0 END) AS feb_sales
FROM sales
GROUP BY user_id;

```

---

### **Q04. What is a Window Function?**

Window functions perform calculations across a set of table rows that are somehow related to the current row, without collapsing them into a single output row (unlike `GROUP BY`).

**Example (Running Total):**

```sql
SELECT 
    customer_id,
    SUM(order_amount) OVER (
        PARTITION BY customer_id 
        ORDER BY order_date
    ) AS running_total
FROM orders;

```

---

### **Q05. Difference Between RANK(), DENSE_RANK(), and ROW_NUMBER()**

These functions handle "ties" (identical values) differently:

| Function | Behavior | Result Example (for ties) |
| --- | --- | --- |
| **ROW_NUMBER()** | Assigns a unique, sequential integer. | 1, 2, 3, 4 |
| **RANK()** | Leaves gaps in the sequence after ties. | 1, 2, 2, 4 |
| **DENSE_RANK()** | No gaps in the sequence after ties. | 1, 2, 2, 3 |

---

### **Q06. Remove Duplicate Rows Efficiently**

The most efficient way to target duplicates while keeping the original record is using a **CTE** (Common Table Expression) with `ROW_NUMBER()`:

```sql
WITH ranked AS (
  SELECT 
    user_id, 
    ROW_NUMBER() OVER (PARTITION BY email ORDER BY created_at ASC) AS rn
  FROM users
)
DELETE FROM users 
WHERE user_id IN (SELECT user_id FROM ranked WHERE rn > 1);

```

---

### **Q07. Full-Text Search Strategy**

Standard `LIKE '%keyword%'` queries are slow because they cannot use standard B-Tree indexes effectively. For large-scale text search (e.g., in PostgreSQL), use **TSVECTOR**:

```sql
-- Searching for articles containing both 'data' and 'science'
SELECT * FROM articles
WHERE to_tsvector('english', content) @@ to_tsquery('data & science');

```

✅ **Pro Tip:** This approach supports stemming (e.g., searching "run" finds "running") and is significantly faster than pattern matching.

---
