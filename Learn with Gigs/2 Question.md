```

1. How to connect two discnnected table - one table have city nam and amount and other have years 2012, 2013, i want for every row of table 1 there will be two rows for 2012 and 2013

You want to create a cross join in Power BI so that every row from Table1 is repeated for each year in Table2. This is common when you have a “fact table” with values (City, Amount) and a “dimension table” with years.

Using DAX (Calculated Table)

CityAmount_Years =
CROSSJOIN('CityAmount', 'Years')

CROSSJOIN creates all possible combinations of rows from both tables.

Using Power Query (Merge Queries → Full Outer / Custom)

Go to Home → Merge Queries → Merge as New.

Select Table1 as first, Table2 as second.

Don’t select any matching columns.

Choose Join Kind = Full Outer.

Expand the second table → you get all combinations.

(Power Query doesn’t have a direct “Cross Join” button, but you can achieve it by adding an Index column to both tables and merging on 1 = 1.)


2. How to get sales of material containing "Bio"

Method 1

Sales_KemBio =
CALCULATE(
    SUM(Sales[Amount]),
    SEARCH("kem bio", Sales[Material], 1, 0) > 0
)

Method 2

Sales_KemBio =
CALCULATE(
    SUM(Sales[Amount]),
    CONTAINSSTRING(Sales[Material], "kem bio")
)


3. How to append multiple file with same columns


Load all files from a folder

Go to Home → Get Data → Folder.

Browse to the folder containing all your files.

Click Combine & Transform Data.

Power Query will automatically combine files with the same structure.

Power BI automatically creates a sample file and appends the rest.


```
