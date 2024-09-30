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

WITH city_clean AS (
    SELECT CASE
            WHEN city IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE city
        END as city_clean,
        city
    FROM all_sessions
	WHERE city IS NOT NULL
)


SELECT city_clean, ROUND(AVG(total_ordered),2) as total_ordered 
FROM city_clean
JOIN all_sessions USING(city)
JOIN sales_report USING(productsku)
JOIN products ON sales_report.productsku = sku
WHERE total_ordered >0 AND city IS NOT NULL
GROUP BY city_clean
ORDER BY total_ordered DESC

---

-- Avgerage number of products ordered from each country

country_clean AS(
    SELECT CASE
            WHEN country IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE country
        END as country_clean,
        country
    FROM all_sessions
)

SELECT country_clean, ROUND(AVG(total_ordered),2) as total_ordered 
FROM country_clean
JOIN all_sessions USING(country)
JOIN sales_report USING(productsku)
JOIN products ON sales_report.productsku = sku
WHERE total_ordered >0 AND country IS NOT NULL
GROUP BY country_clean
ORDER BY total_ordered DESC



Answer:
Cities
    "Brno"	319.00
    "Riyadh"	319.00
    "Rexburg"	250.50
    "Sacramento"	189.00
    "Lisbon"	189.00
    "Sherbrooke"	145.50
    "Saint Petersburg"	135.00
    "Rome"	130.33
    "Dubai"	111.50
    "Kalamazoo"	105.00
    "Avon"	100.00
    "Longtan District"	97.00
    "Nashville"	94.00
    "Pune"	85.30
    "Seoul"	75.38
    "Shinjuku"	73.00
    "Westville"	70.00
    "Athens"	68.33
    "Rio de Janeiro"	65.33
    "Vladivostok"	62.00
    "Bellingham"	62.00
    "Palo Alto"	60.44
    "Santa Fe"	60.00
    "San Antonio"	58.17
    "Salem"	56.68
    "Boston"	55.18
    "Menlo Park"	51.50
    "Santa Clara"	49.00
    "Zhongli District"	46.00
    "Bangkok"	45.56
    "San Bruno"	44.82
    "Munich"	44.33
    "Indore"	44.00
    "Belo Horizonte"	43.00
    "Hong Kong"	42.19
    "Phoenix"	40.25
    "Tel Aviv-Yafo"	40.00
    "Philadelphia"	39.89
    "Hamburg"	39.14
    "Calgary"	39.00
    "Dublin"	38.90
    "Hanoi"	37.67
    "Redwood City"	35.67
    "Toronto"	35.02
    "Mumbai"	34.50
    "Irvine"	34.36
    "Pozuelo de Alarcon"	34.33
    "Los Angeles"	32.34
    "Seattle"	31.91
    "Kitchener"	31.33
    "Mountain View"	30.68
    "Chicago"	30.05
    "Bellflower"	30.00
    "Cork"	30.00
    "Buenos Aires"	29.67
    "Ann Arbor"	29.65
    "Charlotte"	29.56
    "Madrid"	29.50
    "Redmond"	29.50
    "Milan"	29.33
    "Austin"	28.86
    "Amsterdam"	28.43
    "San Francisco"	28.23
    "Bogota"	27.82
    "Oakland"	27.75
    "Cupertino"	27.00
    "Pittsburgh"	26.67
        26.58
    "Melbourne"	26.52
    "San Jose"	26.47
    "Sydney"	26.20
    "New York"	25.71
    "Mississauga"	24.67
    "Sunnyvale"	24.58
    "Houston"	24.39
    "Sao Paulo"	23.60
    "London"	23.38
    "Montreuil"	23.00
    "Hyderabad"	22.73
    "Kuala Lumpur"	22.71
    "Mexico City"	22.64
    "Chennai"	21.79
    "Vancouver"	21.00
    "Budapest"	21.00
    "Santiago"	20.75
    "Kolkata"	20.67
    "Ho Chi Minh City"	20.13
    "La Victoria"	20.00
    "Warsaw"	20.00
    "Lake Oswego"	20.00
    "Prague"	20.00
    "New Delhi"	19.31
    "Berlin"	18.43
    "Ahmedabad"	18.00
    "Washington"	17.95
    "Paris"	17.92
    "Bucharest"	17.83
    "Singapore"	17.16
    "Barcelona"	17.15
    "Zurich"	16.42
    "Cambridge"	16.09
    "Montreal"	16.08
    "Ghent"	16.00
    "Copenhagen"	16.00
    "Minato"	15.89
    "Jakarta"	15.60
    "Montevideo"	15.00
    "Courbevoie"	15.00
    "Taguig"	14.50
    "Dallas"	14.33
    "San Diego"	14.30
    "Quebec City"	14.00
    "Perth"	14.00
    "Kirkland"	13.70
    "Istanbul"	13.17
    "Moscow"	13.14
    "Wrexham"	13.00
    "Santa Monica"	13.00
    "The Dalles"	13.00
    "Asuncion"	13.00
    "LaFayette"	13.00
    "Akron"	13.00
    "Chandigarh"	13.00
    "Bengaluru"	12.83
    "Atlanta"	12.73
    "Detroit"	12.33
    "Kiev"	12.15
    "Gurgaon"	11.83
    "Stockholm"	11.71
    "Osaka"	11.60
    "San Mateo"	11.00
    "Ipoh"	11.00
    "Jaipur"	11.00
    "Manila"	10.50
    "Yokohama"	9.67
    "Burnaby"	9.50
    "Portland"	9.50
    "South San Francisco"	8.67
    "Timisoara"	8.00
    "Culiacan"	8.00
    "Fremont"	7.90
    "Medellin"	7.00
    "Thessaloniki"	7.00
    "East Lansing"	7.00
    "Frankfurt"	7.00
    "St. Louis"	7.00
    "Oviedo"	6.50
    "Quezon City"	6.50
    "Denver"	6.40
    "Vienna"	6.33
    "Helsinki"	6.00
    "Kharagpur"	6.00
    "Nanded"	6.00
    "Bandung"	6.00
    "Tempe"	6.00
    "Milpitas"	6.00
    "Boulder"	5.33
    "Colombo"	5.25
    "Jacksonville"	5.00
    "University Park"	5.00
    "Columbia"	5.00
    "Eau Claire"	4.71
    "Orlando"	4.50
    "Las Vegas"	4.00
    "Jersey City"	4.00
    "Cluj-Napoca"	4.00
    "Richardson"	4.00
    "Marseille"	4.00
    "Poznan"	4.00
    "Ashburn"	4.00
    "Doha"	4.00
    "Brussels"	4.00
    "Kharkiv"	4.00
    "Izmir"	3.50
    "Brisbane"	3.50
    "Petaling Jaya"	3.00
    "Stanford"	3.00
    "Antwerp"	3.00
    "Madison"	3.00
    "Council Bluffs"	3.00
    "Adelaide"	3.00
    "Wellesley"	3.00
    "Zagreb"	3.00
    "Druid Hills"	3.00
    "Norfolk"	3.00
    "Edmonton"	2.00
    "Salford"	2.00
    "Iasi"	2.00
    "Patna"	2.00
    "Chico"	2.00
    "Maracaibo"	1.50
    "Kansas City"	1.00
    "Oslo"	1.00
    "Vilnius"	1.00
    "Piscataway Township"	1.00
    "Lahore"	1.00
    "Rosario"	1.00
    "Greer"	1.00
