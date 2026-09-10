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
title: FREETEXT (Transact-SQL) - SQL Server | Microsoft Learn
canonicalUrl: https://learn.microsoft.com/en-us/sql/t-sql/queries/freetext-transact-sql?view=sql-server-ver17
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
description: FREETEXT (Transact-SQL)
author: VanMSFT
ms.author: vanto
ms.date: 2017-10-23T00:00:00.0000000Z
ms.service: sql
ms.subservice: t-sql
ms.topic: reference
ms.custom:
- ignite-2025
locale: en-us
document_id: 03b79f47-7668-2a8a-a932-3ef3d1b48de9
document_version_independent_id: 83aacb08-c21b-45cb-56e7-133a87e2a487
updated_at: 2026-08-24T17:40:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/sql-docs-pr/blob/live/docs/t-sql/queries/freetext-transact-sql.md
gitcommit: https://github.com/MicrosoftDocs/sql-docs-pr/blob/35290234fd67e105f228d27ea7a57f42512e8231/docs/t-sql/queries/freetext-transact-sql.md
git_commit_id: 35290234fd67e105f228d27ea7a57f42512e8231
default_moniker: sql-server-ver17
site_name: Docs
depot_name: SQL.sql-content
page_type: conceptual
toc_rel: ../../toc.json
pdf_url_template: https://learn.microsoft.com/pdfstore/en-us/SQL.sql-content/{branchName}{pdfName}
search.mshattr.devlang: tsql
word_count: 1190
asset_id: t-sql/queries/freetext-transact-sql
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
source_path: docs/t-sql/queries/freetext-transact-sql.md
cmProducts:
- https://microsoft-devrel.poolparty.biz/DevRelOfferingOntology/12ed19f9-ebdf-4c8a-8bcd-7a681836774d
- https://authoring-docs-microsoft.poolparty.biz/devrel/cbe4ca68-43ac-4375-aba5-5945a6394c20
- https://microsoft-devrel.poolparty.biz/DevRelOfferingOntology/c6f99e62-1cf6-4b71-af9b-649b05f80cce
spProducts:
- https://microsoft-devrel.poolparty.biz/DevRelOfferingOntology/3a764584-4f97-452b-8f1d-36f19b12f6ae
- https://authoring-docs-microsoft.poolparty.biz/devrel/ced846cc-6a3c-4c8f-9dfb-3de0e90e2742
- https://microsoft-devrel.poolparty.biz/DevRelOfferingOntology/3f56b378-07a9-4fa1-afe8-9889fdc77628
platformId: 6277a682-1b0d-4c49-4b4b-5e66aa90cf25
---

# FREETEXT (Transact-SQL) - SQL Server | Microsoft Learn

**Applies to:**![](../../includes/media/yes-icon.svg)[SQL Server](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Database](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Managed Instance](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL database in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)

Is a predicate used in the Transact-SQL [WHERE clause](where-transact-sql) of a Transact-SQL SELECT statement to perform a SQL Server full-text search on full-text indexed columns containing character-based data types. This predicate searches for values that match the meaning and not just the exact wording of the words in the search condition. When FREETEXT is used, the full-text query engine internally performs the following actions on the *freetext\_string*, assigns each term a weight, and then finds the matches:

- Separates the string into individual words based on word boundaries (word-breaking).
- Generates inflectional forms of the words (stemming).
- Identifies a list of expansions or replacements for the terms based on matches in the thesaurus.

Note

For information about the forms of full-text searches that are supported by SQL Server, see [Query with Full-Text Search](../../relational-databases/search/query-with-full-text-search).

**Applies to**: SQL Server ( SQL Server 2008 (10.0.x) through [current version](/en-us/troubleshoot/sql/general/determine-version-edition-update-level)).

![](../../includes/media/topic-link-icon.svg)[Transact-SQL syntax conventions](../language-elements/transact-sql-syntax-conventions-transact-sql)

## Syntax

```syntaxsql
FREETEXT ( { column_name | (column_list) | * }   
          , 'freetext_string' [ , LANGUAGE language_term ] )  
```

## Arguments

*column\_name* Is the name of one or more full-text indexed columns of the table specified in the FROM clause. The columns can be of type **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary**, or **varbinary(max)**.

*column\_list* Indicates that several columns, separated by a comma, can be specified. *column\_list* must be enclosed in parentheses. Unless *language\_term* is specified, the language of all columns of *column\_list* must be the same.

\* Specifies that all columns that have been registered for full-text searching should be used to search for the given *freetext\_string*. If more than one table is in the FROM clause, \* must be qualified by the table name. Unless *language\_term* is specified, the language of all columns of the table must be the same.

