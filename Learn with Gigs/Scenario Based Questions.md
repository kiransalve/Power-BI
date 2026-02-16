# 1. Calculate Last N Days Sales

```
Last 7 days sales =
CALCULATE(
    [Total Sales],
    DATESINPERIOD(
        'Date'[Date],
        MAX('Date'[Date]),
        -7,
        DAY
    )
)
```

# 2. Calculate Top N products

```
TopNProduct =
VAR Product_Rank = RANKX(
    ALL('Sales Register'[Item Description]),
    [Total Sales],,
    desc,
    Dense)

RETURN
IF(
   Product_Rank <= 5,
     [Total Sales]
   )
```
