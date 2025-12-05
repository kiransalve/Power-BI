I have one sheet with Customer, Product, HQ, Sales Register, Sales Budget in excel sheet

need to convert it separate csv file

```

import pandas as pd
excel_file = "CBL-Sales Register.xlsx"

# Read File
xls = pd.ExcelFile(excel_file)

# Loop through all sheet names
for sheet_name in xls.sheet_names:
    # read each sheet into dataframe
    df = pd.read_excel(excel_file, sheet_name = sheet_name)

    # save the dataframe as CSV
    csv_file = f"{sheet_name}.csv"
    df.to_csv(csv_file, index=False)
    print(f"Saved {csv_file}")
    
```
