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

# 3. Sales using calculatetable

```
Sales using calculate table =
SUMX (
    CALCULATETABLE (
        'Sales Register',
        'Sales Register'[Item Code] = 10063077
    ),
    'Sales Register'[AMOUNT]
)
```

# 4. Total revenue with inactive relationship

```
Rev = 
    CALCULATE(
        [Total Sales],
        USERELATIONSHIP('Date'[Date], 'Sales Register'[Invoice Date]))
```

# 5. Uncommon from left table

```
Table = EXCEPT(Left_table, Right_table)

```

# 6. Default selection

```
Default = 
var a = SELECTEDVALUE(HQ[Business],"Dairy")
var b = CALCULATE([Total Sales], HQ[Business] = a)
RETURN b
```
