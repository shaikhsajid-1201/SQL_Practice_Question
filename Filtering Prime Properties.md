# Can You Solve This? Airbnb - Medium: Filtering Prime Properties

### Question  
Airbnb wants to analyze prime properties in each city based on user ratings and calculate average property ratings for prime properties only. Prime properties are those that are the **top-rated listings in their respective cities**.  

Write a query to display the city, property name, and average rating for prime properties in descending order of average rating.  
- Consider a property to be "prime" if it has the **highest rating** in its city.  
- If multiple properties have the same rating in a city, include all of them.  

### Schema & Dataset  
#### `properties` table  
```sql
CREATE TABLE properties (
    property_id INT PRIMARY KEY,
    property_name VARCHAR(255),
    city VARCHAR(255),
    user_rating DECIMAL(3, 2)
);

INSERT INTO properties (property_id, property_name, city, user_rating)
VALUES
(1, 'Cozy Cottage', 'New York', 4.5),
(2, 'Luxury Loft', 'New York', 4.8),
(3, 'Modern Studio', 'New York', 4.8),
(4, 'Seaside Villa', 'Los Angeles', 4.9),
(5, 'Urban Flat', 'Los Angeles', 4.7),
(6, 'Mountain Retreat', 'Denver', 4.6),
(7, 'Downtown Penthouse', 'Denver', 4.6),
(8, 'Countryside BnB', 'Denver', 4.3);
```

### What Will I Learn?  
- 🏙️ **Identifying grouped maximum values** using window functions or subqueries.  
- 🗃️ **Filtering and ranking rows within partitions** to solve complex business logic.  

### Explanation  
1️⃣ **Group by city** to identify the highest-rated properties within each city.  
2️⃣ **Compare individual property ratings** to the highest rating within their respective city.  
3️⃣ Include all properties tied for the highest rating in a city.  
4️⃣ Calculate and sort the **average rating** of the selected properties.  

### Solution
```sql
WITH prime_properties AS (
    SELECT 
        city,
        property_name,
        user_rating,
        MAX(user_rating) OVER (PARTITION BY city) AS max_rating
    FROM 
        properties
)
SELECT 
    city,
    property_name,
    user_rating
FROM 
    prime_properties
WHERE 
    user_rating = max_rating
ORDER BY 
    user_rating DESC;
```

### Explanation of the Solution
1. **prime_properties (CTE):**    
This Common Table Expression (CTE) computes the maximum user_rating for each city using the MAX() window function.
Each row retains its original columns (city, property_name, user_rating) and includes a new column max_rating for the city.

2. **Filtering the Prime Properties:**  
The WHERE clause ensures that only rows where user_rating matches the max_rating are selected, thus identifying prime properties.

3. **Ordering the Results:**  
The final SELECT orders prime properties by their user_rating in descending order to highlight the top-rated properties.
