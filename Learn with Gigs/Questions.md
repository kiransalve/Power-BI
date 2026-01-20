
# 1. What is difference between Append and Merge Query?

Append will Combine rows of two or more tables vertically, When all tables have same columns, same order and same data types

Append means add rows one below another

We use this when we want to combin multiple months data vertically, like Jan, Feb, Mar till Dec separate sales sheets and when we use append we get one sheet of Jan to Dec sales

To use this we go Transform -> Home -> Append queries 

Merge will Combines data from two tables based on a common column, creating new columns in the resulting table. 

Kind of Join :

a. Left Outer Join means all from First, common from second

b. Right Outer Join means all from second, common from first 

c. Full outer join means all from both - produce null if no match found

d. Inner join means only common

e. Left Anti - uncommon from left

f. Right Anti - uncommon from right

g. Anti Inner - only uncommon

https://www.youtube.com/watch?v=77POcNaCrcI

# 2. What is Query Folding?

Query folding is the process of pushing data transformation steps back to the native data source for execution, like filters, joins, grouping

As you connect to your data source and begin shaping your data, 

Power BI tries to translate each transformation you apply into the source’s native query language, such as SQL

We can check this by right-clicking a step in Power Query and selecting "View Native Query" — if it's greyed out, folding is not happening.

Example: Relational Database that has query engine SQL Server, Snowflake, Amazon Redshift, Google BigQuery, PostgreSQL, and SAP HANA.

Database engines are much faster than Power BI 

If folding happens → database does the processing → Power BI receives only the final, small result.

It helps Faster refresh, Less RAM used, Better performance, Less load on Power BI

Transformation support: Filtering, Group By, Joins, Sorting, Removing duplicates, Renaming header, merging/appending querries from same source

Transformation not support : Adding columns that have complex logic, merge or append querries from different data sources

Query folding is mandatory for incremental refresh.

https://youtu.be/-K8_tARqF0w?si=wqN5T4P5NIr89Bvr

https://www.phdata.io/blog/query-folding-in-power-bi-the-secret-to-faster-data-refresh-performance/


# 3. What is difference between copy and reference?
   
Copy Query :

Creates a completely separate copy of the original query.

Changes in the original query will not affect the copied one.

Reference Query :

Creates a linked version of the original query.

If you change the original query, the referenced one will also reflect those changes.

we have used it to create star schema.  Raw table → cleaned → multiple dimensions/fact tables.

# 4. What is M-Code?

M Code is the formula language used in Power Query to clean and transform data before loading it into your model.

It’s called M because it was originally named “Mashup” language.

It's functional and case sensitive language

Whenever we click: Remove Columns, Filter Rows, Merge Queries, Group By, Change Types Power BI automatically writes M code in the background.

We can see it in: Home → Advanced Editor

M code is used mostly when point-and-click options in Power Query aren’t enough

we use it when we need new columns

# 5. What is a Parameter in Power BI?
   
In Power Query, a Parameter is a dynamic value that can be reused in queries to make your data transformations more flexible, reusable, and user-controlled.

Think of it like a variable that you can change once, and it updates everywhere it’s used.

We can used it values like file paths, date ranges and incremental refresh rules (RangeStart and RangeEnd)

# 6. What is Incremental Refresh in Power BI?
   
Incremental Refresh means Power BI refreshes only new or changed data, instead of loading the entire dataset every time.

It is useful because faster refresh times, less memory and CPU uses and ideal for large dataset

Let’s say you have 5 years of sales data: The first time, Power BI loads all 5 years

Next time, it only refreshes the latest 1 month (or as defined) instead of reloading everything

Incremental Refresh requires:

It need Power BI Pro or premium per user licence 

Date column

RangeStart & RangeEnd parameters

https://www.youtube.com/watch?v=fJ4LQ28-68k

# 7. What types of relationships are there?

-> One-to-Many (1:*) – One value in Table A matches many in Table B.

Customer Table → has one record per customer and Sales Table → has multiple sales for each customer

This is used when a dimension table (like Product, Region, Customer) connects to a fact table (like Sales).

Power BI automatically detects this if columns name and data types are same in both tables

-> Many-to-One (*:1) – Same as Above - Technically same as One-to-Many but direction is reversed.

-> Many-to-Many (:) - Both tables have repeating values, and no unique key

-> One-to-One (1:1) - Both tables have unique values in the join column.


# 8. How to handle many to many?

We can use bridge table with distinct values

Create two One-to-Many relationships:

Bridge → Sales Table

Bridge → Target Table

Now both tables are indirectly connected through the bridge.

https://www.youtube.com/watch?v=BbKQ5eWjE-Y

# 9. What is Bi-Directional Filtering in Power BI? and how to resolve it?

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

```
Count Products Bought by Each Customer

Products Bought = 
CALCULATE(
    DISTINCTCOUNT('Product'[Code]), 
    CROSSFILTER('Sales Register'[Customer No.],Customer[Customer No.], Both),
    CROSSFILTER('Sales Register'[Item Code],'Product'[Code],Both)
    )
```

Filters apply both ways only inside this measure

# 10. What are Relationship Modifiers in Power BI (DAX)?

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

# 11. Difference between Removefilter() and all()?

In DAX, both REMOVEFILTERS() and ALL() are used to remove filters from a table or column, but they serve slightly different purposes.

REMOVEFILTERS() is used purely to clear filters from the specified column or table in the current context. 

It doesn’t return any data — it simply removes the filter effect, which is helpful when we want to ignore slicers or visual filters temporarily.

On the other hand, ALL() not only removes filters but also returns the entire table or column, making it ideal for calculations like percent of total, ranks, and grand totals, where we need to consider the full dataset regardless of current filters.

Example 

```
Total Sales = SUM('Sales'[Amount])

Sales % of Total =
DIVIDE(
    [Total Sales],
    CALCULATE([Total Sales], ALL('Sales'))
)

Sales % of Total (RemoveFilters) =
DIVIDE(
    [Total Sales],
    CALCULATE([Total Sales], REMOVEFILTERS('Sales'))
)


Product Rank =
RANKX(
    ALL('Product'[Product Name]),
    [Total Sales],
    ,
    DESC
)

```

