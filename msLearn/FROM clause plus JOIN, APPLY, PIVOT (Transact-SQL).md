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
title: FROM clause plus JOIN, APPLY, PIVOT (T-SQL) - SQL Server | Microsoft Learn
canonicalUrl: https://learn.microsoft.com/en-us/sql/t-sql/queries/from-transact-sql?view=sql-server-ver17
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
description: FROM clause plus JOIN, APPLY, PIVOT (Transact-SQL)
author: VanMSFT
ms.author: vanto
ms.reviewer: randolphwest
ms.date: 2025-05-08T00:00:00.0000000Z
ms.service: sql
ms.subservice: t-sql
ms.topic: reference
ms.custom:
- ignite-2025
locale: en-us
document_id: 3553153e-f6bf-2bc7-288c-ee03a3ba0781
document_version_independent_id: b4e3cfdf-aad7-e8a6-ffd7-acba7571de5c
updated_at: 2026-08-24T17:40:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/sql-docs-pr/blob/live/docs/t-sql/queries/from-transact-sql.md
gitcommit: https://github.com/MicrosoftDocs/sql-docs-pr/blob/35290234fd67e105f228d27ea7a57f42512e8231/docs/t-sql/queries/from-transact-sql.md
git_commit_id: 35290234fd67e105f228d27ea7a57f42512e8231
default_moniker: sql-server-ver17
site_name: Docs
depot_name: SQL.sql-content
page_type: conceptual
toc_rel: ../../toc.json
pdf_url_template: https://learn.microsoft.com/pdfstore/en-us/SQL.sql-content/{branchName}{pdfName}
search.mshattr.devlang: tsql
word_count: 5825
asset_id: t-sql/queries/from-transact-sql
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
source_path: docs/t-sql/queries/from-transact-sql.md
cmProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/cbe4ca68-43ac-4375-aba5-5945a6394c20
- https://authoring-docs-microsoft.poolparty.biz/devrel/540ac133-a371-4dbb-8f94-28d6cc77a70b
- https://authoring-docs-microsoft.poolparty.biz/devrel/6ab7faaf-d791-4a26-96a2-3b11738538e7
spProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/ced846cc-6a3c-4c8f-9dfb-3de0e90e2742
- https://authoring-docs-microsoft.poolparty.biz/devrel/60bfc045-f127-4841-9d00-ea35495a5800
- https://authoring-docs-microsoft.poolparty.biz/devrel/302e28b0-1f09-4811-9a9b-2a72e0770581
platformId: 96b86725-5d03-2dc5-f15b-a2ccca046649
---

# FROM clause plus JOIN, APPLY, PIVOT (T-SQL) - SQL Server | Microsoft Learn

**Applies to:**![](../../includes/media/yes-icon.svg) SQL Server 2016 (13.x) and later versions ![](../../includes/media/yes-icon.svg)[Azure SQL Database](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Managed Instance](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure Synapse Analytics](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Analytics Platform System (PDW)](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL analytics endpoint in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Warehouse in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL database in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)

In Transact-SQL, the FROM clause is available on the following statements:

- [DELETE](../statements/delete-transact-sql)
- [UPDATE](update-transact-sql)
- [SELECT](select-transact-sql)

The FROM clause is usually required on the SELECT statement. The exception is when no table columns are listed, and the only items listed are literals or variables or arithmetic expressions.

This article also discusses the following keywords that can be used on the FROM clause:

- [JOIN](../../relational-databases/performance/joins)
- APPLY
- [PIVOT](from-using-pivot-and-unpivot)

![](../../includes/media/topic-link-icon.svg)[Transact-SQL syntax conventions](../language-elements/transact-sql-syntax-conventions-transact-sql)

## Syntax

Syntax for SQL Server, Azure SQL Database, and SQL database in Fabric:

```syntaxsql
[ FROM { <table_source> } [ , ...n ] ]
<table_source> ::=
{
    table_or_view_name [ FOR SYSTEM_TIME <system_time> ] [ [ AS ] table_alias ]
        [ <tablesample_clause> ]
        [ WITH ( < table_hint > [ [ , ] ...n ] ) ]
    | rowset_function [ [ AS ] table_alias ]
        [ ( bulk_column_alias [ , ...n ] ) ]
    | user_defined_function [ [ AS ] table_alias ]
    | OPENXML <openxml_clause>
    | derived_table [ [ AS ] table_alias ] [ ( column_alias [ , ...n ] ) ]
    | <joined_table>
    | <pivoted_table>
    | <unpivoted_table>
    | @variable [ [ AS ] table_alias ]
    | @variable.function_call ( expression [ , ...n ] )
        [ [ AS ] table_alias ] [ (column_alias [ , ...n ] ) ]
}
<tablesample_clause> ::=
    TABLESAMPLE [ SYSTEM ] ( sample_number [ PERCENT | ROWS ] )
        [ REPEATABLE ( repeat_seed ) ]

<joined_table> ::=
{
    <table_source> <join_type> <table_source> ON <search_condition>
    | <table_source> CROSS JOIN <table_source>
    | left_table_source { CROSS | OUTER } APPLY right_table_source
    | [ ( ] <joined_table> [ ) ]
}
<join_type> ::=
    [ { INNER | { { LEFT | RIGHT | FULL } [ OUTER ] } } [ <join_hint> ] ]
    JOIN

<pivoted_table> ::=
    table_source PIVOT <pivot_clause> [ [ AS ] table_alias ]

<pivot_clause> ::=
        ( aggregate_function ( value_column [ [ , ] ...n ] )
        FOR pivot_column
        IN ( <column_list> )
    )

<unpivoted_table> ::=
    table_source UNPIVOT <unpivot_clause> [ [ AS ] table_alias ]

<unpivot_clause> ::=
    ( value_column FOR pivot_column IN ( <column_list> ) )

<column_list> ::=
    column_name [ , ...n ]

<system_time> ::=
{
      AS OF <date_time>
    | FROM <start_date_time> TO <end_date_time>
    | BETWEEN <start_date_time> AND <end_date_time>
    | CONTAINED IN (<start_date_time> , <end_date_time>)
    | ALL
}

    <date_time>::=
        <date_time_literal> | @date_time_variable

    <start_date_time>::=
        <date_time_literal> | @date_time_variable

    <end_date_time>::=
        <date_time_literal> | @date_time_variable
```

