---
layout: Conceptual
monikers:
- fabric
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
title: Query Hints (Transact-SQL) - SQL Server | Microsoft Learn
canonicalUrl: https://learn.microsoft.com/en-us/sql/t-sql/queries/hints-transact-sql-query?view=sql-server-ver17
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
description: Query hints specify that the indicated hints are used in the scope of a query. They affect all operators in the statement.
author: rwestMSFT
ms.author: randolphwest
ms.reviewer: wiassaf
ms.date: 2025-10-03T00:00:00.0000000Z
ms.service: sql
ms.subservice: t-sql
ms.topic: reference
ms.custom:
- ignite-2025
locale: en-us
document_id: 33da0c41-feb0-82a9-0ce9-9e2dbe58a1c4
document_version_independent_id: 4a97457a-c149-3c41-b78a-5b4e3869f078
updated_at: 2026-08-24T17:40:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/sql-docs-pr/blob/live/docs/t-sql/queries/hints-transact-sql-query.md
gitcommit: https://github.com/MicrosoftDocs/sql-docs-pr/blob/35290234fd67e105f228d27ea7a57f42512e8231/docs/t-sql/queries/hints-transact-sql-query.md
git_commit_id: 35290234fd67e105f228d27ea7a57f42512e8231
default_moniker: sql-server-ver17
site_name: Docs
depot_name: SQL.sql-content
page_type: conceptual
toc_rel: ../../toc.json
pdf_url_template: https://learn.microsoft.com/pdfstore/en-us/SQL.sql-content/{branchName}{pdfName}
search.mshattr.devlang: tsql
word_count: 6501
asset_id: t-sql/queries/hints-transact-sql-query
moniker_range_name: 74ce0eb17bc6a2e55932e4617743878d
monikers:
- fabric
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
source_path: docs/t-sql/queries/hints-transact-sql-query.md
cmProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/cbe4ca68-43ac-4375-aba5-5945a6394c20
- https://authoring-docs-microsoft.poolparty.biz/devrel/6ab7faaf-d791-4a26-96a2-3b11738538e7
spProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/ced846cc-6a3c-4c8f-9dfb-3de0e90e2742
- https://authoring-docs-microsoft.poolparty.biz/devrel/302e28b0-1f09-4811-9a9b-2a72e0770581
platformId: 4a6e4428-1dfc-7483-04d1-42c46a60dbe8
---

# Query Hints (Transact-SQL) - SQL Server | Microsoft Learn

**Applies to:**![](../../includes/media/yes-icon.svg)[SQL Server](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Database](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Managed Instance](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL analytics endpoint in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Warehouse in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL database in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)

Query hints specify that the indicated hints are used in the scope of a query. They affect all operators in the statement. If `UNION` is involved in the main query, only the last query involving a `UNION` operation can have the `OPTION` clause. Query hints are specified as part of the [OPTION clause](option-clause-transact-sql). Error 8622 occurs if one or more query hints cause the Query Optimizer not to generate a valid plan.

Caution

Because the SQL Server Query Optimizer typically selects the best execution plan for a query, we recommend only using hints as a last resort for experienced developers and database administrators.

**Applies to:**

- [DELETE](../statements/delete-transact-sql)
- [INSERT](../statements/insert-transact-sql)
- [SELECT](select-transact-sql)
- [UPDATE](update-transact-sql)
- [MERGE](../statements/merge-transact-sql)

![](../../includes/media/topic-link-icon.svg)[Transact-SQL syntax conventions](../language-elements/transact-sql-syntax-conventions-transact-sql)

## Syntax

```syntaxsql
<query_hint> ::=
{ { HASH | ORDER } GROUP
  | { CONCAT | HASH | MERGE } UNION
  | { LOOP | MERGE | HASH } JOIN
  | DISABLE_OPTIMIZED_PLAN_FORCING
  | EXPAND VIEWS
  | FAST <integer_value>
  | FORCE ORDER
  | { FORCE | DISABLE } EXTERNALPUSHDOWN
  | { FORCE | DISABLE } SCALEOUTEXECUTION
  | IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX
  | KEEP PLAN
  | KEEPFIXED PLAN
  | MAX_GRANT_PERCENT = <numeric_value>
  | MIN_GRANT_PERCENT = <numeric_value>
  | MAXDOP <integer_value>
  | MAXRECURSION <integer_value>
  | NO_PERFORMANCE_SPOOL
  | OPTIMIZE FOR ( @variable_name { UNKNOWN | = <literal_constant> } [ , ...n ] )
  | OPTIMIZE FOR UNKNOWN
  | PARAMETERIZATION { SIMPLE | FORCED }
  | QUERYTRACEON <integer_value>
  | RECOMPILE
  | ROBUST PLAN
  | USE HINT ( 'hint_name' [ , ...n ] )
  | USE PLAN N'<xml_plan>'
  | TABLE HINT ( <exposed_object_name> [ , <table_hint> [ [ , ] ...n ] ] )
  | FOR TIMESTAMP AS OF '<point_in_time>'
}

<table_hint> ::=
{ NOEXPAND [ , INDEX ( <index_value> [ , ...n ] ) | INDEX = ( <index_value> ) ]
  | INDEX ( <index_value> [ , ...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [ , ... ] ) ) ]
  | FORCESCAN
  | HOLDLOCK
  | NOLOCK
  | NOWAIT
  | PAGLOCK
  | READCOMMITTED
  | READCOMMITTEDLOCK
  | READPAST
  | READUNCOMMITTED
  | REPEATABLEREAD
  | ROWLOCK
  | SERIALIZABLE
  | SNAPSHOT
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>
  | TABLOCK
  | TABLOCKX
  | UPDLOCK
  | XLOCK
}
```

## Arguments

#### { HASH | ORDER } GROUP

Specifies that aggregations that the query's `GROUP BY` or `DISTINCT` clause describes should use hashing or ordering.

- Generally, a hash-based algorithm can improve the performance of queries that involve large or complex grouping sets.
- Generally, a sort-based algorithm can improve the performance of queries that involve small or simple grouping sets.

#### { MERGE | HASH | CONCAT } UNION

Specifies that all `UNION` operations are run by merging, hashing, or concatenating `UNION` sets. If more than one `UNION` hint is specified, the Query Optimizer selects the least expensive strategy from those hints specified.