# 12. What are Fact and Dimension Tables in Power BI or Data Modeling?

A Dimension table describes the “who”, “what”, “where”, and “when” of the business.
It contains textual or descriptive attributes that help you slice and analyze fact data.

Example:

```
Who bought? → Customer Dimension

What product? → Product Dimension

When did it happen? → Date Dimension

Where? → Region / HQ Dimension
```

Key Characteristics :

1. Contains descriptive text columns like Customer Name, City, Product Category, Employee Name, Brand, HQ, Region.

2. Does not grow frequently - Only changes when new customers, products, or employees are added.

3. Has a Primary Key - This key connects to the Fact table.

Types :

1. Conformed Dimension - It is a dimension table that is shared across multiple fact tables.

Example:

The Date table is used by:

```
Sales Fact
Purchase Fact
Inventory Fact
The same Date table works for all.
```

2. Role-Playing Dimension - A single dimension used in multiple roles inside the same model.

Example:

```
The Date Dimension can act as:

Order Date
Invoice Date
Delivery Date

Power BI allows you to create multiple relationships with inactive relationships and use USERELATIONSHIP in DAX.
```

3. Slowly Changing Dimension (SCD) - These dimensions change slowly, not every day.

There are 4 important types:

SCD Type 0 – Fixed Record (No Change) some values were never change.

Example: Date of Birth, GST Registration ID, These values remain constant.

SCD Type 1 – Old value is replaced with the new value.

Example:

Customer Name corrected from

“Rahul Kumar” → “Rahul K.”

You don’t need to track the wrong spelling.

SCD Type 2 – Keep History - A new row is inserted for every change.

Example (customer changed HQ):

| CustomerID | CustomerName | HQ     | StartDate | EndDate |
| ---------- | ------------ | ------ | --------- | ------- |
| C001       | Rahul        | Mumbai | 2019      | 2022    |
| C001       | Rahul        | Delhi  | 2022      | NULL    |

SCD Type 3 – Keep Partial History - Only stores previous value and current value.

Example :

| CustomerID | HQ_Current | HQ_Previous |
| ---------- | ---------- | ----------- |
| C001       | Delhi      | Mumbai      |

4. Junk Dimension

A table that stores miscellaneous attributes that don’t fit anywhere.

Example: Flags, yes/no fields, IsNewCustomer (Y/N), PaymentStatus (Paid/Unpaid), PriorityFlag (High/Low)

5. Degenerate Dimension - A dimension that has no separate table; it lives inside the fact table.

Example: Invoice Number, Order Number, Transaction ID

These are identifiers but not descriptive enough to create a separate dimension table.

6. Outrigger Dimension - A dimension table that is connected to another dimension (not directly to the fact).

Example:
```
Product Dimension
→ linked to Brand Dimension

HQ Dimension
→ linked to Customer Dimension
```


Fact table - 

A Fact Table stores the numerical, measurable, or transactional data of the business.

It answers “how much?”, “how many?”, “what is the value?”.

It contains: Numbers, Calculations, Foreign keys (link to dimensions), Transaction-level details

A fact table stores the business performance data — like sales, profit, quantity, budget, payments, inventory levels, etc.

Key Characteristics

1. Stores Numeric Metrics - Sales amount, Quantity, Profit, Cost, Inventory level, Score, Budget. These values are used for DAX Measures.

2. Contains Foreign Keys - It links to dimension tables using: DateKey, CustomerID, ProductID, HQID, These keys create relationships in Power BI.

3. Huge Row Count (Grows Daily) - Fact tables grow continuously.

4. Granularity (Grain) - Fact tables represent the lowest level of detail.

Types of Fact Tables

1. Transaction Fact Table - Stores transaction-level details. Example: Sales Register → each invoice row.

2. Periodic Snapshot Fact Table - Captures data at regular intervals (daily, monthly, yearly).

3. Accumulating Snapshot Fact Table - Tracks the status of a process over time. It updates as the process moves through stages.

Example: Order Processing (Stages: Order Received → Packed → Shipped → Delivered)

4. Factless Fact Table - Contains no numeric values. Only stores relationships or events.

Example: Student attendance, Employee badge-in events, Customer promotion eligibility

https://www.youtube.com/watch?v=_0IdAb9Z5n4

# 13. What is the types of schemas?

In a star schema, we have a central fact table directly connected to dimension tables and no dim table connected to other dim table.

The dimensions are usually denormalized, meaning all descriptive fields are in one single table per dimension.

```
           Dim Customer
                  |
Dim Product — Fact Sales — Dim Date
                  |
             Dim Region

```

In a snowflake schema, dimensions are normalized — meaning they are broken into multiple related tables. 

here one dim table connected to multiple dim tables.

For example, a Product dimension may be split into Product, Category, and Subcategory.

```
Dim Product → Dim Category
          \ 
           Fact Sales → Dim Date → Dim Fiscal Calendar

```

Galaxy Schema (Fact Constellation)

Multiple fact tables sharing common dimensions

Used when modeling multiple business processes

```
              Dim Customer     Dim Date  
                |     |         |  |       
       Fact Sales  Fact Inventory  Fact Orders

```

# 14. What is Composite Model in Power BI?

A Composite Model in Power BI allows you to combine multiple data sources in the same data model, using both:

Import mode (faster, cached in memory)

DirectQuery mode (real-time connection to database)

Example

Let’s say we are building a sales dashboard:

we import Product and Customer tables (static data)

We use DirectQuery for the Sales Fact table (which updates frequently)

Power BI lets me model them together using Composite Model

https://www.youtube.com/watch?v=sq5kY5WPq0g

# 15. Import and Direct query in power BI?

Import mode -

Power BI imports a copy of the data into its in-memory engine (VertiPaq).

Data is stored inside the .pbix file (Power BI experience file format)

there is 1 GB of limit for the data we want to publish to power bi service

