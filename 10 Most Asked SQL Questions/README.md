# 🚀 Top 10 SQL Interview Questions

---

## Q1. Second Highest Salary

### Problem
Find the second highest salary from the Employee table.

### Query

```sql
SELECT MAX(salary) AS second_highest_salary
FROM Employee
WHERE salary < (SELECT MAX(salary) FROM Employee);
```

### Explanation
- Inner query finds highest salary.
- Outer query finds the maximum salary less than highest.

---

## Q2. Rank Employees by Salary Within Each Department

### Problem
Rank employees department-wise by salary.

### Query

```sql
SELECT employee_id,
       department_id,
       salary,
       RANK() OVER (
           PARTITION BY department_id
           ORDER BY salary DESC
       ) AS salary_rank
FROM Employee;
```

### Explanation
- PARTITION BY splits ranking per department.
- ORDER BY sorts salary descending.
- RANK assigns ranking with gaps for ties.

---

## Q3. Moving Average of Sales (Last 3 Days)

### Problem
Calculate 3-day rolling average revenue.

### Query

```sql
SELECT order_date,
       AVG(total_amount) OVER (
           ORDER BY order_date
           ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
       ) AS moving_avg
FROM Orders;
```

### Explanation
- Window includes current row + 2 previous rows.
- Computes rolling average.

---

## Q4. Customers Who Ordered Every Month in 2023

### Problem
Find customers who placed orders in all 12 months.

### Query

```sql
SELECT customer_id
FROM Orders
WHERE EXTRACT(YEAR FROM order_date) = 2023
GROUP BY customer_id
HAVING COUNT(DISTINCT TO_CHAR(order_date, 'YYYY-MM')) = 12;
```

### Explanation
- Filters orders in 2023.
- Groups by customer.
- Ensures 12 distinct months.

---

## Q5. Find Churned Customers (No Orders in Last 6 Months)

### Problem
Find customers inactive for 6 months.

### Query

```sql
SELECT customer_id
FROM Orders
GROUP BY customer_id
HAVING MAX(order_date) < (NOW() - INTERVAL '6 months');
```

### Explanation
- Finds each customer’s last order.
- Checks if older than 6 months.

---

## Q6. Cumulative Revenue by Day

### Problem
Calculate running revenue.

### Query

```sql
SELECT order_date,
       SUM(total_amount) OVER (
           ORDER BY order_date
       ) AS cumulative_revenue
FROM Orders;
```

### Explanation
- Window function calculates running total.
- No GROUP BY needed.

---

## Q7. Customers With Consecutive Purchase Days

### Problem
Find customers who purchased on consecutive days.

### Query

```sql
WITH cte AS (
    SELECT customer_id,
           order_date,
           LAG(order_date) OVER (
               PARTITION BY customer_id
               ORDER BY order_date
           ) AS prev_order
    FROM Orders
)
SELECT *
FROM cte
WHERE order_date - prev_order = INTERVAL '1 day';
```

### Explanation
- LAG gets previous order date.
- Compares difference of 1 day.

---

## Q8. Product Revenue Contribution (%)

### Problem
Find percentage contribution of each product to total revenue.

### Query

```sql
WITH total AS (
    SELECT SUM(quantity * price) AS total_revenue
    FROM Sales
)
SELECT product_id,
       SUM(quantity * price) AS revenue,
       SUM(quantity * price) * 100.0 / t.total_revenue AS revenue_pct
FROM Sales
CROSS JOIN total t
GROUP BY product_id, t.total_revenue;
```

### Explanation
- CTE calculates total revenue.
- CROSS JOIN attaches total to each row.
- Calculates percentage.

---

## Q9. Customers Above 90th Percentile Spending

### Problem
Find customers above 90th percentile of spending.

### Query

```sql
WITH customer_total AS (
    SELECT customer_id,
           SUM(total_amount) AS total_spent
    FROM Orders
    GROUP BY customer_id
)
SELECT *
FROM customer_total
WHERE total_spent >= (
    SELECT PERCENTILE_CONT(0.9)
    WITHIN GROUP (ORDER BY total_spent)
    FROM customer_total
);
```

### Explanation
- Calculates total per customer.
- Computes 90th percentile.
- Filters top spenders.

---

## Q10. Products Contributing to 80% Revenue (Pareto Rule)

### Problem
Find products generating top 80% of revenue.

### Query

```sql
WITH sales_cte AS (
    SELECT product_id,
           SUM(quantity * price) AS revenue
    FROM Sales
    GROUP BY product_id
),
total_revenue AS (
    SELECT SUM(revenue) AS total FROM sales_cte
)
SELECT *
FROM (
    SELECT s.product_id,
           s.revenue,
           SUM(s.revenue) OVER (ORDER BY s.revenue DESC) AS cumulative_revenue,
           t.total
    FROM sales_cte s
    CROSS JOIN total_revenue t
) x
WHERE cumulative_revenue <= total * 0.8;
```

### Explanation
- Calculates product revenue.
- Sorts descending.
- Computes cumulative sum.
- Filters top 80%.

---

# Key Concepts Covered

- Aggregation (SUM, AVG, COUNT, MAX)
- GROUP BY & HAVING
- Subqueries
- CTE (WITH)
- Window Functions
- RANK / LAG
- Running Totals
- Percentiles
- Business Analytics (Churn, Pareto)
