In my current role, I worked on a sales project.

I built dashboards to track sales versus targets for the sales team. 

The data has sales transaction over 50 headquarters and 3 divisions and has about 1 Lakhs rows.

The main purpose was to help sales managers and the VP quickly understand how sales are hapening, what is growth, which HQs are achiveing there targets and which are underperformer and which region have most deficit etc.

The data is in MySQL, so I connected Power BI to MySQL and designed star schema using fact tables like sales register, budget and dimmension table like customer, product, date.

To Create fiscal date table i created month index that map april as 1st month and march as last month.

for budget table, budget are hq wise and months are in separate columns, so i have used unpivot so all budget comes in one single rows.

I have created DAX measures like Total sales, budget and YTD sales, budget also year on year growth 

I also made KPIs like ach %, variance from last year, surplus or deficit from target.

For visuals I used KPI cards, line chart for monthly trend, and bar chart to compare ly sales, cy sales and targets.

I have added drill throgh so users can check customer or product details.

I also applied dynamic row-level security, so each divisional manager can see only their own division data

Because of this dashboard, manual reporting reduced by more than 2 hours every day, and management could take faster decisions, like planning field visits or stopping low-performing products.
