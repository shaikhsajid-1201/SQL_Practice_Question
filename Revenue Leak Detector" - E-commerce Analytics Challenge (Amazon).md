# **Revenue Leak Detector" - E-commerce Analytics Challenge (Amazon)**

#### Question:
An e-commerce company noticed discrepancies in their revenue records. They suspect some transactions might have been recorded incorrectly. Your task is to identify potential revenue leaks by finding transactions where the `total_price` of an order does not match the sum of `price_per_unit * quantity` for the items in the order.

Return the `order_id`, `total_price`, and `calculated_price` (calculated as `SUM(price_per_unit * quantity)`) for orders where the `total_price` and `calculated_price` differ.

#### Schema and Dataset:
Here’s the database schema and sample dataset:

##### Orders Table:
```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    total_price DECIMAL(10, 2)
);

INSERT INTO Orders (order_id, total_price)
VALUES
(1, 150.00),
(2, 100.00),
(3, 200.00),
(4, 75.50);
```

##### Order_Items Table:
```sql
CREATE TABLE Order_Items (
    item_id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    price_per_unit DECIMAL(10, 2),
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id)
);

INSERT INTO Order_Items (order_id, price_per_unit, quantity)
VALUES
(1, 50.00, 2),
(1, 25.00, 1),
(2, 25.00, 4),
(3, 50.00, 2),
(3, 20.00, 1),
(4, 25.00, 3);
```

#### Explanation:
1. **`Orders` table** contains a summary of each order's total price.  
2. **`Order_Items` table** stores the item-level details for each order.  
3. Calculate the `calculated_price` for each `order_id` by summing `price_per_unit * quantity` from the `Order_Items` table.  
4. Compare the `calculated_price` with `total_price` in the `Orders` table.  
5. Identify discrepancies where the values don't match.

#### Solution:
```SQL
SELECT 
    o.order_id,
    o.total_price,
    SUM(oi.price_per_unit * oi.quantity) AS calculated_price
FROM 
    Orders o
JOIN 
    Order_Items oi 
ON 
    o.order_id = oi.order_id
GROUP BY 
    o.order_id, o.total_price
HAVING 
    o.total_price != SUM(oi.price_per_unit * oi.quantity);
```
