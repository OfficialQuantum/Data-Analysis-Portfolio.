# Amazon Bestseller Project

<p align='center'>
  <img src='https://github.com/OfficialQuantum/assests/blob/main/assests/Amazon Bestseller data exploration.png', alt='Welcome'>
</p>

**Goal:** To extract actionable business insights from Amazon's bestseller data, identifying key factors driving sales success and informing strategic decision-making in product development and marketing.

**Code:** [Amazon Bestseller Project](https://github.com/OfficialQuantum/Portfolio-Projects/blob/main/Amazon%20Bestseller%20Project.sql)

**Description:** The dataset comprises Amazon's top-selling books, encompassing genres, pricing data, customer reviews, author details, and sales performance metrics. Through rigorous analysis of genre preferences, pricing strategies, customer sentiment from reviews, and regional sales trends, this study aims to uncover patterns that businesses can leverage to optimize their product offerings, enhance customer engagement, and capitalize on global market opportunities.

**Skills:** Data Transformation and Cleaning, Time-series analysis, Data definition, Data Querying, Data Filtering, Data Aggregation and Summarization.

**Technology:** SQL Query

---

## Query

```ruby
SELECT 
    *
FROM
    bestsellers;

-- DATA CLEANING
SET SQL_SAFE_UPDATES = 0;

-- Extracting the year from the datetime format and update the Year column
ALTER TABLE bestsellers ADD COLUMN Cleaned_Year INT;

UPDATE bestsellers 
SET 
    Cleaned_Year = CASE
        WHEN LENGTH(Year) = 4 THEN Year
        ELSE YEAR(STR_TO_DATE(Year, '%Y-%m-%d %H:%i:%s'))
    END;
    
ALTER TABLE bestsellers drop column Year;
ALTER TABLE bestsellers change column Cleaned_Year Year int;

SELECT DISTINCT
    Year
FROM
    bestsellers;
-- -------------------------------------------------
    
--  Removing Duplicates
WITH CTE_1 AS (
    SELECT 
        Name,
        Author,
        'User Rating',
        ROW_NUMBER() OVER (PARTITION BY Name ORDER BY Name DESC) AS row_num
    FROM bestsellers
)

DELETE FROM bestsellers
WHERE Name IN (
    SELECT Name
    FROM CTE_1
    WHERE row_num > 1
);

SELECT 
    *
FROM
    bestsellers;
-- -------------------------------------------------

-- Handling missing values
SELECT 
    *
FROM
    bestsellers
WHERE
    Name IS NULL OR Author IS NULL
        OR 'User Rating' IS NULL
        OR Reviews IS NULL
        OR Price IS NULL
        OR Year IS NULL
        OR Genre IS NULL;
-- -------------------------------------------------

-- Handling Whitespaces
UPDATE bestsellers 
SET 
    Name = TRIM(Name),
    Author = TRIM(Author),
    Genre = TRIM(Genre);
-- ------------------------------------------------- 
 
-- Ensuring Data Type Consistency
ALTER TABLE bestsellers
MODIFY COLUMN `User Rating` DECIMAL(2,1),
MODIFY COLUMN Reviews INT,
MODIFY COLUMN Price INT,
MODIFY COLUMN Year INT;
-- -------------------------------------------------

-- Verifying Data Integrity
DELETE FROM bestsellers 
WHERE
    Year < 1900 OR Year > 2024;
-- -------------------------------------------------

-- DERIVING INSIGHTS

SELECT 
    Name, Author, `User Rating`
FROM
    bestsellers
WHERE
    `User Rating` >= 4.9;

-- Identifying Genres of the Highest Rated Books (>= 4.9)
SELECT 
    Genre, COUNT(`User Rating`) AS Rating_Count
FROM
    bestsellers
WHERE
    `User Rating` >= 4.9
GROUP BY Genre;
-- -------------------------------------------------

-- Identifying the Release Year of the Highest Rated Books (>= 4.9)
SELECT 
    Name, Year
FROM
    bestsellers
WHERE
    `User Rating` >= 4.9
ORDER BY 2 DESC;
-- -------------------------------------------------

-- Identifying Year of the Highest Rated Books (>= 4.5)
SELECT 
    Year, COUNT(`User Rating`) AS Rating_Count
FROM
    bestsellers
WHERE
    `User Rating` >= 4.5
GROUP BY Year
ORDER BY Rating_Count DESC;
-- -------------------------------------------------

-- Most Reviewd Books
SELECT 
    Name, Author, Reviews, `User Rating`, Year
FROM
    bestsellers
ORDER BY Reviews DESC
LIMIT 10;
-- -------------------------------------------------

-- Identifying Price of the Highest/Lowest Rated Bestsellers
SELECT 
    `User Rating`, ROUND(AVG(Price), 1) AS Average_Price
FROM
    bestsellers
GROUP BY `User Rating`
ORDER BY `User Rating` DESC;
-- -------------------------------------------------

-- Identifying Price Distribution
SELECT 
    Price, COUNT(*) AS Count
FROM
    bestsellers
GROUP BY Price
ORDER BY Price;
-- -------------------------------------------------

-- Identifying Price Distribution by Year
SELECT 
    Year,
    ROUND(AVG(Price)) AS Average_Price,
    COUNT(*) AS Bestseller_Published
FROM
    bestsellers
GROUP BY Year
ORDER BY Year;
-- -------------------------------------------------

-- Identifying the Years with the Most Published Bestsellers
SELECT 
    Year, COUNT(*) AS Bestseller_Published
FROM
    bestsellers
GROUP BY Year
ORDER BY count DESC;
-- -------------------------------------------------

-- Identifying Most Popular Genre
SELECT 
    Genre,
    ROUND(AVG(`User Rating`), 1) AS Average_Rating,
    SUM(Reviews) AS Total_Reviews
FROM
    bestsellers
GROUP BY Genre
ORDER BY Total_Reviews DESC;
-- -------------------------------------------------

-- Identifying Best Authors by User Rating
SELECT 
    Author, ROUND(AVG(`User Rating`), 1) AS Average_Rating
FROM
    bestsellers
GROUP BY Author
ORDER BY Average_Rating DESC;
-- -------------------------------------------------

-- Identifying Authors with the Most Published Books
SELECT 
    Author, COUNT(*) AS Bestseller_Published
FROM
    bestsellers
GROUP BY Author
ORDER BY Bestseller_Published DESC;
-- -------------------------------------------------

-- Identifying Bestsellers Published Based on their Year And Genre
SELECT 
    Year, Genre, COUNT(*) AS Book_Count
FROM
    bestsellers
GROUP BY Year , Genre
ORDER BY Year , Genre;
-- -------------------------------------------------

-- Evaluating If Bestseller Prices Have Influence on User Ratings
SELECT 
    Price, ROUND(AVG(`User Rating`), 1) AS Avg_Rating
FROM
    bestsellers
GROUP BY Price
HAVING COUNT(*) > 5
ORDER BY Avg_Rating DESC;
-- -------------------------------------------------
```
