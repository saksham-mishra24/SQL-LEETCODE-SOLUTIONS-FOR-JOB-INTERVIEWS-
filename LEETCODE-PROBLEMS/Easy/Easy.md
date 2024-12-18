# 🧠 LeetCode SQL Problems 🏆

Welcome to the **LeetCode Easy SQL Problems** collection! This repository contains solutions to SQL problems from LeetCode, with detailed explanations to help you master SQL concepts.

---

## 📋 Table of Contents

| Problem No. | Title                                | Difficulty | Solution Link                                     |
|-----------|--------------------------------------|------------|--------------------------------------------------|
| 1         | Find Highest Salary                 | Easy       | [View Solution](./easy/problem-1-find-highest-salary.md) |
| 2         | Count Employees                     | Easy       | [View Solution](./easy/problem-2-count-employees.md)     |
| 3         | Employee Department                 | Medium     | [View Solution](./medium/problem-1-employee-department.md) |
| 4         | Sales Performance                   | Medium     | [View Solution](./medium/problem-2-sales-performance.md)  |
| 5         | Consecutive Orders                  | Hard       | [View Solution](./hard/problem-1-consecutive-orders.md)   |
| 6         | Recursive CTE                       | Hard       | [View Solution](./hard/problem-2-recursive-cte.md)        |

---

## 🚀 Problems and Solutions

---







# 📝 SQL Problem #1: [Title of the Problem]

## 🚀 Problem Statement
Write a SQL query to **[brief description of the task]**. 

### Example Input
Here’s an example of the input table(s):

#### `Table: employees`
| employee_id | name        | department | salary  |
|-------------|-------------|------------|---------|
| 1           | John Doe    | HR         | 5000    |
| 2           | Jane Smith  | IT         | 7000    |
| 3           | Alice Brown | Finance    | 6000    |
| 4           | Bob White   | IT         | 8000    |

### Expected Output
#### Example Output
| department | avg_salary |
|------------|------------|
| HR         | 5000       |
| IT         | 7500       |
| Finance    | 6000       |

---

## 💡 Solution

### Query
```sql
SELECT 
    department, 
    AVG(salary) AS avg_salary
FROM 
    employees
GROUP BY 
    department;
