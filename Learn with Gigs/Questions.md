1. What is difference between Append and Merge Query?

Append, Combine rows of two or more tables vertically, When all tables have same columns

Merge, Combines data from two tables based on a common column, creating new columns in the resulting table. 

Kind of Join :

a. Left Outer Join means all from First, matching from second

b. Right Outer Join means all from second, matching from first 

c. Full outer join means all from both - produce null if no match found

d. Inner join means only matching

e. Left Anti

f. Right Anti

g. Anti Inner

https://www.youtube.com/watch?v=77POcNaCrcI

2. What is Query Folding?

Query folding is the process of pushing data transformation steps back to the data source for execution, like filters, joins, grouping

We can check this by right-clicking a step in Power Query and selecting "View Native Query" — if it's greyed out, folding is not happening.

Example: Relational Database SQL Server, MySQL, Oracle, SAP Hana etc.

Database engines are much faster than Power BI at: Filtering, Group By, Joins, Selecting columns, Sorting, Removing duplicates

If folding happens → database does the processing → Power BI receives only the final, small result.

It helps Faster refresh, Less RAM used, Better performance, Less load on Power BI


3. What is difference between copy and reference?
   
Copy Query :

Creates a completely separate copy of the original query.

Changes in the original query will not affect the copied one.

Reference Query :

Creates a linked version of the original query.

If you change the original query, the referenced one will also reflect those changes.

4. What is M-Code?

M Code is the formula language used in Power Query to clean and transform data before loading it into your model.

It’s called M because it was originally named “Mashup” language.

It's functional and case sensitive language

Whenever we click: Remove Columns, Filter Rows, Merge Queries, Group By, Change Types Power BI automatically writes M code in the background.

We can see it in: Home → Advanced Editor

5. What is a Parameter in Power BI?
   
A parameter is a user-defined value that you can use to make your queries dynamic and flexible in Power BI.
We can used it values like file paths, date ranges, thresholds, or region names.

6. What is Incremental Refresh in Power BI?
   
Incremental Refresh means Power BI refreshes only new or changed data, instead of loading the entire dataset every time.

It is useful because faster refresh times, less memory and CPU uses and ideal for large dataset

Let’s say you have 5 years of sales data: The first time, Power BI loads all 5 years

Next time, it only refreshes the latest 1 month (or as defined) instead of reloading everything

Incremental Refresh requires:

It need Power BI Pro or premium per user licence 

Date column

RangeStart & RangeEnd parameters

https://www.youtube.com/watch?v=fJ4LQ28-68k

7. What types of relationships are there?

-> One-to-Many (1:*) – One value in Table A matches many in Table B.

Customer Table → has one record per customer and Sales Table → has multiple sales for each customer

This is used when a dimension table (like Product, Region, Customer) connects to a fact table (like Sales).

Power BI automatically detects this if columns name and data types are same in both tables

-> Many-to-One (*:1) – Same as Above - Technically same as One-to-Many but direction is reversed.

-> Many-to-Many (:) - Both tables have repeating values, and no unique key

-> One-to-One (1:1) - Both tables have unique values in the join column.

8. How to handle many to many?

We can use bridge table with distinct values

Create two One-to-Many relationships:

Bridge → Sales Table

Bridge → Target Table

Now both tables are indirectly connected through the bridge.

https://www.youtube.com/watch?v=BbKQ5eWjE-Y

9. What is Bi-Directional Filtering in Power BI? and how to resolve it?

Bi-directional filtering means that filters flow in both directions between two related tables — from Table A to B and from Table B to A.

In a normal one-to-many relationship, filters go:

Dimension table → Fact table

Example: If you filter Product Category, the visuals that use Sales get filtered.

But filtering Sales does NOT filter the Product table.

When you enable bi-directional filtering, the filter travels:

Dimension → Fact and Fact → Dimension

Makes relationships behave like both tables are affecting each other.

Bi-directional filtering can create:

Ambiguous relationships

Circular relationships

https://www.youtube.com/watch?v=wMppYKKn-rc

Resolving :

https://www.youtube.com/watch?v=rOGjSt4AVts

If you only need cross-filtering in a single measure, NOT the whole model:

Total Sales by Product :=
CALCULATE(
    SUM(Sales[Amount]),
    CROSSFILTER(Product[ProductID], Sales[ProductID], BOTH)
)

