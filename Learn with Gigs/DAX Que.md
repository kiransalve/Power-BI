1. GIve last 3 months sales.

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

DATESINPERIOD(
    DATE COLUMN TO FILTER,
        DATE WHERE FROM TO START,
        HOW MANY FORWARD/BACKWARD,
        WHAT - DAY/MONTH/YEAR
      )

DATESINPERIOD(
    DATE TABLE MADHUN JO COLUMN FILTER KARAYACHA TYACHE NAV,
        KONTYA TARKHE PASUN PUDHE KINVA MAGE JAYCH AAHE,
        KITI MAGE KINVA PUDHE JAYCH AAHE,
        KAY FILTER KARAYCH AAHE - DIVAS/MAHINA/VARSH
      )
      








        
