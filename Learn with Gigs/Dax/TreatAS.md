```
What is TREATAS?

👉 TREATAS is used to apply filters from one table to another table
👉 Even when NO relationship exists
👉 Works inside CALCULATE

We can use TREATAS when Tables are not connected or relationship is inactive and we need to filter the column

Highlight selected month dark and other months light using disconnected table + TREATAS.

1. Create Date table

2. Create disconnected month table

Month Selector =
DISTINCT (
    SELECTCOLUMNS (
        Date,
        "MonthNo", Date[MonthNo],
        "MonthName", Date[MonthName]
    )
)


3. Create base measure

Total Sales = SUM ( Sales[Amount] )

4. Highlighed Month -

Selected Month Sales =
CALCULATE (
    [Total Sales],
    TREATAS (
        VALUES ( 'Month Selector'[MonthNo] ),
        Date[MonthNo]
    )
)

5. Create Color Logic

Month Color =
IF (
    ISBLANK ( [Selected Month Sales] ),
    "#D3D3D3",   -- light color
    "#1F4E79"    -- dark color
)

6. Build Column chart

X-axis → Date[MonthName]
Values → Total Sales

7. Format Color - In Rule choose Month Color

8. Add Month Slicer - Use Month Selector[MonthName]

```
