

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

it has limit of storage per license 10 GB

Power BI Premium - Licesing based on didicated capacity content can be viewed without additional per user total

it has two types - 

premium per user - per user license but wih premium users

premium by capacity - you pay for dedicated capacity and then many users (free also) can consume content till capacity

it has limit of storage per license 100 TB

can make paginated reports 

24. What is Dataflow?

A Dataflow is a cloud-based ETL (Extract, Transform, Load) process in Power BI Service 

It allows you to extract data from multiple sources, transform it using Power Query, and store it in a reusable format (CDM entities) for use in multiple datasets and reports.

25. What is Data Gateway and explain its types?

Data Gateways basically act like a bridge creating connection between our local machine to Power BI Service

Types :

Personal Mode gateway : 

can only be used by you, can't be used with other apps or services, only supports scheduled refresh in power bi

Enterprise Mode gateway :

can be shared and used by multiple users, can be used by power bi , power apps, flow etc

supports schedule refresh and live query (dataflow) for power bi

26. What are different access levels that one can have in a workplace?

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

27. What is difference between calculated measure and calculated column?

Measure :

Works on filter context, return different data results depending on filters applied, not stored in RAM and can be reused for other measures

Columns :

Works on row context means used for row by row calculation, takes up space in RAM and recalculates itself only on data source refresh 

28. What is difference between Count() and CountA() function?

Count() :

Counts only numeric values in a column.

Ignores text, blanks, and logical values.

Example: COUNT([Sales]) → counts only rows with numbers.

COUNTA() :

Counts all non-blank values (text, numbers, dates, TRUE/FALSE).

Only blanks are ignored.

Example: COUNTA([CustomerName]) → counts names (text values)

29. Explain Calculate() function?

CALCULATE() perform calculations after modifying the existing filter context.

It allows you to override, add, or remove filters.

It is used for all advanced DAX calculations like YTD, MTD, LY, conditional totals, etc.

Sales_India = CALCULATE( SUM(Sales[Amount]), Sales[Country] = "India" )

30. How can you create a one row table using DAX?

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

31. Difference between Hasonevalue(), Hasonefilter(), isfilter() functions?

HASONEVALUE(): Checks whether ONLY ONE distinct value exists in a column.

HASONEFILTER() : Checks whether a column (or table) has exactly one filter applied.

ISFILTERED() : Checks whether any filter is applied on a column or table.

Returns TRUE when:

Slicer applied

Manual filter applied

Visual level filter

Page level filter

Report level filter

32. What is difference between Sum() and SumX() function?

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


33. Sales = Calculate(Sum[amt], product[product] = "apple"), make it using calculatetable().

Sales =
SUMX(
    CALCULATETABLE(
        Sales,
        Product[product] = "apple"
    ),
    Sales[amt]
)

34. Optmize these DAX codes
 
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

35. Difference between Related() and Lookupvalue() function?

RELATED() :

Purpose: Returns a value from a related table via an existing relationship.

Requirement: There must be a relationship defined in the model between the tables.

Returns: A single value for the current row

LOOKUPVALUE() : 

Purpose: Returns a value from a column in another table without needing a relationship.

Requirement: You specify matching columns manually.

Returns: The value from the row where the condition matches.


36. Will Related() function work in any measure if you have many to many relationship between two tables, why?

RELATED() retrieves a value from a related table.

It only works along a one-to-many (or many-to-one) relationship, from the “many” side to the “one” side.

In a many-to-many relationship:

There isn’t a single row on the “other side” for each row in the current table — multiple matches exist.

RELATED() cannot resolve multiple values.

Use LOOKUPVALUE() if you can define a unique match.

Or, better, use bridging tables (a “dimension” table) to break the many-to-many into two one-to-many relationships.

37. What will you do if some of your visual are not filtering out on clicking a perticular visual?

1. We check "Edit Interaction"

2. We check relationship in model view


38. How to create contant line?

Select your visual (e.g., column chart, line chart).

Go to the Analytics pane (next to the Fields pane).

