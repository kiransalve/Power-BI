
In my sales project, I built and automated Power BI dashboards to track sales performance against targets across 100+ headquarters and about 700 SKU (stock keeping units) all over India. There are 8 divisions with different segments.

The goal was to provide Sales Managers and the VP with real-time visibility into trends, KPIs, and underperforming regions and products

I connected data from MySQL Server for all division. I designed a star schema with a central sales fact table and dimension tables for Products, Customers, and Date to optimize performance. 

For dimmension tables, I used Import Mode and DirectQuery for live sales data, and implemented incremental refresh to keep the dashboards fast and up-to-date.

I created key DAX measures such as Total Sales, Sales vs. Target, Cumulative Sales, and Year-over-Year growth. 

Visualizations included cards for KPIs, line chart for monthly trend, and bar chart to highlight underperforming regions using conditional formating.

Used Drill-through to get detailed view of Customer and Product contribution, bookmarks for Achivement and Growth chart toggle, and tooltips to know YTD Sales of each month

I also implemented row-level security so divisional managers could see only their respective division sales. 

As a result, manual reporting time was reduced by over 80%, and leadership could proactively make data-driven decisions, like reallocating resources or discontinuing low-performing products.

The biggest challenge was handling performance with such large datasets, which I solved through optimized data modeling and efficient DAX measures
