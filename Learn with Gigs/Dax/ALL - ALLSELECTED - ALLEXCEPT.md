```**
ALL() removes filters and returns the entire table or column
**

% of Total Sales

Show each product’s % contribution to total sales, ignoring all filters.

% of Total Sales =
DIVIDE (
    [Total Sales],
    CALCULATE ( [Total Sales], ALL ( Sales ) )
)


**ALLSELECTED() - respects slicers, ignores visual level filters
**

1. % of Selected Months (respect slicer)

% of Selected Sales =
      DIVIDE(
        [Total Sales],
        CALCULATE(
            [Total Sales],
            ALLSELECTED('Sales Register')
          )
        )


2. Running Total (respect slicer)

Running Total Sales =
CALCULATE(
  [TOTAL SALES],
  FILTER(
      ALLSELECTED('Date'[Date]),
      Date'[Date] <= MAX ( 'Date'[Date] )
      )
    )


3. Highlight Highest Month (dynamic)

Max Selected Month Sales =
CALCULATE (
    MAXX (
        VALUES ( 'Date'[MonthName] ),
        [Total Sales]
    ),
    ALLSELECTED ( 'Date' )
)

ALLEXCEPT() - respect only given filters

Category Total Sales =
CALCULATE (
    [Total Sales],
    ALLEXCEPT ( Product, Product[Category] )
)


**
ALL → “Ignore everything”
ALLSELECTED → “Respect slicer only”
ALLEXCEPT → “Ignore all except this”
**
```

