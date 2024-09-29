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

Remove Nulls from productquantity

    SELECT productquantity
    FROM all_sessions
    WHERE productquantity IS NOT NULL

---

