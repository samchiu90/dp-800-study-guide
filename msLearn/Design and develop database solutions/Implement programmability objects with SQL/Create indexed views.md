---
layout: Conceptual
monikers:
- azuresqldb-current
- azuresqldb-mi-current
- fabric-sqldb
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
title: Create Indexed Views - SQL Server | Microsoft Learn
canonicalUrl: https://learn.microsoft.com/en-us/sql/relational-databases/views/create-indexed-views?view=sql-server-ver17
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
description: Creating a unique clustered index on a view improves query performance, because the view is stored in the same way as a clustered index is stored.
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: randolphwest
ms.date: 2025-01-14T00:00:00.0000000Z
ms.service: sql
ms.subservice: table-view-index
ms.topic: how-to
ms.custom:
- ignite-2025
locale: en-us
document_id: 0a452207-5ac2-1a41-c5b1-3baf085a79b4
document_version_independent_id: a590e66c-ed9f-4d80-1511-1864128d2e8e
updated_at: 2026-08-24T22:38:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/sql-docs-pr/blob/live/docs/relational-databases/views/create-indexed-views.md
gitcommit: https://github.com/MicrosoftDocs/sql-docs-pr/blob/58c9ba7063458281263c05cba4e0e8dc7da7358a/docs/relational-databases/views/create-indexed-views.md
git_commit_id: 58c9ba7063458281263c05cba4e0e8dc7da7358a
default_moniker: sql-server-ver17
site_name: Docs
depot_name: SQL.sql-content
page_type: conceptual
toc_rel: ../../toc.json
pdf_url_template: https://learn.microsoft.com/pdfstore/en-us/SQL.sql-content/{branchName}{pdfName}
word_count: 2482
asset_id: relational-databases/views/create-indexed-views
moniker_range_name: 627d70ef1081c08390b0e386b3e9f49d
monikers:
- azuresqldb-current
- azuresqldb-mi-current
- fabric-sqldb
- sql-server-linux-2017
- sql-server-linux-ver15
- sql-server-linux-ver16
- sql-server-linux-ver17
- sql-server-2017
- sql-server-ver15
- sql-server-ver16
- sql-server-ver17
item_type: Content
source_path: docs/relational-databases/views/create-indexed-views.md
cmProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/8b896464-3b7d-4e1f-84b0-9bb45aeb5f64
- https://authoring-docs-microsoft.poolparty.biz/devrel/540ac133-a371-4dbb-8f94-28d6cc77a70b
- https://authoring-docs-microsoft.poolparty.biz/devrel/cbe4ca68-43ac-4375-aba5-5945a6394c20
spProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/b1d2d671-9549-46e8-918c-24349120dbf5
- https://authoring-docs-microsoft.poolparty.biz/devrel/60bfc045-f127-4841-9d00-ea35495a5800
- https://authoring-docs-microsoft.poolparty.biz/devrel/ced846cc-6a3c-4c8f-9dfb-3de0e90e2742
platformId: 6f43702a-e35a-d97e-495e-a8d85bb114df
---

# Create Indexed Views - SQL Server | Microsoft Learn

**Applies to:**![](../../includes/media/yes-icon.svg)[SQL Server](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Database](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Managed Instance](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL database in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)

This article describes how to create indexes on a view. The first index created on a view must be a unique clustered index. After the unique clustered index has been created, you can create more nonclustered indexes. Creating a unique clustered index on a view improves query performance, because the view is stored in the database in the same way a table with a clustered index is stored. The query optimizer can use indexed views to speed up the query execution. The view doesn't have to be referenced in the query for the optimizer to consider that view for a substitution.

## Steps

The following steps are required to create an indexed view and are critical to the successful implementation of the indexed view:

1. Verify the `SET` options are correct for all existing tables that will be referenced in the view.
2. Verify that the `SET` options for the session are set correctly before you create any tables and the view.
3. Verify that the view definition is deterministic.
4. Verify that the base table has the same owner as the view.
5. Create the view by using the `WITH SCHEMABINDING` option.
6. Create the unique clustered index on the view.

