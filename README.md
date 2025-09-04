# SQL Bike Data Exploration
 In this project I performed an exploratory data analysis (EDA) on a bike sales dataset using SQL. My goal and objectives was to uncover trends, patterns, and business insights about customer demographics, sales performance, and customers behavior relative to their purchase system


## Problem Statement:

A bike company wants to better understand its customer base to optimize marketing strategies, identifying key customer segments, and increases sales. The goal of the project is to analyze a dataset of bike buyers using SQL to uncover valuable insights into their purchasing behavior and demographic profiles.


## Tools and Technology
-	**SQL (MySQL):** I used this tool for querying, manipulating, and analyzing the data.
-	**Database Management System**: I wrote the queries for a relational database environment.

## Key Questions Answered
This analysis addresses several core business questions to inform strategic decisions.
-	**Market Segmentation**:  Which age groups, educational levels, and occupations represent the most profitable customer segments?
-	**Customer Profiling**: What is the typical demographic profile of a bike purchaser ( e.g., average income, home ownership, family size)?
-	**Customer Targeting**: How can we identify high value customers for premium product promotions?
-	**Behavioral Trends**: How does a customer’s income or commute distance relate to their likelihood of purchasing a bike?
-	**Summary Statistics**: What are the minimum, maximum, and average income levels across different customer groups?


## Solutions/Queries:

The following SQL queries were used to answer the key questions and demonstrate proficiency in various advanced SQL concepts

1.	Analyzing Customer Demographics With (CASE Statement)
This query segments customs into distinct age groups to see which one buys the most bikes, providing a clear picture of the primary target audience. 



- SELECT
- CASE
- WHEN Age < 30 THEN 'Young Adult'
- WHEN Age BETWEEN 20 AND 50 THEN 'middle-Aged'
- ELSE 'Senior' 
- END AS Age_Group,
- COUNT(Purchased_Bike) AS Total_Customers,
- SUM(CASE WHEN Purchased_Bike = 'Yes' THEN 1 ELSE 0 END) AS Bike_Purchased
- FROM business_db.enugu23
- GROUP BY Age_Group
- ORDER BY Bike_Purchased DESC;

2.	**Identifying High Income Buyers With (Nested Subquery)**
This query identifies customers whose income is higher than the average income of all bike buyers, a key step in finding potential customers for premium or high end models.



 - SELECT        
- ID,
- Income,
- 'Purchased Bike'
- FROM business_db.enugu23
- WHERE Income > (
- SELECT AVG(Income)
- FROM business_db.enugu23
- WHERE 'Purchased Bike'='Yes'
- );

3.	**Comparative Analysis Using (LEAD and LAG)**: I used the LEAD and LAG, query to compare customers income to the income of the next and previous customers in a sorted list. This helps analyze income trends within specific customs segments.



- SELECT
- ID,
- Age,
- Income,
- Region,
- LEAD (Income ) OVER (ORDER BY Age) AS Next_Customer_Income,
- LAG (Income ) OVER (ORDER BY Age) AS Previous_customer_Income
 - FROM business_db.enugu23
 - WHERE 
 - Region IN ('Europe','Pacific','North America')
 - ORDER BY Age ASC;

4.	**Multi Criteria Filtering With (Double Selection with Nested Query)**: This query helps to isolate a specific high value customers segments: those with both above-average income and longer-than-average commute distances. This group is a prime target for high end commuter bikes.
 
- SELECT       
- ID,
- Income,
- Commute_Distance
- FROM business_db.enugu23
- WHERE 
- Income > (SELECT AVG (Income) FROM business_db.enugu23)
- AND
- Commute_Distance > (SELECT AVG (Commute_Distance) FROM business_db.enugu23);


5.	**Filtering Aggregated Date With (GROUP BY and HAVING)**: This query identifies occupations with a proven track record of significant bike purchases (more than 10), helping the business focus on the most profitable professional demographics.



 - SELECT
 - Occupation,
 - COUNT('Purchased Bike') AS Total_purchases,
 - AVG (Income) as Avg_Income
 - FROM business_db.enugu23
 - WHERE 
 - 'Purchased Bike' = 'Yes'
 - GROUP BY Occupation
 - HAVING
 - COUNT('Purchased Bike') > 10
 - ORDER BY Total_purchases DESC,


6.	**Ranking Customers With (RANK() OVER(PARTITION BY))**: This query assigns a rank to each customers income within their specific education level, revealing the top earners within each educational group.



- SELECT
- ID,
- Education,
- Income,
- RANK() OVER (PARTITION BY Education ORDER BY Income ASC) AS Income_Rank
- FROM business_db.enugu23;

7.	**Basic Filtering for Specific Segments With (WHERE, IN BETWEEN)**: I wrote this foundational queries to demonstrate the ability to filter for specific values and ranges, such as finding customers with a certain education level or within a salary range.



- SELECT
- Gender,
- Income,
- Education,
- Region
 - FROM business_db.enugu23
 - WHERE Occupation NOT IN ('Bachelors','Partial College','High School')
 - OR 
 - Occupation NOT IN ('Skilled Manual', 'Clerical','Proffessional')
 - AND
 - Income > '$160,000.00' AND '$90,000.00'
 - ORDER BY Income DESC;


8.	**Advanced Aggregation With Nested Queries**: I wrote this query using a nested subquery to calculate the minimum, maximum, and total income for each education level, providing a comprehensive financial summary.


- SELECT
- Education,
- Income,
- (SELECT SUM(Income) FROM business_db.enugu23) AS Total_Income,
- (SELECT MIN(Income) FROM business_db.enugu23) AS Minimumn_Income,
- (SELECT MAX(Income) FROM business_db.enugu23) AS Maximumn_Income
- FROM business_db.enugu23
- ORDER BY Income ASC;


## My Key Findings/ Insights
Based on the analysis here are some of the insights I gained from the data

-	**Primary Target Market**: The analysis revealed that the **Middle-Aged** segment which is (30-50 years old) with professional occupations and high incomes are the most frequent bike buyers.
-	**High Value Segments**: Customers with an income above the average and a long commute distance are strong candidates for targeted marketing of higher priced and durable bikes.
-	**Financial Trends**: The summary queries provided a clear financial overview, showing a wide range of incomes within different education levels. This insights can help the business tailor product offering to suit different purchasing power segments
-	**Actionable Intelligence**: The findings suggest focusing marketing campaigns on specific demographic and occupational groups. The data also provide a clear profile for creating look alike audience in digital marketing efforts.

