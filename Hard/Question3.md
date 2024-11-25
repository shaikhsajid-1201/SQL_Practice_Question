### Amazon HARD SQL Practice Question

**Question:**  
You are given a table `orders` containing the following schema:  
- `order_id` (INT): Unique identifier for each order  
- `customer_id` (INT): Unique identifier for each customer  
- `order_date` (DATE): The date the order was placed  
- `order_status` (VARCHAR): Status of the order, e.g., 'Completed', 'Pending', 'Cancelled'  
- `total_amount` (DECIMAL): Total amount of the order  

Find the percentage of completed orders for each customer relative to their total number of orders. Include only those customers who have placed more than 5 orders in total. The output should have 2 columns: `customer_id` and `completion_rate` (rounded to 2 decimal places). Sort the result by the highest completion rate first, and if there is a tie, by the lowest customer ID.

**Schema and Dataset (MySQL):**  
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    order_status VARCHAR(20),
    total_amount DECIMAL(10, 2)
);

INSERT INTO orders (order_id, customer_id, order_date, order_status, total_amount)
VALUES 
(1, 101, '2024-01-01', 'Completed', 150.00),
(2, 102, '2024-01-02', 'Pending', 200.00),
(3, 101, '2024-01-03', 'Completed', 300.00),
(4, 103, '2024-01-04', 'Cancelled', 100.00),
(5, 101, '2024-01-05', 'Pending', 250.00),
(6, 104, '2024-01-06', 'Completed', 400.00),
(7, 101, '2024-01-07', 'Completed', 100.00),
(8, 103, '2024-01-08', 'Completed', 300.00),
(9, 101, '2024-01-09', 'Completed', 200.00),
(10, 105, '2024-01-10', 'Completed', 500.00),
(11, 101, '2024-01-11', 'Completed', 350.00),
(12, 103, '2024-01-12', 'Pending', 400.00);
```

**Explanation to Solve This Question:**  
1. Group the orders by `customer_id` to calculate the total orders for each customer.  
2. Filter customers with more than 5 orders.  
3. Count the number of 'Completed' orders for each customer.  
4. Calculate the `completion_rate` as the ratio of completed orders to total orders, rounded to 2 decimal places.  
5. Sort the result by `completion_rate` in descending order and resolve ties using `customer_id`.

**CALL TO ACTION:**  
"Try solving this question to enhance your SQL skills! Share your queries and thoughts in the comments. 🚀"  

#sqlinterview #mysqlqueries #dataanalytics #amazonjobs #sqlpractice
