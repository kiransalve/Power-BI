```

1. Differerece between primary key and foreign key ?

Primary Key

A Primary Key uniquely identifies each record (row) in a table.

Values must be unique (no duplicate allowed)

Cannot be NULL

Only one primary key per table (but can consist of multiple columns — Composite Key)


Foreign Key

A Foreign Key creates a relationship between two tables.

It refers to a Primary Key in another table.

Can have duplicate values

Can be NULL (unless restricted)

Used to maintain referential integrity


2. Difference between UNION and UNION ALL?

UNION

Combines the result sets of two queries

Removes duplicate rows

Performs sorting/distinct check, so it’s slower compared to UNION ALL

SELECT City FROM Customers
UNION
SELECT City FROM Vendors;

If both tables have the same city name, it will appear only once.


UNION ALL

Combines result sets from two queries

Does NOT remove duplicates

Faster because no comparison or sorting

SELECT City FROM Customers
UNION ALL
SELECT City FROM Vendors;

If the same city appears in both tables, it will show multiple times.

UNION removes duplicate rows and is slower, while UNION ALL returns all rows including duplicates and is faster.


3. Use of Having"

The HAVING clause in SQL is used to filter records after aggregation (after GROUP BY).
It works like a WHERE clause but is applied on grouped results — not on individual rows.

WHERE filters rows before grouping, HAVING filters groups after aggregation


4. Query to Delete Duplicate Records (Keeping Only One)

WITH CTE AS (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY EmpID, Name, City ORDER BY EmpID) AS rn
    FROM Employees
)
DELETE FROM CTE WHERE rn > 1;

ROW_NUMBER() assigns sequence numbers to duplicate rows.

rn = 1 → first/unique record (kept)

rn > 1 → duplicate records (deleted) 


5. Second highest salasy using subqueries?

SELECT MAX(salary) AS Second_Highest_Salary
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

```