it has fast performance than any other mode.

Disadvantages - 

Large dataset → .pbix becomes heavy

Data not real-time

Refresh limits in Service (for power bi pro - 8 schedule refresh per day and refresh must complete within 2 hours and for premium - 48 schedule refresh per day)

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

# 16. What is bookmarks in Power BI?

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

# 17. Drill Throgh

Drillthrough in Power BI is a feature that allows users to right-click a data point and navigate to a detailed report page that focuses only on that specific context (customer, product, region, etc.).

When you right-click "TATA Motors" → Drillthrough → Customer Details, Power BI opens another page showing:

Monthly Sales

Orders

Products purchased

Trends

KPIs only for TATA Motors

How to setup

create new page and make visual you wants

drag field of main page to "Add drill throgh feild here"

https://www.youtube.com/watch?v=k-uWcjbLv0E

# 18. Whatif parameter?

A What-If Parameter lets users increase/decrease a value and see changes in dashboards.

When you create a What-If parameter, Power BI automatically creates:

A table (e.g., Discount Table with values 0% to 20%)

A slicer

A measure (the selected value)

used in forecasting and scenario testing

https://www.youtube.com/watch?v=28PDdA3SROU&t=134s

# 19. Tell few scenarios where waterfall charts is useful?

To plot annual profit by showing various sources of revenue and arrive at the total profit or loss

# 20. What are Tooltip and explain its types?

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

# 21. What is Decomposition Tree?

A decomposition tree helps break down a metric into multiple levels to identify key contributors. 

It supports AI splits, dynamic drill-down, and is mainly used for root-cause analysis. 

It's ideal when users want flexibility to explore data in any order instead of a fixed hierarchy.

https://www.youtube.com/watch?v=Nm5ImMmi-y8

# 22. What is RLS ( Row level Security )?

RLS is a way to restric the data on the basis of logged in user 

RLS allows you to filter rows of data for different users.

Example:

Sales Manager of North Region sees only North data

Sales Manager of South Region sees only South data

It protect sensetive data

Gives personalized views without creating separate reports

We can have two types of RLS static and dynamic

Userprinciplename() : shows the username of currently logged in user in both Power bi dekstop and servive

Username() : shows dekstop name in power bi dekstop and username in power bi service.

https://www.youtube.com/watch?v=VVkcWG42e3E

# 23. Difference between power bi pro and premium licence?

Power BI Pro - User based license basically for developers can leverage all content creation and interaction features

you can share dashboard/report to only pro user

it has limit of storage per license 10 GB

Power BI Premium - Licesing based on didicated capacity content can be viewed without additional per user total

it has two types - 

premium per user - per user license but wih premium users

premium by capacity - you pay for dedicated capacity and then many users (free also) can consume content till capacity

it has limit of storage per license 100 TB

can make paginated reports 

# 24. What is Dataflow?

A Dataflow is a cloud-based ETL (Extract, Transform, Load) process in Power BI Service 

It allows you to extract data from multiple sources, transform it using Power Query, and store it in a reusable format (CDM entities) for use in multiple datasets and reports.

# 25. What is Data Gateway and explain its types?

Data Gateways basically act like a bridge creating connection between our local machine to Power BI Service

Types :

Personal Mode gateway : 

can only be used by you, can't be used with other apps or services, only supports scheduled refresh in power bi

Enterprise Mode gateway :

can be shared and used by multiple users, can be used by power bi , power apps, flow etc

supports schedule refresh and live query (dataflow) for power bi

https://www.youtube.com/watch?v=TQrEGfR0ChQ

# 26. What are different access levels that one can have in a workplace?

Power BI Admin :

Person owns/manage the workplace with full control over other members in workplace

Member :

The Developer of report has a member access having capabilities of doing modifications, sharing report through apps, managing gatways, and many more 

Member can give contributor and viewer access while adding new people in the workplace

Contributor :

Contributes by doing modifications if any and publishing it to the service.

can give only viewer access to the new people added to the workplace.

Viewer :

The Business/End User are given Viewer's access who can only view the report shared to them

# 27. What is difference between calculated measure and calculated column?

Measure :

Works on filter context, return different data results depending on filters applied, not stored in RAM and can be reused for other measures

Columns :

Works on row context means used for row by row calculation, takes up space in RAM and recalculates itself only on data source refresh 

# 28. What is difference between Count() and CountA() function?

Count() :

Counts only numeric values in a column.

Ignores text, blanks, and logical values.

Example: COUNT([Sales]) → counts only rows with numbers.

COUNTA() :

Counts all non-blank values (text, numbers, dates, TRUE/FALSE).

Only blanks are ignored.

Example: COUNTA([CustomerName]) → counts names (text values)

# 29. Explain Calculate() function?

CALCULATE() perform calculations after modifying the existing filter context.

It allows you to override, add, or remove filters.

It is used for all advanced DAX calculations like YTD, MTD, LY, conditional totals, etc.

Sales_India = CALCULATE( SUM(Sales[Amount]), Sales[Country] = "India" )

# 30. How can you create a one row table using DAX?

Using ROW() : 

We can use Row() function to get this done

OneRowTable =
ROW(
    "Year", 2024,
    "Country", "India",
    "Sales", 150000
)

Using {} with SELECTCOLUMNS() :

OneRowTable =
SELECTCOLUMNS(
    { (1) },
    "Category", "Electronics",
    "Amount", 5000
)

{ (1) } creates a table with a single value.

Then SELECTCOLUMNS adds your custom columns.

Using DATATABLE() — If you want data types fixed :

OneRowTable =
DATATABLE(
    "Product", STRING,
    "Price", INTEGER,
    {
        { "Laptop", 60000 }
    }
)

# 31. Difference between Hasonevalue(), Hasonefilter(), isfilter() functions?

HASONEVALUE(): Checks whether ONLY ONE distinct value exists in a column.

HASONEFILTER() : Checks whether a column (or table) has exactly one filter applied.

ISFILTERED() : Checks whether any filter is applied on a column or table.

Returns TRUE when:

Slicer applied

Manual filter applied

Visual level filter

Page level filter

Report level filter

# 32. What is difference between Sum() and SumX() function?

SUM() :

SUM() adds all numeric values of a single column.

Works only on one column

No row-by-row logic

Fast and simple aggregation

Total Sales = SUM(Sales[Amount]) // Just sums the column Amount.

SUMX() :

SUMX() evaluates an expression row-by-row and then adds the results.

Works on a row context

Allows calculations per row

Used when total cannot be calculated directly from one column

Used when we dont want to create additional columns to evaluate


# 33. Sales = Calculate(Sum[amt], product[product] = "apple"), make it using calculatetable().

Sales =
SUMX(
    CALCULATETABLE(
        Sales,
        Product[product] = "apple"
    ),
    Sales[amt]
)

# 34. Optmize these DAX codes
 
1. CALCULATE(SUM(REVENUE), REGION="INDIA")

ANSWER : 

CALCULATE(
    SUM(Sales[REVENUE]),
    FILTER(
        VALUES(Sales[REGION]),
        Sales[REGION] = "INDIA"
    )
)

2. SUMMERIZE(SALES, CALENDER[YEAR], PRODUCT[CATEGORY], "SUM OF SALES", SUM[REVENUE])

Sales_Summarize =
SUMMARIZECOLUMNS(
    'Date'[Fiscal Year],
    'Sales Register'[Item Description],
    "Sales of product",
    SUM('Sales Register'[AMOUNT])
)

# 35. Difference between Related() and Lookupvalue() function?

RELATED() :

Purpose: Returns a value from a related table via an existing relationship.

Requirement: There must be a relationship defined in the model between the tables.

Returns: A single value for the current row

LOOKUPVALUE() : 

Purpose: Returns a value from a column in another table without needing a relationship.

Requirement: You specify matching columns manually.

Returns: The value from the row where the condition matches.


# 36. Will Related() function work in any measure if you have many to many relationship between two tables, why?

RELATED() retrieves a value from a related table.

It only works along a one-to-many (or many-to-one) relationship, from the “many” side to the “one” side.

In a many-to-many relationship:

There isn’t a single row on the “other side” for each row in the current table — multiple matches exist.

RELATED() cannot resolve multiple values.

Use LOOKUPVALUE() if you can define a unique match.

Or, better, use bridging tables (a “dimension” table) to break the many-to-many into two one-to-many relationships.

# 37. What will you do if some of your visual are not filtering out on clicking a perticular visual?

1. We check "Edit Interaction"

2. We check relationship in model view

# 38. How to create contant line?

Select your visual (e.g., column chart, line chart).

Go to the Analytics pane (next to the Fields pane).

Under X-axis / Y-axis, you’ll see “Constant Line” or “Average Line”.

Click Add → a constant line appears on the chart.

# 39. Can you use one dataset to create different reports, if yes, how?

Publish your dataset to the Power BI Service.

Dataset can come from Power BI Desktop (.pbix) or dataflow.

In Power BI Service, click “Create → Report”.

Instead of importing new data, choose “Pick a published dataset”.

Now you can build different reports from the same dataset without duplicating the data.


# 40. How can you create a Data Alert if something is going above threshold?

After Power BI dashboard published on power bi service, at the bottom of card, KPI or Gauge visual click on three dots

# 41. How to Convert Star Schema → Snowflake Schema?

A Star Schema means:

One central fact table

Surrounding dimension tables

Dimensions are denormalized (flattened)

No sub-dimensions

A Snowflake Schema means:

Dimension tables are normalized (broken into multiple related tables)

More relationships

More tables with less duplicate data

To convert to Snowflake, you normalize each dime

1. Identify Repeating Attribute Groups in Dimensions

2. Split the dimension into multiple tables


To convert Star → Snowflake, break each large dimension table into smaller normalized hierarchy tables and create relationships between them.

# 42. What is a Role-Playing Dimension?

A Role-Playing Dimension is one physical dimension table that is used multiple times for different roles in the data model.

Example:

You have one Date dimension, but your fact table has multiple date columns:

FactSales
Order Date
Ship Date
Invoice Date
Due Date

All of these represent different roles of the same Date table

So the Date dimension plays several roles:

Date (Order)
Date (Ship)
Date (Invoice)
Date (Due)

In Power BI, this is implemented either by duplicating the dimension or by using inactive relationships with USERELATIONSHIP() inside measures.

# 43. How good are you in writing DAX queries?

I am very comfortable writing DAX and have strong hands-on experience with both basic and advanced DAX.
I work confidently with CALCULATE, FILTER, SUMX, and all Time-Intelligence functions like YTD, MTD, LY, rolling periods, and moving averages.
I also understand evaluation context deeply—row context vs filter context, context transition, role-playing dimensions, active/inactive relationships, and performance-optimized DAX.

I regularly use functions like:
• RANKX, TOPN
• ALL, ALLEXCEPT, ALLSELECTED
• TREATAS, CROSSFILTER
• USERELATIONSHIP, RELATED, RELATEDTABLE
• CALCULATETABLE for virtual tables

I can write complex measures like dynamic period selection, rolling 12-month, dynamic segmentation, ABC analysis, and KPI comparisons.

Overall, I am very strong in DAX and can build optimized, accurate measures even for complicated business requirements.

# 44. Explain Row Context vs Filter Context.

Row Context:

Row-by-row evaluation. Exists in calculated columns & iterators like SUMX.

Example:
In a calculated column:
SalesAmount = Sales[Qty] * Sales[Price]

Each row has its own context.

Filter Context:
Filters applied from slicers, visuals, relationships, CALCULATE, etc.

Example:
Total Sales = SUM(Sales[Amount])

This respects filters like Year, Region, Product.

# 45. What is Context Transition?

When a row context becomes filter context, usually inside CALCULATE or measures.

Inside a row context (like SUMX), CALCULATE converts the current row into filter context.


# 46. Difference between CALCULATE and CALCULATETABLE?

CALCULATE → returns a scalar (single value).
CALCULATETABLE → returns a table.