"Villeneuve-d'Ascq"	1.00

Countries
    "Saudi Arabia"	134.80
    "Kuwait"	114.33
    "Honduras"	112.00
    "Ethiopia"	85.00
    "Oman"	85.00
    "Laos"	85.00
    "United Arab Emirates"	65.13
    "Papua New Guinea"	57.50
    "Croatia"	57.44
    "South Korea"	56.08
    "Albania"	50.00
    "San Marino"	50.00
    "Greece"	47.55
    "Trinidad & Tobago"	47.00
    "Hong Kong"	44.88
    "Kazakhstan"	43.00
    "Czechia"	39.48
    "Italy"	38.63
    "Guatemala"	37.86
    "Denmark"	37.56
    "Taiwan"	36.31
    "Israel"	35.88
    "Japan"	35.60
    "Spain"	35.30
    "Mexico"	35.13
    "Slovenia"	34.50
    "Malaysia"	34.15
    "Germany"	34.14
    "Romania"	33.85
    "Colombia"	32.39
    "Réunion"	32.00
    "Thailand"	31.82
    "Ireland"	31.73
    "Brazil"	31.69
    "Mauritius"	31.00
    "Ukraine"	30.79
    "Nepal"	30.50
    "Morocco"	30.33
    "Montenegro"	30.00
    "Moldova"	30.00
    "Mali"	30.00
    "Russia"	29.74
    "Tunisia"	29.50
    "Pakistan"	28.09
    "United States"	27.74
    "Argentina"	27.61
    "Bulgaria"	27.13
    "Singapore"	26.68
    "Canada"	26.63
    "United Kingdom"	26.19
    "Portugal"	24.77
    "Venezuela"	24.69
    "Netherlands"	24.38
    "Switzerland"	23.76
    "Myanmar (Burma)"	23.67
    "Georgia"	22.75
    "Australia"	22.53
    "India"	22.51
    "Belgium"	21.93
    "Slovakia"	21.88
    "Indonesia"	21.70
    "Peru"	21.21
    "Vietnam"	21.00
    "Côte d’Ivoire"	20.67
    "New Zealand"	19.95
    "Uruguay"	19.50
    "Philippines"	19.40
    "Turkey"	19.04
    "France"	18.69
    "Dominican Republic"	18.17
    "Macedonia (FYROM)"	17.67
    "Poland"	17.53
    "Sweden"	16.97
    "Finland"	16.88
    "Nigeria"	16.75
    "Panama"	16.67
    "Kyrgyzstan"	16.00
    "Armenia"	16.00
    "Costa Rica"	15.80
    "Ghana"	15.00
    "Puerto Rico"	14.80
    "Lithuania"	14.43
    "Hungary"	14.00
    "El Salvador"	13.00
    "Paraguay"	13.00
    "Uganda"	13.00
    "South Africa"	12.43
    "Zimbabwe"	12.00
    "Chile"	11.71
    "Bahrain"	11.50
    "Egypt"	11.38
    "Serbia"	11.33
    "Jersey"	11.00
    "Bangladesh"	10.60
    "Tanzania"	9.00
    "Algeria"	9.00
    "Sri Lanka"	8.75
    "Norway"	8.59
    "Austria"	8.08
    "Bolivia"	8.00
    "Cambodia"	6.50
    "Latvia"	6.33
    "China"	6.08
    "Botswana"	5.00
    "Cyprus"	5.00
    "Nicaragua"	5.00
    "Jordan"	5.00
    "Qatar"	4.00
    "Estonia"	3.60
    "Palestine"	3.00
    "Jamaica"	3.00
    "Bahamas"	2.50
    "Gibraltar"	2.00
    "Sint Maarten"	2.00
    "Macau"	2.00
    "Brunei"	2.00
    "Kenya"	2.00
    "Bosnia & Herzegovina"	2.00
    "Martinique"	2.00
    "Belarus"	2.00
    "Ecuador"	2.00
    "Kosovo"	1.00
    "Iraq"	1.00
    "Maldives"	1.00
   




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

 --- Cities

