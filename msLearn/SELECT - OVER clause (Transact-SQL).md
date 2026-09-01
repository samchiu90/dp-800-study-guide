---
layout: Conceptual
monikers:
- fabric
- azure-sqldw-latest
- azuresqldb-current
- azuresqldb-mi-current
- fabric-sqldb
- aps-pdw-2016
- aps-pdw-2016-au7
- sql-server-linux-2017
- sql-server-linux-ver15
- sql-server-linux-ver16
- sql-server-linux-ver17
- sql-server-2017
- sql-server-ver15
- sql-server-ver16
- sql-server-ver17
defaultMoniker: sql-server-ver17
versioningType: Ranged
title: OVER Clause (Transact-SQL) - SQL Server | Microsoft Learn
canonicalUrl: https://learn.microsoft.com/en-us/sql/t-sql/queries/select-over-clause-transact-sql?view=sql-server-ver17
config_moniker_range: =azuresqldb-current || =azuresqldb-mi-current || =azure-sqldw-latest || >=aps-pdw-2016 || >=sql-server-2017 || >=sql-server-linux-2017 || =fabric || =fabric-sqldb
uhfHeaderId: MSDocsHeader-DocsSQL
toc_preview: true
feedback_system: Standard
feedback_product_url: https://feedback.azure.com/d365community/forum/04fe6ee0-3b25-ec11-b6e6-000d3a4f0da0
feedback_help_link_url: https://learn.microsoft.com/answers/tags/191/sql-server
feedback_help_link_type: get-help-at-qna
recommendations: true
breadcrumb_path: ../../breadcrumb/toc.json
ms.update-cycle: 1825-days
description: Transact-SQL reference for the OVER clause, which defines a user-specified set of rows within a query result set.
author: VanMSFT
ms.author: vanto
ms.reviewer: randolphwest, wiassaf
ms.date: 2026-06-12T00:00:00.0000000Z
ms.service: sql
ms.subservice: t-sql
ms.topic: reference
ai-usage: ai-assisted
ms.custom:
- ignite-2025
locale: en-us
document_id: 81477bb5-e2f4-dd0b-b025-19a30041622e
document_version_independent_id: 482fcc64-0e86-3cac-7c5a-1a312d4ba088
updated_at: 2026-08-24T17:40:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/sql-docs-pr/blob/live/docs/t-sql/queries/select-over-clause-transact-sql.md
gitcommit: https://github.com/MicrosoftDocs/sql-docs-pr/blob/35290234fd67e105f228d27ea7a57f42512e8231/docs/t-sql/queries/select-over-clause-transact-sql.md
git_commit_id: 35290234fd67e105f228d27ea7a57f42512e8231
default_moniker: sql-server-ver17
site_name: Docs
depot_name: SQL.sql-content
page_type: conceptual
toc_rel: ../../toc.json
pdf_url_template: https://learn.microsoft.com/pdfstore/en-us/SQL.sql-content/{branchName}{pdfName}
search.mshattr.devlang: tsql
word_count: 3997
asset_id: t-sql/queries/select-over-clause-transact-sql
moniker_range_name: f3ddb4a4ec3193bff1ec3401d447b815
monikers:
- fabric
- azure-sqldw-latest
- azuresqldb-current
- azuresqldb-mi-current
- fabric-sqldb
- aps-pdw-2016
- aps-pdw-2016-au7
- sql-server-linux-2017
- sql-server-linux-ver15
- sql-server-linux-ver16
- sql-server-linux-ver17
- sql-server-2017
- sql-server-ver15
- sql-server-ver16
- sql-server-ver17
item_type: Content
source_path: docs/t-sql/queries/select-over-clause-transact-sql.md
cmProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/bcbcbad5-4208-4783-8035-8481272c98b8
- https://authoring-docs-microsoft.poolparty.biz/devrel/540ac133-a371-4dbb-8f94-28d6cc77a70b
- https://authoring-docs-microsoft.poolparty.biz/devrel/cbe4ca68-43ac-4375-aba5-5945a6394c20
spProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/43b2e5aa-8a6d-4de2-a252-692232e5edc8
- https://authoring-docs-microsoft.poolparty.biz/devrel/60bfc045-f127-4841-9d00-ea35495a5800
- https://authoring-docs-microsoft.poolparty.biz/devrel/ced846cc-6a3c-4c8f-9dfb-3de0e90e2742
platformId: 311be459-b9dd-dcf2-1b14-0bff4b7b8f1b
---

# OVER Clause (Transact-SQL) - SQL Server | Microsoft Learn

