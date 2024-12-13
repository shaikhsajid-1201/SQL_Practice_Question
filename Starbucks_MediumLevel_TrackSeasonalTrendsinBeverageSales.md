# Starbucks - Medium: Track Seasonal Trends in Beverage Sales

#### Question:
Write a query to find the top 3 beverages sold during the fall season (September-November) across all stores in 2023, along with the total quantity sold for each. Order the results by total quantity in descending order.

#### Schema and Dataset (MySQL):
```sql
CREATE TABLE sales (
    sale_id INT PRIMARY KEY,
    store_id INT,
    beverage_name VARCHAR(100),
    sale_date DATE,
    quantity_sold INT
);

CREATE TABLE stores (
    store_id INT PRIMARY KEY,
    store_name VARCHAR(100),
    city VARCHAR(100),
    state VARCHAR(100)
);

INSERT INTO sales VALUES
(1, 101, 'Pumpkin Spice Latte', '2023-09-10', 150),
(2, 102, 'Caramel Macchiato', '2023-09-15', 120),
(3, 103, 'Pumpkin Spice Latte', '2023-10-05', 200),
(4, 101, 'Peppermint Mocha', '2023-11-20', 180),
(5, 102, 'Caramel Macchiato', '2023-11-22', 160),
(6, 103, 'Pumpkin Spice Latte', '2023-10-15', 140);

INSERT INTO stores VALUES
(101, 'Starbucks Downtown', 'Seattle', 'WA'),
(102, 'Starbucks Central', 'Los Angeles', 'CA'),
(103, 'Starbucks Uptown', 'New York', 'NY');
```

#### Learning Objective:
**Learn how to:**
- Use DATE functions to filter specific months 📅
- Group and aggregate data to analyze seasonal trends 📊
- Rank results using ORDER BY and LIMIT.

#### Explanation:
1️⃣ Understand the dataset:
The sales table contains transaction details, including sale_date and quantity_sold.
The stores table provides details about store locations.

2️⃣ Identify the seasonal range:
Fall season includes September, October, and November.

3️⃣ Aggregate sales data:
Sum quantity_sold grouped by beverage_name for the specified months.

4️⃣ Rank and filter results:
Use ORDER BY on total sales and limit to the top 3 beverages.

#### Solution :
```SQL
SELECT 
    beverage_name, 
    SUM(quantity_sold) AS total_quantity_sold
FROM 
    sales
WHERE 
    MONTH(sale_date) IN (9, 10, 11) 
    AND YEAR(sale_date) = 2023
GROUP BY 
    beverage_name
ORDER BY 
    total_quantity_sold DESC
LIMIT 3;
```
