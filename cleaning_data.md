What issues will you address by cleaning the data?





Queries:
Below, provide the SQL queries you used to clean your data.


Methods to refer to:
1. remove unwanted/irrelevant observations ()
2. remove duplicate data
3. fix structural erros - fixing typos ()
4. ensure accurate datatypes ()
5. address missing values ()
6. deal with outliers
7. standardize/normalize data
8. validate data

Plan:
1. Went through each tables and see what which table represents 
 
TABLE: all_sessions
-get rid of duplicate date
-find unique id (set as PK)
1. time  
- make sense of what the numbers meant
- change data type 
- replace 0 with median?
2. country & city 
- replace '(not set)' OR 'not available in demo dataset' with NULL values
SELECT city,
CASE
	WHEN city = '(not set)' THEN NULL
	WHEN city= 'not available in demo dataset' THEN NULL
	ELSE city
END AS modified_city
FROM all_sessions
3. timeonsite
- make sense of the numbers
- change data type
- what to do with NULL
4. productprice
- change to integer & divide by??
5. v2productname
- get rid of Google, Youtube, Android in the front
6. v2productcategory
- get rid of Home in the front
7. productvariant 
- all to NULL
8. pagetitle
- tidy it up
extra: relationship between productsku & v2productcategory
- numbers and certain G code produces '(not set)'

TABLE: products
-relationship between these 3 
select sku, orderedquantity, stocklevel from products 
--where sku like '9%'
--where orderedquantity = 0 and stocklevel = 0

TABLE: sales_by_sku
-shows distinct products with total_ordered value
- why show zeros?

TABLE: sales_report
- starts with 9 and some G-code are all 0
select * from sales_report order by stocklevel

TABLE: analytics
- get rid of duplicate data
- find unique id 