Syntax for Parallel Data Warehouse, Azure Synapse Analytics:

```syntaxsql
FROM { <table_source> [ , ...n ] }

<table_source> ::=
{
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias
    [ <tablesample_clause> ]
    | derived_table [ AS ] table_alias [ ( column_alias [ , ...n ] ) ]
    | <joined_table>
}

<tablesample_clause> ::=
    TABLESAMPLE ( sample_number [ PERCENT ] ) -- Azure Synapse Analytics Dedicated SQL pool only

<joined_table> ::=
{
    <table_source> <join_type> <table_source> ON search_condition
    | <table_source> CROSS JOIN <table_source>
    | left_table_source { CROSS | OUTER } APPLY right_table_source
    | [ ( ] <joined_table> [ ) ]
}

<join_type> ::=
    [ INNER ] [ <join_hint> ] JOIN
    | LEFT  [ OUTER ] JOIN
    | RIGHT [ OUTER ] JOIN
    | FULL  [ OUTER ] JOIN

<join_hint> ::=
    REDUCE
    | REPLICATE
    | REDISTRIBUTE
```

Syntax for Microsoft Fabric Data Warehouse:

```syntaxsql
FROM { <table_source> [ , ...n ] }

<table_source> ::=
{
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias
    | derived_table [ AS ] table_alias [ ( column_alias [ , ...n ] ) ]
    | <joined_table>
}

<joined_table> ::=
{
    <table_source> <join_type> <table_source> ON search_condition
    | <table_source> CROSS JOIN <table_source>
    | left_table_source { CROSS | OUTER } APPLY right_table_source
    | [ ( ] <joined_table> [ ) ]
}

<join_type> ::=
    [ INNER ] [ <join_hint> ] JOIN
    | LEFT  [ OUTER ] JOIN
    | RIGHT [ OUTER ] JOIN
    | FULL  [ OUTER ] JOIN

<join_hint> ::=
    REDUCE
    | REPLICATE
    | REDISTRIBUTE
```

## Arguments

#### &lt;table\_source&gt;

Specifies a table, view, table variable, or derived table source, with or without an alias, to use in the Transact-SQL statement. Up to 256 table sources can be used in a statement, although the limit varies depending on available memory and the complexity of other expressions in the query. Individual queries may not support up to 256 table sources.

Note

Query performance may suffer with lots of tables referenced in a query. Compilation and optimization time is also affected by additional factors. These include the presence of indexes and indexed views on each &lt;table\_source&gt; and the size of the &lt;select\_list&gt; in the SELECT statement.

The order of table sources after the FROM keyword doesn't affect the result set that is returned. SQL Server returns errors when duplicate names appear in the FROM clause.

#### *table\_or\_view\_name*

The name of a table or view.

If the table or view exists in another database on the same instance of SQL Server, use a fully qualified name in the form *database*.*schema*.*object\_name*.

If the table or view exists outside the instance of SQL Server, use a four-part name in the form *linked\_server*.*catalog*.*schema*.*object*. For more information, see [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql). A four-part name that is constructed by using the [OPENDATASOURCE](../functions/opendatasource-transact-sql) function as the server part of the name can also be used to specify the remote table source. When OPENDATASOURCE is specified, *database\_name* and *schema\_name* may not apply to all data sources and is subject to the capabilities of the OLE DB provider that accesses the remote object.

#### [AS] *table\_alias*

An alias for *table\_source* that can be used either for convenience or to distinguish a table or view in a self-join or subquery. An alias is frequently a shortened table name used to refer to specific columns of the tables in a join. If the same column name exists in more than one table in the join, SQL Server may require that the column name is qualified by a table name, view name, or alias to distinguish these columns. The table name can't be used if an alias is defined.

When a derived table, rowset or table-valued function, or operator clause (such as PIVOT or UNPIVOT) is used, the required *table\_alias* at the end of the clause is the associated table name for all columns, including grouping columns, returned.

#### WITH (&lt;table\_hint&gt; )

Specifies that the query optimizer uses an optimization or locking strategy with this table and for this statement. For more information, see [Table Hints (Transact-SQL)](hints-transact-sql-table).

#### *rowset\_function*

**Applies to**: SQL Server and SQL Database.

Specifies one of the rowset functions, such as OPENROWSET, which returns an object that can be used instead of a table reference. For more information about a list of rowset functions, see [Rowset Functions (Transact-SQL)](../functions/opendatasource-transact-sql).

Using the OPENROWSET and OPENQUERY functions to specify a remote object depends on the capabilities of the OLE DB provider that accesses the object.

#### *bulk\_column\_alias*

**Applies to**: SQL Server and SQL Database.

An optional alias to replace a column name in the result set. Column aliases are allowed only in SELECT statements that use the OPENROWSET function with the BULK option. When you use *bulk\_column\_alias*, specify an alias for every table column in the same order as the columns in the file.

Note

This alias overrides the NAME attribute in the COLUMN elements of an XML format file, if present.

