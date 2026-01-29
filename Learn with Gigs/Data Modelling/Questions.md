# 1. Best Practice to Optimize your Data Model in Both Direct Query and Import Mode?

In many of my projects, especially enterprise-level ones, the real problem I faced was that dashboards were slow, refresh was taking hours,
and business users were complaining that filters and visuals were lagging—particularly when data size crossed 20–30 million rows 
and we had to support both Import and DirectQuery models.

So my first step was always to understand the business usage—what visuals are actually used, 
which columns are filtered, and what level of granularity is required. Many times, 
we were loading data that no one was even using.

Step two was redesigning the data model. 
I moved from a snowflake-heavy model to a clean star schema—one fact table with clear dimension tables. 
This immediately reduced relationship complexity and improved query performance in both Import and DirectQuery.

Step three was data reduction at the source. 
I removed unused columns, filtered historical data, and pre-aggregated where possible. 
In Import mode, this reduced model size and refresh time. In DirectQuery, it reduced the number of SQL queries hitting the source system.

Step four was choosing the right optimization technique per mode.
For Import mode, I applied:

Correct data types

Low-cardinality columns

Incremental refresh for large fact tables

For DirectQuery, I kept:

Relationships simple

Visuals lightweight

DAX measures optimized, avoiding heavy iterators

Next, I ensured Power Query transformations supported query folding, so the heavy work stayed in the database, not in Power BI.

Finally, I validated everything using Performance Analyzer and DAX Studio and fine-tuned measures based on real user behavior.

By following these steps, I converted slow, unstable reports into fast, scalable dashboards. 

This structured approach is something I’ve consistently applied across end-to-end projects, and it has worked reliably in both Import and DirectQuery scenarios.

