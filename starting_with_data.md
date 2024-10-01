Question 1: What months are the most and least profitable

SQL Queries:

WITH
-- replace null with 0
tottransactionrev AS (SELECT CASE WHEN totaltransactionrevenue IS NULL THEN 0 ELSE  totaltransactionrevenue END as totaltransactionrevenue_noNULLS, date FROM all_sessions )

--order months (with names) by their total transaction revenue
SELECT TO_CHAR(all_sessions.date, 'Month'), sum(totaltransactionrevenue_noNULLS)
FROM all_sessions
JOIN tottransactionrev ON all_sessions.date = tottransactionrev.date
GROUP BY TO_CHAR(all_sessions.date, 'Month')
ORDER BY sum(totaltransactionrevenue_noNULLS) DESC

Answer: 

"December ","1020420000"
"July     ","716000000"
"January  ","202000000"
"March    ","200000000"
"April    ","196090000"
"February ","152000000"
"October  ","19590000"
"May      ","16990000"
"June     ","0"




Question 2: Rank cities from most profitable to least

SQL Queries:

SELECT city, RANK() OVER(ORDER BY sum(totaltransactionrevenue)DESC)
FROM all_sessions
GROUP BY city
HAVING sum(totaltransactionrevenue)>0


Answer:

"Atlanta","1"
"San Francisco","2"
"Seattle","3"
"Sunnyvale","4"
"Mountain View","5"
"Palo Alto","6"
"Chicago","7"
"New York","8"




Question 3: Does the cost of a product affect the likelyhood of it being purchased?

SQL Queries:

SELECT v2productname, productprice, total_ordered
FROM all_sessions
JOIN sales_by_sku USING(productsku)
Order By productprice DESC

Answer:

Based on the query above it would apear that lower cost items are not as often bought as higher priced items.



Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