#### *user\_defined\_function*

Specifies a table-valued function.

#### OPENXML &lt;openxml\_clause&gt;

**Applies to**: SQL Server and SQL Database.

Provides a rowset view over an XML document. For more information, see [OPENXML (Transact-SQL)](../functions/openxml-transact-sql).

#### *derived\_table*

A subquery that retrieves rows from the database. *derived\_table* is used as input to the outer query.

*derived\_table* can use the Transact-SQL table value constructor feature to specify multiple rows. For example, `SELECT * FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);`. For more information, see [Table Value Constructor (Transact-SQL)](table-value-constructor-transact-sql).

#### *column\_alias*

An optional alias to replace a column name in the result set of the derived table. Include one column alias for each column in the select list, and enclose the complete list of column aliases in parentheses.

#### *table\_or\_view\_name* FOR SYSTEM\_TIME &lt;system\_time&gt;

**Applies to**: SQL Server 2016 (13.x) and later versions, and SQL Database.

Specifies that a specific version of data is returned from the specified temporal table and its linked system-versioned history table

#### TABLESAMPLE clause

**Applies to**: SQL Server, Azure SQL Database, Azure SQL Managed Instance, and Azure Synapse Analytics dedicated SQL pools

Specifies that a sample of data from the table is returned. The sample may be approximate. This clause can be used on any primary or joined table in a SELECT or UPDATE statement. TABLESAMPLE can't be specified with views.

#### SYSTEM

An implementation-dependent sampling method specified by ISO standards. In SQL Server, Azure SQL Database, and Azure SQL Managed Instance, this is the only sampling method available and is applied by default. SYSTEM applies a page-based sampling method in which a random set of pages from the table is chosen for the sample, and all the rows on those pages are returned as the sample subset.

