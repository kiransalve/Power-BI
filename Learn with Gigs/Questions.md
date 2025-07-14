What is difference between Append and Merge Query?

Appending, stacks data from multiple tables vertically, adding rows to an existing table
Merging combines data from different tables based on a shared column, creating new columns in the resulting table. 

S – Situation:

In my Power BI reporting work, Once I had multiple monthly Excel files that has hq wise weekly sales planing given by sales executive and

T – Task:

I need to stack all files into one so that we can analyse that planing vs actual sales and also needed to combine zone and region also 

A – Action

So that time I copy the folder path where all excel files are kept and then import that folder in power BI,
I have extracted data from content column using power query, like making custom column and giving M-Code - Excel.Workbook([Content]) and then expanded tables.
also i have merge region and zone from master data, HQ as column column - Here I have used default Left Outer join

Left Outer Join means all from First, matching from second
Right Outer Join means all from second, matching from first 
full outer join means all from both
inner join means only matching

R – Result:
Using this I created dynamically consolidated sales plan tracker that updates automatically when new month added.