# 47. Difference between ALL, ALLEXCEPT, ALLSELECTED?

ALL: Removes all filters from the specified table/columns.
ALLEXCEPT: Removes all filters except the mentioned columns.
ALLSELECTED: Removes filters but preserves filters coming from visuals (user selections)

# 48. RELATED() vs LOOKUPVALUE()?

RELATED():
Uses existing relationship to fetch a single value from a dimension table → works only with one-to-many.

LOOKUPVALUE():
Does not require relationships, performs lookup based on matching column values.
Used when relationship doesn’t exist or is complex.


# 49. When does a measure ignore slicers? How to control?

A measure ignores slicers when using:

ALL()
REMOVEFILTERS()
Using a different table not connected in model
Using disconnected slicers

You control it using:

KEEPFILTERS
TREATAS
CROSSFILTER
Removing ALL()


# 50. What is a Virtual Table? Example?

A table created inside a DAX expression (not physical).

Example:

SUMX(
    FILTER(Sales, Sales[Qty] > 10),
    Sales[Amount]
)

FILTER returns a virtual table, evaluated row by row.

# 51. Purpose of TREATAS()?

To apply filters from a disconnected table to another table.

Selected Sales =
CALCULATE(
    [CY Sales],
    TREATAS(VALUES(Disconnected[Month]), Date[Month])
)

# 52. How does RANKX work internally?

RANKX creates a virtual table, computes a measure for each row, sorts it, and then assigns the rank.
Ranks change when slicer changes because the underlying virtual table changes.

# 53. What is EARLIER()?

Used when you have row context inside row context (nested).

Example:
Ranking within a calculated column before RANKX existed.


# 54. Why does a measure show circular dependency?

Occurs when:

A measure depends on itself

Two measures refer to each other

A calculated column references a measure that depends on that column

Fix:
Rewrite logic using iterators, avoid mutual references, separate base measures.

# 55. Why Time Intelligence doesn't work in DirectQuery?

Because time-intelligence requires:

Full date table in memory

Ability to fetch entire date ranges

DAX engine performs date operations before query

In DirectQuery, the data stays in SQL, so DAX cannot generate date ranges like YTD, PYTD.

Workaround:
Use server-side SQL logic or custom DAX without built-ins.

# 56. What happens when multiple filters are used inside CALCULATE?

All filters are combined using AND logic
(unless you use OR/PARAMETER logic manually).

Example:
```
CALCULATE(
    [Sales],
    Product[Category] = "A",
    Region[Zone] = "West"
)
```

Both conditions apply.


# 57. can i make like this -> CALCULATE([TOTAL SALES], [TOTAL COST] > 5000)

NO, Because CALCULATE does NOT accept a measure-to-measure comparison as a filter.

Why it doesn't work?

The filter inside CALCULATE must be:

a column filter, or

a table expression that returns rows

But this expression:

[TOTAL COST] > 5000

is a measure, not a column → so DAX has no row context to evaluate it.

If you want to filter rows where Cost > 5000, you must use the column, not the measure:
CALCULATE([TOTAL SALES], Sales[Cost] > 5000)

If you want to filter using a measure
```
CALCULATE(
    [TOTAL SALES],
    FILTER(
        ALL(Sales),
        [TOTAL COST] > 5000
    )
)
```
This works because FILTER creates a row-by-row table, and the measure can evaluate row context.


# 58. How to Optimize Power BI Model?

1. Keep only those columns your users needs in report.

2. Similar to columns, keep only those rows you need.

3. Aggregate your data whenever possible.

4. Use proper Data Types, avoid using Text data types if column has number

5. Avoid using calculated columns whenever possible

6. Try to push all calculations to data source using query folding or use power query editor

7. Reduce column cardinality

8. Try to use more and more variable in your measures, calculated columns and tables.

9. Do not use Bi-directional filtering in data model, Instead Crossfilter.

10. Try to use Report Level tooltip instead of standared tooltip for better performance.



# 59. What is Cardinality?

Cardinality refers to the number of unique values in a column or the nature of the relationship between tables.



| Cardinality Type       | Meaning                                                     | Example                              |
| ---------------------- | ----------------------------------------------------------- | ------------------------------------ |
| **One-to-One (1:1)**   | One record in Table A relates to only one record in Table B | One employee → one ID card           |
| **One-to-Many (1:M)**  | One record in Table A can relate to many in Table B         | One customer → many orders           |
| **Many-to-One (M:1)**  | Opposite of one-to-many                                     | Many orders → belong to one customer |
| **Many-to-Many (M:M)** | Multiple records in both tables relate to each other        | Students ↔ Courses                   |


| Cardinality Type       | Description                   | Example                                        |
| ---------------------- | ----------------------------- | ---------------------------------------------- |
| **High Cardinality**   | Column has many unique values | Email, Phone number, Invoice ID                |
| **Low Cardinality**    | Column has few unique values  | Gender (Male/Female), Status (Active/Inactive) |
| **Medium Cardinality** | Not too many, not too few     | City names, Product categories                 |


# 60. Find the rows in Table A that are not in Table B in Power BI.

Usin DAX Function
```
ExtraRecords =
EXCEPT(
    'TableA',
    'TableB'
)
```
EXCEPT returns all rows in TableA that do not exist in TableB.

The columns must have the same names and order (or you can select specific columns)

Using Power Query (Merge Queries → Anti Join)

Go to Home → Merge Queries.

Select TableA as first, TableB as second.

Join on all columns (select all).

In Join Kind, choose Left Anti.

Left Anti = Only rows in TableA that don’t exist in TableB.

Click OK → You will get the 2 extra rows.

# 61. How to connect two discnnected table - one table have city nam and amount and other have years 2012, 2013, i want for every row of table 1 there will be two rows for 2012 and 2013

You want to create a cross join in Power BI so that every row from Table1 is repeated for each year in Table2. This is common when you have a “fact table” with values (City, Amount) and a “dimension table” with years.