When using SYSTEM, rows might be read under the READ UNCOMMITTED [transaction isolation level](../statements/set-transaction-isolation-level-transact-sql#read-uncommitted).

#### *sample\_number*

An exact or approximate constant numeric expression that represents the percent or number of rows. When specified with PERCENT, *sample\_number* is implicitly converted to a **float** value; otherwise, it is converted to **bigint**. PERCENT is the default.

#### PERCENT

Specifies that a *sample\_number* percent of the rows of the table should be retrieved from the table. When PERCENT is specified, SQL Server returns an approximate of the percent specified. When PERCENT is specified the *sample\_number* expression must evaluate to a value from 0 to 100.

#### ROWS

Specifies that approximately *sample\_number* of rows are retrieved. When ROWS is specified, SQL Server returns an approximation of the number of rows specified. When ROWS is specified, the *sample\_number* expression must evaluate to an integer value greater than zero.

#### REPEATABLE

Indicates that the selected sample can be returned again. When specified with the same *repeat\_seed* value, SQL Server returns the same subset of rows as long as no changes have been made to any rows in the table. When specified with a different *repeat\_seed* value, SQL Server will likely return some different sample of the rows in the table. The following actions to the table are considered changes: insert, update, delete, index rebuild or defragmentation, and database restore or attach.

#### *repeat\_seed*

A constant integer expression used by SQL Server to generate a random number. *repeat\_seed* is **bigint**. If *repeat\_seed* isn't specified, SQL Server assigns a value at random. For a specific *repeat\_seed* value, the sampling result is always the same if no changes have been applied to the table. The *repeat\_seed* expression must evaluate to an integer greater than zero.

### Joined table

A joined table is a result set that is the product of two or more tables. For multiple joins, use parentheses to change the natural order of the joins.

### Join type

Specifies the type of join operation.

#### INNER

Specifies all matching pairs of rows are returned. Discards unmatched rows from both tables. When no join type is specified, this is the default.

#### FULL [ OUTER ]

Specifies that a row from either the left or right table that doesn't meet the join condition is included in the result set, and output columns that correspond to the other table are set to NULL. This is in addition to all rows typically returned by the INNER JOIN.

#### LEFT [ OUTER ]

Specifies that all rows from the left table not meeting the join condition are included in the result set, and output columns from the other table are set to NULL in addition to all rows returned by the inner join.

#### RIGHT [ OUTER ]

Specifies all rows from the right table not meeting the join condition are included in the result set, and output columns that correspond to the other table are set to NULL, in addition to all rows returned by the inner join.

### Join hint

For SQL Server and SQL Database, specifies that the SQL Server query optimizer uses one join hint, or execution algorithm, per join specified in the query FROM clause. For more information, see [Join Hints (Transact-SQL)](hints-transact-sql-join).

For Azure Synapse Analytics, Analytics Platform System (PDW), and Microsoft Fabric Data Warehouse, these join hints apply to `INNER` joins on two distribution incompatible columns. They can improve query performance by restricting the amount of data movement that occurs during query processing.

For more information on `REDUCE`, `REPLICATE`, and `REDISTRIBUTE`, see [Join hints (Transact-SQL)](hints-transact-sql-join).

#### JOIN

Indicates that the specified join operation should occur between the specified table sources or views.

#### ON &lt;search\_condition&gt;

Specifies the condition on which the join is based. The condition can specify any predicate, although columns and comparison operators are frequently used, for example:

```sql
SELECT p.ProductID,
    v.BusinessEntityID
FROM Production.Product AS p
INNER JOIN Purchasing.ProductVendor AS v
    ON (p.ProductID = v.ProductID);
```

When the condition specifies columns, the columns don't have to have the same name or same data type; however, if the data types aren't the same, they must be either compatible or types that SQL Server can implicitly convert. If the data types can't be implicitly converted, the condition must explicitly convert the data type by using the CONVERT function.

There can be predicates that involve only one of the joined tables in the ON clause. Such predicates also can be in the WHERE clause in the query. Although the placement of such predicates doesn't make a difference for INNER joins, they might cause a different result when OUTER joins are involved. This is because the predicates in the ON clause are applied to the table before the join, whereas the WHERE clause is semantically applied to the result of the join.

For more information about search conditions and predicates, see [Search Condition (Transact-SQL)](search-condition-transact-sql).

#### CROSS JOIN

Specifies the cross-product of two tables. Returns the same rows as if no WHERE clause was specified in an old-style, non-SQL-92-style join.

#### *left\_table\_source* { CROSS | OUTER } APPLY *right\_table\_source*

Specifies that the *right\_table\_source* of the APPLY operator is evaluated against every row of the *left\_table\_source*. This functionality is useful when the *right\_table\_source* contains a table-valued function that takes column values from the *left\_table\_source* as one of its arguments.

Either CROSS or OUTER must be specified with APPLY. When CROSS is specified, no rows are produced when the *right\_table\_source* is evaluated against a specified row of the *left\_table\_source* and returns an empty result set.

When OUTER is specified, one row is produced for each row of the *left\_table\_source* even when the *right\_table\_source* evaluates against that row and returns an empty result set.

For more information, see the Remarks section.

#### *left\_table\_source*

A table source as defined in the previous argument. For more information, see the Remarks section.

#### *right\_table\_source*

A table source as defined in the previous argument. For more information, see the Remarks section.

### PIVOT clause

#### *table\_source* PIVOT &lt;pivot\_clause&gt;

Specifies that the *table\_source* is pivoted based on the *pivot\_column*. *table\_source* is a table or table expression. The output is a table that contains all columns of the *table\_source* except the *pivot\_column* and *value\_column*. The columns of the *table\_source*, except the *pivot\_column* and *value\_column*, are called the grouping columns of the pivot operator. For more information about PIVOT and UNPIVOT, see [Using PIVOT and UNPIVOT](from-using-pivot-and-unpivot).

PIVOT performs a grouping operation on the input table with regard to the grouping columns and returns one row for each group. Additionally, the output contains one column for each value specified in the *column\_list* that appears in the *pivot\_column* of the *input\_table*.

For more information, see the Remarks section that follows.

#### *aggregate\_function*

A system or user-defined aggregate function that accepts one or more inputs. The aggregate function should be invariant to null values. An aggregate function invariant to null values doesn't consider null values in the group while it is evaluating the aggregate value.

The COUNT(\*) system aggregate function isn't allowed.

#### *value\_column*

The value column of the PIVOT operator. When used with UNPIVOT, *value\_column* can't be the name of an existing column in the input *table\_source*.

#### FOR *pivot\_column*

The pivot column of the PIVOT operator. *pivot\_column* must be of a type implicitly or explicitly convertible to **nvarchar()**. This column can't be **image** or **rowversion**.

When UNPIVOT is used, *pivot\_column* is the name of the output column that becomes narrowed from the *table\_source*. There can't be an existing column in *table\_source* with that name.

#### IN ( *column\_list* )

In the PIVOT clause, lists the values in the *pivot\_column* that becomes the column names of the output table. The list can't specify any column names that already exist in the input *table\_source* that is being pivoted.

In the UNPIVOT clause, lists the columns in *table\_source* that is narrowed into a single *pivot\_column*.

#### *table\_alias*

The alias name of the output table. *pivot\_table\_alias* must be specified.

#### UNPIVOT &lt;unpivot\_clause&gt;

Specifies that the input table is narrowed from multiple columns in *column\_list* into a single column called *pivot\_column*. For more information about PIVOT and UNPIVOT, see [Using PIVOT and UNPIVOT](from-using-pivot-and-unpivot).

#### AS OF &lt;date\_time&gt;

**Applies to**: SQL Server 2016 (13.x) and later versions, and SQL Database.

Returns a table with single record for each row containing the values that were actual (current) at the specified point in time in the past. Internally, a union is performed between the temporal table and its history table and the results are filtered to return the values in the row that was valid at the point in time specified by the *&lt;date\_time&gt;* parameter. The value for a row is deemed valid if the *system\_start\_time\_column\_name* value is less than or equal to the *&lt;date\_time&gt;* parameter value and the *system\_end\_time\_column\_name* value is greater than the *&lt;date\_time&gt;* parameter value.

#### FROM &lt;start\_date\_time&gt; TO &lt;end\_date\_time&gt;

**Applies to**: SQL Server 2016 (13.x) and later versions, and SQL Database.

Returns a table with the values for all record versions that were active within the specified time range, regardless of whether they started being active before the *&lt;start\_date\_time&gt;* parameter value for the FROM argument or ceased being active after the *&lt;end\_date\_time&gt;* parameter value for the TO argument. Internally, a union is performed between the temporal table and its history table and the results are filtered to return the values for all row versions that were active at any time during the time range specified. Rows that became active exactly on the lower boundary defined by the FROM endpoint are included and rows that became active exactly on the upper boundary defined by the TO endpoint aren't included.

#### BETWEEN &lt;start\_date\_time&gt; AND &lt;end\_date\_time&gt;

**Applies to**: SQL Server 2016 (13.x) and later versions, and SQL Database.

Same as above in the **FROM &lt;start\_date\_time&gt; TO &lt;end\_date\_time&gt;** description, except it includes rows that became active on the upper boundary defined by the &lt;end\_date\_time&gt; endpoint.

#### CONTAINED IN (&lt;start\_date\_time&gt; , &lt;end\_date\_time&gt;)

**Applies to**: SQL Server 2016 (13.x) and later versions, and SQL Database.

Returns a table with the values for all record versions that were opened and closed within the specified time range defined by the two datetime values for the CONTAINED IN argument. Rows that became active exactly on the lower boundary or ceased being active exactly on the upper boundary are included.

#### ALL

Returns a table with the values from all rows from both the current table and the history table.

## Remarks

The FROM clause supports the SQL-92 syntax for joined tables and derived tables. SQL-92 syntax provides the INNER, LEFT OUTER, RIGHT OUTER, FULL OUTER, and CROSS join operators.

UNION and JOIN within a FROM clause are supported within views and in derived tables and subqueries.

A self-join is a table that is joined to itself. Insert or update operations that are based on a self-join follow the order in the FROM clause.

Because SQL Server considers distribution and cardinality statistics from linked servers that provide column distribution statistics, the REMOTE join hint isn't required to force evaluating a join remotely. The SQL Server query processor considers remote statistics and determines whether a remote-join strategy is appropriate. REMOTE join hint is useful for providers that don't provide column distribution statistics.

## Use APPLY

Both the left and right operands of the APPLY operator are table expressions. The main difference between these operands is that the *right\_table\_source* can use a table-valued function that takes a column from the *left\_table\_source* as one of the arguments of the function. The *left\_table\_source* can include table-valued functions, but it can't contain arguments that are columns from the *right\_table\_source*.

The APPLY operator works in the following way to produce the table source for the FROM clause:

1. Evaluates *right\_table\_source* against each row of the *left\_table\_source* to produce rowsets.

    The values in the *right\_table\_source* depend on *left\_table\_source*. *right\_table\_source* can be represented approximately this way: `TVF(left_table_source.row)`, where `TVF` is a table-valued function.
2. Combines the result sets that are produced for each row in the evaluation of *right\_table\_source* with the *left\_table\_source* by performing a UNION ALL operation.

    The list of columns produced by the result of the APPLY operator is the set of columns from the *left\_table\_source* that is combined with the list of columns from the *right\_table\_source*.

## Use PIVOT and UNPIVOT

The *pivot\_column* and *value\_column* are grouping columns that are used by the PIVOT operator. PIVOT follows the following process to obtain the output result set:

1. Performs a GROUP BY on its *input\_table* against the grouping columns and produces one output row for each group.

    The grouping columns in the output row obtain the corresponding column values for that group in the *input\_table*.
2. Generates values for the columns in the column list for each output row by performing the following:

    1. Grouping additionally the rows generated in the GROUP BY in the previous step against the *pivot\_column*.

        For each output column in the *column\_list*, selecting a subgroup that satisfies the condition:

        `pivot_column = CONVERT(<data type of pivot_column>, 'output_column')`
    2. *aggregate\_function* is evaluated against the *value\_column* on this subgroup and its result is returned as the value of the corresponding *output\_column*. If the subgroup is empty, SQL Server generates a null value for that *output\_column*. If the aggregate function is COUNT and the subgroup is empty, zero (0) is returned.

Note

The column identifiers in the `UNPIVOT` clause follow the catalog collation. For SQL Database, the collation is always `SQL_Latin1_General_CP1_CI_AS`. For SQL Server partially contained databases, the collation is always `Latin1_General_100_CI_AS_KS_WS_SC`. If the column is combined with other columns, then a collate clause (`COLLATE DATABASE_DEFAULT`) is required to avoid conflicts.

For more information about PIVOT and UNPIVOT including examples, see [Using PIVOT and UNPIVOT](from-using-pivot-and-unpivot).

## Permissions

Requires the permissions for the DELETE, SELECT, or UPDATE statement.

## Examples

### A. Use a FROM clause

The following example retrieves the `TerritoryID` and `Name` columns from the `SalesTerritory` table in the AdventureWorks2025 sample database.

```sql
SELECT TerritoryID,
    Name
FROM Sales.SalesTerritory
ORDER BY TerritoryID;
```

Here's the result set.

```output
TerritoryID Name
----------- ------------------------------
1           Northwest
2           Northeast
3           Central
4           Southwest
5           Southeast
6           Canada
7           France
8           Germany
9           Australia
10          United Kingdom
(10 row(s) affected)
```

### B. Use the TABLOCK and HOLDLOCK optimizer hints

The following partial transaction shows how to place an explicit shared table lock on `Employee` and how to read the index. The lock is held throughout the whole transaction.

```sql
BEGIN TRANSACTION

SELECT COUNT(*)
FROM HumanResources.Employee WITH (TABLOCK, HOLDLOCK);
```

### C. Use the SQL-92 CROSS JOIN syntax

The following example returns the cross product of the two tables `Employee` and `Department` in the AdventureWorks2025 database. A list of all possible combinations of `BusinessEntityID` rows and all `Department` name rows are returned.

```sql
SELECT e.BusinessEntityID,
    d.Name AS Department
FROM HumanResources.Employee AS e
CROSS JOIN HumanResources.Department AS d
ORDER BY e.BusinessEntityID,
    d.Name;
```

### D. Use the SQL-92 FULL OUTER JOIN syntax

The following example returns the product name and any corresponding sales orders in the `SalesOrderDetail` table in the AdventureWorks2025 database. It also returns any sales orders that have no product listed in the `Product` table, and any products with a sales order other than the one listed in the `Product` table.

```sql
-- The OUTER keyword following the FULL keyword is optional.
SELECT p.Name,
    sod.SalesOrderID
FROM Production.Product AS p
FULL JOIN Sales.SalesOrderDetail AS sod
    ON p.ProductID = sod.ProductID
ORDER BY p.Name;
```

### E. Use the SQL-92 LEFT OUTER JOIN syntax

The following example joins two tables on `ProductID` and preserves the unmatched rows from the left table. The `Product` table is matched with the `SalesOrderDetail` table on the `ProductID` columns in each table. All products, ordered and not ordered, appear in the result set.

```sql
SELECT p.Name,
    sod.SalesOrderID
FROM Production.Product AS p
LEFT OUTER JOIN Sales.SalesOrderDetail AS sod
    ON p.ProductID = sod.ProductID
ORDER BY p.Name;
```

### F. Use the SQL-92 INNER JOIN syntax

The following example returns all product names and sales order IDs.

```sql
-- By default, SQL Server performs an INNER JOIN if only the JOIN
-- keyword is specified.
SELECT p.Name,
    sod.SalesOrderID
FROM Production.Product AS p
INNER JOIN Sales.SalesOrderDetail AS sod
    ON p.ProductID = sod.ProductID
ORDER BY p.Name;
```

### G. Use the SQL-92 RIGHT OUTER JOIN syntax

The following example joins two tables on `TerritoryID` and preserves the unmatched rows from the right table. The `SalesTerritory` table is matched with the `SalesPerson` table on the `TerritoryID` column in each table. All salespersons appear in the result set, whether or not they are assigned a territory.

```sql
SELECT st.Name AS Territory,
    sp.BusinessEntityID
FROM Sales.SalesTerritory AS st
RIGHT OUTER JOIN Sales.SalesPerson AS sp
    ON st.TerritoryID = sp.TerritoryID;
```

### H. Use HASH and MERGE join hints

The following example performs a three-table join among the `Product`, `ProductVendor`, and `Vendor` tables to produce a list of products and their vendors. The query optimizer joins `Product` and `ProductVendor` (`p` and `pv`) by using a MERGE join. Next, the results of the `Product` and `ProductVendor` MERGE join (`p` and `pv`) are HASH joined to the `Vendor` table to produce (`p` and `pv`) and `v`.

Important

After a join hint is specified, the INNER keyword is no longer optional and must be explicitly stated for an INNER JOIN to be performed.

```sql
SELECT p.Name AS ProductName,
    v.Name AS VendorName
FROM Production.Product AS p
INNER MERGE JOIN Purchasing.ProductVendor AS pv
    ON p.ProductID = pv.ProductID
INNER HASH JOIN Purchasing.Vendor AS v
    ON pv.BusinessEntityID = v.BusinessEntityID
ORDER BY p.Name,
    v.Name;
```

### I. Use a derived table

The following example uses a derived table, a `SELECT` statement after the `FROM` clause, to return the first and last names of all employees and the cities in which they live.

```sql
SELECT RTRIM(p.FirstName) + ' ' + LTRIM(p.LastName) AS Name,
    d.City
FROM Person.Person AS p
INNER JOIN HumanResources.Employee e
    ON p.BusinessEntityID = e.BusinessEntityID
INNER JOIN (
    SELECT bea.BusinessEntityID,
        a.City
    FROM Person.Address AS a
    INNER JOIN Person.BusinessEntityAddress AS bea
        ON a.AddressID = bea.AddressID
    ) AS d
    ON p.BusinessEntityID = d.BusinessEntityID
ORDER BY p.LastName,
    p.FirstName;
```

### J. Use TABLESAMPLE to read data from a sample of rows in a table

The following example uses `TABLESAMPLE` in the `FROM` clause to return approximately `10` percent of all the rows in the `Customer` table.

```sql
SELECT *
FROM Sales.Customer TABLESAMPLE SYSTEM(10 PERCENT);
```

### K. Use APPLY

The following example assumes that the following tables and table-valued function exist in the database:

| Object Name | Column Names |
| --- | --- |
| Departments | DeptID, DivisionID, DeptName, DeptMgrID |
| EmpMgr | MgrID, EmpID |
| Employees | EmpID, EmpLastName, EmpFirstName, EmpSalary |
| GetReports(MgrID) | EmpID, EmpLastName, EmpSalary |

The `GetReports` table-valued function returns the list of all employees that report directly or indirectly to the specified `MgrID`.

The example uses `APPLY` to return all departments and all employees in that department. If a particular department doesn't have any employees, there won't be any rows returned for that department.

```sql
SELECT DeptID,
    DeptName,
    DeptMgrID,
    EmpID,
    EmpLastName,
    EmpSalary
FROM Departments d
CROSS APPLY dbo.GetReports(d.DeptMgrID);
```

If you want the query to produce rows for those departments without employees, which will produce null values for the `EmpID`, `EmpLastName` and `EmpSalary` columns, use `OUTER APPLY` instead.

```sql
SELECT DeptID,
    DeptName,
    DeptMgrID,
    EmpID,
    EmpLastName,
    EmpSalary
FROM Departments d
OUTER APPLY dbo.GetReports(d.DeptMgrID);
```

### L. Use CROSS APPLY

The following example retrieves a snapshot of all query plans residing in the plan cache, by querying the `sys.dm_exec_cached_plans` dynamic management view to retrieve the plan handles of all query plans in the cache. Then the `CROSS APPLY` operator is specified to pass the plan handles to `sys.dm_exec_query_plan`. The XML Showplan output for each plan currently in the plan cache is in the `query_plan` column of the table that is returned.

```sql
USE master;
GO

SELECT dbid,
    object_id,
    query_plan
FROM sys.dm_exec_cached_plans AS cp
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);
GO
```

### M. Use FOR SYSTEM\_TIME

**Applies to**: SQL Server 2016 (13.x) and later versions, and SQL Database.

The following example uses the `FOR SYSTEM_TIME AS OF *date_time_literal_or_variable*` argument to return table rows that were actual (current) as of January 1, 2014.

```sql
SELECT DepartmentNumber,
    DepartmentName,
    ManagerID,
    ParentDepartmentNumber
FROM DEPARTMENT
FOR SYSTEM_TIME AS OF '2014-01-01'
WHERE ManagerID = 5;
```

The following example uses the `FOR SYSTEM_TIME FROM *date_time_literal_or_variable* TO *date_time_literal_or_variable*` argument to return all rows that were active during the period defined as starting with January 1, 2013 and ending with January 1, 2014, exclusive of the upper boundary.

```sql
SELECT DepartmentNumber,
    DepartmentName,
    ManagerID,
    ParentDepartmentNumber
FROM DEPARTMENT
FOR SYSTEM_TIME FROM '2013-01-01' TO '2014-01-01'
WHERE ManagerID = 5;
```

The following example uses the `FOR SYSTEM_TIME BETWEEN *date_time_literal_or_variable* AND *date_time_literal_or_variable*` argument to return all rows that were active during the period defined as starting with January 1, 2013 and ending with January 1, 2014, inclusive of the upper boundary.

```sql
SELECT DepartmentNumber,
    DepartmentName,
    ManagerID,
    ParentDepartmentNumber
FROM DEPARTMENT
FOR SYSTEM_TIME BETWEEN '2013-01-01' AND '2014-01-01'
WHERE ManagerID = 5;
```

The following example uses the `FOR SYSTEM_TIME CONTAINED IN (*date_time_literal_or_variable*, *date_time_literal_or_variable*)` argument to return all rows that were opened and closed during the period defined as starting with January 1, 2013 and ending with January 1, 2014.

```sql
SELECT DepartmentNumber,
    DepartmentName,
    ManagerID,
    ParentDepartmentNumber
FROM DEPARTMENT
FOR SYSTEM_TIME CONTAINED IN ('2013-01-01', '2014-01-01')
WHERE ManagerID = 5;
```

The following example uses a variable rather than a literal to provide the date boundary values for the query.

```sql
DECLARE @AsOfFrom DATETIME2 = DATEADD(month, -12, SYSUTCDATETIME());
DECLARE @AsOfTo DATETIME2 = DATEADD(month, -6, SYSUTCDATETIME());

SELECT DepartmentNumber,
    DepartmentName,
    ManagerID,
    ParentDepartmentNumber
FROM DEPARTMENT
FOR SYSTEM_TIME
FROM @AsOfFrom TO @AsOfTo
WHERE ManagerID = 5;
```

## Examples: Azure Synapse Analytics and Analytics Platform System (PDW)

### N. Use the INNER JOIN syntax

The following example returns the `SalesOrderNumber`, `ProductKey`, and `EnglishProductName` columns from the `FactInternetSales` and `DimProduct` tables where the join key, `ProductKey`, matches in both tables. The `SalesOrderNumber` and `EnglishProductName` columns each exist in one of the tables only, so it isn't necessary to specify the table alias with these columns, as is shown; these aliases are included for readability. The keyword `AS` before an alias name isn't required but is recommended for readability and to conform to the ANSI standard.

```sql
-- Uses AdventureWorks
  
SELECT fis.SalesOrderNumber,
    dp.ProductKey,
    dp.EnglishProductName
FROM FactInternetSales AS fis
INNER JOIN DimProduct AS dp
    ON dp.ProductKey = fis.ProductKey;
```

Since the `INNER` keyword isn't required for inner joins, this same query could be written as:

```sql
-- Uses AdventureWorks
  
SELECT fis.SalesOrderNumber,
    dp.ProductKey,
    dp.EnglishProductName
FROM FactInternetSales AS fis
INNER JOIN DimProduct AS dp
    ON dp.ProductKey = fis.ProductKey;
```

A `WHERE` clause could also be used with this query to limit results. This example limits results to `SalesOrderNumber` values higher than 'SO5000':

```sql
-- Uses AdventureWorks
  
SELECT fis.SalesOrderNumber,
    dp.ProductKey,
    dp.EnglishProductName
FROM FactInternetSales AS fis
INNER JOIN DimProduct AS dp
    ON dp.ProductKey = fis.ProductKey
WHERE fis.SalesOrderNumber > 'SO50000'
ORDER BY fis.SalesOrderNumber;
```

### O. Use the LEFT OUTER JOIN and RIGHT OUTER JOIN syntax

The following example joins the `FactInternetSales` and `DimProduct` tables on the `ProductKey` columns. The left outer join syntax preserves the unmatched rows from the left (`FactInternetSales`) table. Since the `FactInternetSales` table doesn't contain any `ProductKey` values that don't match the `DimProduct` table, this query returns the same rows as the first inner join example earlier in this article.

```sql
-- Uses AdventureWorks
  
SELECT fis.SalesOrderNumber,
    dp.ProductKey,
    dp.EnglishProductName
FROM FactInternetSales AS fis
LEFT OUTER JOIN DimProduct AS dp
    ON dp.ProductKey = fis.ProductKey;
```

This query could also be written without the `OUTER` keyword.

In right outer joins, the unmatched rows from the right table are preserved. The following example returns the same rows as the left outer join example above.

```sql
-- Uses AdventureWorks
  
SELECT fis.SalesOrderNumber,
    dp.ProductKey,
    dp.EnglishProductName
FROM DimProduct AS dp
RIGHT OUTER JOIN FactInternetSales AS fis
    ON dp.ProductKey = fis.ProductKey;
```

The following query uses the `DimSalesTerritory` table as the left table in a left outer join. It retrieves the `SalesOrderNumber` values from the `FactInternetSales` table. If there are no orders for a particular `SalesTerritoryKey`, the query returns a `NULL` for the `SalesOrderNumber` for that row. This query is ordered by the `SalesOrderNumber` column, so that any NULLs in this column appear at the top of the results.

```sql
-- Uses AdventureWorks
  
SELECT dst.SalesTerritoryKey,
    dst.SalesTerritoryRegion,
    fis.SalesOrderNumber
FROM DimSalesTerritory AS dst
LEFT OUTER JOIN FactInternetSales AS fis
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey
ORDER BY fis.SalesOrderNumber;
```

This query could be rewritten with a right outer join to retrieve the same results:

```sql
-- Uses AdventureWorks
  
SELECT dst.SalesTerritoryKey,
    dst.SalesTerritoryRegion,
    fis.SalesOrderNumber
FROM FactInternetSales AS fis
RIGHT OUTER JOIN DimSalesTerritory AS dst
    ON fis.SalesTerritoryKey = dst.SalesTerritoryKey
ORDER BY fis.SalesOrderNumber;
```

### P. Use the FULL OUTER JOIN syntax

The following example demonstrates a full outer join, which returns all rows from both joined tables but returns `NULL` for values that don't match from the other table.

```sql
-- Uses AdventureWorks
  
SELECT dst.SalesTerritoryKey,
    dst.SalesTerritoryRegion,
    fis.SalesOrderNumber
FROM DimSalesTerritory AS dst
FULL OUTER JOIN FactInternetSales AS fis
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey
ORDER BY fis.SalesOrderNumber;
```

This query could also be written without the `OUTER` keyword.

```sql
-- Uses AdventureWorks
  
SELECT dst.SalesTerritoryKey,
    dst.SalesTerritoryRegion,
    fis.SalesOrderNumber
FROM DimSalesTerritory AS dst
FULL JOIN FactInternetSales AS fis
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey
ORDER BY fis.SalesOrderNumber;
```

### Q. Use the CROSS JOIN syntax

The following example returns the cross-product of the `FactInternetSales` and `DimSalesTerritory` tables. A list of all possible combinations of `SalesOrderNumber` and `SalesTerritoryKey` are returned. Notice the absence of the `ON` clause in the cross join query.

```sql
-- Uses AdventureWorks
  
SELECT dst.SalesTerritoryKey,
    fis.SalesOrderNumber
FROM DimSalesTerritory AS dst
CROSS JOIN FactInternetSales AS fis
ORDER BY fis.SalesOrderNumber;
```

### R. Use a derived table

The following example uses a derived table (a `SELECT` statement after the `FROM` clause) to return the `CustomerKey` and `LastName` columns of all customers in the `DimCustomer` table with `BirthDate` values later than January 1, 1970 and the last name 'Smith'.

```sql
-- Uses AdventureWorks
  
SELECT CustomerKey,
    LastName
FROM (
    SELECT *
    FROM DimCustomer
    WHERE BirthDate > '01/01/1970'
    ) AS DimCustomerDerivedTable
WHERE LastName = 'Smith'
ORDER BY LastName;
```

### S. REDUCE join hint example

The following example uses the `REDUCE` join hint to alter the processing of the derived table within the query. When using the `REDUCE` join hint in this query, the `fis.ProductKey` is projected, replicated and made distinct, and then joined to `DimProduct` during the shuffle of `DimProduct` on `ProductKey`. The resulting derived table is distributed on `fis.ProductKey`.

```sql
-- Uses AdventureWorks
  
SELECT SalesOrderNumber
FROM (
    SELECT fis.SalesOrderNumber,
        dp.ProductKey,
        dp.EnglishProductName
    FROM DimProduct AS dp
    INNER REDUCE JOIN FactInternetSales AS fis
        ON dp.ProductKey = fis.ProductKey
    ) AS dTable
ORDER BY SalesOrderNumber;
```

### T. REPLICATE join hint example

This next example shows the same query as the previous example, except that a `REPLICATE` join hint is used instead of the `REDUCE` join hint. Use of the `REPLICATE` hint causes the values in the `ProductKey` (joining) column from the `FactInternetSales` table to be replicated to all nodes. The `DimProduct` table is joined to the replicated version of those values.

```sql
-- Uses AdventureWorks

SELECT SalesOrderNumber
FROM (
    SELECT fis.SalesOrderNumber,
        dp.ProductKey,
        dp.EnglishProductName
    FROM DimProduct AS dp
    INNER REPLICATE JOIN FactInternetSales AS fis
        ON dp.ProductKey = fis.ProductKey
    ) AS dTable
ORDER BY SalesOrderNumber;
```

### U. Use the REDISTRIBUTE hint to guarantee a Shuffle move for a distribution incompatible join

The following query uses the `REDISTRIBUTE` query hint on a distribution incompatible join. This guarantees the query optimizer uses a Shuffle move in the query plan. This also guarantees the query plan won't use a Broadcast move, which moves a distributed table to a replicated table.

In the following example, the `REDISTRIBUTE` hint forces a Shuffle move on the `FactInternetSales` table because `ProductKey` is the distribution column for `DimProduct`, and isn't the distribution column for `FactInternetSales`.

```sql
-- Uses AdventureWorks
  
SELECT dp.ProductKey,
    fis.SalesOrderNumber,
    fis.TotalProductCost
FROM DimProduct AS dp
INNER REDISTRIBUTE JOIN FactInternetSales AS fis
    ON dp.ProductKey = fis.ProductKey;
```

### V. Use TABLESAMPLE to read data from a sample of rows in a table

The following example uses `TABLESAMPLE` in the `FROM` clause to return approximately `10` percent of all the rows in the `Customer` table.

```sql
SELECT *
FROM Sales.Customer TABLESAMPLE SYSTEM(10 PERCENT);
```