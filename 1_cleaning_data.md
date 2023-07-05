## What are the data cleaning steps taken?

# 1. Check for duplicated rows in all_sessions. 


WITH duplicate_rows AS (
		(SELECT *,
    		ROW_NUMBER() OVER ( 
        	PARTITION BY fullvisitorid 
        	ORDER BY fullvisitorid
        ) AS Row_Number
			FROM all_sessions)) 
SELECT * FROM duplicate_rows WHERE Row_Number <> 1


# 2. Replace inconsistent data values in country, city columns with NULL.  


SELECT city,
CASE
	WHEN city = '(not set)' THEN NULL
	WHEN city= 'not available in demo dataset' THEN NULL
	ELSE city
END AS modified_city
FROM all_sessions

SELECT country,
CASE
	WHEN country = '(not set)' THEN NULL
	ELSE country
END AS modified_country
FROM all_sessions


# 3. Address missing values by replacing null values with median value in the timeonsite column. 


SELECT timeonsite, 
	COALESCE(timeonsite, 
			 (SELECT PERCENTILE_CONT(0.5) WITHIN GROUP(ORDER BY timeonsite) FROM all_sessions)) AS timeonsite
FROM all_sessions


# 4. Fix the productprice column by diving price by 1000000 (in millions). 


SELECT productprice, 
	CAST((productprice / 1000000.) AS DECIMAL (10,2))	
FROM all_sessions


# 5. Remove redundant observations in v2productname and v2productcategory columns. 


SELECT v2productname,  
	CASE WHEN v2productname like 'Google%' THEN REPLACE(v2productname, 'Google', '')
		WHEN v2productname like 'YouTube%' THEN REPLACE(v2productname, 'YouTube', '')
		WHEN v2productname like 'Android%' THEN REPLACE(v2productname, 'Android', '')
		ELSE v2productname END
		AS new_v2productname
FROM all_sessions

SELECT v2productcategory,  
	REPLACE(v2productcategory, 'Home/', '') AS new_v2productcategory
FROM all_sessions

