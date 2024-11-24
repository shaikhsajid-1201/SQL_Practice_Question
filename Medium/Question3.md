### Spotify Medium SQL Practice Question  

**Question:**  
"Find the top 3 most listened-to songs for each genre based on the total number of plays. If there are ties in the number of plays, include all tied songs. The output should contain the genre, song title, and total number of plays, sorted alphabetically by genre and then by plays in descending order."

#### Schema and Dataset  

```sql
CREATE TABLE Songs (
    song_id INT PRIMARY KEY,
    title VARCHAR(100),
    genre VARCHAR(50)
);

CREATE TABLE Plays (
    play_id INT PRIMARY KEY,
    song_id INT,
    user_id INT,
    play_count INT,
    FOREIGN KEY (song_id) REFERENCES Songs(song_id)
);

INSERT INTO Songs (song_id, title, genre) VALUES
(1, 'Shape of You', 'Pop'),
(2, 'Blinding Lights', 'Pop'),
(3, 'Bohemian Rhapsody', 'Rock'),
(4, 'Hotel California', 'Rock'),
(5, 'Stairway to Heaven', 'Rock'),
(6, 'Despacito', 'Latin'),
(7, 'Bailando', 'Latin');

INSERT INTO Plays (play_id, song_id, user_id, play_count) VALUES
(1, 1, 101, 150),
(2, 1, 102, 200),
(3, 2, 103, 300),
(4, 3, 104, 400),
(5, 3, 105, 100),
(6, 4, 106, 250),
(7, 5, 107, 200),
(8, 6, 108, 500),
(9, 7, 109, 450);
```

#### Explanation to Solve This Question  

1. **Join Tables:** Combine `Songs` and `Plays` tables to calculate the total number of plays for each song.  
2. **Group and Aggregate:** Group by `song_id` and sum the `play_count` values to find the total number of plays for each song.  
3. **Rank Within Genres:** Use a window function (`RANK()` or `DENSE_RANK()`) to rank songs within each genre based on their total number of plays in descending order.  
4. **Filter Top 3:** Select only the top 3 ranked songs for each genre, including ties.  
5. **Sort:** Ensure the output is sorted alphabetically by genre and then by total plays in descending order.

**SOLUTION**
```SQL
WITH songplays AS (
    -- Aggregate play counts per song before ranking
    SELECT 
        s.title, 
        s.genre, 
        SUM(p.play_count) AS total_plays,
        RANK() OVER (PARTITION BY s.genre ORDER BY SUM(p.play_count) DESC) AS rnk
    FROM Songs s
    JOIN Plays p 
        ON s.song_id = p.song_id
    GROUP BY s.song_id, s.title, s.genre
)
SELECT genre, title, total_plays
FROM songplays
WHERE rnk <= 3
ORDER BY genre ASC, total_plays DESC;
```

#### Call to Action  

Attempt this question in your MySQL environment. Explore window functions and aggregation techniques to derive the solution!  

#sql #interviewprep #mysql
