
#1. How to handle Power bi report while migration from tally to ms Navision?

In my first company MRK Healthcare, During the migration from Tally to Microsoft Dynamics NAV, my role in Power BI was focused on ensuring data continuity, accuracy, and minimal business disruption

First, I worked closely with the data/ERP team to understand schema differences between Tally and NAV—especially around key tables like ledger, invoices, and inventory. Since both systems had different data structures and naming conventions, I created a mapping logic document to align old fields with the new ones.

Then, in Power BI:

I updated data sources and queries to connect with NAV instead of Tally.
Performed data transformation in Power Query to standardize formats (dates, account hierarchy, etc.).
Ensured historical data consistency by combining Tally and NAV data where required for continuous reporting.

For validation:

I did reconciliation checks between old Tally reports and new NAV-based reports.
Created temporary comparison dashboards to identify mismatches.
Worked with finance stakeholders to validate KPIs like revenue, expenses, and profit.

I also optimized the model:

Rebuilt relationships where needed due to schema change
Updated DAX measures to align with new data logic
Ensured performance was maintained despite increased data volume

Finally, I handled user transition by:

Updating report visuals if business logic changed
Communicating changes clearly to stakeholders
Ensuring reports remained stable post-migration

#2. What is DAX?

DAX means Data Analysis Expression that used to create dynamic calculation as per business logic.

DAX used to create Calculated measure, calculated column and calculated tables.

Measures are the most commonly used because measure compute results dynamically based on the current filter context of a report
For example, a simple measure like Total Sales can be written using the SUM function, but its output changes automatically depending on how the user filter the data by month/year, geography like zone/region/headquarters, product category, or any other dimension.

I try to avoid unnecessary calculated columns when measures could achieve the same result dynamically
like suppose their is qty and rate column and we need total sales we can use sumx function which do calculation row by row and give total sales without creating separate column, it called context transition where the dax convert row context to filter context.

Their are Aggregation dax function like sum, average, max, min, count.
then their are iterator dax function also called x function like sumx and averagex
their are filter function to slice the data like calculate, filter, all, allselected, allexcept 
their are time intelligence function which work only on import mode and required separate date table like totalytd, dateadd, sameperiodlastyear, parallelperiod, datesinperiod, datediff etc.
their are relationship dax function also like related, Userelationship, lookup, treatas
