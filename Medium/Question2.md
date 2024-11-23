### Must Try: User Session & Order Analysis (Medium Level) #SQL Interview Question — Solution 

**Question:**  
Identify users who have started a session and placed an order on the same day. For these users, calculate the total number of orders and the total order value for that day. Your output should include the user, the session date, the total number of orders, and the total order value.

🔍 By solving this, you'll enhance your skills in **JOIN**, **GROUP BY**, and **HAVING** clauses. Challenge yourself to solve it before looking at the solution! Share your output below! 👇

---

### Schema and Dataset:

```sql
CREATE TABLE sessions (
  session_id INT PRIMARY KEY, 
  user_id INT, 
  session_date DATETIME
);

INSERT INTO sessions (session_id, user_id, session_date) VALUES 
(1, 1, '2024-01-01 00:00:00'),
(2, 2, '2024-01-02 00:00:00'),
(3, 3, '2024-01-05 00:00:00'),
(4, 3, '2024-01-05 00:00:00'),
(5, 4, '2024-01-03 00:00:00'),
(6, 4, '2024-01-03 00:00:00'),
(7, 5, '2024-01-04 00:00:00'),
(8, 5, '2024-01-04 00:00:00'),
(9, 3, '2024-01-05 00:00:00'),
(10, 5, '2024-01-04 00:00:00');

CREATE TABLE order_summary (
  order_id INT PRIMARY KEY, 
  user_id INT, 
  order_value INT, 
  order_date DATETIME
);

INSERT INTO order_summary (order_id, user_id, order_value, order_date) VALUES 
(1, 1, 152, '2024-01-01 00:00:00'),
(2, 2, 485, '2024-01-02 00:00:00'),
(3, 3, 398, '2024-01-05 00:00:00'),
(4, 3, 320, '2024-01-05 00:00:00'),
(5, 4, 156, '2024-01-03 00:00:00'),
(6, 4, 121, '2024-01-03 00:00:00'),
(7, 5, 238, '2024-01-04 00:00:00'),
(8, 5, 70, '2024-01-04 00:00:00'),
(9, 3, 152, '2024-01-05 00:00:00'),
(10, 5, 171, '2024-01-04 00:00:00');
```

---

### Explanation to Solve Query:

1. **JOIN:**  
   We join the `sessions` table with the `order_summary` table on `user_id`. We ensure that the session date and order date are the same day by using the `CONVERT(DATE, session_date)` and `CONVERT(DATE, order_date)` functions. This removes the time component and matches only the date.

2. **COUNT and SUM:**  
   Use `COUNT(order_id)` to find the number of orders placed by each user on the same session day. Use `SUM(order_value)` to calculate the total value of those orders.

3. **GROUP BY:**  
   Group the result by `user_id` and `session_date` to aggregate the orders for each user on each day.

4. **HAVING Clause:**  
   The `HAVING COUNT(order_id) > 0` ensures that only users who have placed at least one order are included in the result.

---

### Solution Query:

```sql
SELECT 
    s.user_id, 
    CONVERT(DATE, s.session_date) AS session_date, 
    COUNT(o.order_id) AS total_orders, 
    SUM(o.order_value) AS total_order_value
FROM 
    sessions s
JOIN 
    order_summary o ON s.user_id = o.user_id 
    AND CONVERT(DATE, s.session_date) = CONVERT(DATE, o.order_date)
GROUP BY 
    s.user_id, CONVERT(DATE, s.session_date)
HAVING 
    COUNT(o.order_id) > 0;
```

---

🔥 **Try it out!**  
I’m posting one SQL interview question every day!  
▪️ **Check out previous questions here**:   

Join me for updates on SQL, Python, and much more!  
 

❣️ **Enjoy solving?** Solve, share, and spread the knowledge!  
🤝 Stay consistent with your learning and growth, stay active!
