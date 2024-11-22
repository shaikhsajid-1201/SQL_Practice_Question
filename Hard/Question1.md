**Title** - Hard SQL Practice Question  

**Question** -  
Find the top three highest-paid employees in each department. The output should include the department name, employee name, and their salary. If multiple employees have the same salary, include them all in the result.  

**What you will learn** - 🔍 By solving this, you'll learn how to use window functions like `RANK()` or `DENSE_RANK()` and handle advanced filtering using subqueries. Give it a try and share the output!👇  

**Schema and Dataset** -  
```sql
CREATE TABLE Employees (
    Employee_ID INT,
    Name VARCHAR(255),
    Department_ID INT,
    Salary DECIMAL(10, 2),
    Join_Date DATE
);

CREATE TABLE Departments (
    Department_ID INT,
    Department_Name VARCHAR(255)
);

INSERT INTO Employees (Employee_ID, Name, Department_ID, Salary, Join_Date) VALUES
(1, 'John Doe', 1, 60000.00, '2022-01-10'),
(2, 'Jane Smith', 1, 55000.00, '2023-03-15'),
(3, 'Tom Brown', 2, 70000.00, '2021-06-20'),
(4, 'Lucy Green', 2, 65000.00, '2022-07-11'),
(5, 'Emma White', 1, 75000.00, '2023-04-22'),
(6, 'Jack Black', 2, 72000.00, '2024-05-01'),
(7, 'Olivia Blue', 3, 90000.00, '2024-07-12'),
(8, 'Chris Gray', 3, 85000.00, '2024-08-05'),
(9, 'Ethan Red', 3, 87000.00, '2024-09-20'),
(10, 'Sophia Yellow', 1, 75000.00, '2024-09-25');

INSERT INTO Departments (Department_ID, Department_Name) VALUES
(1, 'HR'),
(2, 'Engineering'),
(3, 'Marketing');
```

**Explanation to solve this Question**  
To solve this question, you'll need to:
1. Use a **window function** like `RANK()` or `DENSE_RANK()` to assign rankings to employees within each department based on their salary.
2. Filter to include only the top three ranked employees in each department.
3. Use a `JOIN` to combine department names with employee details.

**SOLUTION**  
```sql
WITH RankedSalaries AS (
    SELECT 
        d.Department_Name,
        e.Name AS Employee_Name,
        e.Salary,
        RANK() OVER (PARTITION BY e.Department_ID ORDER BY e.Salary DESC) AS Rank
    FROM 
        Employees e
    JOIN 
        Departments d
    ON 
        e.Department_ID = d.Department_ID
)
SELECT 
    Department_Name,
    Employee_Name,
    Salary
FROM 
    RankedSalaries
WHERE 
    Rank <= 3
ORDER BY 
    Department_Name, Rank, Employee_Name;
```
