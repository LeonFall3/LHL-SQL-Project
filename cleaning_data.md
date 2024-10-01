# Cleaning Data

### Missing city values  
  Setting "empty" values to ``NULL``
```sql
UPDATE all_sessions
Set city = CASE
WHEN city in('(not set)', 'not available in demo dataset') THEN NULL
ELSE city
END
```
  Removing ``NULLs``
```sql  

Delete from all_sessions
Where city IS NULL
```
  

---

### Missing country values

Setting "empty" values to ``NULL``
```sql
UPDATE all_sessions
Set country = CASE
WHEN country in('(not set)', 'not available in demo dataset') THEN NULL
ELSE country
END
```
 Removing ``NULLs``
  ```sql

Delete from all_sessions
Where country IS NULL

  ```
  
  

---

  

### Remove NULLs from ``productquantity``

  ```sql

DELETE FROM all_sessions
WHERE productquantity IS NULL
```
  

---

  

### Removing all rows that don't have a product category

  
```sql
DELETE FROM all_sessions
WHERE v2productcategory IN ('(not set)', '${escCatTitle}')
```
  

---

  
### Incorrect Cities/Countries
Check number of countries which city has.
```sql
SELECT city,
COUNT(DISTINCT country)
FROM all_sessions
GROUP BY city
```

Double check if all those countries have a city by that name. If not update name with correct country.
```sql 

UPDATE all_sessions
SET country = CASE
WHEN city = 'San Francisco' THEN 'United States'
WHEN city = 'Dublin' THEN 'Ireland'
WHEN city = 'Watford' THEN 'United Kingdom'
WHEN city = 'New York' THEN 'United States'
WHEN city = 'Coventry' THEN 'United Kingdom'
WHEN city = 'Mexico City' THEN 'Mexico'
WHEN city = 'Hong Kong' THEN 'Hong Kong'
WHEN city = 'Singapore' THEN 'Singapore'
WHEN city = 'Bangkok' THEN 'Thailand'
WHEN city = 'Los Angeles' THEN 'United States'
WHEN city = 'Yokohama' THEN 'Japan'
WHEN city = 'Amsterdam' THEN 'Netherlands'
WHEN city = 'Istanbul' THEN 'Turkey'
WHEN city = 'London' THEN 'United Kingdom'
WHEN city = 'Mountain View' THEN 'United States'
ELSE country
END;  
```


---

  

### Update ``all_sessions.time`` into seconds for easier reading

  ```sql

ALTER TABLE all_sessions
ALTER COLUMN time TYPE real

  

UPDATE all_sessions
SET time = time / 1000
```
  
  

---

  
  

### Remove all all_sessions.timeonsite with  ``NULLs``

If they spent no time on the site, their info isn't useful.
```sql
DELETE FROM all_sessions
WHERE timeonsite IS NULL
```

---

  

### Remove all ``analytics.timeonsite = NULL``

If they spent no time on the site, their info isn't useful.

  ```sql

DELETE FROM analytics
WHERE timeonsite IS NULL
```
  

---

  

### Remove all sessions without categories  
  
```sql
DELETE FROM all_sessions
WHERE v2productcategory IN ('(not set)', '${escCatTitle}')
```
  

---

  

### Remove ``sales_report.name`` as it is doubling up on info that should only live in the products table

  ```sql
ALTER TABLE sales_report
DROP COLUMN name
```