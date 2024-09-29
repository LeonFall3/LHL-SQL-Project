Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
-- Cities with the most transaction revenue
SELECT CASE
        WHEN city IN ('(not set)', 'not available in demo dataset') THEN NULL
        ELSE city
    END,
    sum(transactionrevenue)as sum_transact_rev,
    RANK() OVER (ORDER BY sum(transactionrevenue))
FROM all_sessions
GROUP BY city
HAVING sum(transactionrevenue) > 0 AND city NOT IN ('(not set)', 'not available in demo dataset')
LIMIT 10

--- Countries with the most transaction revenue

SELECT CASE
        WHEN country IN ('(not set)', 'not available in demo dataset') THEN NULL
        ELSE country
    END,
    sum(transactionrevenue)as sum_transact_rev,
    RANK() OVER (ORDER BY sum(transactionrevenue))
FROM all_sessions
GROUP BY country
HAVING sum(transactionrevenue) > 0 AND country NOT IN ('(not set)', 'not available in demo dataset')
LIMIT 10


Answer:
City : Sunnyvale 
Transaction Revenue : 200000000 

Country : United States
Transaction Revenue : 2390950000



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

-- Avgerage number of products ordered from each city

WITH
city_clean AS (
    SELECT CASE
            WHEN city IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE city
        END as city_clean, city
    FROM all_sessions
),

country_clean AS(
    SELECT CASE
            WHEN country IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE country
        END as country_clean, country
    FROM all_sessions
)

SELECT ROUND(AVG(productquantity),2) as avg_number_of_product, city_clean as city
FROM all_sessions
JOIN city_clean USING (city)
WHERE productquantity IS NOT NULL AND city_clean IS NOT NULL
GROUP BY city_clean

---

-- Avgerage number of products ordered from each country

WITH
city_clean AS (
    SELECT CASE
            WHEN city IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE city
        END as city_clean, city
    FROM all_sessions
),

country_clean AS(
    SELECT CASE
            WHEN country IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE country
        END as country_clean, country
    FROM all_sessions
)

SELECT ROUND(AVG(productquantity),2) as avg_number_of_product, country_clean as country
FROM all_sessions
JOIN country_clean USING(country)
WHERE productquantity IS NOT NULL AND country_clean IS NOT NULL
GROUP BY country_clean


Answer:
Cities

    1.00	"San Jose"
    8.00	"Salem"
    1.17	"New York"
    1.00	"San Francisco"
    1.00	"Dallas"
    1.00	"Dublin"
    1.00	"Palo Alto"
    1.00	"Chicago"
    2.00	"Houston"
    1.00	"Seattle"
    1.00	"Columbus"
    1.00	"Los Angeles"
    1.00	"Ann Arbor"
    1.00	"Mountain View"
    1.00	"Detroit"
    1.00	"Bengaluru"
    1.00	"Sunnyvale"
    10.00	"Madrid"
    4.00	"Atlanta"

Countries
    1.00	"Ireland"
    1.00	"Finland"
    1.00	"Colombia"
    1.00	"France"
    4.02	"United States"
    10.00	"Spain"
    1.00	"Canada"
    1.00	"Argentina"
    1.00	"India"
    1.00	"Mexico"




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







