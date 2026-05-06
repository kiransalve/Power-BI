```
How to handle Power bi report while migration from tally to ms Navision?

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
