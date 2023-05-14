Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**

SQL Queries:

--from all_sessions: country, city, productprice**, productsku (key)
--from sales_report: productsku (key), total_ordered 
--INNER join to find the overlapping rows in both tables

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

select 
	country,
	v2productcategory, 
	count(*) as count 
from all_sessions
--where country = 'United States'
group by country, v2productcategory
having count(*)>1
order by count desc

*get only one row for each country and city

Answer:


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

select 
	country,
	v2productname, 
	count(*) as count 
from all_sessions
--where country = 'United States'
group by country, v2productname
having count(*)>1
order by count desc

*get only one row for each country and city

Answer:


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