Using DAX (Calculated Table)
```
CityAmount_Years =
CROSSJOIN('CityAmount', 'Years')
```
CROSSJOIN creates all possible combinations of rows from both tables.

Using Power Query (Merge Queries → Full Outer / Custom)

Go to Home → Merge Queries → Merge as New.

Select Table1 as first, Table2 as second.

Don’t select any matching columns.

Choose Join Kind = Full Outer.

Expand the second table → you get all combinations.

(Power Query doesn’t have a direct “Cross Join” button, but you can achieve it by adding an Index column to both tables and merging on 1 = 1.)


# 62. How to get sales of material containing "Bio"

Method 1
```
Sales_KemBio =
CALCULATE(
    SUM(Sales[Amount]),
    SEARCH("kem bio", Sales[Material], 1, 0) > 0
)
```

Method 2

```
Sales_KemBio =
CALCULATE(
    SUM(Sales[Amount]),
    CONTAINSSTRING(Sales[Material], "kem bio")
)
```

# 63. How to append multiple file with same columns

Load all files from a folder

Go to Home → Get Data → Folder.

Browse to the folder containing all your files.

Click Combine & Transform Data.

Power Query will automatically combine files with the same structure.

Power BI automatically creates a sample file and appends the rest.

Use Folder load if you expect new files to be added regularly — Power BI will automatically include them after refresh.


# 64. We want a default selection in a Power BI visual (or measure) where:

By default, the measure shows CBS Enterprises sales

If the user selects a different customer, it shows sales for the selected customer

```
Customer_Sales :=
VAR SelectedCustomer = SELECTEDVALUE(Customers[CustomerName], "CBS Enterprises")
RETURN
CALCULATE(
    SUM(Sales[Amount]),
    Customers[CustomerName] = SelectedCustomer
)
```

# 65. How to calculate working days between two dates

```
WorkingDays =
VAR StartDate = Sales[StartDate]
VAR EndDate = Sales[EndDate]
RETURN
CALCULATE(
    COUNTROWS(
        FILTER(
            CALENDAR(StartDate, EndDate),
            WEEKDAY([Date], 2) < 6   -- 2: Monday=1, Sunday=7, so <6 = Mon-Fri
        )
    )
)
```

# 66. How to get rank by customer?

```
Rankbycustomer =
RANKX(
 ALLSELECTED('Sales Register'[Customer Name]),
               [CY Sales],,
                  DESC,
                   Dense
               )
```

# 67. How to get sales of Top N customer?

```
Top N Values =
VAR selected_top = SELECTEDVALUE('Top'[top], "Top 5")   -- default Top 5
VAR TopN =
    SWITCH(
        selected_top,
        "Top 3", 3,
        "Top 5", 5,
        "Top 10", 10,
        5
    )
RETURN
IF([Rankbycustomer] <= TopN, [CY Sales])
```

# 68. How to caclculate YTD Sales (for fiscal year apr-mar) using TOTALYTD()?

```
Sales YTD =
TOTALYTD(
    [CY Sales],
    'Date'[Date],
    "03/31"          -- fiscal year end
)
```

# 69. How to caclculate YTD Sales (for fiscal year apr-mar) using DATESYTD()?

```
Sales YTD =
CALCULATE(
    [CY Sales],
    DATESYTD('Date'[Date], "03/31")
)
```

| Feature                            | **TOTALYTD()**                                          | **DATESYTD()**                                                                |
| ---------------------------------- | ------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **Type**                           | Calculation function                                    | Date table function (returns a table of dates)                                |
| **Purpose**                        | Calculates a YTD value (sum, count, etc.) automatically | Returns the set of dates that represent the YTD period, used inside CALCULATE |
| **Does aggregation?**              | ✅ Yes                                                   | ❌ No (needs aggregation separately)                                           |
| **Use case**                       | When you want a direct measure (like YTD sales)         | When you need more control and want to pass YTD dates into CALCULATE          |
| **Syntax simplicity**              | Easy                                                    | More flexible                                                                 |
| **Supports Fiscal Year end date?** | Yes (optional parameter)                                | Yes (optional parameter)                                                      |


# 70. If one column have data like this RS1000 then how to get only number in power query?

Method 1

```
= Text.Select([ColumnName], {"0".."9"})
```

Method 2

```
Home → Split Column → By Non-Digit to Digit
```

# 71. Give last 3 months sales.

```
Last 3 Month Sale = 
VAR SelectedDate =
    IF(
        ISFILTERED('Date'[Date]),
        MAX('Date'[Date]),               
        CALCULATE(MAX('Sales Register'[Invoice Date]))
    )
RETURN 
CALCULATE(
    [CY Sales],
    DATESINPERIOD(
        'Date'[Date],
        SelectedDate,
        -3,
        MONTH))


DATESINPERIOD(
    DATE COLUMN TO FILTER,
        DATE WHERE FROM TO START,
        HOW MANY FORWARD/BACKWARD,
        WHAT - DAY/MONTH/YEAR
      )

DATESINPERIOD(
    DATE TABLE MADHUN JO COLUMN FILTER KARAYACHA TYACHE NAV,
        KONTYA TARKHE PASUN PUDHE KINVA MAGE JAYCH AAHE,
        KITI MAGE KINVA PUDHE JAYCH AAHE,
        KAY FILTER KARAYCH AAHE - DIVAS/MAHINA/VARSH
      )

```


# 72. GIVE LAST 12 MONTHS ROLLING SALES EXCULDING CURRENT MONTH?

```

Rolling12M_ExclCurrent = 
CALCULATE(
    [CY Sales],
    DATESINPERIOD(
        'Date'[Date],
        EOMONTH(MAX('Date'[Date]), -1),   -- Last date of previous month
        -10,
        MONTH
    )
)

EOMONTH(
    DATE,
    NO. OF MONTH FORWARD/BACKWARD
    )

DILELYA TARKHEPASUN PUDHE KINVA MAGE JAUN TYA MAHINYACHA SHEVATCHA DIVAS DETO

```

# 73. CAN YOU CALCULATE YTD CALCULATION IN DIRECT QUERY MODE?