- Generally, a merge-based algorithm operation can improve the performance of queries that involve sorted inputs.
- Generally, a hash-based algorithm can improve the performance of queries that involve unsorted or large inputs.
- Generally, a concatenation-based algorithm can improve the performance of queries that involve distinct or small inputs.

#### { LOOP | MERGE | HASH } JOIN

Specifies that all join operations are performed by `LOOP JOIN`, `MERGE JOIN`, or `HASH JOIN` in the whole query. If you specify more than one join hint, the optimizer selects the least expensive join strategy from the allowed ones.

If you specify a join hint in the same query's `FROM` clause for a specific table pair, this join hint takes precedence in the joining of the two tables. The query hints, though, must still be honored. The join hint for the pair of tables might only restrict the selection of allowed join methods in the query hint. For more information, see [Join hints](hints-transact-sql-join).

#### DISABLE\_OPTIMIZED\_PLAN\_FORCING

**Applies to:** SQL Server (Starting with SQL Server 2022 (16.x))

Disables [Optimized plan forcing](../../relational-databases/performance/optimized-plan-forcing-query-store) for a query.

Optimized plan forcing reduces compilation overhead for repeating forced queries. Once the query execution plan is generated, specific compilation steps are stored for reuse as an optimization replay script. An optimization replay script is stored as part of the compressed showplan XML in [Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store), in a hidden `OptimizationReplay` attribute.

#### EXPAND VIEWS

Specifies the indexed views are expanded. Also specifies the Query Optimizer doesn't consider any indexed view as a replacement for any query part. A view is expanded when the view definition replaces the view name in the query text.

This query hint virtually disallows direct use of indexed views and indexes on indexed views in the query plan.

Note

