## **~~ Tricky Advance Level Questions ~~ **

------------------------------------------------------------------------------------------------------------------

## **Q01. What is an Index? Why Do Queries Get Faster?**

✅ Index = **Pointer structure (B-tree)**

✅ Speeds up **row retrieval by skipping full scans.**

------------------------------------------------------------------------------------------------------------------

## **Q02. How Do You Optimize a Slow Query?**

Checklist:

- ✅ Use `EXPLAIN` to analyze
- ✅ Add proper indexes
- ✅ Avoid SELECT *
- ✅ Rewrite subqueries as JOINs
- ✅ Use partition filters if available

------------------------------------------------------------------------------------------------------------------

## **Q03. How Do You Pivot Data in SQL?**

```sql

SELECT user_id,
       SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END) AS jan_sales,
       SUM(CASE WHEN month = 'Feb' THEN sales ELSE 0 END) AS feb_sales
FROM sales
GROUP BY user_id;

------------------------------------------------------------------------------------------------------------------

## **Q04. What is a Window Function?**

✅ **Example:**

```sql

SELECT customer_id,
       SUM(order_amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS running_total
FROM orders;

```

➡️ **Key:** Computes across a "window" of rows.

------------------------------------------------------------------------------------------------------------------


## **Q05. Difference Between RANK(), DENSE_RANK(), ROW_NUMBER()**
Ans :- 

Function	Behavior
ROW_NUMBER()	Unique sequence (no ties)
RANK()	Skips ranks if duplicates (1,2,2,4)
DENSE_RANK()	No gaps (1,2,2,3)

------------------------------------------------------------------------------------------------------------------


## **Q06. Remove Duplicate Rows Efficiently**

```sql

WITH ranked AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY email ORDER BY created_at ASC) AS rn
  FROM users
)
DELETE FROM users WHERE user_id IN (SELECT user_id FROM ranked WHERE rn > 1);

------------------------------------------------------------------------------------------------------------------


## **Q07. Full-Text Search Strategy**

Postgres:

```sql

SELECT * FROM articles
WHERE to_tsvector('english', content) @@ to_tsquery('data & science');

```

✅ Faster than LIKE '%...%'

------------------------------------------------------------------------------------------------------------------