DirectQuery does not support many DAX Time-Intelligence functions like:

❌ TOTALYTD()
❌ DATESYTD()
❌ PREVIOUSYEAR()
❌ SAMEPERIODLASTYEAR()

```
YTD_SALES = 
    CALCULATE(
            [CY SALES],
            FILTER(
                ALL("DATE"),
                "DATE"[DATE] <= MAX("DATE"[DATE]) && 
                YEAR("DATE"[DATE]) = YEAR(MAX("DATE"[DATE]))
            )
        )


MTD

MTD_Sales_DQ =
CALCULATE(
    [CY Sales],
    FILTER(
        ALL('Date'),
        'Date'[Date] <= MAX('Date'[Date]) &&
        MONTH('Date'[Date]) = MONTH(MAX('Date'[Date])) &&
        YEAR('Date'[Date]) = YEAR(MAX('Date'[Date]))
    )
)

```

# 73. HOW TO RESTRICT OUR TOTALYTD TO LAST SALES DATE?

```
TotalYTD = 
    var max_selected_date = MAX('Sales Register'[Invoice Date])
    var total_ytd_restricted = TOTALYTD([CY Sales],'Date'[Date], 'Date'[Date] <= max_selected_date)
    RETURN total_ytd_restricted
```


# 74. HOW TO GET WEEKEND AND WEEKDAYS SALES?

```
Weekday Sales = 
CALCULATE(
    [CY Sales],
    FILTER(
        ALL('Date'),
        WEEKDAY('Date'[Date], 2) <= 5
    )
)


Weekend Sales = 
CALCULATE(
    [CY Sales],
    FILTER(
        ALL('Date'),
        WEEKDAY('Date'[Date], 2) > 5
    )
)
```

# 75. Cumalative Sum over the year?

```
Cumulative Sales = 
CALCULATE(
    [CY Sales],
    FILTER(
        ALLSELECTED('Date'[Date]),           -- removes only the date filter within the selected year(s)
        'Date'[Date] <= MAX('Date'[Date])    -- up to current date
    )
)
```

# 76. Top 5 Product Sales

```
Top 5 Products = 
CALCULATE(
    [CY Sales],
    FILTER(
        VALUES('Sales Register'[Item Description]), // gives unique values
        RANKX('Sales Register', 
        [CY Sales],,
        DESC, 
        Dense) <=5))
```

# 77. Sales of CBC using Calculate table?

```
ABP Sales using CT =
SUMX(
    CALCULATETABLE(
        'Sales Register',
        'Sales Register'[Customer Name] = "CBC Enterprises"
    ),
    [CY Sales]
)
```

78. How to get activate inactive relationship in DAX.

In your data model, you may have multiple relationships between two tables, but only one can be active at a time.

USERELATIONSHIP temporarily activates an inactive relationship for the calculation

```
Inactive Revenue =
CALCULATE(
    [CY Sales],
    USERELATIONSHIP('Date'[Date], 'Sales Register'[Shipping Date])
)
```

# 79. what is paginated reports?

Paginated Reports in Power BI are a type of formatted, print-ready report designed to fit nicely on a page — like invoices, statements, bills, forms, or reports you want to download/export as PDF, Excel, or print.

They are called paginated because the content is organized into pages, and if data is long (like hundreds of rows), the report automatically continues on the next page — just like a traditional report.

# 80. Explain process of dashboard deployment?

Power BI deployment means moving reports and datasets from Development → Testing → Production in a controlled and secure way.

Development (Dev Environment)

This is where the report is initially built.

✔ Connect to source systems (SQL, Excel, APIs, ERP)

✔ Build data model (Star schema, relationships)

✔ Create DAX Measures

✔ Design visuals, bookmarks, drill-through

✔ Apply performance optimization

✔ Test with sample or full dataset


Version Control & Documentation

✔ Store PBIX and scripts in Git, SharePoint, OneDrive, Azure DevOps

✔ Maintain naming standards, change logs, deployment notes


Publish to Workspace

Publish the PBIX to a Dev Workspace in Power BI Service.

✔ Create data source credentials

✔ Configure refresh schedule

✔ Connect gateway if using on-premises data

Apply Security

✔ Implement Row-Level Security (RLS)

✔ Test with "View As Role"

User Acceptance Testing (UAT)

✔ Move the report from Dev → Test/UAT workspace

✔ Business users validate numbers, filters, logic

✔ Fix issues based on feedback

✔ Freeze version after approval

Deploy to Production (Prod)

Once approved, move the final PBIX to the Prod workspace.

# 81. What is Power BI?

Power BI is an end-to-end analytics platform that enables data modeling, visualization, and enterprise reporting

It includes components such as:

Power Query for ETL and data transformation,

Power BI Desktop for building data models and reports,

DAX for advanced calculations,

Power BI Service for publishing, managing workspaces, security, and collaboration,

Power BI Gateway for scheduled refresh from on-premise sources,

Row-Level Security (RLS) for controlled access.

Power BI supports Import, DirectQuery, and Live Connection modes depending on performance and data size needs. 

It integrates with Azure, SQL, Excel, and other 160+ enterprise applications, making it suitable for operational and strategic analytics

# 82. What is SSRS?

SSRS, or SQL Server Reporting Services, is an enterprise reporting platform used to generate interactive and paginated reports. 

It supports parameterized and drill-through reporting, role-based security, caching, and scheduled report delivery.

Users can consume reports via:

Web portal

Email subscriptions

Embedded reports in applications

SSRS integrates with SQL Server, Power BI Report Server, SharePoint (older versions), and supports exporting reports into multiple formats such as Excel, PDF, Word, and CSV."


# 84. Where did data stored in Power BI?

Power BI stores data in a compressed, in-memory storage engine called VertiPaq when using Import mode. 

The data is saved in the .pbix file locally, and once published, it is stored in Power BI Service storage, which is part of Azure cloud.

# 85. Limitation of storage?