When you execute `UPDATE`, `DELETE` or `INSERT` operations (Data Manipulation Language, or DML) on a table referenced by a large number of indexed views, or fewer but complex indexed views, those referenced indexed views have to be updated as well. As a result, DML query performance can degrade significantly, or in some cases, a query plan can't even be produced.

In such scenarios, test your DML queries before production use, analyze the query plan and tune/simplify the DML statement.

## Required SET options for indexed views

Evaluating the same expression can produce different results in the Database Engine when different `SET` options are active when the query is executed. For example, after the `SET` option `CONCAT_NULL_YIELDS_NULL` is set to `ON`, the expression `'abc' + NULL` returns the value `NULL`. However, after `CONCAT_NULL_YIELDS_NULL` is set to `OFF`, the same expression produces `abc`.

To make sure that the views can be maintained correctly and return consistent results, indexed views require fixed values for several `SET` options. The `SET` options in the following table must be set to the values shown in the `Required value` column whenever the following conditions occur:

- The view and subsequent indexes on the view are created.
- The base tables referenced in the view at the time the view is created.
- When any insert, update, or delete operation is performed on any table that participates in the indexed view. This requirement includes operations such as bulk copy, replication, and distributed queries.
- The indexed view is used by the query optimizer to produce the query plan.

| SET options | Required value | Default server value | DefaultOLE DB and ODBC value | DefaultDB-Library value |
| --- | --- | --- | --- | --- |
| `ANSI_NULLS` | `ON` | `ON` | `ON` | `OFF` |
| `ANSI_PADDING` | `ON` | `ON` | `ON` | `OFF` |
| `ANSI_WARNINGS`^1^ | `ON` | `ON` | `ON` | `OFF` |
| `ARITHABORT` | `ON` | `ON` | `OFF` | `OFF` |
| `CONCAT_NULL_YIELDS_NULL` | `ON` | `ON` | `ON` | `OFF` |
| `NUMERIC_ROUNDABORT` | `OFF` | `OFF` | `OFF` | `OFF` |
| `QUOTED_IDENTIFIER` | `ON` | `ON` | `ON` | `OFF` |

^1^ Setting `ANSI_WARNINGS` to `ON` implicitly sets `ARITHABORT` to `ON`.

If you use an OLE DB or ODBC server connection, the only value that must be modified is the `ARITHABORT` setting. All DB-Library values must be set correctly either at the server level by using `sp_configure` or from the application by using the `SET` command.

Important

We strongly recommend that you set the `ARITHABORT` user option to `ON` server-wide as soon as the first indexed view or index on a computed column is created in any database on the server.

## Deterministic view requirement

The definition of an indexed view must be *deterministic*. A view is deterministic if all expressions in the select list, and the `WHERE` and `GROUP BY` clauses, are deterministic. Deterministic expressions always return the same result whenever they're evaluated with a specific set of input values. Only deterministic functions can participate in deterministic expressions. For example, the `DATEADD` function is deterministic because it always returns the same result for any given set of argument values for its three parameters. `GETDATE` isn't deterministic because it's always invoked with the same argument, but the value it returns changes each time it executes.

To determine whether a view column is deterministic, use the `IsDeterministic` property of the [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql) function. To determine if a deterministic column in a view with schema binding is precise, use the `IsPrecise` property of the `COLUMNPROPERTY` function. `COLUMNPROPERTY` returns `1` if `TRUE`, `0` if `FALSE`, and `NULL` for input that isn't valid. This means the column isn't deterministic or not precise.

Even if an expression is deterministic, if it contains float expressions, the exact result depends on the processor architecture or version of microcode. To ensure data integrity, such expressions can participate only as non-key columns of indexed views. Deterministic expressions that don't contain float expressions are called *precise*. Only precise deterministic expressions can participate in key columns and in `WHERE` or `GROUP BY` clauses of indexed views.

## Additional requirements

The following requirements must also be met, in addition to the `SET` options and deterministic function requirements

