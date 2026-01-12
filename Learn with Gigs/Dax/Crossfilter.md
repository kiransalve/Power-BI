```
What is CROSSFILTER?

👉 CROSSFILTER is used to temporarily change the direction of a relationship
👉 It works only inside a measure
👉 It does not change the data model

Normal Relationship (default)
Product  ───▶  Sales  (one direction)

Product filters Sales
Sales cannot filter Product

So, Product filter → Sales works
Sales filter → Product ❌ doesn’t work


Example -

Count Products Bought by Each Customer

Syantax -

CROSSFILTER (
    Column1,
    Column2,
    Direction
)

Column1 & Column2 → must be relationship columns

Direction:

BOTH → filter both ways
NONE → ignore relationship
ONEWAY → single direction


Products Bought =
CALCULATE(
    DISTINCTCOUNT ( 'Product'[Item Description] ),
    CROSSFILTER(
        'Sales Register'[Customer No.],
        Customer[Customer No.],
        BOTH
    ),
    CROSSFILTER(
        'Sales Register'[Item Code],
        'Product'[Item Code],
        BOTH
    )
)

```
