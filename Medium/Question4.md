### Netflix Medium SQL Practice Question

**Question:**  
Identify the most popular movie genre(s) in each country by calculating the total number of movies for each genre. If multiple genres have the same count for a country, include all of them. The output should contain three columns: `Country`, `Genre`, and `Total Movies`, sorted by country alphabetically and then by genre count in descending order.

**Schema and Dataset:**  
```sql
CREATE TABLE netflix_data (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    type VARCHAR(50),
    country VARCHAR(100),
    release_year INT,
    listed_in VARCHAR(255) -- Stores genres as a comma-separated string
);

INSERT INTO netflix_data (title, type, country, release_year, listed_in)
VALUES 
('Movie A', 'Movie', 'USA', 2020, 'Action, Drama'),
('Movie B', 'Movie', 'USA', 2020, 'Drama'),
('Movie C', 'Movie', 'UK', 2018, 'Comedy, Drama'),
('Movie D', 'Movie', 'India', 2021, 'Action'),
('Movie E', 'Movie', 'India', 2021, 'Action, Thriller'),
('Movie F', 'Movie', 'UK', 2019, 'Comedy'),
('Movie G', 'Movie', 'USA', 2020, 'Thriller'),
('Movie H', 'Movie', 'India', 2020, 'Drama'),
('Movie I', 'Movie', 'USA', 2021, 'Comedy'),
('Movie J', 'Movie', 'UK', 2019, 'Drama');
```

**Explanation to solve this question:**  
1. Split the `listed_in` column (genres) into separate rows (one genre per row).  
2. Count the number of movies for each genre in each country.  
3. Use a window function (`RANK()` or `DENSE_RANK()`) to identify the top genre(s) for each country.  
4. Filter results to include only the top-ranked genres for each country.  
5. Sort the output as specified in the question.  

**CALL TO ACTION:**  
Test and solve this SQL question using a MySQL database. Let us know your solution in the comments below!  

**SOLUTION:**
```sql
WITH genre_split AS (
    SELECT 
        country,
        TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(listed_in, ',', n.n), ',', -1)) AS genre
    FROM netflix_data
    JOIN (
        SELECT 1 AS n UNION ALL
        SELECT 2 UNION ALL
        SELECT 3 UNION ALL
        SELECT 4 UNION ALL
        SELECT 5
    ) n ON CHAR_LENGTH(listed_in) - CHAR_LENGTH(REPLACE(listed_in, ',', '')) >= n.n - 1
),
genre_counts AS (
    SELECT 
        country,
        genre,
        COUNT(*) AS total_movies
    FROM genre_split
    GROUP BY country, genre
),
ranked_genres AS (
    SELECT 
        country,
        genre,
        total_movies,
        RANK() OVER (PARTITION BY country ORDER BY total_movies DESC) AS rnk 
    FROM genre_counts
)
SELECT 
    country,
    genre,
    total_movies
FROM ranked_genres
WHERE rnk = 1
ORDER BY country, total_movies DESC;
```

#sqlpractice #mediumlevelsql #dataanalysis #netflixsql