WITH --- remove nulls from cities and categories
cities_categories_NOT_NULL_all_sessions AS (
    SELECT CASE
            WHEN all_sessions.v2productcategory IN ('${escCatTitle}', '(not set)') THEN NULL
            ELSE all_sessions.v2productcategory
        END as v2productcategory,
        CASE
            WHEN all_sessions.city IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE all_sessions.city
        END as city
    FROM all_sessions
    WHERE all_sessions.city IS NOT NULL
        AND all_sessions.v2productcategory IS NOT NULL
),
--- rank categories based on total products from that category sold in each city
ranked_categories_w_cities AS(
    SELECT DISTINCT cities_categories_NOT_NULL_all_sessions.city,
        cities_categories_NOT_NULL_all_sessions.v2productcategory,
        RANK () OVER(
            PARTITION BY cities_categories_NOT_NULL_all_sessions.city
            ORDER BY SUM(sales_report.total_ordered)
        ),
        SUM(sales_report.total_ordered) as total_ordered
    FROM all_sessions
        JOIN products ON all_sessions.productsku = products.sku
        JOIN sales_report on all_sessions.productsku = sales_report.productsku
        JOIN cities_categories_NOT_NULL_all_sessions ON all_sessions.city = cities_categories_NOT_NULL_all_sessions.city
    GROUP BY cities_categories_NOT_NULL_all_sessions.city,
        cities_categories_NOT_NULL_all_sessions.v2productcategory
    ORDER BY rank,
        cities_categories_NOT_NULL_all_sessions.city
),

