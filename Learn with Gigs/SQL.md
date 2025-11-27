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

```