- The user that executes `CREATE INDEX` must be the owner of the view.
- When you create the index, the `IGNORE_DUP_KEY` index option must be set to `OFF` (the default setting).
- Tables must be referenced by two-part names, `<schema>.<tablename>`, in the view definition.
- User-defined functions referenced in the view must be created by using the `WITH SCHEMABINDING` option.
- Any user-defined functions referenced in the view must be referenced by two-part names, `<schema>.<function>`.
- The data access property of a user-defined function must be `NO SQL`, and external access property must be `NO`.
- Common language runtime (CLR) functions can appear in the select list of the view, but can't be part of the definition of the clustered index key. CLR functions can't appear in the `WHERE` clause of the view or the `ON` clause of a `JOIN` operation in the view.
- CLR functions and methods of CLR user-defined types used in the view definition must have the properties set as shown in the following table.

    | Property | Note |
    | --- | --- |
    | DETERMINISTIC = TRUE | Must be declared explicitly as an attribute of the Microsoft .NET Framework method. |
    | PRECISE = TRUE | Must be declared explicitly as an attribute of the .NET Framework method. |
    | DATA ACCESS = NO SQL | Determined by setting the `DataAccess` attribute to `DataAccessKind.None` and `SystemDataAccess` attribute to `SystemDataAccessKind.None`. |
    | EXTERNAL ACCESS = NO | This property defaults to NO for CLR routines. |
- The view must be created by using the `WITH SCHEMABINDING` option.
- The view must reference only base tables that are in the same database as the view. The view can't reference other views.
- If `GROUP BY` is present, the VIEW definition must contain `COUNT_BIG(*)` and must not contain `HAVING`. These `GROUP BY` restrictions are applicable only to the indexed view definition. A query can use an indexed view in its execution plan even if it doesn't satisfy these `GROUP BY` restrictions.
- If the view definition contains a `GROUP BY` clause, the key of the unique clustered index can reference only the columns specified in the `GROUP BY` clause.
- The `SELECT` statement in the view definition must not contain the following Transact-SQL syntax:

    | Transact-SQL function | Possible alternatives |
    | --- | --- |
    | `COUNT` | Use `COUNT_BIG` |
    | `ROWSET` functions (`OPENDATASOURCE`, `OPENQUERY`, `OPENROWSET`, and `OPENXML`) |  |
    | Arithmetic mean (`AVG`) | Use `COUNT_BIG` and `SUM` as separate columns |
    | Statistical aggregate functions (`STDEV`,`STDEVP`,`VAR`, and `VARP`) |  |
    | `SUM` function that references a nullable expression | Use `ISNULL` inside `SUM()` to make the expression non-nullable |
    | Other aggregate functions (`MIN`,`MAX`,`CHECKSUM_AGG`, and `STRING_AGG`) |  |
    | User-defined aggregate functions (SQL CLR) |  |

    | SELECT clause | Transact-SQL element | Possible alternative |
    | --- | --- | --- |
    | `WITH cte AS` | Common table expressions (CTE) `WITH` |  |
    | `SELECT` | Subqueries |  |
    | `SELECT` | `SELECT [ <table>. ] *` | Explicitly name columns |
    | `SELECT` | `SELECT DISTINCT` | Use `GROUP BY` |
    | `SELECT` | `SELECT TOP` |  |
    | `SELECT` | `OVER` clause, which includes ranking or aggregate window functions |  |
    | `FROM` | `LEFT OUTER JOIN` |  |
    | `FROM` | `RIGHT OUTER JOIN` |  |
    | `FROM` | `FULL OUTER JOIN` |  |
    | `FROM` | `OUTER APPLY` |  |
    | `FROM` | `CROSS APPLY` |  |
    | `FROM` | Derived table expressions (that is, using `SELECT` in the `FROM` clause) |  |
    | `FROM` | Self-joins |  |
    | `FROM` | Table variables |  |
    | `FROM` | Inline table-valued function |  |
    | `FROM` | Multi-statement table-valued function |  |
    | `FROM` | `PIVOT`, `UNPIVOT` |  |
    | `FROM` | `TABLESAMPLE` |  |
    | `FROM` | `FOR SYSTEM_TIME` | Query the temporal history table directly |
    | `WHERE` | Full-text predicates (`CONTAINS`, `FREETEXT`, `CONTAINSTABLE`, `FREETEXTTABLE`) |  |
    | `GROUP BY` | `CUBE`, `ROLLUP`, or `GROUPING SETS` operators | Define separate indexed views for each combination of `GROUP BY` columns |
    | `GROUP BY` | `HAVING` |  |
    | Set operators | `UNION`, `UNION ALL`, `EXCEPT`, `INTERSECT` | Use `OR`, `AND NOT`, and `AND` in the `WHERE` clause respectively |
    | `ORDER BY` | `ORDER BY` |  |
    | `ORDER BY` | `OFFSET` |  |

    | Source column type | Possible alternative |
    | --- | --- |
    | Deprecated large value column types (**text**, **ntext**, and **image**) | Migrate columns to **varchar(max)**, **nvarchar(max)**, and **varbinary(max)** respectively. |
    | **xml** or FILESTREAM columns |  |
    | **float**^1^ columns in index key |  |
    | Sparse column sets |  |

    ^1^ The indexed view can contain **float** columns; however, such columns can't be included in the clustered index key.

    Important

    Indexed views aren't supported on top of temporal queries (queries that use `FOR SYSTEM_TIME` clause).