**Applies to:**![](../../includes/media/yes-icon.svg)[SQL Server](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Database](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Managed Instance](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure Synapse Analytics](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Analytics Platform System (PDW)](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL analytics endpoint in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Warehouse in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL database in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)

The `OVER` clause determines the partitioning and ordering of a rowset before the associated window function is applied. That is, the `OVER` clause defines a window or user-specified set of rows within a query result set. A window function then computes a value for each row in the window. You can use the `OVER` clause with functions to compute aggregated values such as moving averages, cumulative aggregates, running totals, or top *N* per group results.

- [Ranking Functions](../functions/ranking-functions-transact-sql)
- [Aggregate Functions](../functions/aggregate-functions-transact-sql)
- [Analytic functions](../functions/analytic-functions-transact-sql)
- [NEXT VALUE FOR](../functions/next-value-for-transact-sql)

![](../../includes/media/topic-link-icon.svg)[Transact-SQL syntax conventions](../language-elements/transact-sql-syntax-conventions-transact-sql)

## Syntax

```syntaxsql
OVER (
       [ <PARTITION BY clause> ]
       [ <ORDER BY clause> ]
       [ <ROW or RANGE clause> ]
      )

<PARTITION BY clause> ::=
PARTITION BY value_expression , ... [ n ]

<ORDER BY clause> ::=
ORDER BY order_by_expression
    [ COLLATE collation_name ]
    [ ASC | DESC ]
    [ , ...n ]

<ROW or RANGE clause> ::=
{ ROWS | RANGE } <window frame extent>

<window frame extent> ::=
{   <window frame preceding>
  | <window frame between>
}
<window frame between> ::=
  BETWEEN <window frame bound> AND <window frame bound>

<window frame bound> ::=
{   <window frame preceding>
  | <window frame following>
}

<window frame preceding> ::=
{
    UNBOUNDED PRECEDING
  | <unsigned_value_specification> PRECEDING
  | CURRENT ROW
}

<window frame following> ::=
{
    UNBOUNDED FOLLOWING
  | <unsigned_value_specification> FOLLOWING
  | CURRENT ROW
}

<unsigned value specification> ::=
{  <unsigned integer literal> }
```

Syntax only for Analytics Platform System (PDW):

```syntaxsql
OVER ( [ PARTITION BY value_expression ] [ order_by_clause ] )
```

## Arguments

Window functions can include the following arguments in their `OVER` clause:

- PARTITION BY divides the query result set into partitions.
- ORDER BY defines the logical order of the rows within each partition of the result set.
- ROWS or RANGE limits the rows within the partition by specifying start and end points within the partition. It requires an `ORDER BY` argument. If you specify `ORDER BY`, the default value is from the start of the partition to the current element.

If you don't specify any argument, the window functions apply to the entire result set.

```sql
SELECT object_id,
       MIN(object_id) OVER () AS [min],
       MAX(object_id) OVER () AS [max]
FROM sys.objects;
```

| object\_id | min | max |
| --- | --- | --- |
| 3 | 3 | 2139154666 |
| 5 | 3 | 2139154666 |
| ... | ... | ... |
| 2123154609 | 3 | 2139154666 |
| 2139154666 | 3 | 2139154666 |

### PARTITION BY

Divides the query result set into partitions. The window function applies to each partition separately, and computation restarts for each partition.

```syntaxsql
PARTITION BY <value_expression>
```

If you don't specify `PARTITION BY`, the function treats all rows of the query result set as a single partition.

If you don't specify an `ORDER BY` clause, the function applies to all rows in the partition.

#### PARTITION BY *value\_expression*

Specifies the column by which the rowset is partitioned. *value\_expression* can only refer to columns made available by the `FROM` clause. *value\_expression* can't refer to expressions or aliases in the select list. *value\_expression* can be a column expression, scalar subquery, scalar function, or user-defined variable.

```sql
SELECT object_id,
       type,
       MIN(object_id) OVER (PARTITION BY type) AS [min],
       MAX(object_id) OVER (PARTITION BY type) AS [max]
FROM sys.objects;
```

| object\_id | type | min | max |
| --- | --- | --- | --- |
| 68195293 | PK | 68195293 | 711673583 |
| 631673298 | PK | 68195293 | 711673583 |
| 711673583 | PK | 68195293 | 711673583 |
| ... | ... | ... | ... |
| 3 | S | 3 | 98 |
| 5 | S | 3 | 98 |
| ... | ... | ... | ... |
| 98 | S | 3 | 98 |
| ... | ... | ... | ... |

