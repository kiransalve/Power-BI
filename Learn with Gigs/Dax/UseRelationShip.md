```
What is USERELATIONSHIP?

👉 USERELATIONSHIP is used to activate an inactive relationship
👉 Mostly used when you have multiple date columns
👉 Works only inside CALCULATE
👉 Does not change the model permanently

Example -

You have Order Date and Ship Date
But Power BI allows only one active relationship at a time with Date Table

If you connect sales register to Date Table 

OrderDate ───▶ Date   (Active)
ShipDate  ───▶ Date   (Inactive)

Date slicer filters Order Date, Ship Date is ignored

Syntax - USERELATIONSHIP ( Column1, Column2 )

Columns must be part of an inactive relationship
Used only in CALCULATE

Shipped Sales = 
CALCULATE(
    [Total Sales],
    USERELATIONSHIP('Sales Register'[Ship Date], 'Date'[Date])
    )
```
