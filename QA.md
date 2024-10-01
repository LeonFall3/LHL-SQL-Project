What are your risk areas? Identify and describe them.
 The largest risk is incorrect or missing data. A number of columns have little to no data in them at all. Other columns have clearly incorrect data, suah as the city and country columns. 

 Another risk is data being in the incorrect format. Productprice for example has 5 zeros at the end of each value. Is this the actual price? is it in cents? 


QA Process:
Describe your QA process and include the SQL queries used to execute it.

Check for duplicates in primary keys
-- checking for duplicate in product SKUs

SELECT COUNT(DISTINCT sku), COUNT(sku) FROM products

Compare values across tables
-- comparing productsku to products.sku across tables

SELECT all_sessions.productsku, sales_by_sku.productsku, sales_report.productsku, products.sku
FROM products
FULL OUTER JOIN all_sessions ON all_sessions.productsku = products.sku
FULL OUTER JOIN sales_by_sku ON sales_by_sku.productsku = products.sku
FULL OUTER JOIN sales_report ON sales_report.productsku = products.sku


SELECT sku, productsku
FROM products
FULL OUTER JOIN all_sessions ON sku = productsku



-- compare stock level across products and sales_report

SELECT products.stocklevel, sales_report.stocklevel
FROM products
FULL OUTER JOIN sales_report USING(stocklevel)