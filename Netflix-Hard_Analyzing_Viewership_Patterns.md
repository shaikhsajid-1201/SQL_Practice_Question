# Can You Solve This? Netflix - Hard: Analyzing Viewership Patterns  

### Question  
Netflix wants to identify the **most binge-watched shows** in each genre. A show is considered "most binge-watched" if it has the **highest total hours watched** within its genre.  

Write a query to display the `genre`, `title`, and `total_hours_watched` for the most binge-watched shows across all genres, sorted in descending order of `total_hours_watched`.  
- If multiple shows have the same `total_hours_watched` in a genre, include all of them.  

### Schema & Dataset  
#### `shows` table  
```sql
CREATE TABLE shows (
    show_id INT PRIMARY KEY,
    title VARCHAR(255),
    genre VARCHAR(100),
    total_hours_watched BIGINT
);

INSERT INTO shows (show_id, title, genre, total_hours_watched)
VALUES
(1, 'Stranger Things', 'Sci-Fi', 500000),
(2, 'Black Mirror', 'Sci-Fi', 500000),
(3, 'The Crown', 'Drama', 300000),
(4, 'Breaking Bad', 'Drama', 450000),
(5, 'Bridgerton', 'Drama', 450000),
(6, 'Squid Game', 'Thriller', 800000),
(7, 'Money Heist', 'Thriller', 700000),
(8, 'The Witcher', 'Fantasy', 350000),
(9, 'Game of Thrones', 'Fantasy', 350000),
(10, 'Friends', 'Comedy', 600000);
```

### What Will I Learn?  
- 📺 **Handling grouped maximum values** across categories efficiently.  
- 📊 **Dealing with ties** in ranking scenarios to ensure fairness.  

### Explanation  
1️⃣ **Group shows by genre** to find the maximum `total_hours_watched` for each genre.  
2️⃣ **Filter the shows** to include only those with the maximum `total_hours_watched` in their genre.  
3️⃣ **Order results** by `total_hours_watched` in descending order to highlight the most binge-watched shows.  

### Solution
```SQL
WITH RankedShows AS (
    SELECT 
        genre,
        title,
        total_hours_watched,
        MAX(total_hours_watched) OVER (PARTITION BY genre) AS max_hours_watched
    FROM 
        shows
)
SELECT 
    genre,
    title,
    total_hours_watched
FROM 
    RankedShows
WHERE 
    total_hours_watched = max_hours_watched
ORDER BY 
    total_hours_watched DESC;
```

### Explanation of the Solution
1️⃣ RankedShows CTE:
Uses MAX() as a window function with PARTITION BY genre to find the maximum total_hours_watched for each genre.
Adds a new column max_hours_watched to indicate the highest value in each genre.

2️⃣ Filtering:
Filters rows where total_hours_watched = max_hours_watched to include only the most binge-watched shows in each genre.
This ensures tied shows are included (e.g., both Stranger Things and Black Mirror in Sci-Fi).

3️⃣ Ordering:
The result is ordered by total_hours_watched DESC to display the most binge-watched shows at the top, regardless of genre.
