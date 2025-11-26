```
1. GIve last 3 months sales.
```
Last 3 Month Sale = 
VAR SelectedDate =
    IF(
        ISFILTERED('Date'[Date]),
        MAX('Date'[Date]),               
        CALCULATE(MAX('Sales Register'[Invoice Date]))
    )
RETURN 
CALCULATE(
    [CY Sales],
    DATESINPERIOD(
        'Date'[Date],
        SelectedDate,
        -3,
        MONTH))
```

```
DATESINPERIOD(
    DATE COLUMN TO FILTER,
        DATE WHERE FROM TO START,
        HOW MANY FORWARD/BACKWARD,
        WHAT - DAY/MONTH/YEAR
      )
```

```
DATESINPERIOD(
    DATE TABLE MADHUN JO COLUMN FILTER KARAYACHA TYACHE NAV,
        KONTYA TARKHE PASUN PUDHE KINVA MAGE JAYCH AAHE,
        KITI MAGE KINVA PUDHE JAYCH AAHE,
        KAY FILTER KARAYCH AAHE - DIVAS/MAHINA/VARSH
      )
```
2. GIVE LAST 12 MONTHS ROLLING SALES EXCULDING CURRENT MONTH?

```
Rolling12M_ExclCurrent = 
CALCULATE(
    [CY Sales],
    DATESINPERIOD(
        'Date'[Date],
        EOMONTH(MAX('Date'[Date]), -1),   -- Last date of previous month
        -10,
        MONTH
    )
)

EOMONTH(
    DATE,
    NO. OF MONTH FORWARD/BACKWARD
    )
```

DILELYA TARKHEPASUN PUDHE KINVA MAGE JAUN TYA MAHINYACHA SHEVATCHA DIVAS DETO

3. CAN YOU CALCULATE YTD CALCULATION IN DIRECT QUERY MODE?

DirectQuery does not support many DAX Time-Intelligence functions like:
```
❌ TOTALYTD()
❌ DATESYTD()
❌ PREVIOUSYEAR()
❌ SAMEPERIODLASTYEAR()
```

```
YTD_SALES = 
    CALCULATE(
            [CY SALES],
            FILTER(
                ALL("DATE"),
                "DATE"[DATE] <= MAX("DATE"[DATE]) && 
                YEAR("DATE"[DATE]) = YEAR(MAX("DATE"[DATE]))
            )
        )
```
MTD

```
MTD_Sales_DQ =
CALCULATE(
    [CY Sales],
    FILTER(
        ALL('Date'),
        'Date'[Date] <= MAX('Date'[Date]) &&
        MONTH('Date'[Date]) = MONTH(MAX('Date'[Date])) &&
        YEAR('Date'[Date]) = YEAR(MAX('Date'[Date]))
    )
)
```

3. HOW TO RESTRICT OUR TOTALYTD TO LAST SALES DATE?

```
TotalYTD = 
    var max_selected_date = MAX('Sales Register'[Invoice Date])
    var total_ytd_restricted = TOTALYTD([CY Sales],'Date'[Date], 'Date'[Date] <= max_selected_date)
    RETURN total_ytd_restricted
```

4. HOW TO GET WEEKEND AND WEEKDAYS SALES?

```
Weekday Sales = 
CALCULATE(
    [CY Sales],
    FILTER(
        ALL('Date'),
        WEEKDAY('Date'[Date], 2) <= 5
    )
)
```

```
Weekend Sales = 
CALCULATE(
    [CY Sales],
    FILTER(
        ALL('Date'),
        WEEKDAY('Date'[Date], 2) > 5
    )
)
```

5. Cumalative Sum over the year?

```
Cumulative Sales = 
CALCULATE(
    [CY Sales],
    FILTER(
        ALLSELECTED('Date'[Date]),           -- removes only the date filter within the selected year(s)
        'Date'[Date] <= MAX('Date'[Date])    -- up to current date
    )
)
```

6. Top 5 Product Sales

```
Top 5 Products = 
CALCULATE(
    [CY Sales],
    FILTER(
        VALUES('Sales Register'[Item Description]), // gives unique values
        RANKX('Sales Register', 
        [CY Sales],,
        DESC, 
        Dense) <=5))


```
