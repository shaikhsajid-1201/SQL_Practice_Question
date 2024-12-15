# ByteBakes - Medium: Tracking Customer Loyalty with Fun Analytics

#### Question  
ByteBakes is a local bakery startup that offers personalized pastries and cakes. Write a query to identify **customers who made purchases on every weekend** (Saturday or Sunday) in the last month.  

#### Schema and Dataset
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

INSERT INTO customers (customer_id, customer_name) VALUES
(1, 'Alice'),
(2, 'Bob'),
(3, 'Charlie'),
(4, 'Diana');

INSERT INTO orders (order_id, customer_id, order_date, total_amount) VALUES
(101, 1, '2024-11-04', 25.50),
(102, 2, '2024-11-11', 15.00),
(103, 1, '2024-11-18', 30.75),
(104, 3, '2024-11-03', 12.00),
(105, 3, '2024-11-09', 22.50),
(106, 4, '2024-11-10', 18.25),
(107, 2, '2024-11-17', 40.00),
(108, 1, '2024-11-24', 20.00),
(109, 3, '2024-11-25', 15.75);
```

#### Learning Objective  
🎯 **Master Date Functions and Filtering Techniques:** Learn to work with date-related operations, filtering specific days, and analyzing customer behavior patterns.

#### Explanation  
1️⃣ Identify orders made on weekends using **DAYOFWEEK()** or similar date functions.  
2️⃣ Filter only the weekends of the **last month** using date logic (e.g., `MONTH(order_date) = MONTH(CURDATE()) - 1`).  
3️⃣ Use **GROUP BY customer_id** to track customer purchases across weekends.  
4️⃣ Ensure customers have orders on **both Saturday and Sunday** by checking weekend counts.  
5️⃣ Output only the `customer_id` and `customer_name` for qualifying customers.  

#### Solution  
```sql
WITH cte AS(
	SELECT 
		c.customer_name,
		DAYOFWEEK(o.order_date) as weekdays
	FROM orders o 
	JOIN customers c
		ON o.customer_id = c.customer_id
	WHERE 
		MONTH(o.order_date) = MONTH(CURDATE()) -1
)
SELECT customer_name
FROM cte
WHERE weekdays IN (1, 7)
GROUP BY customer_name
HAVING COUNT(*) = 2;
```
