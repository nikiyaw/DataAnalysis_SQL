What are your risk areas? Identify and describe them.


1. There are a lot of missing data on the city column in the all_sessions table. 
- The data we obtained by ignoring the city column might skew our data largely. 
- Since United States generates the largest revenue out of all countries, it is crucial to obtain more relevant data regarding different cities. 
- There are 8727 total rows when we apply the WHERE function: country = 'United States' but when we include (city = 'not available in demo dataset' OR city = '(not set)') into the function, there are 4214 rows. 
- It indicates that we lost close to half of the data that can be very valuable. 

Improvements to be made:
- investigating the data collecting methods and see if changes can be made

Query:
SELECT 
	country, city 
FROM all_sessions
WHERE 
	country = 'United States' AND
	(city = 'not available in demo dataset' OR city = '(not set)')


2. The relationship between the tables: sales_report and sales_by_sku. 
- According to my understanding, both reports should have the same number of rows for productsku/sku but there seems to be some discrepancies. 
- I ran a query using a left join to look at the extra null values (8 extra rows in sales_by_sku)

Improvements can be made by defining constraints in these tables: 
- identify the parent table and child table 
- ensure changes that are made to either table are synced up
- make sure the data is consistent across different tables

Query:
SELECT
	sbs.productsku AS sales_by_sku, 
	sr.productsku AS sales_report
FROM sales_by_sku sbs 
LEFT JOIN sales_report sr
ON sr.productsku = sbs.productsku