Filters apply both ways only inside this measure

10. What are Relationship Modifiers in Power BI (DAX)?

Relationship Modifiers are DAX functions used to override or modify the default relationships and filter behavior between tables in Power BI.

USERELATIONSHIP() - Use an inactive relationship in your calculation

Example: You have two date columns (Invoice Date & Payment Date), and only one active relationship

Total Payment = CALCULATE(SUM(Sales[Amount]), USERELATIONSHIP(Sales[Payment Date], Calendar[Date]))

https://www.youtube.com/watch?v=W4Vl6ox_BpM

CROSSFILTER() - Change the filter direction (single or both) in a specific measure

Sales Both = CALCULATE(SUM(Sales[Amount]), CROSSFILTER(Sales[ProductID], Product[ProductID], BOTH))

https://www.youtube.com/watch?v=KPEiLH5J-Cw



ALL() - Returns all rows in a table, or all values in column ignoring any filter that might have been applied

REMOVEFILTERS() - Removes filters from a column or table similar to ALL, but does not return a table, it only clears filters.

https://www.youtube.com/watch?v=XsGPPWxOIJc



TREATAS() is used to apply the values of one column (a table expression) as filters on another column—even if there is no relationship between the tables.

https://www.youtube.com/watch?v=PJCrLxdY-gE

give example of date and buget table (with month level)

11. Difference between Removefilter() and all()?

In DAX, both REMOVEFILTERS() and ALL() are used to remove filters from a table or column, but they serve slightly different purposes.

REMOVEFILTERS() is used purely to clear filters from the specified column or table in the current context. 

It doesn’t return any data — it simply removes the filter effect, which is helpful when we want to ignore slicers or visual filters temporarily.

On the other hand, ALL() not only removes filters but also returns the entire table or column, making it ideal for calculations like percent of total, ranks, and grand totals, where we need to consider the full dataset regardless of current filters.

12. What are Fact and Dimension Tables in Power BI or Data Modeling?

A Fact Table contains quantitative, measurable data — like sales, revenue, profit, or quantities. 

These tables are usually large and store transaction-level or aggregated numeric values.

It typically includes foreign keys that link to dimension tables, along with metrics like:

Sales Amount, Quantity ,Cost,Discount

types = Transaction, Snapshot, Accumulating Snapshot, Factless, and Aggregate.

A Dimension Table contains descriptive, categorical data — like names, regions, product categories, customer segments, etc. 

These tables are smaller and help provide context to the facts.

They usually have a primary key that connects to the fact table and are used for filtering, grouping, or slicing data.

types = Conformed, Role-Playing, SCD Types 1–2–3, Junk, Degenerate, Outrigger, and Hierarchical dimensions.

https://www.youtube.com/watch?v=_0IdAb9Z5n4

13. What is the difference between Star Schema and Snowflake Schema?

In a star schema, we have a central fact table directly connected to dimension tables and no dim table connected to other dim table.

The dimensions are usually denormalized, meaning all descriptive fields are in one single table per dimension.

In a snowflake schema, dimensions are normalized — meaning they are broken into multiple related tables. 

here one dim table connected to multiple dim tables.

For example, a Product dimension may be split into Product, Category, and Subcategory.


14. What is Composite Model in Power BI?

A Composite Model in Power BI allows you to combine multiple data sources in the same data model, using both:

Import mode (faster, cached in memory)

DirectQuery mode (real-time connection to database)

Example

Let’s say we are building a sales dashboard:

we import Product and Customer tables (static data)

We use DirectQuery for the Sales Fact table (which updates frequently)

Power BI lets me model them together using Composite Model

https://www.youtube.com/watch?v=sq5kY5WPq0g

15. import and direct query in power BI?

Import mode -

Power BI imports a copy of the data into its in-memory engine (VertiPaq).

Data is stored inside the .pbix file

there is 1 GB of limit for the data we want to publish to power bi service

it has fast performance than any other mode.

Disadvantages - 

Large dataset → .pbix becomes heavy

Data not real-time

Refresh limits in Service (for power bi pro - 8 schedule refresh per day and refresh must complete within 2 hours and for premium - 48 schedule refresh per day)

Power BI does NOT store data.

Direct Query Mode - 

