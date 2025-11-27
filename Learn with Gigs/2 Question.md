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

Use Folder load if you expect new files to be added regularly — Power BI will automatically include them after refresh.


4. We want a default selection in a Power BI visual (or measure) where:
By default, the measure shows CBS Enterprises sales
If the user selects a different customer, it shows sales for the selected customer


Customer_Sales :=
VAR SelectedCustomer = SELECTEDVALUE(Customers[CustomerName], "CBS Enterprises")
RETURN
CALCULATE(
    SUM(Sales[Amount]),
    Customers[CustomerName] = SelectedCustomer
)


5. How to calculate working days between two dates

WorkingDays =
VAR StartDate = Sales[StartDate]
VAR EndDate = Sales[EndDate]
RETURN
CALCULATE(
    COUNTROWS(
        FILTER(
            CALENDAR(StartDate, EndDate),
            WEEKDAY([Date], 2) < 6   -- 2: Monday=1, Sunday=7, so <6 = Mon-Fri
        )
    )
)

6. How to get rank by customer?

Rankbycustomer = RANKX(ALLSELECTED('Sales Register'[Customer Name]), [CY Sales],, DESC,Dense)


7. How to get sales of Top N customer?

Top N Values =
VAR selected_top = SELECTEDVALUE('Top'[top], "Top 5")   -- default Top 5
VAR TopN =
    SWITCH(
        selected_top,
        "Top 3", 3,
        "Top 5", 5,
        "Top 10", 10,
        5
    )
RETURN
IF([Rankbycustomer] <= TopN, [CY Sales])


8. How to caclculate YTD Sales (for fiscal year apr-mar) using TOTALYTD()?

Sales YTD =
TOTALYTD(
    [CY Sales],
    'Date'[Date],
    "03/31"          -- fiscal year end
)

9. How to caclculate YTD Sales (for fiscal year apr-mar) using DATESYTD()?

Sales YTD =
CALCULATE(
    [CY Sales],
    DATESYTD('Date'[Date], "03/31")
)





```
