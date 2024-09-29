What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

/* Check for duplicates */

--- checking for duplicate in product SKUs

SELECT COUNT(DISTINCT sku), COUNT(sku) FROM products