# Task 3: SQL for Data Analysis

This project demonstrates SQL-based data analysis using a shopping dataset. I have used MySQL to run various queries including filters, aggregations, joins, subqueries, views, and indexing for performance.


# Dataset Used
- Name: shopping_trends_updated.csv
- Contains: Customer demographics, item purchases, payment details, subscription info, shipping methods, etc.
- Imported as table: shopping_trends_updated


# Objective
Apply key SQL concepts including:
- SELECT, WHERE, ORDER BY, GROUP BY
- JOINS (LEFT JOIN shown)
- Subqueries
- Aggregate functions (SUM, AVG)
- Views
- Indexes

# 1. SELECT, WHERE, ORDER BY, GROUP BY
```sql
SELECT Gender, AVG(`Purchase Amount (USD)`) AS AvgSpent
FROM shopping_trends_updated
WHERE Season = 'Spring'
GROUP BY Gender
ORDER BY AvgSpent DESC;
```
Explanation: This query calculates the average amount spent during the spring season, grouped by gender. It demonstrates basic filtering (`WHERE`), aggregation (`AVG`), grouping, and ordering.


# 2. JOINS (LEFT JOIN Example)
```sql
CREATE TABLE shipping_cost (
    `Shipping Type` TEXT,
    `Cost` INT
);

INSERT INTO shipping_cost VALUES
('Express', 10),
('Free Shipping', 0),
('Next Day Air', 20);

SELECT s.`Customer ID`, s.`Shipping Type`, c.`Cost`
FROM shopping_trends_updated s
LEFT JOIN shipping_cost c ON s.`Shipping Type` = c.`Shipping Type`;
```
Explanation: This LEFT JOIN brings in the shipping cost from another table based on the shipping type. All customer records are preserved even if the cost is missing.



# 3. Subqueries
```sql
SELECT *
FROM shopping_trends_updated
WHERE `Purchase Amount (USD)` > (
    SELECT AVG(`Purchase Amount (USD)`)
    FROM shopping_trends_updated
);
```
Explanation: Filters and displays customers who spent more than the average purchase amount. Demonstrates use of subqueries.


# 4. Aggregate Functions
```sql
SELECT Location, COUNT(*) AS TotalOrders,
       AVG(`Purchase Amount (USD)`) AS AvgSpend
FROM shopping_trends_updated
GROUP BY Location
ORDER BY AvgSpend DESC;
```
Explanation: Groups data by location to count total orders and calculate average spend. Useful for regional analysis.

# 5. Views
```sql
CREATE VIEW CategorySalesSummary AS
SELECT Category, COUNT(*) AS ItemsSold,
       SUM(`Purchase Amount (USD)`) AS TotalSales
FROM shopping_trends_updated
GROUP BY Category;

SELECT * FROM CategorySalesSummary LIMIT 10;
```
Explanation: The view summarizes item sales and revenue by category, allowing reuse of this result set without re-running the entire query.

# 6. Indexes for Optimization
```sql
ALTER TABLE shopping_trends_updated
MODIFY `Gender` VARCHAR(10),
MODIFY `Location` VARCHAR(50);

CREATE INDEX idx_gender ON shopping_trends_updated(`Gender`);
CREATE INDEX idx_location ON shopping_trends_updated(`Location`);
```
Explanation: Since `Gender` and `Location` were `TEXT`, they were converted to `VARCHAR` to allow indexing. Indexes help speed up queries using these fields, especially with WHERE and GROUP BY.

# Confirming Index Usage
```sql
SHOW INDEXES FROM shopping_trends_updated;
```
Use this to confirm index creation.

You can also verify usage with:
```sql
EXPLAIN SELECT * FROM shopping_trends_updated WHERE Gender = 'Female';
```

## Submission
This task was submitted along with:
- SQL scripts
- Pdf of the query outputs
- This README file
Author- Iqraa