### ORDER BY

```syntaxsql
ORDER BY <order_by_expression> [ COLLATE <collation_name> ] [ ASC | DESC ]
```

Defines the logical order of the rows within each partition of the result set. It specifies the logical order in which the window function calculation is performed.

- If you don't specify an order, the default order is `ASC` and the window function uses all rows in the partition.
- If you specify an order but don't specify `ROWS` or `RANGE`, the functions that can accept an optional `ROWS` or `RANGE` specification (for example, `MIN` or `MAX`) use `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` as the default window frame.

```sql
SELECT object_id,
       type,
       MIN(object_id) OVER (PARTITION BY type ORDER BY object_id) AS [min],
       MAX(object_id) OVER (PARTITION BY type ORDER BY object_id) AS [max]
FROM sys.objects;
```

| object\_id | type | min | max |
| --- | --- | --- | --- |
| 68195293 | PK | 68195293 | 68195293 |
| 631673298 | PK | 68195293 | 631673298 |
| 711673583 | PK | 68195293 | 711673583 |
| ... | ... | ... |  |
| 3 | S | 3 | 3 |
| 5 | S | 3 | 5 |
| 6 | S | 3 | 6 |
| ... | ... | ... |  |
| 97 | S | 3 | 97 |
| 98 | S | 3 | 98 |
| ... | ... | ... |  |

#### *order\_by\_expression*

Specifies a column or expression on which to sort. *order\_by\_expression* can only refer to columns made available by the `FROM` clause. You can't specify an integer to represent a column name or alias.

#### COLLATE *collation\_name*

Specifies that the `ORDER BY` operation should be performed according to the collation specified in *collation\_name*. *collation\_name* can be either a Windows collation name or a SQL collation name. For more information, see [Collation and Unicode support](../../relational-databases/collations/collation-and-unicode-support). `COLLATE` is applicable only for columns of type **char**, **varchar**, **nchar**, and **nvarchar**.

#### { ASC | DESC }

Specifies that the values in the specified column should be sorted in ascending or descending order. `ASC` is the default sort order. `NULL` values are the lowest possible values.

### ROWS or RANGE

**Applies to**: SQL Server 2012 (11.x) and later versions.

These options further limit the rows within the partition by specifying start and end points within the partition. You specify a range of rows with respect to the current row either by logical association or physical association. You achieve physical association by using the `ROWS` clause.

The `ROWS` clause limits the rows within a partition by specifying a fixed number of rows preceding or following the current row. Alternatively, the `RANGE` clause logically limits the rows within a partition by specifying a range of values with respect to the value in the current row. Preceding and following rows are defined based on the ordering in the `ORDER BY` clause. The window frame `RANGE ... CURRENT ROW ...` includes all rows that have the same values in the `ORDER BY` expression as the current row. For example, `ROWS BETWEEN 2 PRECEDING AND CURRENT ROW` means that the window of rows that the function operates on is three rows in size, starting with two rows preceding until and including the current row.

```sql
SELECT object_id,
       COUNT(*) OVER (ORDER BY object_id ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS [preceding],
       COUNT(*) OVER (ORDER BY object_id ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING) AS [central],
       COUNT(*) OVER (ORDER BY object_id ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS [following]
FROM sys.objects
ORDER BY object_id ASC;
```

| object\_id | preceding | central | following |
| --- | --- | --- | --- |
| 3 | 1 | 3 | 156 |
| 5 | 2 | 4 | 155 |
| 6 | 3 | 5 | 154 |
| 7 | 4 | 5 | 153 |
| 8 | 5 | 5 | 152 |
| ... | ... | ... | ... |
| 2112726579 | 153 | 5 | 4 |
| 2119678599 | 154 | 5 | 3 |
| 2123154609 | 155 | 4 | 2 |
| 2139154666 | 156 | 3 | 1 |

`ROWS` or `RANGE` requires that you specify the `ORDER BY` clause. If `ORDER BY` contains multiple order expressions, `CURRENT ROW FOR RANGE` considers all columns in the `ORDER BY` list when determining the current row.

#### UNBOUNDED PRECEDING

**Applies to**: SQL Server 2012 (11.x) and later versions.

Specifies that the window starts at the first row of the partition. You can only specify `UNBOUNDED PRECEDING` as the window starting point.

#### &lt;unsigned value specification&gt; PRECEDING

Specify with `<unsigned value specification>` to indicate the number of rows or values to precede the current row. This specification isn't allowed for `RANGE`.