*freetext\_string* Is text to search for in the *column\_name*. Any text, including words, phrases or sentences, can be entered. Matches are generated if any term or the forms of any term is found in the full-text index.

Unlike in the CONTAINS and CONTAINSTABLE search condition where AND is a keyword, when used in *freetext\_string* the word 'and' is considered a noise word, or [stopword](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search), and will be discarded.

Use of WEIGHT, FORMSOF, wildcards, NEAR and other syntax is not allowed. *freetext\_string* is wordbroken, stemmed, and passed through the thesaurus.

*freetext\_string* is **nvarchar**. An implicit conversion occurs when another character data type is used as input. Large string data types nvarchar(max) and varchar(max) cannot be used. In the following example, the `@SearchWord` variable, which is defined as `varchar(30)`, causes an implicit conversion in the `FREETEXT` predicate.

```sql
USE AdventureWorks2022;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```

Because "parameter sniffing" does not work across conversion, use **nvarchar** for better performance. In the example, declare `@SearchWord` as `nvarchar(30)`.

```sql
USE AdventureWorks2022;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```

You can also use the OPTIMIZE FOR query hint for cases in which a nonoptimal plan is generated.

LANGUAGE *language\_term* Is the language whose resources will be used for word breaking, stemming, and thesaurus and stopword removal as part of the query. This parameter is optional and can be specified as a string, integer, or hexadecimal value corresponding to the locale identifier (LCID) of a language. If *language\_term* is specified, the language it represents will be applied to all elements of the search condition. If no value is specified, the column full-text language is used.

If documents of different languages are stored together as binary large objects (BLOBs) in a single column, the locale identifier (LCID) of a given document determines what language is used to index its content. When querying such a column, specifying LANGUAGE *language\_term* can increase the probability of a good match.

When specified as a string, *language\_term* corresponds to the **alias** column value in the [sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql) compatibility view. The string must be enclosed in single quotation marks, as in '*language\_term*'. When specified as an integer, *language\_term* is the actual LCID that identifies the language. When specified as a hexadecimal value, *language\_term* is 0x followed by the hexadecimal value of the LCID. The hexadecimal value must not exceed eight digits, including leading zeros.

If the value is in double-byte character set (DBCS) format, Microsoft SQL Server will convert it to Unicode.

If the language specified is not valid or there are no resources installed that correspond to that language, Microsoft SQL Server returns an error. To use the neutral language resources, specify 0x0 as *language\_term*.

## General Remarks

Full-text predicates and functions work on a single table, which is implied in the FROM predicate. To search on multiple tables, use a joined table in your FROM clause to search on a result set that is the product of two or more tables.

Full-text queries using FREETEXT are less precise than those full-text queries using CONTAINS. The SQL Server full-text search engine identifies important words and phrases. No special meaning is given to any of the reserved keywords or wildcard characters that typically have meaning when specified in the &lt;contains\_search\_condition&gt; parameter of the CONTAINS predicate.

Full-text predicates are not allowed in the [OUTPUT clause](output-clause-transact-sql) when the database compatibility level is set to 100.

Note

The FREETEXTTABLE function is useful for the same kinds of matches as the FREETEXT predicate. You can reference this function like a regular table name in the [FROM clause](from-transact-sql) of a SELECT statement. For more information, see [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql).

## Querying Remote Servers

You can use a four-part name in the [CONTAINS](contains-transact-sql) or FREETEXT predicate to query full-text indexed columns of the target tables on a linked server. To prepare a remote server to receive full-text queries, create a full-text index on the target tables and columns on the remote server and then add the remote server as a linked server.

## Comparison of LIKE to Full-Text Search

In contrast to full-text search, the [LIKE](../language-elements/like-transact-sql)Transact-SQL predicate works on character patterns only. Also, you cannot use the LIKE predicate to query formatted binary data. Furthermore, a LIKE query against a large amount of unstructured text data is much slower than an equivalent full-text query against the same data. A LIKE query against millions of rows of text data can take minutes to return; whereas a full-text query can take only seconds or less against the same data, depending on the number of rows that are returned.

## Examples

### A. Using FREETEXT to search for words containing specified character values

The following example searches for all documents containing the words related to vital, safety, components.

```sql
USE AdventureWorks2022;  
GO  
SELECT Title  
FROM Production.Document  
WHERE FREETEXT (Document, 'vital safety components' );  
GO  
```

### B. Using FREETEXT with variables

The following example uses a variable instead of a specific search term.

```sql
USE AdventureWorks2022;  
GO  
DECLARE @SearchWord NVARCHAR(30);  
SET @SearchWord = N'high-performance';  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
GO  
```