Power BI does not store data. Every visual sends a live SQL query to the database

the data will be pulled by query to database with every user interaction

The data view section removed from report.

no limit on data as we have "live" data

has performance issue 

limited use of DAX function - 

Time Intelligence Function - TOTALYTD, TOTALMTD, TOTALQTD

SAMEPERIODLASTYEAR

DATESYTD, DATESMTD

https://www.youtube.com/watch?v=J_keoBQ5EGk

16. What is bookmarks in Power BI?

In Power BI, a bookmark is essentially a snapshot of the current state of a report page. It captures things like:

Filters and slicers applied

Selected visuals

Sort order

Visibility of visuals (show/hide)

Drill state (expanded/collapsed visuals)

We can save this state and later return to it with a click. Bookmarks are mainly used for:

Navigation: Creating buttons that take users to different views or report pages.

Storytelling / presentations: Showing different insights step by step.

Switching views: For example, switching between a detailed view and a summary view.

Dynamic visuals: Toggling visual elements like charts or tables on/off without creating separate pages.

https://www.youtube.com/watch?v=1RfJOlaqSUw

17. Drill Throgh

Drillthrough lets the user select a value (for example, a customer, product, region, or date) → right-click → Drillthrough → Detail Page, and the destination page automatically filters to show details about that selected item.

Typical drillthrough scenarios:

View Customer details from a summary sales page

View Product-level analysis from a category-level report

View Order details from aggregated sales

Jump into Year → Quarter → Month → Day analysis

https://www.youtube.com/watch?v=k-uWcjbLv0E

18. Whatif parameter?

A What-If Parameter lets users increase/decrease a value and see changes in dashboards.

When you create a What-If parameter, Power BI automatically creates:

A table (e.g., Discount Table with values 0% to 20%)

A slicer

A measure (the selected value)

used in forecasting and scenario testing

https://www.youtube.com/watch?v=28PDdA3SROU&t=134s

19. Tell few scenarios where waterfall charts is useful?

To plot annual profit by showingv various sources of revenue and arrive at the total profit or loss

20. What are Tooltip and explain its types?

A Tooltip in Power BI is a small pop-up that appears when you hover over a visual.

It shows additional information without cluttering the main report.

Types :

1. Default Tooltip (Auto Tooltip)

Automatically provided by Power BI

Shows basic information like category, value, date, etc

You don't need to create anything — it appears automatically.

2. Report Page Tooltip (Custom Tooltip Page)

You manually design a separate tooltip report page.

Highly customizable

Can show charts, KPIs, trends

3. Visual-Level Tooltip Fields

On any visual, you can drag extra fields into the Tooltip area (in Fields pane).

Power BI will show these fields inside the tooltip when hovering.

21. What is Decomposition Tree?

A Decomposition Tree is an AI-powered Power BI visual used to break down a metric step-by-step across different dimensions to understand why a value is high/low.

It allows users to drill down interactively and see contribution at each level.

Use it when you want to answer questions like:

Why is sales low this month?

Which region → segment → product contributed most to profit?

Which store is causing the highest return rate?

A decomposition tree helps break down a metric into multiple levels to identify key contributors. It supports AI splits, dynamic drill-down, and is mainly used for root-cause analysis. It's ideal when users want flexibility to explore data in any order instead of a fixed hierarchy.

https://www.youtube.com/watch?v=Nm5ImMmi-y8

22. What is RLS ( Row level Security )?

RLS is a way to restric the data on the basis of logged in user 

RLS allows you to filter rows of data for different users.

Example:

Sales Manager of North Region sees only North data

Sales Manager of South Region sees only South data

It protect sensetive data

Gives personalized views without creating separate reports

We can have two types of RLS static and dynamic

Userprinciplename() : shows the username of currently logged in user in both Power bi dekstop and servive

https://www.youtube.com/watch?v=r5XCpeQxXl4

Username() : shows dekstop name in power bi dekstop and username in power bi service.

23. Difference between power bi pro and premium licence?

Power BI Pro - User based license basically for developers can leverage all content creation and interaction features

you can share dashboard/report to only pro user

Power BI Premium - Licesing based on didicated capacity content can be viewed without additional per user total

it has two types - 

premium per user - per user license but wih premium users

premium by capacity - you pay for dedicated capacity and then many users (free also) can consume content till capacity


