Question 1: 
Are there any products that are in shortage (ordered amount is larger than stock level)? If so, how much more should we order to keep up with the orders? 

SQL Queries:
SELECT
	sr.name, sr.productsku, sr.total_ordered, p.stocklevel, 
	(sr.total_ordered - p.stocklevel) AS AmountNeeded
FROM sales_report sr
INNER JOIN products p
	ON sr.productsku = p.sku
WHERE sr.total_ordered > p.stocklevel

Answer: 
By obtaining the name and ordered amount from sales_report and stock level from products, we can use an inner join to find the overlapping cases. There are 2 cases where we need to order more to keep up with the orders. I added a column to indicate the amount that is needed for each item. 


Question 2: 
Which way of channelgrouping brings is the most popular in the United States? How much total revenue each channels bring in? 

SQL Queries:
SELECT channelgrouping, count(*) AS ReferralAmount, (SUM(productprice)/1000000) AS TotalRevenue
FROM all_sessions
WHERE country = 'United States'
GROUP BY channelgrouping
ORDER BY ReferralAmount DESC

Answer:
Organic Search is the most common method in accessing the site. It also brings in the largest revenue when compare to other channels. 


Question 3: 
Which month or year has the highest sales? How much orders are there? Is there a pattern? 

SQL Queries:
SELECT 
	EXTRACT(year FROM date) AS purchased_year,
	EXTRACT(month FROM date) AS purchased_month,
	COUNT(*) AS count
FROM all_sessions
GROUP BY purchased_year, purchased_month
ORDER BY purchased_year, purchased_month

Answer:
August 2016 seems to hold the highest sales whereas August 2017 has the lowest sales. Sales have been pretty average throughout the year. 