Under X-axis / Y-axis, you’ll see “Constant Line” or “Average Line”.

Click Add → a constant line appears on the chart.


39. Can you use one dataset to create different reports, if yes, how?

Publish your dataset to the Power BI Service.

Dataset can come from Power BI Desktop (.pbix) or dataflow.

In Power BI Service, click “Create → Report”.

Instead of importing new data, choose “Pick a published dataset”.

Now you can build different reports from the same dataset without duplicating the data.


40. How can you create a Data Alert if something is going above threshold?

After Power BI dashboard published on power bi service, at the bottom of card, KPI or Gauge visual click on three dots


41. How to Convert Star Schema → Snowflake Schema?

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

42. What is a Role-Playing Dimension?

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

43. How good are you in writing DAX queries?

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

44. Explain Row Context vs Filter Context.

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

45. What is Context Transition?

When a row context becomes filter context, usually inside CALCULATE or measures.

Inside a row context (like SUMX), CALCULATE converts the current row into filter context.


46. Difference between CALCULATE and CALCULATETABLE?

CALCULATE → returns a scalar (single value).
CALCULATETABLE → returns a table.

47. Difference between ALL, ALLEXCEPT, ALLSELECTED?

ALL: Removes all filters from the specified table/columns.
ALLEXCEPT: Removes all filters except the mentioned columns.
ALLSELECTED: Removes filters but preserves filters coming from visuals (user selections)

48. RELATED() vs LOOKUPVALUE()?

RELATED():
Uses existing relationship to fetch a single value from a dimension table → works only with one-to-many.

LOOKUPVALUE():
Does not require relationships, performs lookup based on matching column values.
Used when relationship doesn’t exist or is complex.


49. When does a measure ignore slicers? How to control?

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


50. What is a Virtual Table? Example?

A table created inside a DAX expression (not physical).

Example:

SUMX(
    FILTER(Sales, Sales[Qty] > 10),
    Sales[Amount]
)

FILTER returns a virtual table, evaluated row by row.

51. Purpose of TREATAS()?

To apply filters from a disconnected table to another table.

Selected Sales =
CALCULATE(
    [CY Sales],
    TREATAS(VALUES(Disconnected[Month]), Date[Month])
)

52. How does RANKX work internally?

RANKX creates a virtual table, computes a measure for each row, sorts it, and then assigns the rank.
Ranks change when slicer changes because the underlying virtual table changes.

53. What is EARLIER()?

Used when you have row context inside row context (nested).

Example:
Ranking within a calculated column before RANKX existed.


54. Why does a measure show circular dependency?

Occurs when:

A measure depends on itself

Two measures refer to each other

A calculated column references a measure that depends on that column

Fix:
Rewrite logic using iterators, avoid mutual references, separate base measures.

55. Why Time Intelligence doesn't work in DirectQuery?

Because time-intelligence requires:

Full date table in memory

Ability to fetch entire date ranges

DAX engine performs date operations before query

In DirectQuery, the data stays in SQL, so DAX cannot generate date ranges like YTD, PYTD.

Workaround:
Use server-side SQL logic or custom DAX without built-ins.

56. What happens when multiple filters are used inside CALCULATE?

All filters are combined using AND logic
(unless you use OR/PARAMETER logic manually).

Example:

CALCULATE(
    [Sales],
    Product[Category] = "A",
    Region[Zone] = "West"
)


Both conditions apply.


57. can i make like this -> CALCULATE([TOTAL SALES], [TOTAL COST] > 5000)

NO,
Because CALCULATE does NOT accept a measure-to-measure comparison as a filter.

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
CALCULATE(
    [TOTAL SALES],
    FILTER(
        ALL(Sales),
        [TOTAL COST] > 5000
    )
)

This works because FILTER creates a row-by-row table, and the measure can evaluate row context.


58. How to Optimize Power BI Model?

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



59. What is Cardinality?

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


60. Find the rows in Table A that are not in Table B in Power BI.

Usin DAX Function

ExtraRecords =
EXCEPT(
    'TableA',
    'TableB'
)

