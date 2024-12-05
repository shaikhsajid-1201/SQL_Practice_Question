### Can you solve this? Uber - Hard 🚗 Analyzing Driver Efficiency

#### Question  
Uber wants to evaluate the efficiency of its drivers by analyzing their daily driving behavior. Write a query to find the **most efficient driver** for each city on each day based on the total earnings per mile. If there’s a tie in earnings per mile between multiple drivers, return the driver with the **alphabetically smallest name**.  

#### Schema & Dataset  
```sql
CREATE TABLE Driver_Trips (
    driver_id INT,          -- Driver's unique ID
    driver_name VARCHAR(50),-- Driver's name
    trip_date DATE,         -- Date of the trip
    city VARCHAR(50),       -- City of the trip
    distance FLOAT,         -- Distance covered in miles
    earnings FLOAT          -- Total earnings from the trip
);

INSERT INTO Driver_Trips (driver_id, driver_name, trip_date, city, distance, earnings)
VALUES
(1, 'Alice', '2024-12-01', 'New York', 30, 150),
(2, 'Bob', '2024-12-01', 'New York', 20, 80),
(3, 'Charlie', '2024-12-01', 'New York', 40, 160),
(4, 'Dave', '2024-12-01', 'Chicago', 50, 200),
(5, 'Eve', '2024-12-01', 'Chicago', 25, 125),
(6, 'Frank', '2024-12-02', 'New York', 10, 70),
(7, 'Alice', '2024-12-02', 'New York', 25, 90),
(8, 'Bob', '2024-12-02', 'New York', 15, 90),
(9, 'Charlie', '2024-12-02', 'New York', 20, 100),
(10, 'Eve', '2024-12-02', 'Chicago', 35, 175);
```

#### What I will learn?  
- Master aggregating data and resolving ties using **window functions** and **string comparisons**. ✨  
- Deepen understanding of **GROUP BY**, **RANK**, and handling multiple conditions. 🎯  

#### Explanation  
1️⃣ Use **total earnings per mile** as the metric for efficiency.  
2️⃣ Group data by `city` and `trip_date`, then calculate total earnings and miles for each driver.  
3️⃣ Apply a **window function** (`RANK()`) to rank drivers within each city and day based on efficiency.  
4️⃣ Handle ties by sorting drivers alphabetically using `driver_name`.  
5️⃣ Filter to return only the top-ranked driver for each city and day.

#### Solution 
```sql
WITH DriverEfficiency AS (
    SELECT 
        city,
        trip_date,
        driver_id,
        driver_name,
        SUM(earnings) / SUM(distance) AS earnings_per_mile
    FROM 
        Driver_Trips
    GROUP BY 
        city, trip_date, driver_id, driver_name
),
RankedDrivers AS (
    SELECT 
        city,
        trip_date,
        driver_id,
        driver_name,
        earnings_per_mile,
        RANK() OVER (
            PARTITION BY city, trip_date 
            ORDER BY earnings_per_mile DESC, driver_name ASC
        ) AS rank
    FROM 
        DriverEfficiency
)
SELECT 
    city,
    trip_date,
    driver_id,
    driver_name,
    earnings_per_mile
FROM 
    RankedDrivers
WHERE 
    rank = 1
ORDER BY 
    city, trip_date;
```

#### Output  
Based on the provided dataset, the query will return:  

| City      | Trip_Date   | Driver_ID | Driver_Name | Earnings_Per_Mile |
|-----------|-------------|-----------|-------------|--------------------|
| Chicago   | 2024-12-01  | 4         | Dave        | 4.00              |
| Chicago   | 2024-12-02  | 10        | Eve         | 5.00              |
| New York  | 2024-12-01  | 3         | Charlie     | 4.00              |
| New York  | 2024-12-02  | 9         | Charlie     | 5.00              |

### Explanation of Steps  

1️⃣ **Calculate Efficiency**  
- In the `DriverEfficiency` CTE, compute `earnings_per_mile` for each driver grouped by `city` and `trip_date`.  

2️⃣ **Rank Drivers by Efficiency**  
- In the `RankedDrivers` CTE, rank drivers using `RANK()`. Sort first by `earnings_per_mile` (descending) and then alphabetically by `driver_name` for tie-breaking.  

3️⃣ **Filter Top Drivers**  
- Use `WHERE rank = 1` to filter for the most efficient driver per city and date.  

4️⃣ **Display Results**  
- Sort the final results by `city` and `trip_date` for clarity.  
