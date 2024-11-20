# 💡Can You Solve This? EASY #SQL Practice Question

**❓𝗤𝘂𝗲𝘀𝘁𝗶𝗼𝗻**  
"Find the total sales for each region where the average discount given in that region is more than 10%. Include the columns Region, Total Sales, and Average Discount. Sort the output by Total Sales in descending order."

**📖 𝗪𝗵𝗮𝘁 𝘆𝗼𝘂 𝘄𝗶𝗹𝗹 𝗹𝗲𝗮𝗿𝗻**  
🔍 By solving this, you'll learn how to use `GROUP BY`, `HAVING`, and aggregate functions like `SUM` and `AVG`. It’s a great exercise to understand filtering grouped data with conditions! Give it a try and share your results! 👇  

**👇 𝗦𝗰𝗵𝗲𝗺𝗮 𝗮𝗻𝗱 𝗗𝗮𝘁𝗮𝘀𝗲𝘁**  
```sql
-- Schema
CREATE TABLE sales_data (
    region VARCHAR(50),
    sales DECIMAL(10, 2),
    discount DECIMAL(5, 2)
);

-- Dataset
INSERT INTO sales_data (region, sales, discount) VALUES
('North', 5000, 12),
('South', 3000, 8),
('North', 7000, 15),
('East', 4000, 9),
('West', 6000, 11),
('South', 5000, 13),
('East', 8000, 10),
('West', 9000, 14),
('North', 3000, 5),
('South', 10000, 12);
```

**🔑 𝗘𝘅𝗽𝗹𝗮𝗻𝗮𝘁𝗶𝗼𝗻 𝘁𝗼 𝘀𝗼𝗹𝘃𝗲 𝘁𝗵𝗶𝘀 𝗤𝘂𝗲𝘀𝘁𝗶𝗼𝗻**  
1. **Use the `GROUP BY` clause:** Group the records by the `region` column to calculate aggregated values per region.  
2. **Calculate Total Sales and Average Discount:** Use `SUM(sales)` for total sales and `AVG(discount)` for the average discount.  
3. **Filter with `HAVING`:** Ensure only regions where the average discount is greater than 10% appear in the results.  
4. **Sort by Total Sales:** Order the output in descending order of total sales using `ORDER BY SUM(sales) DESC`.  

🔥 𝗜'𝗺 𝗽𝗼𝘀𝘁𝗶𝗻𝗴 𝟭 𝗦𝗤𝗟 𝗾𝘂𝗲𝘀𝘁𝗶𝗼𝗻 𝗲𝘃𝗲𝗿𝘆 𝗱𝗮𝘆!  

- LinkedIn Post Link (This Post)- 


🚀 Can you solve this SQL challenge? Share your approach in the comments! 

Follow Sajid Shaikh for more such SQL questions

#SQLChallenge #DataAnalytics #InterviewPreparation #JOB

```sql
SELECT
  region,
  SUM(sales) AS total_sales,
  avg(discount) AS avg_discount
FROM
  sales_data
GROUP BY
  region
HAVING
  avg(discount) > 10
ORDER BY
  total_sales DESC;
```
