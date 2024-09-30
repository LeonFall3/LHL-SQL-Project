Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
-- Cities with the most transaction revenue
SELECT city,
    sum(totaltransactionrevenue) as sum_transact_rev,
    RANK() OVER (
        ORDER BY sum(totaltransactionrevenue)
    )
FROM all_sessions
GROUP BY city
HAVING sum(totaltransactionrevenue) > 0

--- 

--- Countries with the most transaction revenue

SELECT country,
    sum(totaltransactionrevenue) as sum_transact_rev,
    RANK() OVER (
        ORDER BY sum(totaltransactionrevenue)
    )
FROM all_sessions
GROUP BY country
HAVING sum(totaltransactionrevenue) > 0


Answer:

Cities
    "New York","19590000","1"
    "Chicago","123940000","2"
    "Palo Alto","152000000","3"
    "Mountain View","170990000","4"
    "Sunnyvale","200000000","5"
    "Seattle","358000000","6"
    "San Francisco","398090000","7"
    "Atlanta","742480000","8"

Countries
    "United States","2165090000","1"




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

-- Avgerage number of products ordered from each city


SELECT city,
    ROUND(AVG(total_ordered), 2) as total_ordered
FROM all_sessions
    JOIN sales_report USING(productsku)
    JOIN products ON sales_report.productsku = sku
GROUP BY city
ORDER BY total_ordered DESC


-- Avgerage number of products ordered from each country

SELECT country,
    ROUND(AVG(total_ordered), 2) as total_ordered
FROM all_sessions
    JOIN sales_report USING(productsku)
    JOIN products ON sales_report.productsku = sku
GROUP BY country
ORDER BY total_ordered DESC


Answer:

Cities
    "San Francisco","112.00"
    "Sunnyvale","107.00"
    "San Jose","102.00"
    "Seattle","102.00"
    "Palo Alto","94.00"
    "Mountain View","53.00"
    "Chicago","26.00"
    "New York","24.00"
    "Detroit","23.00"
    "Dublin","3.00"
    "Ann Arbor","1.00"


Countries
    "United States","59.88"
    "Ireland","3.00"




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

(Code reused for Countries by swaping out 'city' or 'cities' for 'country' and 'countries')

WITH
--- rank categories based on total products from that category sold in each city
ranked_categories_w_cities AS(
    SELECT DISTINCT city,
        v2productcategory,
        RANK () OVER(
            PARTITION BY city
            ORDER BY SUM(sales_report.total_ordered)
        ),
        SUM(sales_report.total_ordered) as total_ordered
    FROM all_sessions
        JOIN products ON all_sessions.productsku = products.sku
        JOIN sales_report on all_sessions.productsku = sales_report.productsku
    GROUP BY city,
        v2productcategory
    ORDER BY rank,
        city
),
--- limit the results to only the top most ranked categories
 WITH
 rank1_cities AS (
    SELECT DISTINCT ranked_categories_w_cities.rank,
        ranked_categories_w_cities.v2productcategory,
        ranked_categories_w_cities.city
    FROM ranked_categories_w_cities
    WHERE ranked_categories_w_cities.rank = 1
        AND ranked_categories_w_cities.v2productcategory IS NOT NULL
    ORDER BY ranked_categories_w_cities.city,
        ranked_categories_w_cities.rank
)
SELECT (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Apparel%'
    ) AS Apparel,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Accessories%'
    ) AS Accessories,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Bags%'
    ) AS Bags,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Brands%'
    ) AS Brands,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Clearance%'
    ) AS Clearance,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Drinkware%'
    ) AS Drinkware,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Electronices%'
    ) AS Electronices,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Fruit%'
    ) AS FruitGames,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Fun%'
    ) AS Fun,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Gift%'
    ) AS GiftCards,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Kids%'
    ) AS Kids,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Lifestyle%'
    ) AS Lifestyle,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Limited%'
    ) AS LimitedSupply,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Nest%'
    ) AS Nest,
    (
        SELECT COUNT(rank1_cities.v2productcategory)
        FROM rank1_cities
        WHERE rank1_cities.v2productcategory LIKE 'Home/Office%'
    ) AS Office
FROM rank1_cities
Limit 1



