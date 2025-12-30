In my sales project, I created and automated Power BI dashboards to track sales team performance against targets.
The data covered 50+ headquarters, around 300 SKUs, 3 divisions, and 4 zones across India.

The main goal was to give Sales Managers and the VP a clear and real-time view of sales trends, key KPIs, and low-performing regions and products.

I connected Power BI to MySQL for all divisions.
I designed a star schema with three fact tables: Sales, Collection, and Budget, and dimension tables for Product, Customer, Region, and Date to improve performance.

I created important DAX measures like Total Sales, Target, YTD Sales, YTD Target, and Year-over-Year Growth.
I also built KPIs such as target achievement %, surplus or deficit vs target, and last year variance.

For visuals, I used:

KPI cards for key numbers

Line charts for monthly trends

Bar charts to compare last year sales, this year sales, and budget month-wise

I used drill-through to see detailed customer and product contribution, bookmarks to switch between achievement and growth views, and tooltips to show YTD sales for each month.

I also implemented row-level security, so divisional managers could see only their own division data.

As a result, manual reporting time reduced by more than 2 hours daily, and leadership could take better decisions like planning tour programs or stopping low-performing products.

The biggest challenge was handling large data and performance, which I solved by using good data modeling and optimized DAX.
