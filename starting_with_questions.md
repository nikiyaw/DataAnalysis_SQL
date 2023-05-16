Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

SQL Queries:

--from all_sessions: country, city, productprice**, productsku (key)
--from sales_report: productsku (key), total_ordered 

SELECT 
	alls.city, alls.country,
	SUM(sr.total_ordered * (alls.productprice/1000000)) AS TransactionRevenues
FROM sales_report as sr
INNER JOIN all_sessions alls
ON sr.productsku = alls.productsku
GROUP BY alls.city, alls.country
ORDER BY SUM(sr.total_ordered * alls.productprice) DESC

Answer:
United States is the country that has the highest level of transaction revenues on the site. 
As for cities, the option 'not available in demo dataset' has the highest but it might be a combination of several unknown cities so there is no way of knowning.  
The available information indicates that Mountain View is the city with the highest level of transaction revenue.


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:



Answer:


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

--from sales_report: productsku(key), total_ordered
--from all_sessions: productsku(key), v2productcategory, city, country

SELECT 
	alls.country, alls.city, alls.v2productcategory, 
	sr.total_ordered
FROM sales_report sr
INNER JOIN all_sessions alls
ON sr.productsku = alls.productsku
GROUP BY alls.country, alls.city, alls.v2productcategory, sr.total_ordered
ORDER BY sr.total_ordered DESC

Answer:


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

SELECT 
	alls.country, alls.city, alls.v2productname, 
	sr.total_ordered
FROM sales_report sr
INNER JOIN all_sessions alls
ON sr.productsku = alls.productsku
GROUP BY alls.country, alls.city, alls.v2productname, sr.total_ordered
ORDER BY sr.total_ordered DESC
Answer:


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

Same as Question 1??

Answer:




















