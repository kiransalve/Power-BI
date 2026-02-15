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

