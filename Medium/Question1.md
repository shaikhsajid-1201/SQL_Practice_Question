**Title** - Medium SQL Practice Question  

**Question** -  
Find the average salary of employees in each department where the department has more than 3 employees. The result should include the department name, the number of employees in the department, and the average salary. Sort the result by department name.  

**What you will learn** - 🔍 By solving this, you'll learn how to use `GROUP BY`, `HAVING`, and aggregate functions like `AVG()` and `COUNT()`. Give it a try and share the output!👇  

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
(6, 'Jack Black', 3, 80000.00, '2024-05-01'),
(7, 'Olivia Blue', 3, 90000.00, '2024-07-12'),
(8, 'Chris Gray', 3, 85000.00, '2024-08-05');

INSERT INTO Departments (Department_ID, Department_Name) VALUES
(1, 'HR'),
(2, 'Engineering'),
(3, 'Marketing');
```

**Explanation to solve this Question**  
To solve this question, you'll need to:
1. Use `GROUP BY` to group the employees by department.
2. Apply `HAVING` to filter departments that have more than 3 employees.
3. Use `AVG()` to calculate the average salary of employees in each department.
4. Use `COUNT()` to get the number of employees in each department.
5. Sort the result by `Department_Name`.  


[LinkedIn Post Link]()  


**SOLUTION** -
```sql

```