## Recommendations for datetime and smalldatetime

When you refer to **datetime** and **smalldatetime** string literals in indexed views, we recommend that you explicitly convert the literal to the date type you want by using a deterministic date format style. For a list of the date format styles that are deterministic, see [CAST and CONVERT](../../t-sql/functions/cast-and-convert-transact-sql). For more information about deterministic and nondeterministic expressions, see the Considerations section in this page.

Expressions that involve implicit conversion of character strings to **datetime** or **smalldatetime** are considered nondeterministic. For more information, see [Nondeterministic conversion of literal date strings into DATE values](../../t-sql/data-types/nondeterministic-convert-date-literals).

## Performance considerations with indexed views

When you execute DML (such as `UPDATE`, `DELETE` or `INSERT`) on a table referenced by a large number of indexed views, or fewer but complex indexed views, those indexed views have to be updated as well during DML execution. As a result, DML query performance can degrade significantly, or in some cases, a query plan can't even be produced. In such scenarios, test your DML queries before production use, analyze the query plan and tune/simplify the DML statement.

To prevent the Database Engine from using indexed views, include the [OPTION (EXPAND VIEWS)](../../t-sql/queries/hints-transact-sql-query#expand-views) hint on the query. Also, if any of the listed options are incorrectly set, this option prevents the optimizer from using the indexes on the views. For more information about the `OPTION (EXPAND VIEWS)` hint, see [SELECT](../../t-sql/queries/select-transact-sql).

## Additional considerations

- The setting of the `large_value_types_out_of_row` option of columns in an indexed view is inherited from the setting of the corresponding column in the base table. This value is set by using [sp_tableoption](../system-stored-procedures/sp-tableoption-transact-sql). The default setting for columns formed from expressions is `0`. This means that large value types are stored in-row.
- Indexed views can be created on a partitioned table, and can themselves be partitioned.
- All indexes on a view are dropped when the view is dropped. All nonclustered indexes and auto-created statistics on the view are dropped when the clustered index is dropped. User-created statistics on the view are maintained. Nonclustered indexes can be individually dropped. Dropping the clustered index on the view removes the stored result set, and the optimizer returns to processing the view like a standard view.
- Indexes on tables and views can be disabled. When a clustered index on a table is disabled, indexes on views associated with the table are also disabled.

## Permissions

To create the view, a user needs to hold the `CREATE VIEW` permission in the database and `ALTER` permission on the schema in which the view is being created. If the base table resides within a different schema, the `REFERENCES` permission on the table is required as a minimum. If the user creating the index differs from the users who created the view, for the index creation alone the `ALTER` permission on the view is required (covered by `ALTER` on the schema).

Indexes can only be created on views that have the same owner as the referenced table or tables. This concept is also called an intact *ownership chain* between the view and the tables. Typically, when table and view reside within the same schema, the same schema owner applies to all objects within the schema. Therefore it's possible to create a view and not be the owner of the view. On the other hand, it's also possible that individual objects within a schema have different explicit owners. The `principal_id` column in `sys.tables` contains a value if the owner is different from the schema owner.

## Create an indexed view: a T-SQL example

The following example creates a view and an index on that view, in the `AdventureWorks` database.

```sql
--Set the options to support indexed views.
SET NUMERIC_ROUNDABORT OFF;
SET ANSI_PADDING,
    ANSI_WARNINGS,
    CONCAT_NULL_YIELDS_NULL,
    ARITHABORT,
    QUOTED_IDENTIFIER,
    ANSI_NULLS ON;

--Create view with SCHEMABINDING.
IF OBJECT_ID('Sales.vOrders', 'view') IS NOT NULL
    DROP VIEW Sales.vOrders;
GO

CREATE VIEW Sales.vOrders
    WITH SCHEMABINDING
AS
SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Revenue,
    OrderDate,
    ProductID,
    COUNT_BIG(*) AS COUNT
FROM Sales.SalesOrderDetail AS od,
    Sales.SalesOrderHeader AS o
WHERE od.SalesOrderID = o.SalesOrderID
GROUP BY OrderDate,
    ProductID;
GO

--Create an index on the view.
CREATE UNIQUE CLUSTERED INDEX IDX_V1 ON Sales.vOrders (
    OrderDate,
    ProductID
);
GO
```

The next two queries demonstrate how the indexed view can be used, even though the view isn't specified in the `FROM` clause.

```sql
--This query can use the indexed view even though the view is
--not specified in the FROM clause.
SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev,
    OrderDate,
    ProductID
FROM Sales.SalesOrderDetail AS od
INNER JOIN Sales.SalesOrderHeader AS o
    ON od.SalesOrderID = o.SalesOrderID
        AND o.OrderDate >= CONVERT(DATETIME, '05/01/2012', 101)
WHERE od.ProductID BETWEEN 700
        AND 800
GROUP BY OrderDate,
    ProductID
ORDER BY Rev DESC;
GO

--This query will also use the above indexed view.
SELECT OrderDate,
    SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev
FROM Sales.SalesOrderDetail AS od
INNER JOIN Sales.SalesOrderHeader AS o
    ON od.SalesOrderID = o.SalesOrderID
        AND o.OrderDate >= CONVERT(DATETIME, '03/01/2012', 101)
        AND o.OrderDate < CONVERT(DATETIME, '04/01/2012', 101)
GROUP BY OrderDate
ORDER BY OrderDate ASC;
```

Finally, this example shows querying directly from the indexed view. Automatic use of an indexed view by the query optimizer is supported only in specific editions of SQL Server. On SQL Server Standard edition, you must use the `NOEXPAND` query hint to query the indexed view directly. Azure SQL Database and Azure SQL Managed Instance support automatic use of indexed views without specifying the `NOEXPAND` hint. For more information, see [Table Hints (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table#using-noexpand).

```sql
--This query uses the indexed view directly, on Enterprise edition.
SELECT OrderDate, Revenue
FROM Sales.vOrders
WHERE OrderDate >= CONVERT(DATETIME, '03/01/2012', 101)
    AND OrderDate < CONVERT(DATETIME, '04/01/2012', 101)
ORDER BY OrderDate ASC;

--This query uses the indexed view directly, with the NOEXPAND hint.
SELECT OrderDate, Revenue
FROM Sales.vOrders WITH (NOEXPAND)
WHERE OrderDate >= CONVERT(DATETIME, '03/01/2012', 101)
    AND OrderDate < CONVERT(DATETIME, '04/01/2012', 101)
ORDER BY OrderDate ASC;
```

For more information, see [CREATE VIEW](../../t-sql/statements/create-view-transact-sql).