--- limit the results to only the top most ranked categories
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

SELECT

(SELECT COUNT(rank1_cities.v2productcategory)
FROM rank1_cities
WHERE rank1_cities.v2productcategory LIKE 'Home/Apparel%') AS Apparel,

(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Accessories%') AS Accessories
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Bags%') AS Bags
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Brands%') AS Brands
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Clearance%') AS Clearance
    ,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Drinkware%') AS Drinkware
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Electronices%') AS Electronices
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Fruit%') AS FruitGames
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Fun%') AS Fun
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Gift%') AS GiftCards
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Kids%') AS Kids
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Lifestyle%') AS Lifestyle
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Limited%') AS LimitedSupply
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Nest%') AS Nest
,
(
    SELECT COUNT(rank1_cities.v2productcategory)
    FROM rank1_cities
    WHERE rank1_cities.v2productcategory LIKE 'Home/Office%') AS Office 
FROM rank1_cities
Limit 1


Answer:
Cities
Started by finding top 3 ranked categories in each city. Lowered that to 1 due to many categories being tied for ranks. Apperel is the most common top product category by roughly 200.

Countries
I reused the same query as cities to find the most common top product category for countries. Apparel is also the most common product catergory among countries. This time by aroughly 100. 

The second most common for both cities and countries was accessories. The rest of the catagories were significantly lower. The 3rd most common for both were roughly half of accessories.






**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

--- Cities (Code reused for Countries by swaping out 'city' or 'cities' for 'country' and 'countries')

WITH
--cleaning city
clean_city AS (SELECT CASE
        WHEN city IN ('(not set)', 'not available in demo dataset') THEN NULL
        ELSE city
    END as city
FROM all_sessions
WHERE city IS NOT NULL),


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
END as city, productsku, RANK () OVER(PARTITION BY city ORDER BY total_ordered DESC) as rank
    FROM totalOrderedByCityProductsku
    Order By city)

-- adding names of products and only asking for rank 1s for each city
    SELECT city, products.name as product_name
    FROM rankedSalesByProductCity
    JOIN products ON productsku = sku
    WHERE rank =1



Answer:

Cities (limited to 10 as example of the results)

    "Adelaide"	" Men's Watershed Full Zip Hoodie Grey"
    "Ahmedabad"	" Canvas Tote Natural/Navy"
    "Akron"	" Men's 100% Cotton Short Sleeve Hero Tee Navy"
    "Amsterdam"	" Hard Cover Journal"
    "Ann Arbor"	"Foam Can and Bottle Cooler"
    "Antwerp"	"Colored Pencil Set"
    "Ashburn"	"Compact Selfie Stick"
    "Asuncion"	" Men's 100% Cotton Short Sleeve Hero Tee Navy"
    "Athens"	" Hard Cover Journal"
    "Atlanta"	" Protect Smoke + CO White Wired Alarm-USA"

    Like we saw before, most top sellers are apperel products.  


Countries (limited to 10 as example of the results)

    "Albania"	"22 oz  Bottle Infuser"
    "Algeria"	" Twill Cap"
    "Argentina"	" Hard Cover Journal"
    "Armenia"	"Windup Android"
    "Australia"	" Hard Cover Journal"
    "Austria"	" Custom Decals"
    "Bahamas"	" Twill Cap"
    "Bahrain"	" Men's 100% Cotton Short Sleeve Hero Tee White"
    "Bangladesh"	"Keyboard DOT Sticker"
    "Belarus"	" Women's Performance Full Zip Jacket Black"

    Like we saw before, most top sellers are apperel products.




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







