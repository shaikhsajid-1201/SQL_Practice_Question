**Google HARD SQL Practice Question**  

"Write a query to find the most frequently searched keywords by users over the last 30 days. Include only keywords that have been searched more than **2 times**. The output should have two columns: `keyword` and `search_count`, sorted by `search_count` in descending order. If two keywords have the same count, sort them alphabetically by `keyword`. Ensure the query is optimized for performance."  

**Schema and Dataset**  

```sql
CREATE TABLE search_logs (
  search_id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  keyword VARCHAR(255) NOT NULL,
  search_date DATE NOT NULL
);

INSERT INTO search_logs (user_id, keyword, search_date)
VALUES
(1, 'SQL tutorial', '2024-10-01'),
(2, 'Google analytics', '2024-10-01'),
(3, 'SQL tutorial', '2024-10-02'),
(4, 'data analysis', '2024-10-03'),
(5, 'SQL tutorial', '2024-10-05'),
(6, 'Google analytics', '2024-10-06'),
(7, 'SQL tutorial', '2024-10-10'),
(8, 'data analysis', '2024-10-11'),
(9, 'data visualization', '2024-10-15'),
(10, 'data analysis', '2024-10-20');
```

**Explanation to solve this question:**  
1. Filter rows where the `search_date` falls within the last 30 days using the `DATE_SUB` function.  
2. Group by the `keyword` column to count the number of occurrences.  
3. Use a `HAVING` clause to include only those keywords with a `search_count` greater than 2.  
4. Sort the results by `search_count` in descending order and then by `keyword` alphabetically.  

Write the query, test it on the dataset, and analyze the performance. Discuss your results and optimization techniques.  
#sqlpractice #sqlquery #google #hardlevelsql #dataanalysis

**Solution:**
```SQL
SELECT 
    keyword, 
    COUNT(*) AS search_count
FROM 
    search_logs
WHERE 
    search_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY) -- Filter for the last 30 days
GROUP BY 
    keyword
HAVING 
    COUNT(*) > 2 -- Include only keywords searched more than 2 times
ORDER BY 
    search_count DESC, 
    keyword ASC -- Sort by count descending and keyword alphabetically
LIMIT 5; -- Limit to top 5 results
```
