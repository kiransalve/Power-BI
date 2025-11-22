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

9. What is Bi-Directional Filtering in Power BI?

Bi-directional filtering means that filters flow in both directions between two related tables — from Table A to B and from Table B to A.

Let’s say you have Customer Table and Sales Table with one to many relationship

By default, filter flows from Customer to Sales (one-directional), meaning if you select a customer, you see their sales.

But with bi-directional filtering, you can also: Filter Customers based on sales

example only show customers who made purchases in June

https://www.youtube.com/watch?v=wMppYKKn-rc

10. What are Relationship Modifiers in Power BI (DAX)?

Relationship Modifiers are DAX functions used to override or modify the default relationships and filter behavior between tables in Power BI.

USERELATIONSHIP() - Use an inactive relationship in your calculation

Example: You have two date columns (Invoice Date & Payment Date), and only one active relationship

Total Payment = CALCULATE(SUM(Sales[Amount]), USERELATIONSHIP(Sales[Payment Date], Calendar[Date]))

CROSSFILTER() - Change the filter direction (single or both) in a specific measure

Sales Both = CALCULATE(SUM(Sales[Amount]), CROSSFILTER(Sales[ProductID], Product[ProductID], BOTH))

REMOVEFILTERS() or ALL() - Ignore filters from a related table 

Total Sales All Regions = CALCULATE(SUM(Sales[Amount]), REMOVEFILTERS(Region))

TREATAS() - Apply a custom filter between unrelated tables

CALCULATE(SUM(Sales[Amount]), TREATAS(VALUES(Target[ProductID]), Sales[ProductID]))

11. Difference between Removefilter() and all()?

In DAX, both REMOVEFILTERS() and ALL() are used to remove filters from a table or column, but they serve slightly different purposes.

REMOVEFILTERS() is used purely to clear filters from the specified column or table in the current context. 
It doesn’t return any data — it simply removes the filter effect, which is helpful when we want to ignore slicers or visual filters temporarily.

On the other hand, ALL() not only removes filters but also returns the entire table or column, making it ideal for calculations like percent of total, ranks, and grand totals, where we need to consider the full dataset regardless of current filters.

12. What are Fact and Dimension Tables in Power BI or Data Modeling?

A Fact Table contains quantitative, measurable data — like sales, revenue, profit, or quantities. These tables are usually large and store transaction-level or aggregated numeric values.

It typically includes foreign keys that link to dimension tables, along with metrics like:

Sales Amount, Quantity ,Cost,Discount

A Dimension Table contains descriptive, categorical data — like names, regions, product categories, customer segments, etc. These tables are smaller and help provide context to the facts.

They usually have a primary key that connects to the fact table and are used for filtering, grouping, or slicing data.

13. What is the difference between Star Schema and Snowflake Schema?

In Power BI and data warehousing, Star Schema and Snowflake Schema are two popular ways of organizing data models. They define how fact and dimension tables are structured and related.

In a star schema, we have a central fact table directly connected to dimension tables. The dimensions are usually denormalized, meaning all descriptive fields are in one single table per dimension.

In a snowflake schema, dimensions are normalized — meaning they are broken into multiple related tables. For example, a Product dimension may be split into Product, Category, and Subcategory.

14. What is Composite Model in Power BI?

A Composite Model in Power BI allows you to combine multiple data sources in the same data model, using both:

Import mode (faster, cached in memory)

DirectQuery mode (real-time connection to database)

Example

Let’s say I’m building a sales dashboard:

I import Product and Customer tables (static data)

I use DirectQuery for the Sales Fact table (which updates frequently)

Power BI lets me model them together using Composite Model

