# Can you solve this? Spotify - Medium-Level Query Challenge

### Question:
Find the top 3 users who have listened to the most distinct genres, considering only the genres they've listened to at least twice.

### Schema & Dataset:

```sql
CREATE TABLE users (
  user_id INT PRIMARY KEY,
  user_name VARCHAR(100)
);

CREATE TABLE songs (
  song_id INT PRIMARY KEY,
  song_name VARCHAR(100),
  genre VARCHAR(50)
);

CREATE TABLE listens (
  user_id INT,
  song_id INT,
  listen_count INT,
  PRIMARY KEY (user_id, song_id),
  FOREIGN KEY (user_id) REFERENCES users(user_id),
  FOREIGN KEY (song_id) REFERENCES songs(song_id)
);

-- Sample Data
INSERT INTO users (user_id, user_name) VALUES (1, 'Alice'), (2, 'Bob'), (3, 'Charlie'), (4, 'David');
INSERT INTO songs (song_id, song_name, genre) VALUES 
(1, 'Song A', 'Rock'), 
(2, 'Song B', 'Pop'), 
(3, 'Song C', 'Jazz'), 
(4, 'Song D', 'Rock'), 
(5, 'Song E', 'Pop'), 
(6, 'Song F', 'Jazz');

INSERT INTO listens (user_id, song_id, listen_count) VALUES 
(1, 1, 2), (1, 2, 3), (1, 3, 1), 
(2, 1, 3), (2, 2, 1), (2, 4, 4),
(3, 3, 5), (3, 4, 1), (3, 5, 2), 
(4, 2, 2), (4, 5, 3), (4, 6, 2);
```

### What will you learn?
You will learn how to work with **aggregations, filtering, and subqueries** to analyze data in SQL. 🎯

### Explanation:
1️⃣ **Filtering**: Only consider genres listened to more than once by users.  
2️⃣ **Group and Aggregate**: Use `GROUP BY` to group by `user_id` and `genre`.  
3️⃣ **Counting distinct genres**: Count the distinct genres each user has listened to at least twice.  
4️⃣ **Ranking**: Sort users by the number of distinct genres they listened to and select the top 3.

### Solution:
```SQL
WITH genre_listens AS (
  SELECT 
    l.user_id, 
    s.genre,
    COUNT(*) AS genre_listen_count
  FROM listens l
  JOIN songs s ON l.song_id = s.song_id
  GROUP BY l.user_id, s.genre
  HAVING COUNT(*) >= 2
),
distinct_genres AS (
  SELECT 
    user_id, 
    COUNT(DISTINCT genre) AS distinct_genre_count
  FROM genre_listens
  GROUP BY user_id
),
ranked_users AS (
  SELECT 
    user_id, 
    distinct_genre_count,
    RANK() OVER (ORDER BY distinct_genre_count DESC) AS rank
  FROM distinct_genres
)
SELECT 
  user_id, 
  distinct_genre_count
FROM ranked_users
WHERE rank <= 3;
```

### Explanation of the Solution:
1️⃣ Step 1: In genre_listens, we calculate how many times each user listened to each genre and filter out the genres listened to fewer than twice.  
2️⃣ Step 2: In distinct_genres, count the number of distinct genres for each user from the filtered data.  
3️⃣ Step 3: In ranked_users, rank the users based on their distinct genre count using the RANK() window function.  
4️⃣ Step 4: Retrieve the top 3 users based on their rank. If multiple users tie, all are included due to RANK().  