#### CURRENT ROW

**Applies to**: SQL Server 2012 (11.x) and later versions.

Specifies that the window starts or ends at the current row when used with `ROWS` or the current value when used with `RANGE`. You can specify `CURRENT ROW` as both a starting and ending point.

#### BETWEEN AND

**Applies to**: SQL Server 2012 (11.x) and later versions.

```syntaxsql
BETWEEN <window frame bound> AND <window frame bound>
```

Used with either `ROWS` or `RANGE` to specify the lower (starting) and upper (ending) boundary points of the window. `<window frame bound>` defines the boundary starting point and `<window frame bound>` defines the boundary endpoint. The upper bound can't be smaller than the lower bound.

#### UNBOUNDED FOLLOWING

**Applies to**: SQL Server 2012 (11.x) and later versions.

Specifies that the window ends at the last row of the partition. You can only specify `UNBOUNDED FOLLOWING` as a window endpoint. For example, `RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING` defines a window that starts with the current row and ends with the last row of the partition.

#### &lt;unsigned value specification&gt; FOLLOWING

Specify with `<unsigned value specification>` to indicate the number of rows or values to follow the current row. When you specify `<unsigned value specification> FOLLOWING` as the window starting point, the ending point must be `<unsigned value specification> FOLLOWING`. For example, `ROWS BETWEEN 2 FOLLOWING AND 10 FOLLOWING` defines a window that starts with the second row that follows the current row and ends with the tenth row that follows the current row. This specification isn't allowed for `RANGE`.

#### &lt;unsigned integer literal&gt;

**Applies to**: SQL Server 2012 (11.x) and later versions.

A positive integer literal (including `0`) that specifies the number of rows or values to precede or follow the current row or value. This specification is valid only for `ROWS`.

## Remarks

You can use more than one window function in a single query with a single `FROM` clause. The `OVER` clause for each function can differ in partitioning and ordering.

If you don't specify `PARTITION BY`, the function treats all rows of the query result set as a single group.

Important

If you specify `ROWS` or `RANGE` and use `<window frame preceding>` for `<window frame extent>` (short syntax), the query uses this specification for the window frame boundary starting point and `CURRENT ROW` for the boundary ending point. For example, `ROWS 5 PRECEDING` is equal to `ROWS BETWEEN 5 PRECEDING AND CURRENT ROW`.

If you don't specify `ORDER BY`, the entire partition is used for a window frame. This rule applies only to functions that don't require an `ORDER BY` clause. If you don't specify `ROWS` or `RANGE` but you specify `ORDER BY`, `RANGE UNBOUNDED PRECEDING AND CURRENT ROW` is used as the default for the window frame. This rule applies only to functions that can accept an optional `ROWS` or `RANGE` specification. For example, ranking functions can't accept `ROWS` or `RANGE`, so this window frame isn't applied even though `ORDER BY` is present and `ROWS` or `RANGE` isn't.

## Limitations

You can't use the `OVER` clause with the `DISTINCT` aggregations.

You can't use `RANGE` with `<unsigned value specification> PRECEDING` or `<unsigned value specification> FOLLOWING`.

Support for `ORDER BY` clause, and the `ROWS` and `RANGE` clauses, depends on the ranking, aggregate, or analytic function that you use with the `OVER` clause.

## Performance considerations

With window functions, the SQL Database Engine is often tasked with partitioning and sorting large datasets for complex analytical queries. Use the following techniques to keep queries with window functions efficient.

### Provide a supporting index

For high-value or frequently executed queries that use window functions, consider creating a new nonclustered index. To benefit a query that uses a window function, the position of the key columns in the new nonclustered index must match the `PARTITION BY` columns followed by the `ORDER BY` columns, if any. If an `ORDER BY` clause is present, the order of the index key columns must also match the order specified in the `ORDER BY` clause. For example:

```sql
SELECT CustomerID,
       OrderDate,
       SUM(TotalDue) OVER (PARTITION BY CustomerID ORDER BY OrderDate) AS RunningTotal
FROM Sales.SalesOrderHeader;
```

This query might benefit from this rowstore nonclustered index:

```sql
CREATE INDEX IX_SalesOrderHeader_Customer_OrderDate
    ON Sales.SalesOrderHeader (CustomerID, OrderDate)
    INCLUDE (TotalDue);
```

### Benefit from batch mode execution

