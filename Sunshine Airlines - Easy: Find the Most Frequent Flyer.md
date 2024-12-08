### Sunshine Airlines - Easy: Find the Most Frequent Flyer ✈️

**Question**  
Identify the customer who has taken the most flights with Sunshine Airlines.

**Schema and Dataset**  
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100)
);

CREATE TABLE flights (
    flight_id INT PRIMARY KEY,
    flight_date DATE,
    origin VARCHAR(50),
    destination VARCHAR(50)
);

CREATE TABLE bookings (
    booking_id INT PRIMARY KEY,
    customer_id INT,
    flight_id INT,
    booking_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id),
    FOREIGN KEY (flight_id) REFERENCES flights(flight_id)
);

INSERT INTO customers VALUES
(1, 'Alice', 'Smith', 'alice@example.com'),
(2, 'Bob', 'Johnson', 'bob@example.com'),
(3, 'Carol', 'Williams', 'carol@example.com');

INSERT INTO flights VALUES
(101, '2023-10-01', 'NYC', 'LAX'),
(102, '2023-10-02', 'LAX', 'SF'),
(103, '2023-10-03', 'NYC', 'CHI');

INSERT INTO bookings VALUES
(201, 1, 101, '2023-09-25'),
(202, 2, 102, '2023-09-26'),
(203, 1, 103, '2023-09-27'),
(204, 1, 102, '2023-09-28'),
(205, 3, 103, '2023-09-29');
```

**Learning Objective**  
Students will learn how to **aggregate data** and use SQL functions like `COUNT` and `GROUP BY` to find patterns and insights in a dataset. 📊

**Question Breakdown**  
1️⃣ **Understand the Relationships:**  
   - `customers` table links to `bookings` table through `customer_id`.  
   - `flights` table links to `bookings` table through `flight_id`.  

2️⃣ **Query Aggregation:**  
   - Use `COUNT` to count flights per customer in `bookings`.  
   - Use `GROUP BY` to group data by `customer_id`.  

3️⃣ **Find the Maximum:**  
   - Use `ORDER BY` and `LIMIT` to find the top flyer.  

**Expected Query**

```sql
SELECT c.customer_id, c.first_name, c.last_name, COUNT(b.booking_id) AS total_flights
FROM customers c
JOIN bookings b ON c.customer_id = b.customer_id
GROUP BY c.customer_id
ORDER BY total_flights DESC
LIMIT 1;
```

**Output Explanation**  
The query returns the details of the customer with the highest number of bookings.
