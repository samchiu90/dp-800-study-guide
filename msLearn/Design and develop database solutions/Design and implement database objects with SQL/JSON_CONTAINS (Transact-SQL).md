---
layout: Conceptual
monikers:
- sql-server-2017
- sql-server-ver15
- sql-server-ver16
- sql-server-ver17
defaultMoniker: sql-server-ver17
versioningType: Ranged
title: JSON_CONTAINS (Transact-SQL) - SQL Server | Microsoft Learn
canonicalUrl: https://learn.microsoft.com/en-us/sql/t-sql/functions/json-contains-transact-sql?view=sql-server-ver17
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
description: The JSON_CONTAINS function searches for a SQL value in a path in a JSON document.
author: uc-msft
ms.author: umajay
ms.reviewer: randolphwest
ms.date: 2025-10-27T00:00:00.0000000Z
ms.service: sql
ms.subservice: t-sql
ms.topic: language-reference
ms.custom:
- build-2025
locale: en-us
document_id: f12d8836-0fff-ed41-f1b9-07c7aca779ed
document_version_independent_id: f12d8836-0fff-ed41-f1b9-07c7aca779ed
updated_at: 2026-08-24T17:40:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/sql-docs-pr/blob/live/docs/t-sql/functions/json-contains-transact-sql.md
gitcommit: https://github.com/MicrosoftDocs/sql-docs-pr/blob/35290234fd67e105f228d27ea7a57f42512e8231/docs/t-sql/functions/json-contains-transact-sql.md
git_commit_id: 35290234fd67e105f228d27ea7a57f42512e8231
default_moniker: sql-server-ver17
site_name: Docs
depot_name: SQL.sql-content
page_type: conceptual
toc_rel: ../../toc.json
pdf_url_template: https://learn.microsoft.com/pdfstore/en-us/SQL.sql-content/{branchName}{pdfName}
word_count: 1015
asset_id: t-sql/functions/json-contains-transact-sql
moniker_range_name: 047878d4dd917f6615e82c47a9e6489b
monikers:
- sql-server-2017
- sql-server-ver15
- sql-server-ver16
- sql-server-ver17
item_type: Content
source_path: docs/t-sql/functions/json-contains-transact-sql.md
cmProducts:
- https://microsoft-devrel.poolparty.biz/DevRelOfferingOntology/12ed19f9-ebdf-4c8a-8bcd-7a681836774d
- https://authoring-docs-microsoft.poolparty.biz/devrel/cbe4ca68-43ac-4375-aba5-5945a6394c20
- https://authoring-docs-microsoft.poolparty.biz/devrel/540ac133-a371-4dbb-8f94-28d6cc77a70b
spProducts:
- https://microsoft-devrel.poolparty.biz/DevRelOfferingOntology/3a764584-4f97-452b-8f1d-36f19b12f6ae
- https://authoring-docs-microsoft.poolparty.biz/devrel/ced846cc-6a3c-4c8f-9dfb-3de0e90e2742
- https://authoring-docs-microsoft.poolparty.biz/devrel/60bfc045-f127-4841-9d00-ea35495a5800
platformId: 57eb676a-4790-00a8-d90d-c83faa8b70dc
---

# JSON_CONTAINS (Transact-SQL) - SQL Server | Microsoft Learn

**Applies to:**![](../../includes/media/yes-icon.svg) SQL Server 2025 (17.x)

Searches for a SQL value in a path in a JSON document.

Note

The `JSON_CONTAINS` function is currently in preview and only available in SQL Server 2025 (17.x).

![](../../includes/media/topic-link-icon.svg)[Transact-SQL syntax conventions](../language-elements/transact-sql-syntax-conventions-transact-sql)

## Syntax

```syntaxsql
JSON_CONTAINS( target_expression , search_value_expression [ , path_expression ]  [ , search_mode ] )
```

## Arguments

#### *target\_expression*

An expression that returns a target JSON document to search. The value can be a **json** type or character string value that contains a JSON document.

#### *search\_value\_expression*

An expression that returns a SQL scalar value or **json** type value to search in the specified SQL/JSON document.

#### *path*

A SQL/JSON path that specifies the search target in the JSON document. This parameter is optional.

You can provide a variable as the value of *path*. The JSON path can specify lax or strict mode for parsing. If you don't specify the parsing mode, lax mode is the default. For more info, see [JSON Path Expressions in the SQL Database Engine](../../relational-databases/json/json-path-expressions-sql-server).

The default value for *path* is `$`. As a result, if you don't provide a value for *path*, `JSON_CONTAINS` searches for the value in the entire JSON document.

If the format of *path* isn't valid, `JSON_CONTAINS` returns an error.

#### *search\_mode*

