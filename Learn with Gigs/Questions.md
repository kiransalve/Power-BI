1. What is difference between Append and Merge Query?

Appending, stacks data from multiple tables vertically, adding rows to an existing table
Merging combines data from different tables based on a shared column, creating new columns in the resulting table. 

In my Power BI reporting work, Once I had multiple monthly Excel files that has hq wise weekly sales planing given by sales executive and

I need to stack all files into one so that we can analyse that planing vs actual sales and also needed to combine zone and region also 

So that time I copy the folder path where all excel files are kept and then import that folder in power BI,
I have extracted data from content column using power query, like making custom column and giving M-Code - Excel.Workbook([Content]) and then expanded tables.
also i have merge region and zone from master data, HQ as column column - Here I have used default Left Outer join

Left Outer Join means all from First, matching from second
Right Outer Join means all from second, matching from first 
full outer join means all from both
inner join means only matching

Using this I created dynamically consolidated sales plan tracker that updates automatically when new month added.

2. What is Query Folding?

Query folding is the process of pushing data transformation steps back to the data source for execution, like filters, joins, grouping
we can check this by right-clicking a step in Power Query and selecting "View Native Query" — if it's greyed out, folding is not happening.

3. What is difference between copy and reference?
   
Copy Query
Creates a completely separate copy of the original query.
Changes in the original query will not affect the copied one.

Reference Query
Creates a linked version of the original query.
If you change the original query, the referenced one will also reflect those changes.

4. What is M-Code?
M Code is the formula language used in Power Query (in Power BI, Excel, etc.) to clean, transform, and shape data before loading it into your model.
It’s called M because it was originally named “Mashup” language.
It's functional and case sensitive language

5. What is a Parameter in Power BI?
A parameter is a user-defined value that you can use to make your queries dynamic and flexible in Power BI.
We can used it values like file paths, date ranges, thresholds, or region names.

6. What is Incremental Refresh in Power BI?
Incremental Refresh means Power BI refreshes only new or changed data, instead of loading the entire dataset every time.
It is useful because faster refresh times, less memory and CPU uses and ideal for large dataset

Let’s say you have 5 years of sales data: The first time, Power BI loads all 5 years
Next time, it only refreshes the latest 1 month (or as defined) instead of reloading everything



