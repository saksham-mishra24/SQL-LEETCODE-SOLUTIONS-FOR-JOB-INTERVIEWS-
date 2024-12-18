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
