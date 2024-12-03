# Can You Solve This? LinkedIn - Medium 🚀 Employee Popularity Rank

### Question  
Write a query to find the **employee names along with their departments** ranked by their **popularity score** (calculated as the total endorsements they received) in descending order within each department. Return only the **top 3 employees** from each department based on popularity.  

### Schema & Dataset  

**Table: Employees**  
```sql  
CREATE TABLE Employees (  
    emp_id INT PRIMARY KEY,  
    emp_name VARCHAR(100),  
    department_id INT,  
    hire_date DATE  
);  

INSERT INTO Employees VALUES  
(1, 'Alice', 101, '2022-01-10'),  
(2, 'Bob', 102, '2021-03-15'),  
(3, 'Charlie', 101, '2020-12-20'),  
(4, 'David', 103, '2019-08-10'),  
(5, 'Eve', 102, '2021-07-25'),  
(6, 'Frank', 101, '2020-11-11');  
```  

**Table: Endorsements**  
```sql  
CREATE TABLE Endorsements (  
    endorsement_id INT PRIMARY KEY,  
    emp_id INT,  
    endorsement_score INT  
);  

INSERT INTO Endorsements VALUES  
(1, 1, 50),  
(2, 2, 40),  
(3, 3, 30),  
(4, 4, 70),  
(5, 5, 60),  
(6, 6, 20),  
(7, 1, 30),  
(8, 3, 50),  
(9, 2, 20);  
```  

### What You Will Learn? 🎓  
- Master **window functions** like `RANK()` or `DENSE_RANK()` for advanced ranking operations.  
- Understand **partitioning data** by departments for granular analysis.  

### Explanation:  
1️⃣ **Calculate Popularity Score**: Sum up all endorsements per employee using `GROUP BY`.  
2️⃣ **Rank Employees**: Use `RANK()` to assign ranks based on popularity score within each department.  
3️⃣ **Filter Top Results**: Retrieve only the top 3 employees for each department based on their rank.  
4️⃣ **Final Result**: Join back with the `Employees` table for names and department details.  

### Solution:
```SQL
WITH Popularity AS (
    SELECT 
        e.emp_id, 
        e.emp_name, 
        e.department_id, 
        SUM(en.endorsement_score) AS total_score
    FROM 
        Employees e
    JOIN 
        Endorsements en
    ON 
        e.emp_id = en.emp_id
    GROUP BY 
        e.emp_id, e.emp_name, e.department_id
),
RankedEmployees AS (
    SELECT 
        emp_name, 
        department_id, 
        total_score, 
        RANK() OVER (PARTITION BY department_id ORDER BY total_score DESC) AS rank
    FROM 
        Popularity
)
SELECT 
    emp_name, 
    department_id, 
    total_score, 
    rank
FROM 
    RankedEmployees
WHERE 
    rank <= 3
ORDER BY 
    department_id, rank;
```
