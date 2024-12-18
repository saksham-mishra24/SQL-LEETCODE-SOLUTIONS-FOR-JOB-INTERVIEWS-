# 🧠 LeetCode SQL Problems 🏆

Welcome to the **LeetCode Easy Medium Problems** collection! This repository contains solutions to SQL problems from LeetCode, with detailed explanations to help you master SQL concepts.

---

## 📋 Table of Contents

## 📋 Table of Contents

| Problem No. | Title                                | Difficulty | Solution Link                                     | Video Link              |
|-------------|--------------------------------------|------------|--------------------------------------------------|-------------------------|
| 1613        | Find the Missing IDs                 | Medium     | [View Solution](./easy/problem-1-find-highest-salary.md) | [Watch Video](https://www.youtube.com/watch?v=WBqTj-FYux8)       |
| 1555        | Bank Account Summary             | Medium     | [View Solution](./easy/problem-2-count-employees.md)     | [Watch Video](#)       |
| 1549        | The Most Recent Orders for Each Product                | Medium     | [View Solution](./medium/problem-1-employee-department.md) | [Watch Video](#)       |
| 1532.           | The Most Recent Three Orders   | Medium     | [View Solution](./medium/problem-2-sales-performance.md)  | [Watch Video](#)       |
| 5           | Consecutive Orders                  | Medium     | [View Solution](./hard/problem-1-consecutive-orders.md)   | [Watch Video](#)       |
| 6           | Recursive CTE                       | Medium     | [View Solution](./hard/problem-2-recursive-cte.md)        | [Watch Video](#)       |


---

## 🚀 Problems and Solutions

---







# 📝 SQL Problem #1: Find the Missing IDs

## 🚀 Problem Statement
Write a SQL query to find the missing customer IDs. The missing IDs are ones that are not in the Customers table but are in the range between 1 and the maximum customer_id 
present in the table.

Notice that the maximum customer_id will not exceed 100.

Return the result table ordered by ids in ascending order. 

### Example Input
Here’s an example of the input table(s):

#### `Table: Customer`
Customer table:
+-------------+---------------+
| customer_id | customer_name |
+-------------+---------------+
| 1           | Alice         |
| 4           | Bob           |
| 5           | Charlie       |
+-------------+---------------+

### Expected Output
+-----+
| ids |
+-----+
| 2   |
| 3   |
+-----+
---

## 💡 Solution

### Query
```sql
WITH CTE AS (
    -- Anchor member: Start with the minimum customer_id
    SELECT MIN(customer_id) AS n, MAX(customer_id) AS max_id
    FROM Customer

    UNION ALL

    -- Recursive member: Increment n by 1 until it reaches max_id
    SELECT n + 1 AS n, max_id
    FROM CTE
    WHERE n < max_id
)
-- Final query: Select only the generated sequence
SELECT n
FROM CTE
EXCEPT
SELECT customer_id
FROM Customer
```






# 📝 SQL Problem #2: Bank Account Summary


## 🚀 Problem Statement
eetcode Bank (LCB) helps its coders in making virtual payments. Our bank records all transactions in the table Transaction, we want to find out the current balance of all users and check wheter they have breached their credit limit (If their current credit is less than 0).

Write an  SQL query to report.SQL database courses

user_id
user_name
credit, current balance after performing transactions.  
credit_limit_breached, check credit_limit ("Yes" or "No")
Return the result table in any order.

### Example Input
Here’s an example of the input table(s):

#### `Table: Customer`
Users table:
+------------+--------------+-------------+
| user_id    | user_name    | credit      |
+------------+--------------+-------------+
| 1          | Moustafa     | 100         |
| 2          | Jonathan     | 200         |
| 3          | Winston      | 10000       |
| 4          | Luis         | 800         |
+------------+--------------+-------------+

Transaction table:
+------------+------------+------------+----------+---------------+
| trans_id   | paid_by    | paid_to    | amount   | transacted_on |
+------------+------------+------------+----------+---------------+
| 1          | 1          | 3          | 400      | 2020-08-01    |
| 2          | 3          | 2          | 500      | 2020-08-02    |
| 3          | 2          | 1          | 200      | 2020-08-03    |
+------------+------------+------------+----------+---------------+


### Expected Output
Result table:
+------------+------------+------------+-----------------------+
| user_id    | user_name  | credit     | credit_limit_breached |
+------------+------------+------------+-----------------------+
| 1          | Moustafa   | -100       | Yes                   |
| 2          | Jonathan   | 500        | No                    |
| 3          | Winston    | 9990       | No                    |
| 4          | Luis       | 800        | No                    |
+------------+------------+------------+-----------------------+
Moustafa paid $400 on "2020-08-01" and received $200 on "2020-08-03", credit (100 -400 +200) = -$100
Jonathan received $500 on "2020-08-02" and paid $200 on "2020-08-08", credit (200 +500 -200) = $500
Winston received $400 on "2020-08-01" and paid $500 on "2020-08-03", credit (10000 +400 -500) = $9990
Luis didn't received any transfer, credit = $800

## 💡 Solution

### Query
```sql

SELECT 
    u.user_id,  
    u.user_name,  
    u.credit + 
        COALESCE(SUM(rt.amount), 0) -  -- Total amount received (amount paid_to)
        COALESCE(SUM(st.amount), 0)    -- Total amount sent (amount paid_by)
    AS current_credit,  -- Updated credit balance
    CASE 
        WHEN u.credit + 
             COALESCE(SUM(rt.amount), 0) - 
             COALESCE(SUM(st.amount), 0) < 0 
        THEN 'Yes'  -- Credit limit breached
        ELSE 'No'   -- No breach
    END AS credit_limit_breached  -- Output indicating whether the credit limit is breached
FROM Users u  -- Users table
LEFT JOIN Transaction st ON u.user_id = st.paid_by  -- Left join for sent transactions (paid_by)
LEFT JOIN Transaction rt ON u.user_id = rt.paid_to  -- Left join for received transactions (paid_to)
GROUP BY u.user_id, u.user_name, u.credit  -- Group by user_id to calculate the individual balance
ORDER BY u.user_id;  -- 
```





# 📝 SQL Problem #3: The Most Recent Orders for Each Product


## 🚀 Problem Statement
Write an  SQL query to find the most recent order(s) of each product.SQL database courses

Return the result table sorted by product_name in ascending order and in case of a tie by the product_id in ascending order. If there still a tie, order them by the order_id in ascending order.

### Example Input
Here’s an example of the input table(s):

#### `Table: `

Customers
+-------------+-----------+
| customer_id | name      |
+-------------+-----------+
| 1           | Winston   |
| 2           | Jonathan  |
| 3           | Annabelle |
| 4           | Marwan    |
| 5           | Khaled    |
+-------------+-----------+

Orders
+----------+------------+-------------+------------+
| order_id | order_date | customer_id | product_id |
+----------+------------+-------------+------------+
| 1        | 2020-07-31 | 1           | 1          |
| 2        | 2020-07-30 | 2           | 2          |
| 3        | 2020-08-29 | 3           | 3          |
| 4        | 2020-07-29 | 4           | 1          |
| 5        | 2020-06-10 | 1           | 2          |
| 6        | 2020-08-01 | 2           | 1          |
| 7        | 2020-08-01 | 3           | 1          |
| 8        | 2020-08-03 | 1           | 2          |
| 9        | 2020-08-07 | 2           | 3          |
| 10       | 2020-07-15 | 1           | 2          |
+----------+------------+-------------+------------+

Products
+------------+--------------+-------+
| product_id | product_name | price |
+------------+--------------+-------+
| 1          | keyboard     | 120   |
| 2          | mouse        | 80    |
| 3          | screen       | 600   |
| 4          | hard disk    | 450   |
+------------+--------------+-------+


### Expected Output

Result table:
+--------------+------------+----------+------------+
| product_name | product_id | order_id | order_date |
+--------------+------------+----------+------------+
| keyboard     | 1          | 6        | 2020-08-01 |
| keyboard     | 1          | 7        | 2020-08-01 |
| mouse        | 2          | 8        | 2020-08-03 |
| screen       | 3          | 3        | 2020-08-29 |
+--------------+------------+----------+------------+
keyboard's most recent order is in 2020-08-01, it was ordered two times this day.
mouse's most recent order is in 2020-08-03, it was ordered only once this day.
screen's most recent order is in 2020-08-29, it was ordered only once this day.
The hard disk was never ordered and we don't include it in the result table.



## 💡 Solution

### Query
```sql

SELECT 
    p.product_name,   
    p.product_id,    
    o.order_id,       
    o.order_date     
FROM Orders o  -- Orders table
JOIN Products p
ON o.product_id = p.product_id  -- Join Orders and Products on product_id
WHERE o.order_date = (  -- Filter to get the most recent order date for each product
    SELECT MAX(order_date)  -- Get the maximum order_date for the given product
    FROM Orders
    WHERE product_id = o.product_id  -- Ensure it’s for the current product
)
ORDER BY p.product_name ASC,  -- Sort first by product_name in ascending order
         p.product_id ASC,    -- Then by product_id in ascending order
         o.order_id ASC;      -- Finally, by order_id in ascending order

```



# 📝 SQL Problem #4: The Most Recent Three Orders


## 🚀 Problem Statement
Write an  SQL query to find the most recent 3 orders of each user. If a user ordered less than 3 orders return all of their orders.SQL database courses

Return the result table sorted by customer_name in ascending order and in case of a tie by the customer_id in ascending order. If there still a tie, order them by the order_date in descending order.

### Example Input
Here’s an example of the input table(s):

#### `Table: `

Customers
+-------------+-----------+
| customer_id | name      |
+-------------+-----------+
| 1           | Winston   |
| 2           | Jonathan  |
| 3           | Annabelle |
| 4           | Marwan    |
| 5           | Khaled    |
+-------------+-----------+

Orders
+----------+------------+-------------+------+
| order_id | order_date | customer_id | cost |
+----------+------------+-------------+------+
| 1        | 2020-07-31 | 1           | 30   |
| 2        | 2020-07-30 | 2           | 40   |
| 3        | 2020-07-31 | 3           | 70   |
| 4        | 2020-07-29 | 4           | 100  |
| 5        | 2020-06-10 | 1           | 1010 |
| 6        | 2020-08-01 | 2           | 102  |
| 7        | 2020-08-01 | 3           | 111  |
| 8        | 2020-08-03 | 1           | 99   |
| 9        | 2020-08-07 | 2           | 32   |
| 10       | 2020-07-15 | 1           | 2    |
+----------+------------+-------------+------+


### Expected Output

+---------------+-------------+----------+------------+
| customer_name | customer_id | order_id | order_date |
+---------------+-------------+----------+------------+
| Annabelle     | 3           | 7        | 2020-08-01 |
| Annabelle     | 3           | 3        | 2020-07-31 |
| Jonathan      | 2           | 9        | 2020-08-07 |
| Jonathan      | 2           | 6        | 2020-08-01 |
| Jonathan      | 2           | 2        | 2020-07-30 |
| Marwan        | 4           | 4        | 2020-07-29 |
| Winston       | 1           | 8        | 2020-08-03 |
| Winston       | 1           | 1        | 2020-07-31 |
| Winston       | 1           | 10       | 2020-07-15 |
+---------------+-------------+----------+------------+
Winston has 4 orders, we discard the order of "2020-06-10" because it is the oldest order.
Annabelle has only 2 orders, we return them.
Jonathan has exactly 3 orders.
Marwan ordered only one time.
We sort the result table by customer_name in ascending order, by customer_id in ascending order and by order_date in descending order in case of a tie.



## 💡 Solution

### Query
```sql

WITH RankedOrders AS (
    SELECT
        c.name AS customer_name, 
        o.customer_id,           
        o.order_id,              
        o.order_date,             
        ROW_NUMBER() OVER (PARTITION BY o.customer_id ORDER BY o.order_date DESC) AS rn
    FROM Orders o
    JOIN Customers c ON o.customer_id = c.customer_id
)
SELECT
    customer_name,
    customer_id,
    order_id,
    order_date
FROM RankedOrders
WHERE rn <= 3  -- Only select the top 3 most recent orders
ORDER BY customer_name ASC,  -- Sort by customer name in ascending order
         customer_id ASC,    -- If names tie, sort by customer ID
         order_date DESC;     -- Finally, by order date in descending order

```
