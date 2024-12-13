# Netflix - Hard: Finding the Most Popular Genre Over Time

#### Question
Write a query to determine the most popular genre for each year based on the number of titles added in that genre. If multiple genres have the same count for a year, list them all.

#### Schema and Dataset
```sql
CREATE TABLE netflix (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255),
    genre VARCHAR(100),
    release_year INT,
    date_added DATE,
    country VARCHAR(100),
    director VARCHAR(255),
    rating VARCHAR(10),
    duration VARCHAR(50)
);

INSERT INTO netflix (title, genre, release_year, date_added, country, director, rating, duration) VALUES
('Stranger Things', 'Drama', 2016, '2016-07-15', 'USA', 'Duffer Brothers', 'TV-14', '3 Seasons'),
('Money Heist', 'Crime', 2017, '2017-05-02', 'Spain', 'Álex Pina', 'TV-MA', '5 Seasons'),
('Breaking Bad', 'Drama', 2008, '2014-01-20', 'USA', 'Vince Gilligan', 'TV-MA', '5 Seasons'),
('Friends', 'Comedy', 1994, '2015-01-01', 'USA', 'Various', 'TV-PG', '10 Seasons'),
('Narcos', 'Crime', 2015, '2015-08-28', 'USA', 'Chris Brancato', 'TV-MA', '3 Seasons'),
('The Crown', 'Historical', 2016, '2016-11-04', 'UK', 'Peter Morgan', 'TV-MA', '5 Seasons'),
('Lupin', 'Crime', 2021, '2021-01-08', 'France', 'George Kay', 'TV-MA', '2 Seasons'),
('Dark', 'Sci-Fi', 2017, '2017-12-01', 'Germany', 'Baran bo Odar', 'TV-MA', '3 Seasons');
```

#### Learning Objective:
📊 Master advanced GROUP BY and aggregate functions: Gain expertise in complex queries involving grouping, ranking, and ties in data.

#### Explanation:
1️⃣ Focus on the genre column to find the count of titles added per year for each genre.  
2️⃣ Use GROUP BY year and genre to get counts for each combination.  
3️⃣ Use ranking techniques (e.g., ROW_NUMBER or RANK) to find the top genre(s) for each year.  
4️⃣ Handle ties by including genres with the same count as the highest.  
5️⃣ Sort the result to make it user-friendly.  

#### Solution:
```SQL
```