EXCEPT returns all rows in TableA that do not exist in TableB.

The columns must have the same names and order (or you can select specific columns)


Using Power Query (Merge Queries → Anti Join)

Go to Home → Merge Queries.

Select TableA as first, TableB as second.

Join on all columns (select all).

In Join Kind, choose Left Anti.

Left Anti = Only rows in TableA that don’t exist in TableB.

Click OK → You will get the 2 extra rows.



61. How to connect two discnnected table - one table have city nam and amount and other have years 2012, 2013, i want for every row of table 1 there will be two rows for 2012 and 2013

You want to create a cross join in Power BI so that every row from Table1 is repeated for each year in Table2. This is common when you have a “fact table” with values (City, Amount) and a “dimension table” with years.

Using DAX (Calculated Table)

CityAmount_Years =
CROSSJOIN('CityAmount', 'Years')

CROSSJOIN creates all possible combinations of rows from both tables.

Using Power Query (Merge Queries → Full Outer / Custom)

Go to Home → Merge Queries → Merge as New.

Select Table1 as first, Table2 as second.

Don’t select any matching columns.

Choose Join Kind = Full Outer.

Expand the second table → you get all combinations.

(Power Query doesn’t have a direct “Cross Join” button, but you can achieve it by adding an Index column to both tables and merging on 1 = 1.)


62. How to get sales of material containing "Bio"

Method 1

Sales_KemBio =
CALCULATE(
    SUM(Sales[Amount]),
    SEARCH("kem bio", Sales[Material], 1, 0) > 0
)

Method 2

Sales_KemBio =
CALCULATE(
    SUM(Sales[Amount]),
    CONTAINSSTRING(Sales[Material], "kem bio")
)


63. How to append multiple file with same columns


Load all files from a folder

Go to Home → Get Data → Folder.

Browse to the folder containing all your files.

Click Combine & Transform Data.

Power Query will automatically combine files with the same structure.

Power BI automatically creates a sample file and appends the rest.

Use Folder load if you expect new files to be added regularly — Power BI will automatically include them after refresh.


64. We want a default selection in a Power BI visual (or measure) where:
By default, the measure shows CBS Enterprises sales
If the user selects a different customer, it shows sales for the selected customer


Customer_Sales :=
VAR SelectedCustomer = SELECTEDVALUE(Customers[CustomerName], "CBS Enterprises")
RETURN
CALCULATE(
    SUM(Sales[Amount]),
    Customers[CustomerName] = SelectedCustomer
)


65. How to calculate working days between two dates

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

66. How to get rank by customer?

Rankbycustomer = RANKX(ALLSELECTED('Sales Register'[Customer Name]), [CY Sales],, DESC,Dense)


67. How to get sales of Top N customer?

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


68. How to caclculate YTD Sales (for fiscal year apr-mar) using TOTALYTD()?

Sales YTD =
TOTALYTD(
    [CY Sales],
    'Date'[Date],
    "03/31"          -- fiscal year end
)

69. How to caclculate YTD Sales (for fiscal year apr-mar) using DATESYTD()?

Sales YTD =
CALCULATE(
    [CY Sales],
    DATESYTD('Date'[Date], "03/31")
)


| Feature                            | **TOTALYTD()**                                          | **DATESYTD()**                                                                |
| ---------------------------------- | ------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **Type**                           | Calculation function                                    | Date table function (returns a table of dates)                                |
| **Purpose**                        | Calculates a YTD value (sum, count, etc.) automatically | Returns the set of dates that represent the YTD period, used inside CALCULATE |
| **Does aggregation?**              | ✅ Yes                                                   | ❌ No (needs aggregation separately)                                           |
| **Use case**                       | When you want a direct measure (like YTD sales)         | When you need more control and want to pass YTD dates into CALCULATE          |
| **Syntax simplicity**              | Easy                                                    | More flexible                                                                 |
| **Supports Fiscal Year end date?** | Yes (optional parameter)                                | Yes (optional parameter)                                                      |