Power BI storage limitations depend on the licensing model. In Power BI Desktop, a single PBIX file can store up to 10 GB of data after compression. When published to Power BI Service:

Free and Pro users get 10 GB total storage per user and a dataset size limit of 1 GB (Import mode).

Power BI Premium Per User (PPU) supports datasets up to 100 GB.

Power BI Premium Capacity allows datasets up to 400 GB and total storage of 100 TB or more depending on the capacity tier.

The actual usable storage also depends on refresh limits, DirectQuery mode, and data compression

# 86. Is any performance issue with Direct Query?

Yes. 

DirectQuery can have performance issues because it does not store data inside Power BI — instead, every visual or filter interaction sends a query back to the data source. 

If the source system or network is slow, the report performance may become slow.

Unlike Import mode where data is cached in-memory using VertiPaq, DirectQuery relies on the external system’s speed—so query execution time, concurrency, and database load can affect performance.

Other limitations include limited DAX functions, frequent timeout issues, row-level slow scanning, and restricted data transformations.

# 87. How to improve performance of Direct query?

Remove unnecessary columns and tables

Avoid complex relationships

Use star schema instead of snowflake

Use appropriate data types

Ensure transformations in Power Query can push back to the source

Avoid steps like custom columns that prevent folding

Create proper indexes, partitions, and tuning on source database

Improve SQL source performance

Use materialized views or aggregated stored tables

Limit visuals per page (recommended 8 or fewer)

Avoid high-cardinality slicers and complex DAX calculations

Use measures instead of calculated columns

Use Import mode for summarized data and DirectQuery for detailed drill-down

Enable Query Caching in Power BI Service

Use Composite Models and Incremental Refresh

https://www.youtube.com/watch?v=F1WW6jOipno

# 88. How to see loading time of visual?

In View tab there is "Performance Analyzer" 

In the Performance Analyzer pane → click Start recording

Refresh visuals (or interact with slicers)

You’ll see each visual listed with timing details

# 89. If any visual taking 10 seconds and it is very important?

If it is business critical then we don't remove it, we optimize it

We use performance analyser if dax query is high, then problem in measure logic

if visual display is high, then problem is in visual design

if other is high, then model or relationship issue

for Dax issue, we avoid Calculate + Filter, replace iterator (like SUMX, FILTER) with simple aggregation, use var to avoid recalculations

for Visual issue, avoid too many fields in one visual, avoid custom visual

for other issue, remove unused columns, avoid bi directional relationships, use star schema

# 90. When QnA option not coming after double click?

Go to File → Options and settings → Options

Under Current File → Q&A

Enable “Use Q&A to ask questions about your data”

Restart Power BI Desktop

# 91. Difference between add column and transform tab in power query? Which is better performancce?

1. Transform tab - The same column is updated.

Modifies an existing column

Replaces the original column values

No extra column is created

Examples:

Change data type

Replace values

Trim / Clean

Split column

Extract text

2. Add Column tab - Original columns remain + new column added.

Creates a new column

Keeps original column unchanged

Used for derivations & calculations

Examples:

Conditional column

Custom column

Date difference

Calculated logic


Transform tab is better for performance

No additional column

Less memory usage

# 92. 5 Table in power query and want 3 Tables to import in power BI?

In Queries pane, right-click on the table

Uncheck “Enable Load”

Repeat for the 2 tables you don’t want

# 93. what do when we want to make one schedule refresh after 8 in pro account?

In Power BI Pro, scheduled refresh is limited to 8 times per day. If more refreshes are needed, we do manual refresh in power BI service, or we either optimize using incremental refresh or upgrade to Premium, which supports up to 48 refreshes per day.

# 95. In Power BI Dekstop and size go up on 1.1 GB, what you do to make it 1 GB?

remove unused columns

aggregrate data in daily/monthly summary


# 96. Which are two DAX function in Direct Query?

DirectQuery supports only those DAX functions that can be translated into native source queries

so all aggregration dax like sum, avg, min, max are supported but time intelligent function not supported.

# 97. If you can update and delete the workspace, which role you have?

Admin

# 98. What is Sync Slicer?

Sync Slicer allows the same slicer selection to be applied across multiple pages in a Power BI report, ensuring consistent filtering.

# 99. What you do if map is not working?

File → Options and settings → Options

Security

Enable Map and Filled Map visuals

Restart Power BI Desktop

or check columns have - Location column: City, State, Country

# 100. What is DAX function to generate series while using what if parameter?

GENERATESERIES() - we need to give start value , end value, increament value

# 101. How to create dashboard in power BI service?

Dashboards are created in Power BI Service by pinning visuals or report pages from published reports into a single-page canvas

# 102. In Power BI Service what is Subscription?

A Subscription in Power BI Service is a feature that allows you to automatically receive report pages or dashboard snapshots via email on a scheduled basis. It is only for pro or premium users

# 103. Can there is two active relationship in tables?

No, Power BI does not allow two active relationships between the same two tables.

How to use inactive relationship?

```
Sales by Ship Date =
CALCULATE(
    [Total Sales],
    USERELATIONSHIP(Sales[Ship Date], Date[Date])
)

```

# 104. How to made Fiscal Year Table start with april?

We will create calender table and in that we create month index column

then we create fiscal year column where if the monthindex is greater than 3 then it has current FY Year and if month index is 1, 2 or 3 then it will last FY year


# 105. What are types of context in Power BI?

Power BI has two main contexts: Row Context (row-by-row evaluation, e.g., in calculated columns) and Filter Context (filters applied via slicers, visuals, or CALCULATE).

Row Context

Applies row by row inside a table or calculated column.

DAX evaluates each row individually.

Exists automatically in:

Calculated columns

Iterators (SUMX, FILTER, AVERAGEX, etc.)


Filter Context

Applies filters to a calculation, often via slicers, report filters, or DAX functions like CALCULATE.

Changes the subset of data considered for the calculation

```
Total Sales FY23 = CALCULATE([Total Sales], 'Date'[Fiscal Year] = 2023)

```
Only rows where Fiscal Year = 2023 are included → Filter Contex
