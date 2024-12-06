### Can you solve this? IBM - Medium 🖥️ Tracking Employee Productivity

#### Question  
IBM wants to analyze the **weekly productivity** of employees based on completed tasks. Write a query to identify employees whose **total task completion time exceeds the average task completion time for all employees in the same department during a given week**. Return the employee's ID, name, department, and the total task completion time for the week.  

#### Schema & Dataset  
```sql
CREATE TABLE Employee_Tasks (
    employee_id INT,         -- Employee's unique ID
    employee_name VARCHAR(50),-- Employee's name
    department VARCHAR(50),  -- Department name
    task_date DATE,          -- Date when the task was completed
    task_duration INT        -- Time taken to complete the task (in minutes)
);

INSERT INTO Employee_Tasks (employee_id, employee_name, department, task_date, task_duration)
VALUES
(1, 'Alice', 'IT', '2024-11-25', 120),
(1, 'Alice', 'IT', '2024-11-26', 150),
(1, 'Alice', 'IT', '2024-11-27', 130),
(2, 'Bob', 'IT', '2024-11-25', 100),
(2, 'Bob', 'IT', '2024-11-26', 80),
(2, 'Bob', 'IT', '2024-11-27', 90),
(3, 'Charlie', 'HR', '2024-11-25', 200),
(3, 'Charlie', 'HR', '2024-11-26', 220),
(4, 'Dave', 'HR', '2024-11-25', 150),
(4, 'Dave', 'HR', '2024-11-26', 160),
(5, 'Eve', 'Finance', '2024-11-25', 300),
(5, 'Eve', 'Finance', '2024-11-26', 350),
(6, 'Frank', 'Finance', '2024-11-25', 280),
(6, 'Frank', 'Finance', '2024-11-26', 300);
```

#### What I will learn?  
- Learn to use **window functions** and **conditional filtering** to analyze productivity trends. 📊  
- Understand departmental averages and comparisons in a structured query. 🏢  

#### Explanation  
1️⃣ Calculate total task completion time for each employee grouped by **week**.  
2️⃣ Compute the **average task completion time** per department for the same week.  
3️⃣ Identify employees whose weekly task time exceeds their department’s weekly average.  
4️⃣ Use a combination of **GROUP BY**, **JOIN**, and **HAVING** for efficient analysis.  

### Solution  

Here’s the SQL query to solve the problem:  

```sql
WITH WeeklyEmployeeTaskTime AS (
    SELECT 
        employee_id,
        employee_name,
        department,
        YEARWEEK(task_date) AS week_number,
        SUM(task_duration) AS total_task_time
    FROM 
        Employee_Tasks
    GROUP BY 
        employee_id, employee_name, department, YEARWEEK(task_date)
),
DepartmentAverageTaskTime AS (
    SELECT 
        department,
        week_number,
        AVG(total_task_time) AS avg_task_time
    FROM 
        WeeklyEmployeeTaskTime
    GROUP BY 
        department, week_number
)
SELECT 
    e.employee_id,
    e.employee_name,
    e.department,
    e.total_task_time
FROM 
    WeeklyEmployeeTaskTime e
JOIN 
    DepartmentAverageTaskTime d
ON 
    e.department = d.department AND e.week_number = d.week_number
WHERE 
    e.total_task_time > d.avg_task_time
ORDER BY 
    e.department, e.employee_name;
```

### Output  
Based on the provided dataset, the query will return:  

| Employee_ID | Employee_Name | Department | Total_Task_Time |
|-------------|---------------|------------|-----------------|
| 3           | Charlie       | HR         | 420             |
| 5           | Eve           | Finance    | 650             |

### Explanation of Steps  

1️⃣ **Calculate Weekly Task Time**  
- In `WeeklyEmployeeTaskTime`, group tasks by `YEARWEEK(task_date)` to get weekly totals.  

2️⃣ **Find Departmental Averages**  
- Use the `DepartmentAverageTaskTime` CTE to compute the weekly average task time for each department.  

3️⃣ **Compare and Filter**  
- Join the CTEs and filter employees whose task time exceeds their department’s average for the week.  

4️⃣ **Order Results**  
- Sort the output for easy readability by department and employee name.  

This query allows IBM to pinpoint high-performing employees and identify areas of potential inefficiency.
