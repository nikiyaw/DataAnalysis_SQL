Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

Answer:
United States has the highest level of transaction revenues on the site. 
As for cities, the option 'not available in demo dataset' has the highest but it might be a combination of several unknown cities so there is no way of knowning. 
The available information indicates that Mountain View is the city with the highest level of transaction revenue.

SQL Queries:
--from all_sessions: productsku(PK), country, city, productprice
--from sales_report: productsku(PK), total_ordered 

SELECT 
	alls.city, alls.country,
	SUM(sr.total_ordered * (alls.productprice/1000000)) AS TransactionRevenues
FROM sales_report as sr
INNER JOIN all_sessions as alls
ON sr.productsku = alls.productsku
GROUP BY alls.city, alls.country
ORDER BY SUM(sr.total_ordered * alls.productprice) DESC


**Question 2: What is the average number of products ordered from visitors in each city and country?**

Answer: 
I choose to disregard the city due to lack of data. The query below made a list that includes all the products and the total average based on each country (alphabetically). 

SQL Queries:
--from sales_report: productsku(key), total_ordered
--from all_sessions: productsku(key), v2productname, country

SELECT 
	alls.country, alls.v2productname, sr.total_ordered,
       AVG(sr.total_ordered) OVER
         (PARTITION BY alls.country) AS running_total
  FROM sales_report sr
  INNER JOIN all_sessions alls
	ON sr.productsku = alls.productsku


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**

Answer:
I came up with 4 different queries (trying out different ways to look at data). I choose to ignore the cities and only focus on the countries because there are plenty of missing datas in the city column. The numbers indicate that office supplies seem to be the most popular category followed by drinkwares and apparels. This trend seems to be applicable to most of the countries.

SQL Queries:
--from sales_report: productsku(key), total_ordered
--from all_sessions: productsku(key), v2productcategory, country

The first query shows the highest amount of product categories that are ordered and the countries that are ordering them.  

Query 1: 
SELECT 
	alls.country, alls.v2productcategory, 
	sr.total_ordered
FROM sales_report sr
INNER JOIN all_sessions alls
ON sr.productsku = alls.productsku
GROUP BY alls.country, alls.v2productcategory, sr.total_ordered
ORDER BY sr.total_ordered DESC

The second query shows different countries (alphabetically), the categories that are ordered and the rolling sum for the country. 

Query 2:
SELECT 
	alls.country, alls.v2productcategory, sr.total_ordered,
       SUM(sr.total_ordered) OVER
         (PARTITION BY alls.country) AS running_total
  FROM sales_report sr
  INNER JOIN all_sessions alls
	ON sr.productsku = alls.productsku
	
The third query ranks the categories that are purchased on the amount of total_ordered, based on different countries (alphabetically). It shows whoch categories are most often purchased on each country.

Query 3:
SELECT 
	alls.country, alls.v2productcategory, sr.total_ordered,
       DENSE_RANK() OVER
         (PARTITION BY alls.country ORDER BY sr.total_ordered DESC) AS running_total
  FROM sales_report sr
  INNER JOIN all_sessions alls
	ON sr.productsku = alls.productsku	
	
The fourth query looks at the running total that are ranked first and the product categories in different countries.

Query 4:
WITH no_1 AS (
	(SELECT 
	alls.country, alls.v2productcategory, sr.total_ordered,
       DENSE_RANK() OVER (PARTITION BY alls.country ORDER BY sr.total_ordered DESC) AS running_total
  FROM sales_report sr
  INNER JOIN all_sessions alls
	ON sr.productsku = alls.productsku)) 
SELECT * FROM no_1 WHERE running_total = 1


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**

Answer: 
Similar to Q3, but now looking at products, the query below shows the most (rank 1) sold items in each country. The items are mostly random and doesn't really fit a pattern.

SQL Queries:
--from sales_report: productsku(key), total_ordered
--from all_sessions: productsku(key), v2productname, country

WITH no_1 AS (
	(SELECT 
	alls.country, alls.v2productname, sr.total_ordered,
       DENSE_RANK() OVER (PARTITION BY alls.country ORDER BY sr.total_ordered DESC) AS running_total
  FROM sales_report sr
  INNER JOIN all_sessions alls
	ON sr.productsku = alls.productsku)) 
SELECT * FROM no_1 WHERE running_total = 1


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

Answer: 
I choose to only focus on the country because there are significant missing data in the city column. United States seems to be bringing in the highest revenue total amongst all the other countries by a huge amount of approximately 5.3 million dollars. United Kingdom takes the second place by producing a revenue of a quarter dollars. Canada takes the third place by making around 170,000 dollars. 

SQL Queries:
--from all_sessions: productsku(PK), country, city, productprice
--from sales_report: productsku(PK), total_ordered

SELECT 
	alls.country,
	SUM(sr.total_ordered * CAST((productprice / 1000000.) AS DECIMAL (10,2))) AS TransactionRevenues
FROM sales_report as sr
INNER JOIN all_sessions as alls
ON sr.productsku = alls.productsku
GROUP BY alls.country
ORDER BY SUM(sr.total_ordered * alls.productprice) DESC





