Indicates if the search mode for the value should use an equality or LIKE predicate semantics. This parameter only applies when the *search\_value\_expression* is a character string value. The default value for *search\_mode* is 0, which indicates equality predicate semantics. If the *search\_mode* is 1, then it indicates that LIKE predicate semantics should be used.

## Return value

Returns an **int** value of `0`, `1`, or `NULL`. A value of `1` indicates that the specified search value was contained within the target JSON document, or `0` otherwise. The `JSON_CONTAINS` function returns `NULL` if any of the arguments is `NULL`, or if the specified SQL/JSON path isn't found in the JSON document.

## Remarks

The `JSON_CONTAINS` function follows these rules for searching if a value is contained in a JSON document:

- A scalar search value is contained in a target scalar if and only if they're comparable and are equal. Since **json** types have only JSON number or string or true/false value, the possible SQL scalar types that can be specified as search value are limited to the SQL numeric types, character string types, and the **bit** type.
- The SQL type of the scalar search value is used to perform the comparison with the **json** type value in the specified path. This is different from `JSON_VALUE`-based predicate where the `JSON_VALUE` function always returns a character string value.
- A JSON array search value is contained in a target array if and only if every element in the search array is contained in some element of the target array.
- A scalar search value is contained in a target array if and only if the search value is contained in some element of the target array.
- A JSON object search value is contained in a target object if and only if each key/value in the search object is found in the target object.

## Limitations

Using the `JSON_CONTAINS` function has the following limitations:

- The **json** type isn't supported as search value.
- The JSON object or array returned from `JSON_QUERY` isn't supported as search value.
- The path parameter is currently required.
- If the SQL/JSON path points to an array then wildcard is required in the SQL/JSON path expression. Automatic array unwrapping is currently only at the first level.

JSON index support includes the `JSON_CONTAINS` predicate and the following operators:

- Comparison operators (`=`)
- `IS [NOT] NULL` predicate (not currently supported)

## Examples

### A. Search for a SQL integer value in a JSON path

The following example shows how to search for a SQL **int** value in a JSON array in a JSON path.

```sql
DECLARE @j AS JSON = '{"a": 1, "b": 2, "c": {"d": 4, "ce":["dd"]}, "d": [1, 3, {"df": [89]}, false], "e":null, "f":true}';

SELECT json_contains(@j, 1, '$.a') AS is_value_found;
```

Here's the result set.

```output
is_value_found
--------
1
```

### B. Search for a SQL character string value in a JSON path

The following example shows how to search for a SQL character string value in a JSON array in a JSON path.

```sql
DECLARE @j AS JSON = '{"a": 1, "b": 2, "c": {"d": 4, "ce":["dd"]}, "d": [1, 3, {"df": [89]}, false], "e":null, "f":true}';

SELECT json_contains(@j, 'dd', '$.c.ce[*]') AS is_value_found;
```

Here's the result set.

```output
is_value_found
--------
1
```

### C. Search for a SQL bit value in a JSON array in a JSON path

The following example shows how to search for a **sql** bit value in a JSON array in a JSON path.

```sql
DECLARE @j AS JSON = '{"a": 1, "b": 2, "c": {"d": 4, "ce":["dd"]}, "d": [1, 3, {"df": [89]}, false], "e":null, "f":true}';

SELECT json_contains(@j, CAST (0 AS BIT), '$.d[*]') AS is_value_found;
```

Here's the result set.

```output
is_value_found
--------
1
```

### D. Search for a SQL integer value contained within a nested JSON array

The following example shows how to search for a SQL **int** value contained within a nested JSON array in a JSON path.

```sql
DECLARE @j AS JSON = '{"a": 1, "b": 2, "c": {"d": 4, "ce":["dd"]}, "d": [1, 3, {"df": [89]}, false], "e":null, "f":true}';

SELECT json_contains(@j, 89, '$.d[*].df[*]') AS is_value_found;
```

Here's the result set.

```output
is_value_found
--------
1
```

### E. Search for a SQL integer value contained within a JSON object in a JSON array

The following example shows how to search for a SQL **int** value contained within a JSON object in a JSON array in a JSON path.

```sql
DECLARE @j AS JSON = '[{"a": 1}, {"b": 2}, {"c": 3}, {"a": 56}]';

SELECT json_contains(@j, 56, '$[*].a') AS is_value_found;
```

Here's the result set.

```output
is_value_found
--------
1
```

### F. Search for a SQL character string value in a JSON path using a wildcard pattern

The following example shows how to search for a SQL character string value using a pattern in a JSON array in a JSON path.

```sql
DECLARE @j AS JSON = '{"a": 1, "b": 2, "c": {"d": 4, "ce":["dd"]}, "d": [1, 3, {"df": [89]}, false], "e":null, "f":true}';

SELECT json_contains(@j, 'dd', '$.c.ce[*]') AS is_value_found;
```

Here's the result set.

```output
is_value_found
--------
1
```