The indexed view remains condensed if there's a direct reference to the view in the query's `SELECT` part. The view also remains condensed if you specify `WITH (NOEXPAND)` or `WITH (NOEXPAND, INDEX( <index_value> [ , *...n* ] ) )`. For more information about the query hint `NOEXPAND`, see [Using NOEXPAND](hints-transact-sql-table#using-noexpand).

The hint only affects the views in the statements' `SELECT` part, including those views in `INSERT`, `UPDATE`, `MERGE`, and `DELETE` statements.

#### FAST *integer\_value*

Specifies that the query is optimized for fast retrieval of the first *integer\_value* number of rows. This result is a non-negative integer. After the first *integer\_value* number of rows are returned, the query continues execution and produces its full result set.

#### FORCE ORDER

Specifies that the join order indicated by the query syntax is preserved during query optimization. Using `FORCE ORDER` doesn't affect possible role reversal behavior of the Query Optimizer.

`FORCE ORDER` preserves the join order specified in the query, which might improve the performance or consistency of queries that involve complex join conditions or hints.

Note

In a `MERGE` statement, the source table is accessed before the target table as the default join order, unless the `WHEN SOURCE NOT MATCHED` clause is specified. Specifying `FORCE ORDER` preserves this default behavior.

#### { FORCE | DISABLE } EXTERNALPUSHDOWN

Force or disable the pushdown of the computation of qualifying expressions in Hadoop. Only applies to queries using PolyBase. Doesn't push down to Azure storage.

#### { FORCE | DISABLE } SCALEOUTEXECUTION

Force or disable scale-out execution of PolyBase queries that are using external tables in [SQL Server 2019 Big Data Clusters](../../big-data-cluster/big-data-options). This hint is only honored by a query using the master instance of a SQL Big Data Cluster. The scale-out occurs across the compute pool of the big data cluster.

#### KEEP PLAN

Changes the [recompilation thresholds](../../relational-databases/statistics/statistics#auto_update_statistics-option) for temporary tables, and makes them identical to the thresholds for permanent tables. The estimated recompile threshold starts an automatic recompile for the query when the estimated number of indexed column changes are made to a table by running one of the following statements:

- `UPDATE`
- `DELETE`
- `MERGE`
- `INSERT`

Specifying `KEEP PLAN` makes sure a query isn't recompiled as frequently when there are multiple updates to a table.

#### KEEPFIXED PLAN

Forces the Query Optimizer not to recompile a query because of changes in statistics. Specifying `KEEPFIXED PLAN` makes sure that a query recompiles only if the schema of the underlying tables changes, or if `sp_recompile` runs against those tables.

#### IGNORE\_NONCLUSTERED\_COLUMNSTORE\_INDEX

**Applies to**: SQL Server (starting with SQL Server 2012 (11.x)).

Prevents the query from using a nonclustered memory optimized columnstore index. If the query contains the query hint to avoid the use of the columnstore index, and an index hint to use a columnstore index, the hints are in conflict and the query returns an error.

#### MAX\_GRANT\_PERCENT = &lt;numeric\_value&gt;

**Applies to**: SQL Server (starting with SQL Server 2012 (11.x) Service Pack 3, SQL Server 2014 (12.x) Service Pack 2 and Azure SQL Database.

The maximum memory grant size in `PERCENT` of configured memory limit. The query is guaranteed not to exceed this limit if the query is running in a user defined resource pool. In this case, if the query doesn't have the minimum required memory the system raises an error. If a query is running in the system pool (default), then it gets at minimum the memory required to run. The actual limit can be lower if the Resource Governor setting is lower than the value specified by this hint. Valid values are between 0.0 and 100.0.

The memory grant hint isn't available for index creation or index rebuilding.

#### MIN\_GRANT\_PERCENT = &lt;numeric\_value&gt;

**Applies to**: SQL Server (starting with SQL Server 2012 (11.x) Service Pack 3, SQL Server 2014 (12.x) Service Pack 2 and Azure SQL Database.

The minimum memory grant size in `PERCENT` of configured memory limit. The query is guaranteed to get `MAX(required memory, min grant)` because at least required memory is needed to start a query. Valid values are between 0.0 and 100.0.

The min\_grant\_percent memory grant option overrides the `sp_configure` option (minimum memory per query (KB)) regardless of the size. The memory grant hint isn't available for index creation or index rebuilding.

#### MAXDOP &lt;integer\_value&gt;

**Applies to**: SQL Server (starting with SQL Server 2008 (10.0.x)) and Azure SQL Database.

Overrides the **max degree of parallelism** configuration option of `sp_configure`. Also overrides the Resource Governor for the query specifying this option. The `MAXDOP` query hint can exceed the value configured with `sp_configure`. If `MAXDOP` exceeds the value configured with Resource Governor, the Database Engine uses the Resource Governor `MAXDOP` value, described in [ALTER WORKLOAD GROUP](../statements/alter-workload-group-transact-sql). All semantic rules used with the **max degree of parallelism** configuration option are applicable when you use the `MAXDOP` query hint. For more information, see [Server configuration: max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option).

Warning

If `MAXDOP` is set to zero, then the server chooses the max degree of parallelism.

#### MAXRECURSION &lt;integer\_value&gt;

Specifies the maximum number of recursions allowed for this query. *number* is a positive integer between 0 and 32,767. When 0 is specified, no limit is applied. If this option isn't specified, the default limit for the server is 100.

When the specified or default number for `MAXRECURSION` limit is reached during query execution, the query ends and an error returns.

Because of this error, all effects of the statement are rolled back. If the statement is a `SELECT` statement, partial results or no results might be returned. Any partial results returned might not include all rows on recursion levels beyond the specified maximum recursion level.

For more information, see [WITH common_table_expression](with-common-table-expression-transact-sql).

#### NO\_PERFORMANCE\_SPOOL

**Applies to**: SQL Server (starting with SQL Server 2016 (13.x)) and Azure SQL Database.

Prevents a spool operator from being added to query plans (except for the plans when spool is required to guarantee valid update semantics). The spool operator can reduce performance in some scenarios. For example, the spool uses `tempdb`, and `tempdb` contention can occur if there are many concurrent queries running with the spool operations.

#### OPTIMIZE FOR ( *@variable\_name* { UNKNOWN | = &lt;literal\_constant&gt; } [ , *...n* ] )

Instructs the Query Optimizer to use a particular value for a local variable when the query is compiled and optimized. The value is used only during query optimization, and not during query execution.

- *@variable\_name*

    The name of a local variable used in a query, to which a value can be assigned for use with the `OPTIMIZE FOR` query hint.
- `UNKNOWN`

    Specifies that the Query Optimizer uses statistical data instead of the initial value to determine the value for a local variable during query optimization.
- *literal\_constant*

    A literal constant value to be assigned *@variable\_name* for use with the `OPTIMIZE FOR` query hint. *literal\_constant* is used only during query optimization, and not as the value of *@variable\_name* during query execution. *literal\_constant* can be of any SQL Server system data type that can be expressed as a literal constant. The data type of *literal\_constant* must be implicitly convertible to the data type that *@variable\_name* references in the query.

OPTIMIZE FOR can counteract the optimizer's default parameter detection behavior. Also use `OPTIMIZE FOR` when you create plan guides. For more information, see [Recompile a Stored Procedure](../../relational-databases/stored-procedures/recompile-a-stored-procedure).

#### OPTIMIZE FOR UNKNOWN

Instructs the Query Optimizer to use the average selectivity of the predicate across all column values, instead of using the runtime parameter value when the query is compiled and optimized.

If you use `OPTIMIZE FOR @variable_name = <literal_constant>` and `OPTIMIZE FOR UNKNOWN` in the same query hint, the Query Optimizer uses the *literal\_constant* specified for a specific value. The Query Optimizer uses UNKNOWN for the rest of the variable values. The values are used only during query optimization, and not during query execution.

#### PARAMETERIZATION { SIMPLE | FORCED }

Specifies the parameterization rules that the SQL Server Query Optimizer applies to the query when it compiles.

Important

The `PARAMETERIZATION` query hint can only be specified inside a plan guide to override the current setting of the `PARAMETERIZATION` database `SET` option. It can't be specified directly within a query.

For more information, see [Specify Query Parameterization Behavior by Using Plan Guides](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides).

`SIMPLE` instructs the Query Optimizer to attempt simple parameterization. `FORCED` instructs the Query Optimizer to attempt forced parameterization. For more information, see [Forced Parameterization in the Query Processing Architecture Guide](../../relational-databases/query-processing-architecture-guide#forced-parameterization), and [Simple Parameterization in the Query Processing Architecture Guide](../../relational-databases/query-processing-architecture-guide#simple-parameterization).

#### QUERYTRACEON &lt;integer\_value&gt;

This option lets you enable a plan-affecting trace flag only during single-query compilation. Like other query-level options, you can use it together with plan guides to match the text of a query being executed from any session, and automatically apply a plan-affecting trace flag when this query is being compiled. The `QUERYTRACEON` option is only supported for Query Optimizer trace flags. For more information, see [Set trace flags with DBCC TRACEON](../database-console-commands/dbcc-traceon-trace-flags-transact-sql).

Using this option doesn't return any error or warning if an unsupported trace flag number is used. If the specified trace flag isn't one that affects a query execution plan, the option is silently ignored.

To use more than one trace flag in a query, specify one `QUERYTRACEON` hint for each different trace flag number.

#### RECOMPILE

Instructs the SQL Server Database Engine to generate a new, temporary plan for the query and immediately discard that plan after the query completes execution. The generated query plan doesn't replace a plan stored in cache when the same query runs without the `RECOMPILE` hint. Without specifying `RECOMPILE`, the Database Engine caches query plans and reuses them. When query plans are compiled, the `RECOMPILE` query hint uses the current values of any local variables in the query. If the query is inside a stored procedure, the current values passed to any parameters.

`RECOMPILE` is a useful alternative to creating a stored procedure. `RECOMPILE` uses the `WITH RECOMPILE` clause when only a subset of queries inside the stored procedure, instead of the whole stored procedure, must be recompiled. For more information, see [Recompile a Stored Procedure](../../relational-databases/stored-procedures/recompile-a-stored-procedure). `RECOMPILE` is also useful when you create plan guides.

#### ROBUST PLAN

Forces the Query Optimizer to try a plan that works for the maximum potential row size, possibly at the expense of performance. When the query is processed, intermediate tables and operators might have to store and process rows that are wider than any one of the input rows when the query is processed. The rows might be so wide that, sometimes, the particular operator can't process the row. If rows are that wide, the Database Engine produces an error during query execution. By using `ROBUST PLAN`, you instruct the Query Optimizer not to consider any query plans that might run into this problem.

If such a plan isn't possible, the Query Optimizer returns an error instead of deferring error detection to query execution. Rows can contain variable-length columns; the Database Engine allows for rows to be defined that have a maximum potential size beyond the ability of the Database Engine to process them. Generally, despite the maximum potential size, an application stores rows that have actual sizes within the limits that the Database Engine can process. If the Database Engine comes across a row that is too long, an execution error is returned.

#### USE HINT ( '*hint\_name*' )

**Applies to**: SQL Server (starting with SQL Server 2016 (13.x) SP1), Azure SQL Database, and Azure SQL Managed Instance.

Provides one or more extra hints to the query processor. The extra hints are specified with the hint name *inside single quotation marks*.

Tip

Hint names are case-insensitive.

The following hint names are supported:

| Hint | Description |
| --- | --- |
| `'ABORT_QUERY_EXECUTION'` | Blocks query execution. Intended to be used as a [Query Store hint](../../relational-databases/performance/query-store-hints) to let administrators block future execution of known problematic queries, for example non-essential queries affecting application workloads. For more information, see [Block future execution of problematic queries](../../relational-databases/performance/query-store-hints-best-practices#block-future-execution-of-problematic-queries).**Applies to**: Azure SQL Database, Azure SQL Managed Instance^[AUTD](/en-us/azure/azure-sql/managed-instance/update-policy#always-up-to-date-update-policy)^, and SQL Server 2025 (17.x). |
| `'ASSUME_FIXED_MIN_SELECTIVITY_FOR_REGEXP'` | The [Cardinality Estimation](../../relational-databases/performance/cardinality-estimation-sql-server) model for [REGEXP_LIKE](../functions/regexp-like-transact-sql) provides default selectivity values. Use this hint if the default estimation is too high. It sets the selectivity to a fixed lower selectivity value.**Applies to:** SQL Server 2025 (17.x) and later versions, and Azure SQL Database |
| `'ASSUME_FIXED_MAX_SELECTIVITY_FOR_REGEXP'` | The [Cardinality Estimation](../../relational-databases/performance/cardinality-estimation-sql-server) model for [REGEXP_LIKE](../functions/regexp-like-transact-sql) provides default selectivity values. Use this hint if the default estimation is too low. It sets the selectivity to a fixed higher selectivity value.**Applies to:** SQL Server 2025 (17.x) and later versions, and Azure SQL Database |
| `'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS'` | Generates a query plan using the Simple Containment assumption instead of the default Base Containment assumption for joins, under the Query Optimizer [Cardinality Estimation](../../relational-databases/performance/cardinality-estimation-sql-server) model of SQL Server 2014 (12.x) and later versions. This hint name is equivalent to [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 9476. |
| `'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES'` | Generates a plan using minimum selectivity when estimating AND predicates for filters to account for full correlation. This hint name is equivalent to [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 4137 when used with cardinality estimation model of SQL Server 2012 (11.x) and earlier versions, and has similar effect when [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 9471 is used with cardinality estimation model of SQL Server 2014 (12.x) and later versions. |
| `'ASSUME_FULL_INDEPENDENCE_FOR_FILTER_ESTIMATES'` | Generates a plan using maximum selectivity when estimating AND predicates for filters to account for full independence. This hint name is the default behavior of the cardinality estimation model of SQL Server 2012 (11.x) and earlier versions, and equivalent to [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 9472 when used with cardinality estimation model of SQL Server 2014 (12.x) and later versions.**Applies to**: Azure SQL Database |
| `'ASSUME_PARTIAL_CORRELATION_FOR_FILTER_ESTIMATES'` | Generates a plan using most to least selectivity when estimating AND predicates for filters to account for partial correlation. This hint name is the default behavior of the cardinality estimation model of SQL Server 2014 (12.x) and later versions.**Applies to**: Azure SQL Database |
| `'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'` | Disables batch mode adaptive joins. For more information, see [Batch mode Adaptive Joins](../../relational-databases/performance/intelligent-query-processing-details#batch-mode-adaptive-joins).**Applies to**: SQL Server 2017 (14.x) and later versions, and Azure SQL Database |
| `'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'` | Disables batch mode memory grant feedback. For more information, see [Batch mode memory grant feedback](../../relational-databases/performance/intelligent-query-processing-memory-grant-feedback#batch-mode-memory-grant-feedback).**Applies to**: SQL Server 2017 (14.x) and later versions, and Azure SQL Database |
| `'DISABLE_DEFERRED_COMPILATION_TV'` | Disables table variable deferred compilation. For more information, see [Table variable deferred compilation](../../relational-databases/performance/intelligent-query-processing-details#table-variable-deferred-compilation).**Applies to**: SQL Server 2019 (15.x) and later versions, and Azure SQL Database |
| `'DISABLE_INTERLEAVED_EXECUTION_TVF'` | Disables interleaved execution for multi-statement table-valued functions. For more information, see [Interleaved execution for multi-statement table-valued functions](../../relational-databases/performance/intelligent-query-processing-details#interleaved-execution-for-mstvfs).**Applies to**: SQL Server 2017 (14.x) and later versions, and Azure SQL Database |
| `'DISABLE_OPTIMIZED_NESTED_LOOP'` | Instructs the query processor not to use a sort operation (batch sort) for optimized nested loop joins when generating a query plan. This hint name is equivalent to [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 2340. This hint also applies to explicit sorts and batch sorts. |
| `'DISABLE_OPTIMIZER_ROWGOAL'` | Causes SQL Server to generate a plan that doesn't use row goal modifications with queries that contain these keywords:- `TOP`- `OPTION (FAST N)`- `IN`- `EXISTS`This hint name is equivalent to [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 4138. |
| `'DISABLE_PARAMETER_SNIFFING'` | Instructs Query Optimizer to use average data distribution while compiling a query with one or more parameters. This instruction makes the query plan independent on the parameter value that was first used when the query was compiled. This hint name is equivalent to [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 4136 or [database scoped configuration](../statements/alter-database-scoped-configuration-transact-sql) setting `PARAMETER_SNIFFING = OFF`. |
| `'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'` | Disables row mode memory grant feedback. For more information, see [Row mode memory grant feedback](../../relational-databases/performance/intelligent-query-processing-memory-grant-feedback#row-mode-memory-grant-feedback).**Applies to**: SQL Server 2019 (15.x) and later versions, and Azure SQL Database |
| `'DISABLE_TSQL_SCALAR_UDF_INLINING'` | Disables scalar UDF inlining. For more information, see [Scalar UDF inlining](../../relational-databases/user-defined-functions/scalar-udf-inlining).**Applies to**: SQL Server 2019 (15.x) and later versions, and Azure SQL Database |
| `'DISALLOW_BATCH_MODE'` | Disables batch mode execution. For more information, see [Execution modes](../../relational-databases/query-processing-architecture-guide#execution-modes).**Applies to**: SQL Server 2019 (15.x) and later versions, and Azure SQL Database |
| `'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'` | Enables automatically generated quick statistics (histogram amendment) for any leading index column for which cardinality estimation is needed. The histogram used to estimate cardinality is adjusted at query compile time to account for actual maximum or minimum value of this column. This hint name is equivalent to [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 4139. |
| `'ENABLE_QUERY_OPTIMIZER_HOTFIXES'` | Enables Query Optimizer hotfixes (changes released in SQL Server Cumulative Updates and Service Packs). This hint name is equivalent to [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 4199 or [database scoped configuration](../statements/alter-database-scoped-configuration-transact-sql) setting `QUERY_OPTIMIZER_HOTFIXES = ON`. |
| `'FORCE_DEFAULT_CARDINALITY_ESTIMATION'` | Forces the Query Optimizer to use [Cardinality Estimation](../../relational-databases/performance/cardinality-estimation-sql-server) model that corresponds to the current database compatibility level. Use this hint to override [database scoped configuration](../statements/alter-database-scoped-configuration-transact-sql) setting `LEGACY_CARDINALITY_ESTIMATION = ON` or [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 9481. |
| `'FORCE_LEGACY_CARDINALITY_ESTIMATION'` | Forces the Query Optimizer to use [Cardinality Estimation](../../relational-databases/performance/cardinality-estimation-sql-server) model of SQL Server 2012 (11.x) and earlier versions. This hint name is equivalent to [trace flag](../database-console-commands/dbcc-traceon-trace-flags-transact-sql) 9481 or [database scoped configuration](../statements/alter-database-scoped-configuration-transact-sql) setting `LEGACY_CARDINALITY_ESTIMATION = ON`. |
| `'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'`^1^ | Forces the Query Optimizer behavior at a query level. This behavior happens as if the query was compiled with database compatibility level *n*, where *n* is a supported database compatibility level. For a list of currently supported values for *n*, see [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql).**Applies to**: SQL Server 2017 (14.x) CU 10 and later versions, and Azure SQL Database |
| `'QUERY_PLAN_PROFILE'`^2^ | Enables lightweight profiling for the query. When a query that contains this new hint finishes, a new extended event, `query_plan_profile`, is fired. This extended event exposes execution statistics and actual execution plan XML similar to the `query_post_execution_showplan` extended event but only for queries that contains the new hint.**Applies to**: SQL Server 2016 (13.x) SP 2 CU 3, SQL Server 2017 (14.x) CU 11, and later versions |
| `'DISABLE_RESULT_SET_CACHE'` | Disables result set caching for a specific run of a query, if result set cache is enabled for the currently connected item. This means it will neither generate new result set cache nor use existing result set cache (if any). This could be useful in debugging or A/B testing scenarios. For more information, see [Result set caching](/en-us/fabric/data-warehouse/result-set-caching).**Applies to**: Warehouse in Microsoft Fabric |

^1^ The `QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n` hint doesn't override default or legacy cardinality estimation setting, if you force it through database scoped configuration, trace flag, or another query hint such as `QUERYTRACEON`. This hint only affects the behavior of the Query Optimizer. It doesn't affect other features of SQL Server that might depend on the [database compatibility level](../statements/alter-database-transact-sql-compatibility-level), such as the availability of certain database features. For more information, see [Developer's Choice: Hinting Query Execution model](/en-us/archive/blogs/sql_server_team/developers-choice-hinting-query-execution-model).

^2^ If you enable collecting the `query_post_execution_showplan` extended event, standard profiling infrastructure is added to every query that is running on the server, and therefore can affect overall server performance. If you enable the collection of `query_thread_profile` extended event to use lightweight profiling infrastructure instead, this results in much less performance overhead but still affects overall server performance. If you enable the `query_plan_profile` extended event, this only enables the lightweight profiling infrastructure for a query that executed with the `query_plan_profile` and therefore doesn't affect other workloads on the server. Use this hint to profile a specific query without affecting other parts of the server workload. For more information about lightweight profiling, see [Query Profiling Infrastructure](../../relational-databases/performance/query-profiling-infrastructure).

The list of all supported `USE HINT` names can be queried using the dynamic management view [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql).

Important

Some `USE HINT` hints might conflict with trace flags enabled at the global or session level, or database scoped configuration settings. In this case, the query level hint (`USE HINT`) always takes precedence. If a `USE HINT` conflicts with another query hint, or a trace flag enabled at the query level (such as by `QUERYTRACEON`), SQL Server will generate an error when trying to execute the query.

#### USE PLAN N'*xml\_plan*'

Forces the Query Optimizer to use an existing query plan for a query specified by *xml\_plan*.

The resulting execution plan forced by this feature is the same or similar to the plan being forced. Because the resulting plan might not be identical to the plan specified by `USE PLAN`, the performance of the plans can vary. In rare cases, the performance difference can be significant and negative; in that case, the administrator must remove the forced plan.

#### TABLE HINT ( *exposed\_object\_name* [ , &lt;table\_hint&gt; [ [ , ] *...n* ] ] )

Applies the specified table hint to the table or view that corresponds to *exposed\_object\_name*. We recommend using a table hint as a query hint only in the context of a [plan guide](../../relational-databases/performance/plan-guides).

*exposed\_object\_name* can be one of the following references:

- When an alias is used for the table or view in the [FROM](from-transact-sql) clause of the query, *exposed\_object\_name* is the alias.
- When an alias isn't used, *exposed\_object\_name* is the exact match of the table or view referenced in the `FROM` clause. For example, if the table or view is referenced using a two-part name, *exposed\_object\_name* is the same two-part name.

When you specify *exposed\_object\_name* without also specifying a table hint, any indexes you specify in the query as part of a table hint for the object are disregarded. The Query Optimizer then determines index usage. You can use this technique to eliminate the effect of an `INDEX` table hint when you can't modify the original query. See Example J.

&lt;table\_hint&gt;

NOEXPAND [ , INDEX ( *index\_value* [ ,...n ] ) | INDEX = ( *index\_value* ) ] | INDEX ( *index\_value* [ ,...n ] ) | INDEX = ( *index\_value* ) | FORCESEEK [ ( *index\_value* ( *index\_column\_name* [,... ] ) ) ] | FORCESCAN | HOLDLOCK | NOLOCK | NOWAIT | PAGLOCK | READCOMMITTED | READCOMMITTEDLOCK | READPAST | READUNCOMMITTED | REPEATABLEREAD | ROWLOCK | SERIALIZABLE | SNAPSHOT | SPATIAL\_WINDOW\_MAX\_CELLS = *integer\_value* | TABLOCK | TABLOCKX | UPDLOCK | XLOCK

The table hint to apply to the table or view that corresponds to *exposed\_object\_name* as a query hint. For a description of these hints, see [Table hints](hints-transact-sql-table).

Table hints other than `INDEX`, `FORCESCAN`, and `FORCESEEK` are disallowed as query hints unless the query already has a `WITH` clause specifying the table hint. For more information, see the Remarks section.

Caution

Specifying `FORCESEEK` with parameters limits the number of plans that can be considered by the Query Optimizer more than when specifying `FORCESEEK` without parameters. This might cause a "Plan cannot be generated" error to occur in more cases.

#### FOR TIMESTAMP AS OF '*point\_in\_time*'

**Applies to**: Warehouse in Microsoft Fabric

Use the `TIMESTAMP` syntax in the `OPTION` clause to query data as it existed in the past, part of the time travel feature in Microsoft Fabric Data Warehouse.

Specify the *point\_in\_time* in the format `yyyy-MM-ddTHH:mm:ss[.fff]` to return data as it appeared at that time. The time zone is always in UTC. Use the `CONVERT` syntax for the necessary datetime format with [style 126](../functions/cast-and-convert-transact-sql?view=fabric&amp;preserve-view=true#date-and-time-styles).

The `TIMESTAMP AS OF` hint can be specified only once using the `OPTION` clause. For more information and limitations, see [Query data as it existed in the past](/en-us/fabric/data-warehouse/time-travel).

Session-scoped temp tables (`#temp_table`) are unaffected by `FOR TIMESTAMP AS OF`.

#### FORCE [ SINGLE NODE | DISTRIBUTED ] PLAN

**Applies to**: Warehouse in Microsoft Fabric

Allows user to choose whether to force a single node plan or a distributed plan for query's execution.

## Remarks

Query hints can't be specified in an `INSERT` statement, except when a `SELECT` clause is used inside the statement.

Query hints can be specified only in the top-level query, not in subqueries. When a table hint is specified as a query hint, the hint can be specified in the top-level query or in a subquery. However, the value specified for *exposed\_object\_name* in the `TABLE HINT` clause must match exactly the exposed name in the query or subquery.

## Specify table hints as query hints

We recommend using the `INDEX`, `FORCESCAN`, or `FORCESEEK` table hint as a query hint only in the context of a [plan guide](../../relational-databases/performance/plan-guides). Plan guides are useful when you can't modify the original query, for example, because it's a third-party application. The query hint specified in the plan guide is added to the query before it compiles and is optimized. For ad hoc queries, use the `TABLE HINT` clause only when testing plan guide statements. For all other ad hoc queries, we recommend specifying these hints only as table hints.

When specified as a query hint, the `INDEX`, `FORCESCAN`, and `FORCESEEK` table hints are valid for the following objects:

- Tables
- Views
- Indexed views
- Common table expressions (the hint must be specified in the `SELECT` statement whose result set populates the common table expression)
- Dynamic Management Views (DMVs)
- Named subqueries

You can specify `INDEX`, `FORCESCAN`, and `FORCESEEK` table hints as query hints for a query that doesn't have any existing table hints. You can also use them to replace existing `INDEX`, `FORCESCAN`, or `FORCESEEK` hints in the query, respectively.

Table hints other than `INDEX`, `FORCESCAN`, and `FORCESEEK` are disallowed as query hints unless the query already has a `WITH` clause specifying the table hint. In this case, a matching hint must also be specified as a query hint. Specify the matching hint as a query hint by using `TABLE HINT` in the `OPTION` clause. This specification preserves the query's semantics. For example, if the query contains the table hint `NOLOCK`, the `OPTION` clause in the *@hints* parameter of the plan guide must also contain the `NOLOCK` hint. See Example K.

## Specify hints with Query Store hints

You can enforce hints on queries identified through Query Store without making code changes, using the [Query Store hints](../../relational-databases/performance/query-store-hints) feature. Use the [sys.sp_query_store_set_hints](../../relational-databases/system-stored-procedures/sys-sp-query-store-set-hints-transact-sql) stored procedure to apply a hint to a query. See Example N.

## Query hint support in Fabric Data Warehouse

[Data warehousing in Microsoft Fabric](/en-us/fabric/data-warehouse/data-warehousing) supports a subset of query hints:

- `HASH GROUP`
- `ORDER GROUP`
- `MERGE UNION`
- `HASH UNION`
- `CONCAT UNION`
- `FORCE ORDER`
- `USE HINT`
    - `ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES`
    - `ASSUME_FULL_INDEPENDENCE_FOR_FILTER_ESTIMATES`
    - `ASSUME_PARTIAL_CORRELATION_FOR_FILTER_ESTIMATES`
    - `ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS`

These query hints are exclusive to Microsoft Fabric Data Warehouse:

- `FORCE SINGLE NODE PLAN`, `FORCE DISTRIBUTED PLAN`, `DISABLE_RESULT_SET_CACHE`

## Examples

### A. Use MERGE JOIN

The following example specifies that `MERGE JOIN` runs the `JOIN` operation in the query. The example uses the `AdventureWorks2025` database.

```sql
SELECT *
FROM Sales.Customer AS c
INNER JOIN Sales.CustomerAddress AS ca ON c.CustomerID = ca.CustomerID
WHERE TerritoryID = 5
OPTION (MERGE JOIN);
GO
```

### B. Use OPTIMIZE FOR

The following example instructs the Query Optimizer to use the value `'Seattle'` for `@city_name` and to use the average selectivity of the predicate across all column values for `@postal_code` when optimizing the query. The example uses the `AdventureWorks2025` database.

```sql
CREATE PROCEDURE dbo.RetrievePersonAddress
@city_name NVARCHAR(30),
@postal_code NVARCHAR(15)
AS
SELECT * FROM Person.Address
WHERE City = @city_name AND PostalCode = @postal_code
OPTION ( OPTIMIZE FOR (@city_name = 'Seattle', @postal_code UNKNOWN) );
GO
```

### C. Use MAXRECURSION

`MAXRECURSION` can be used to prevent a poorly formed recursive common table expression from entering into an infinite loop. The following example intentionally creates an infinite loop and uses the `MAXRECURSION` hint to limit the number of recursion levels to two. The example uses the `AdventureWorks2025` database.

```sql
--Creates an infinite loop
WITH cte (CustomerID, PersonID, StoreID) AS
(
    SELECT CustomerID, PersonID, StoreID
    FROM Sales.Customer
    WHERE PersonID IS NOT NULL
  UNION ALL
    SELECT cte.CustomerID, cte.PersonID, cte.StoreID
    FROM cte
    JOIN  Sales.Customer AS e
        ON cte.PersonID = e.CustomerID
)
--Uses MAXRECURSION to limit the recursive levels to 2
SELECT CustomerID, PersonID, StoreID
FROM cte
OPTION (MAXRECURSION 2);
GO
```

After the coding error is corrected, `MAXRECURSION` is no longer required.

### D. Use MERGE UNION

The following example uses the `MERGE UNION` query hint. The example uses the `AdventureWorks2025` database.

```sql
SELECT *
FROM HumanResources.Employee AS e1
UNION
SELECT *
FROM HumanResources.Employee AS e2
OPTION (MERGE UNION);
GO
```

### E. Use HASH GROUP and FAST

The following example uses the `HASH GROUP` and `FAST` query hints. The example uses the `AdventureWorks2025` database.

```sql
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total
FROM Sales.SalesOrderDetail
WHERE UnitPrice < $5.00
GROUP BY ProductID, OrderQty
ORDER BY ProductID, OrderQty
OPTION (HASH GROUP, FAST 10);
GO
```

### F. Use MAXDOP

The following example uses the `MAXDOP` query hint. The example uses the `AdventureWorks2025` database.

```sql
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total
FROM Sales.SalesOrderDetail
WHERE UnitPrice < $5.00
GROUP BY ProductID, OrderQty
ORDER BY ProductID, OrderQty
OPTION (MAXDOP 2);
GO
```

### G. Use INDEX

The following examples use the `INDEX` hint. The first example specifies a single index. The second example specifies multiple indexes for a single table reference. In both examples, because you apply the `INDEX` hint on a table that uses an alias, the `TABLE HINT` clause must also specify the same alias as the exposed object name. The example uses the `AdventureWorks2025` database.

```sql
EXEC sp_create_plan_guide
    @name = N'Guide1',
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title
              FROM HumanResources.Employee AS e
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID
              WHERE e.ManagerID = 2;',
    @type = N'SQL',
    @module_or_batch = NULL,
    @params = NULL,
    @hints = N'OPTION (TABLE HINT(e, INDEX (IX_Employee_ManagerID)))';
GO
EXEC sp_create_plan_guide
    @name = N'Guide2',
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title
              FROM HumanResources.Employee AS e
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID
              WHERE e.ManagerID = 2;',
    @type = N'SQL',
    @module_or_batch = NULL,
    @params = NULL,
    @hints = N'OPTION (TABLE HINT(e, INDEX(PK_Employee_EmployeeID, IX_Employee_ManagerID)))';
GO
```

### H. Use FORCESEEK

The following example uses the `FORCESEEK` table hint. The `TABLE HINT` clause must also specify the same two-part name as the exposed object name. Specify the name when you apply the `INDEX` hint on a table that uses a two-part name. The example uses the `AdventureWorks2025` database.

```sql
EXEC sp_create_plan_guide
    @name = N'Guide3',
    @stmt = N'SELECT c.LastName, c.FirstName, HumanResources.Employee.Title
              FROM HumanResources.Employee
              JOIN Person.Contact AS c ON HumanResources.Employee.ContactID = c.ContactID
              WHERE HumanResources.Employee.ManagerID = 3
              ORDER BY c.LastName, c.FirstName;',
    @type = N'SQL',
    @module_or_batch = NULL,
    @params = NULL,
    @hints = N'OPTION (TABLE HINT( HumanResources.Employee, FORCESEEK))';
GO
```

### I. Use multiple table hints

The following example applies the `INDEX` hint to one table and the `FORCESEEK` hint to another. The example uses the `AdventureWorks2025` database.

```sql
EXEC sp_create_plan_guide
    @name = N'Guide4',
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title
              FROM HumanResources.Employee AS e
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID
              WHERE e.ManagerID = 3;',
    @type = N'SQL',
    @module_or_batch = NULL,
    @params = NULL,
    @hints = N'OPTION (TABLE HINT (e, INDEX( IX_Employee_ManagerID))
                       , TABLE HINT (c, FORCESEEK))';
GO
```

### J. Use TABLE HINT to override an existing table hint

The following example shows how to use the `TABLE HINT` hint. You can use the hint without specifying a hint to override the `INDEX` table hint behavior you specify in the `FROM` clause of the query. The example uses the `AdventureWorks2025` database.

```sql
EXEC sp_create_plan_guide
    @name = N'Guide5',
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title
              FROM HumanResources.Employee AS e WITH (INDEX (IX_Employee_ManagerID))
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID
              WHERE e.ManagerID = 3;',
    @type = N'SQL',
    @module_or_batch = NULL,
    @params = NULL,
    @hints = N'OPTION (TABLE HINT(e))';
GO
```

### K. Specify semantics-affecting table hints

The following example contains two table hints in the query: `NOLOCK`, which is semantic-affecting, and `INDEX`, which is non-semantic-affecting. To preserve the semantics of the query, the `NOLOCK` hint is specified in the `OPTIONS` clause of the plan guide. Along with the `NOLOCK` hint, specify the `INDEX` and `FORCESEEK` hints and replace the non-semantic-affecting `INDEX` hint in the query during statement compilation and optimization. The example uses the `AdventureWorks2025` database.

```sql
EXEC sp_create_plan_guide
    @name = N'Guide6',
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title
              FROM HumanResources.Employee AS e
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID
              WHERE e.ManagerID = 3;',
    @type = N'SQL',
    @module_or_batch = NULL,
    @params = NULL,
    @hints = N'OPTION (TABLE HINT (e, INDEX(IX_Employee_ManagerID), NOLOCK, FORCESEEK))';
GO
```

The following example shows an alternative method to preserving the semantics of the query and allowing the optimizer to choose an index other than the index specified in the table hint. Allow the optimizer to choose by specifying the `NOLOCK` hint in the `OPTIONS` clause. You specify the hint because it's semantic-affecting. Then, specify the `TABLE HINT` keyword with only a table reference and no `INDEX` hint. The example uses the `AdventureWorks2025` database.

```sql
EXEC sp_create_plan_guide
    @name = N'Guide7',
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title
              FROM HumanResources.Employee AS e
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID
              WHERE e.ManagerID = 2;',
    @type = N'SQL',
    @module_or_batch = NULL,
    @params = NULL,
    @hints = N'OPTION (TABLE HINT (e, NOLOCK))';
GO
```

### L. Use USE HINT

The following example uses the `RECOMPILE` and `USE HINT` query hints. The example uses the `AdventureWorks2025` database.

```sql
SELECT * FROM Person.Address
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (RECOMPILE, USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES', 'DISABLE_PARAMETER_SNIFFING'));
GO
```

### M. Use QUERYTRACEON HINT

The following example uses the `QUERYTRACEON` query hints. The example uses the `AdventureWorks2025` database. You can enable all plan-affecting hotfixes controlled by trace flag 4199 for a particular query using the following query:

```sql
SELECT *
FROM Person.Address
WHERE City = 'SEATTLE'
      AND PostalCode = 98104
OPTION (QUERYTRACEON 4199);
```

You can also use multiple trace flags as in the following query:

```sql
SELECT *
FROM Person.Address
WHERE City = 'SEATTLE'
      AND PostalCode = 98104
OPTION (QUERYTRACEON 4199, QUERYTRACEON 4137);
```

### N. Use Query Store hints

The [Query Store hints](../../relational-databases/performance/query-store-hints) feature provides an easy-to-use method for shaping query plans without changing application code.

First, identify the query that has already been executed in the Query Store catalog views, for example:

```sql
SELECT q.query_id, qt.query_sql_text
FROM sys.query_store_query_text qt
INNER JOIN sys.query_store_query q ON
    qt.query_text_id = q.query_text_id
WHERE query_sql_text like N'%ORDER BY ListingPrice DESC%'
  AND query_sql_text not like N'%query_store%';
GO
```

The following example applies the hint to force the [legacy cardinality estimator](../../relational-databases/performance/cardinality-estimation-sql-server) to query\_id 39, identified in Query Store:

```sql
EXECUTE sys.sp_query_store_set_hints
    @query_id = 39,
    @query_hints = N'OPTION (USE HINT (''FORCE_LEGACY_CARDINALITY_ESTIMATION''))';
```

The following example applies the hint to enforce a maximum memory grant size in `PERCENT` of configured memory limit to `query_id` 39, identified in Query Store:

```sql
EXECUTE sys.sp_query_store_set_hints
    @query_id = 39,
    @query_hints = N'OPTION (MAX_GRANT_PERCENT = 10)';
```

The following example applies multiple query hints to query\_id 39, including `RECOMPILE`, `MAXDOP 1`, and the SQL Server 2012 (11.x) query optimizer behavior:

```sql
EXECUTE sys.sp_query_store_set_hints
    @query_id = 39,
    @query_hints = N'OPTION(RECOMPILE, MAXDOP 1, USE HINT (''QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_110''))';
```

The following example blocks query with the query\_id 39 from future execution by applying the `ABORT_QUERY_EXECUTION` hint. The hint is in preview.

```sql
EXECUTE sys.sp_query_store_set_hints
    @query_id = 39,
    @query_hints = N'OPTION (USE HINT (''ABORT_QUERY_EXECUTION''))';
```

### O. Query data as of a point in time

**Applies to**: Warehouse in Microsoft Fabric

Use the `TIMESTAMP` syntax in the `OPTION` clause to query data as it existed in the past, in Fabric Data Warehouse. The following sample query returns data as it appeared on March 13, 2024 at 7:39:35.28 PM UTC. The time zone is always in UTC.

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey
OPTION (FOR TIMESTAMP AS OF '2024-03-13T19:39:35.28');--March 13, 2024 at 7:39:35.28 PM UTC
```

### P. Query force a single node or distributed query

**Applies to**: Warehouse in Microsoft Fabric

To force a query in Fabric Data Warehouse to use a single node, use the FORCE [ SINGLE NODE | DISTRIBUTED ] PLAN hint.

```sql
SELECT OrderDateKey, SalesAmount
FROM FactInternetSales
OPTION (FORCE SINGLE NODE PLAN);
```

To force a query in Fabric Data Warehouse to use a distributed query:

```sql
SELECT OrderDateKey, SalesAmount
FROM FactInternetSales
OPTION (FORCE DISTRIBUTED PLAN);
```

### Q. Disable a query from creating or applying result set cache

**Applies to**: Warehouse in Microsoft Fabric

Use `'DISABLE_RESULT_SET_CACHE'` as a `hint_name` to block result set cache for a particular run of a query. For more information, see [Result set caching](/en-us/fabric/data-warehouse/result-set-caching).

```sql
SELECT OrderDateKey,
       SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey
OPTION (USE HINT('DISABLE_RESULT_SET_CACHE'));
```