**Window Aggregate** operators might run faster in [batch mode](../../relational-databases/performance/intelligent-query-processing-details#batch-mode-on-rowstore) than in row mode. With batch mode processing, query operators work on batches of rows instead of one row at a time. Batch mode is possible in the following cases:

- The query references a table that has a [columnstore index](../../relational-databases/indexes/columnstore-indexes-overview). For more information, see [Columnstore indexes - query performance](../../relational-databases/indexes/columnstore-indexes-query-performance).
- The query runs on rowstore tables (heap or B+ tree) with database compatibility level 150 or higher, starting in SQL Server 2019 (15.x).

You can't force a query to use batch mode. The SQL Database Engine uses it when possible and judged beneficial. A set of execution plan operators can use batch mode for both rowstore and columnstore objects. To confirm that batch mode is used, look for the operator in the actual execution plan, such as **Window Aggregate**, and verify that the operator's `Actual Execution Mode` property is `Batch`. For more information on execution plan operators, see [Logical and physical showplan operator reference](../../relational-databases/showplan-logical-and-physical-operators-reference).

### Avoid sort spills

Underestimated cardinality can cause the sort operation to require more memory at execution time. This process can increase query cost. To mitigate spills:

- Ensure statistics cover the partitioning and ordering columns. Ensure these statistics are up to date. For more information, see [Statistics](../../relational-databases/statistics/statistics).
- Ensure that [Memory Grant Feedback](../../relational-databases/performance/intelligent-query-processing-memory-grant-feedback) is enabled. Memory grant feedback helps the Query Optimizer adjust memory grants on subsequent executions. To ensure your workloads automatically eligible for memory grant feedback, use database compatibility level 140 or higher. When enabled, this setting appears as enabled in [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql).
- Reduce input row count with `WHERE` filters.

## Examples

The code samples in this article use the `AdventureWorks2025` or `AdventureWorksDW2025` sample database, which you can download from the [Microsoft SQL Server Samples and Community Projects](https://go.microsoft.com/fwlink/?LinkID=85384) home page.

### A. Use the OVER clause with the ROW\_NUMBER function

The following example shows how to use the `OVER` clause with the `ROW_NUMBER` function to display a row number for each row within a partition. The `ORDER BY` clause specified in the `OVER` clause orders the rows in each partition by the column `SalesYTD`. The `ORDER BY` clause in the `SELECT` statement determines the order in which the entire query result set is returned.

```sql
USE AdventureWorks2025;
GO

SELECT ROW_NUMBER() OVER (PARTITION BY PostalCode ORDER BY SalesYTD DESC) AS "Row Number",
       p.LastName,
       s.SalesYTD,
       a.PostalCode
FROM Sales.SalesPerson AS s
     INNER JOIN Person.Person AS p
         ON s.BusinessEntityID = p.BusinessEntityID
     INNER JOIN Person.Address AS a
         ON a.AddressID = p.BusinessEntityID
WHERE TerritoryID IS NOT NULL
      AND SalesYTD <> 0
ORDER BY PostalCode;
GO
```

Here's the result set.

```output
Row Number      LastName                SalesYTD              PostalCode
--------------- ----------------------- --------------------- ----------
1               Mitchell                4251368.5497          98027
2               Blythe                  3763178.1787          98027
3               Carson                  3189418.3662          98027
4               Reiter                  2315185.611           98027
5               Vargas                  1453719.4653          98027
6               Ansman-Wolfe            1352577.1325          98027
1               Pak                     4116871.2277          98055
2               Varkey Chudukatil       3121616.3202          98055
3               Saraiva                 2604540.7172          98055
4               Ito                     2458535.6169          98055
5               Valdez                  1827066.7118          98055
6               Mensa-Annan             1576562.1966          98055
7               Campbell                1573012.9383          98055
8               Tsoflias                1421810.9242          98055
```

### B. Use the OVER clause with aggregate functions

The following example uses the `OVER` clause with aggregate functions over all rows returned by the query. In this example, using the `OVER` clause is more efficient than using subqueries to derive the aggregate values.

```sql
USE AdventureWorks2025;
GO

SELECT SalesOrderID,
       ProductID,
       OrderQty,
       SUM(OrderQty) OVER (PARTITION BY SalesOrderID) AS Total,
       AVG(OrderQty) OVER (PARTITION BY SalesOrderID) AS "Avg",
       COUNT(OrderQty) OVER (PARTITION BY SalesOrderID) AS "Count",
       MIN(OrderQty) OVER (PARTITION BY SalesOrderID) AS "Min",
       MAX(OrderQty) OVER (PARTITION BY SalesOrderID) AS "Max"
FROM Sales.SalesOrderDetail
WHERE SalesOrderID IN (43659, 43664);
GO
```

Here's the result set.

```output
SalesOrderID ProductID   OrderQty Total       Avg         Count       Min    Max
------------ ----------- -------- ----------- ----------- ----------- ------ ------
43659        776         1        26          2           12          1      6
43659        777         3        26          2           12          1      6
43659        778         1        26          2           12          1      6
43659        771         1        26          2           12          1      6
43659        772         1        26          2           12          1      6
43659        773         2        26          2           12          1      6
43659        774         1        26          2           12          1      6
43659        714         3        26          2           12          1      6
43659        716         1        26          2           12          1      6
43659        709         6        26          2           12          1      6
43659        712         2        26          2           12          1      6
43659        711         4        26          2           12          1      6
43664        772         1        14          1           8           1      4
43664        775         4        14          1           8           1      4
43664        714         1        14          1           8           1      4
43664        716         1        14          1           8           1      4
43664        777         2        14          1           8           1      4
43664        771         3        14          1           8           1      4
43664        773         1        14          1           8           1      4
43664        778         1        14          1           8           1      4
```

The following example shows how to use the `OVER` clause with an aggregate function in a calculated value.

```sql
USE AdventureWorks2025;
GO

SELECT SalesOrderID,
       ProductID,
       OrderQty,
       SUM(OrderQty) OVER (PARTITION BY SalesOrderID) AS Total,
       CAST (1. * OrderQty / SUM(OrderQty) OVER (PARTITION BY SalesOrderID) * 100 AS DECIMAL (5, 2)) AS [Percent by ProductID]
FROM Sales.SalesOrderDetail
WHERE SalesOrderID IN (43659, 43664);
GO
```

Here's the result set. The aggregates are calculated by `SalesOrderID` and the `Percent by ProductID` is calculated for each line of each `SalesOrderID`.

```output
SalesOrderID ProductID   OrderQty Total       Percent by ProductID
------------ ----------- -------- ----------- ---------------------------------------
43659        776         1        26          3.85
43659        777         3        26          11.54
43659        778         1        26          3.85
43659        771         1        26          3.85
43659        772         1        26          3.85
43659        773         2        26          7.69
43659        774         1        26          3.85
43659        714         3        26          11.54
43659        716         1        26          3.85
43659        709         6        26          23.08
43659        712         2        26          7.69
43659        711         4        26          15.38
43664        772         1        14          7.14
43664        775         4        14          28.57
43664        714         1        14          7.14
43664        716         1        14          7.14
43664        777         2        14          14.29
43664        771         3        14          21.4
43664        773         1        14          7.14
43664        778         1        14          7.14
```

### C. Produce a moving average and cumulative total

The following example uses the `AVG` and `SUM` functions with the `OVER` clause to provide a moving average and cumulative total of yearly sales for each territory in the `Sales.SalesPerson` table. The query partitions the data by `TerritoryID` and logically orders it by `SalesYTD`. This use of `OVER` means that the `AVG` function is computed for each territory based on the sales year. For `TerritoryID` of `1`, two rows exist for sales year `2022`, representing the two sales people with sales that year. The average sales for these two rows are computed, and then the third row representing sales for the year `2023` is included in the computation.

```sql
USE AdventureWorks2025;
GO

SELECT BusinessEntityID,
       TerritoryID,
       DATEPART(yy, ModifiedDate) AS SalesYear,
       CONVERT (VARCHAR (20), SalesYTD, 1) AS SalesYTD,
       CONVERT (VARCHAR (20), AVG(SalesYTD) OVER (PARTITION BY TerritoryID ORDER BY DATEPART(yy, ModifiedDate)), 1) AS MovingAvg,
       CONVERT (VARCHAR (20), SUM(SalesYTD) OVER (PARTITION BY TerritoryID ORDER BY DATEPART(yy, ModifiedDate)), 1) AS CumulativeTotal
FROM Sales.SalesPerson
WHERE TerritoryID IS NULL
      OR TerritoryID < 5
ORDER BY TerritoryID, SalesYear;
```

Here's the result set.

```output
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal
---------------- ----------- ----------- -------------------- -------------------- --------------------
274              NULL        2021        559,697.56           559,697.56           559,697.56
287              NULL        2023        519,905.93           539,801.75           1,079,603.50
285              NULL        2024        172,524.45           417,375.98           1,252,127.95
283              1           2022        1,573,012.94         1,462,795.04         2,925,590.07
280              1           2022        1,352,577.13         1,462,795.04         2,925,590.07
284              1           2023        1,576,562.20         1,500,717.42         4,502,152.27
275              2           2022        3,763,178.18         3,763,178.18         3,763,178.18
277              3           2022        3,189,418.37         3,189,418.37         3,189,418.37
276              4           2022        4,251,368.55         3,354,952.08         6,709,904.17
281              4           2022        2,458,535.62         3,354,952.08         6,709,904.17
```

In this example, the `OVER` clause doesn't include `PARTITION BY`. This means that the function is applied to all rows returned by the query. The `ORDER BY` clause specified in the `OVER` clause determines the logical order to which the `AVG` function is applied. The query returns a moving average of sales by year for all sales territories specified in the `WHERE` clause. The `ORDER BY` clause specified in the `SELECT` statement determines the order in which the rows of the query are displayed.

```sql
SELECT BusinessEntityID,
       TerritoryID,
       DATEPART(yy, ModifiedDate) AS SalesYear,
       CONVERT (VARCHAR (20), SalesYTD, 1) AS SalesYTD,
       CONVERT (VARCHAR (20), AVG(SalesYTD) OVER (ORDER BY DATEPART(yy, ModifiedDate)), 1) AS MovingAvg,
       CONVERT (VARCHAR (20), SUM(SalesYTD) OVER (ORDER BY DATEPART(yy, ModifiedDate)), 1) AS CumulativeTotal
FROM Sales.SalesPerson
WHERE TerritoryID IS NULL
      OR TerritoryID < 5
ORDER BY SalesYear;
```

Here's the result set.

```output
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal
---------------- ----------- ----------- -------------------- -------------------- --------------------
274              NULL        2021        559,697.56           559,697.56           559,697.56
275              2           2022        3,763,178.18         2,449,684.05         17,147,788.35
276              4           2022        4,251,368.55         2,449,684.05         17,147,788.35
277              3           2022        3,189,418.37         2,449,684.05         17,147,788.35
280              1           2022        1,352,577.13         2,449,684.05         17,147,788.35
281              4           2022        2,458,535.62         2,449,684.05         17,147,788.35
283              1           2022        1,573,012.94         2,449,684.05         17,147,788.35
284              1           2023        1,576,562.20         2,138,250.72         19,244,256.47
287              NULL        2023        519,905.93           2,138,250.72         19,244,256.47
285              NULL        2024        172,524.45           1,941,678.09         19,416,780.93
```

### D. Specify the ROWS clause

**Applies to**: SQL Server 2012 (11.x) and later versions.

The following example uses the `ROWS` clause to define a window over which the rows are computed as the current row and the *N* number of rows that follow (one row in this example).

```sql
SELECT BusinessEntityID,
       TerritoryID,
       CONVERT (VARCHAR (20), SalesYTD, 1) AS SalesYTD,
       DATEPART(yy, ModifiedDate) AS SalesYear,
       CONVERT (VARCHAR (20), SUM(SalesYTD) OVER (PARTITION BY TerritoryID ORDER BY DATEPART(yy, ModifiedDate) ROWS BETWEEN CURRENT ROW AND 1 FOLLOWING), 1) AS CumulativeTotal
FROM Sales.SalesPerson
WHERE TerritoryID IS NULL
      OR TerritoryID < 5;
```

Here's the result set.

```output
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal
---------------- ----------- -------------------- ----------- --------------------
274              NULL        559,697.56           2021        1,079,603.50
287              NULL        519,905.93           2023        692,430.38
285              NULL        172,524.45           2024        172,524.45
283              1           1,573,012.94         2022        2,925,590.07
280              1           1,352,577.13         2022        2,929,139.33
284              1           1,576,562.20         2023        1,576,562.20
275              2           3,763,178.18         2022        3,763,178.18
277              3           3,189,418.37         2022        3,189,418.37
276              4           4,251,368.55         2022        6,709,904.17
281              4           2,458,535.62         2022        2,458,535.62
```

In the following example, the `ROWS` clause is specified with `UNBOUNDED PRECEDING`. The result is that the window starts at the first row of the partition.

```sql
SELECT BusinessEntityID,
       TerritoryID,
       CONVERT (VARCHAR (20), SalesYTD, 1) AS SalesYTD,
       DATEPART(yy, ModifiedDate) AS SalesYear,
       CONVERT (VARCHAR (20), SUM(SalesYTD) OVER (PARTITION BY TerritoryID ORDER BY DATEPART(yy, ModifiedDate) ROWS UNBOUNDED PRECEDING), 1) AS CumulativeTotal
FROM Sales.SalesPerson
WHERE TerritoryID IS NULL
      OR TerritoryID < 5;
```

Here's the result set.

```output
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal
---------------- ----------- -------------------- ----------- --------------------
274              NULL        559,697.56           2021        559,697.56
287              NULL        519,905.93           2023        1,079,603.50
285              NULL        172,524.45           2024        1,252,127.95
283              1           1,573,012.94         2022        1,573,012.94
280              1           1,352,577.13         2022        2,925,590.07
284              1           1,576,562.20         2023        4,502,152.27
275              2           3,763,178.18         2022        3,763,178.18
277              3           3,189,418.37         2022        3,189,418.37
276              4           4,251,368.55         2022        4,251,368.55
281              4           2,458,535.62         2022        6,709,904.17
```

## Examples: Analytics Platform System (PDW)

### E. Use the OVER clause with the ROW\_NUMBER function

The following example returns the `ROW_NUMBER` for sales representatives based on their assigned sales quota.

```sql
SELECT ROW_NUMBER() OVER (ORDER BY SUM(SalesAmountQuota) DESC) AS RowNumber,
       FirstName,
       LastName,
       CONVERT (VARCHAR (13), SUM(SalesAmountQuota), 1) AS SalesQuota
FROM dbo.DimEmployee AS e
     INNER JOIN dbo.FactSalesQuota AS sq
         ON e.EmployeeKey = sq.EmployeeKey
WHERE e.SalesPersonFlag = 1
GROUP BY LastName, FirstName;
```

Here's a partial result set.

```output
RowNumber  FirstName  LastName            SalesQuota
---------  ---------  ------------------  -------------
1          Jillian    Carson              12,198,000.00
2          Linda      Mitchell            11,786,000.00
3          Michael    Blythe              11,162,000.00
4          Jae        Pak                 10,514,000.00
```

### F. Use the OVER clause with aggregate functions

The following examples show using the `OVER` clause with aggregate functions. In this example, using the `OVER` clause is more efficient than using subqueries.

```sql
SELECT SalesOrderNumber AS OrderNumber,
       ProductKey,
       OrderQuantity AS Qty,
       SUM(OrderQuantity) OVER (PARTITION BY SalesOrderNumber) AS Total,
       AVG(OrderQuantity) OVER (PARTITION BY SalesOrderNumber) AS AVG,
       COUNT(OrderQuantity) OVER (PARTITION BY SalesOrderNumber) AS COUNT,
       MIN(OrderQuantity) OVER (PARTITION BY SalesOrderNumber) AS MIN,
       MAX(OrderQuantity) OVER (PARTITION BY SalesOrderNumber) AS MAX
FROM dbo.FactResellerSales
WHERE SalesOrderNumber IN (N'SO43659', N'SO43664')
      AND ProductKey LIKE '2%'
ORDER BY SalesOrderNumber, ProductKey;
```

Here's the result set.

```output
OrderNumber  Product  Qty  Total  Avg  Count  Min  Max
-----------  -------  ---  -----  ---  -----  ---  ---
SO43659      218      6    16     3    5      1    6
SO43659      220      4    16     3    5      1    6
SO43659      223      2    16     3    5      1    6
SO43659      229      3    16     3    5      1    6
SO43659      235      1    16     3    5      1    6
SO43664      229      1     2     1    2      1    1
SO43664      235      1     2     1    2      1    1
```

The following example shows using the `OVER` clause with an aggregate function in a calculated value. The aggregates are calculated by `SalesOrderNumber` and the percentage of the total sales order is calculated for each line of each `SalesOrderNumber`.

```sql
SELECT SalesOrderNumber AS OrderNumber,
       ProductKey AS Product,
       OrderQuantity AS Qty,
       SUM(OrderQuantity) OVER (PARTITION BY SalesOrderNumber) AS Total,
       CAST (1. * OrderQuantity / SUM(OrderQuantity) OVER (PARTITION BY SalesOrderNumber) * 100 AS DECIMAL (5, 2)) AS PctByProduct
FROM dbo.FactResellerSales
WHERE SalesOrderNumber IN (N'SO43659', N'SO43664')
      AND ProductKey LIKE '2%'
ORDER BY SalesOrderNumber, ProductKey;
```

The first start of this result set is as follows:

```output
OrderNumber  Product  Qty  Total  PctByProduct
-----------  -------  ---  -----  ------------
SO43659      218      6    16     37.50
SO43659      220      4    16     25.00
SO43659      223      2    16     12.50
SO43659      229      2    16     18.75
```