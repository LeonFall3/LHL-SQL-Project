What issues will you address by cleaning the data?

Missing city values


    SELECT CASE
            WHEN city IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE city
        END
    FROM all_sessions

---

Missing country values


    SELECT CASE
            WHEN country IN ('(not set)', 'not available in demo dataset') THEN NULL
            ELSE country
        END
    FROM all_sessions

---

Remove NULLs from productquantity

    SELECT productquantity
    FROM all_sessions
    WHERE productquantity IS NOT NULL

---

Removing all rows that don't have a product category

SELECT v2productname, v2productcategory
FROM all_sessions
WHERE v2productcategory NOT IN ('(not set)','${escCatTitle}')

---

CTE to remove all NULLs from cities and catergories (later also used for countries and catergories by swapping 'city' to 'countyry)

--- remove nulls from cities and categories
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
)

---