Answer:
Apparel is the most common category among both cities and countries




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

(Code reused for Countries by swaping out 'city' or 'cities' for 'country' and 'countries')


WITH
-- total sales per product and city
totalOrderedByCityProductsku AS (
    SELECT city,
        productsku,
        sum(total_ordered) AS total_ordered
    FROM sales_by_sku
        JOIN all_sessions USING(productsku)
    GROUP BY city,
        productsku
),
--rank sales by product & city
rankedSalesByProductCity AS (
    SELECT CASE
            WHEN city IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE city
        END as city,
        productsku,
        RANK () OVER(
            PARTITION BY city
            ORDER BY total_ordered DESC
        ) as rank
    FROM totalOrderedByCityProductsku
    Order By city
) -- adding names of products and only asking for rank 1s for each city
SELECT city,
    products.name as product_name
FROM rankedSalesByProductCity
    JOIN products ON productsku = sku
WHERE rank = 1



Answer:

Cities
    "Ann Arbor"," Men's Vintage Badge Tee Black"
    "Chicago"," Sunglasses"
    "Detroit"," 22 oz Water Bottle"
    "Dublin"," Laptop Backpack"
    "Mountain View"," Learning Thermostat 3rd Gen-USA - Stainless Steel"
    "New York"," Protect Smoke + CO White Wired Alarm-USA"
    "Palo Alto"," Learning Thermostat 3rd Gen-USA - Stainless Steel"
    "San Francisco"," Cam Outdoor Security Camera - USA"
    "San Jose"," Cam Indoor Security Camera - USA"
    "Seattle"," Cam Indoor Security Camera - USA"
    "Sunnyvale"," Cam Outdoor Security Camera - USA"


    Like we saw before, most top sellers are apperel products.  


Countries

"Ireland"," Laptop Backpack"
"United States"," Cam Indoor Security Camera - USA"


    Like we saw before, most top sellers are apperel products.




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

-- Cities


WITH -- replace null with 0
tottransactionrev AS (
    SELECT CASE
            WHEN totaltransactionrevenue IS NULL THEN 0
            ELSE totaltransactionrevenue
        END as totaltransactionrevenue_noNULLS,
        date
    FROM all_sessions
),
-- find revenue impact from revenue and sentiment measurements
rev_sentiment AS (
    SELECT totaltransactionrevenue_noNULLS *(sentimentscore * sentimentmagnitude) asRevImpact,
        productsku
    FROM sales_report
        JOIN all_sessions USING(productsku)
        JOIN tottransactionrev USING(date)
) -- total revenue impact by city
SELECT DISTINCT city,
    sum(asRevImpact) as impact_score
FROM all_sessions
    JOIN rev_sentiment USING(productsku)
GROUP BY city
ORDER BY sum(asRevImpact) DESC

---

-- Countries


WITH -- replace null with 0
tottransactionrev AS (
    SELECT CASE
            WHEN totaltransactionrevenue IS NULL THEN 0
            ELSE totaltransactionrevenue
        END as totaltransactionrevenue_noNULLS,
        date
    FROM all_sessions
),
-- find revenue impact from revenue and sentiment measurements
rev_sentiment AS (
    SELECT totaltransactionrevenue_noNULLS *(sentimentscore * sentimentmagnitude) asRevImpact,
        productsku
    FROM sales_report
        JOIN all_sessions USING(productsku)
        JOIN tottransactionrev USING(date)
) -- total revenue impact by country
SELECT DISTINCT country,
    sum(asRevImpact) as impact_score
FROM all_sessions
    JOIN rev_sentiment USING(productsku)
GROUP BY country
ORDER BY sum(asRevImpact) DESC



Answer:

The higher the impact score the high the impact that city or country has had on the total revenue

Cities

    "Sunnyvale",154680000
    "San Francisco",121200000
    "Mountain View",91800000
    "Palo Alto",45900000
    "San Jose",33480000
    "Seattle",33480000
    "Chicago",9915200.000000002
    "New York",7052400.000000001
    "Ann Arbor",0
    "Detroit",0
    "Dublin",0

Sunnyvale has had the highest impact on revenue by a city

---

Countries

    "United States",497507600
    "Ireland",0


The United States has had the highest impact on revenue by a country


