
In my sales project, I built and automated Power BI dashboards to track sales team performance against targets across 50+ headquarters and about 300 SKU all over India. There are 3 divisions with 4 zones.

The goal was to provide Sales Managers and the VP with real-time visibility into trends, KPIs, and underperforming regions and products

I connected data from MySQL Server for all division. I designed a star schema with a three fact table like sales register, collection and budget and dimension tables for Products, Customers, Region and Date to optimize performance.

I created key DAX measures such as Total Sales, Total Target, YTD Sales and Target, and Year-over-Year growth.

Also some KPI like Variance from last year, deficit/surplus from target, ach% etc.

Visualizations included cards for KPIs, line chart for monthly trend, and bar chart to highlight monthwise last year sales, this year sales and budget.

Used Drill-through to get detailed view of Customer and Product contribution, bookmarks for Achivement and Growth chart toggle, and tooltips to know YTD Sales of each month

I also implemented row-level security so divisional managers could see only their respective division sales. 

As a result, manual reporting time was reduced by over 2 hours per day, and leadership could proactively make data-driven decisions, like planing tour plan or discontinuing low-performing products.

The biggest challenge was handling performance with such large datasets, which I solved through optimized data modeling and efficient DAX measures
