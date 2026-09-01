---
layout: Conceptual
monikers:
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
title: Modify Data Through a View - SQL Server | Microsoft Learn
canonicalUrl: https://learn.microsoft.com/en-us/sql/relational-databases/views/modify-data-through-a-view?view=sql-server-ver17
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
description: Learn how to modify data through a view.
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: randolphwest
ms.date: 2025-09-26T00:00:00.0000000Z
ms.service: sql
ms.subservice: table-view-index
ms.topic: how-to
ms.custom:
- ignite-2025
locale: en-us
document_id: caab144e-f326-f6b7-aa87-0589c39a9f57
document_version_independent_id: 8b5406c8-9083-af2f-015c-ccbbb502cad6
updated_at: 2026-08-24T22:38:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/sql-docs-pr/blob/live/docs/relational-databases/views/modify-data-through-a-view.md
gitcommit: https://github.com/MicrosoftDocs/sql-docs-pr/blob/58c9ba7063458281263c05cba4e0e8dc7da7358a/docs/relational-databases/views/modify-data-through-a-view.md
git_commit_id: 58c9ba7063458281263c05cba4e0e8dc7da7358a
default_moniker: sql-server-ver17
site_name: Docs
depot_name: SQL.sql-content
page_type: conceptual
toc_rel: ../../toc.json
pdf_url_template: https://learn.microsoft.com/pdfstore/en-us/SQL.sql-content/{branchName}{pdfName}
word_count: 428
asset_id: relational-databases/views/modify-data-through-a-view
moniker_range_name: b55b134c56d21255490a7666a8a966c9
monikers:
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
source_path: docs/relational-databases/views/modify-data-through-a-view.md
cmProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/cbe4ca68-43ac-4375-aba5-5945a6394c20
- https://authoring-docs-microsoft.poolparty.biz/devrel/6ab7faaf-d791-4a26-96a2-3b11738538e7
spProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/ced846cc-6a3c-4c8f-9dfb-3de0e90e2742
- https://authoring-docs-microsoft.poolparty.biz/devrel/302e28b0-1f09-4811-9a9b-2a72e0770581
platformId: fe2a12cb-c1f4-3030-0fbf-b4bdc40859f4
---

# Modify Data Through a View - SQL Server | Microsoft Learn

**Applies to:**![](../../includes/media/yes-icon.svg)[SQL Server](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Database](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Managed Instance](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure Synapse Analytics](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Analytics Platform System (PDW)](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL database in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)

You can modify the data of an underlying base table in SQL Server by using SQL Server Management Studio or Transact-SQL.

## Limitations

See the section 'Updatable Views' in [CREATE VIEW](../../t-sql/statements/create-view-transact-sql).

## Permissions

Requires `UPDATE`, `INSERT`, or `DELETE` permissions on the target table, depending on the action being performed.

## Use SQL Server Management Studio

### Modify table data through a view

1. In **Object Explorer**, expand the database that contains the view and then expand **Views**.
2. Right-click the view and select **Edit Top 200 Rows**.
3. You might need to modify the `SELECT` statement in the **SQL** pane to return the rows to be modified.
4. In the **Results** pane, locate the row to be changed or deleted. To delete the row, right-click the row and select **Delete**. To change data in one or more columns, modify the data in the column.

    You can't delete a row if the view references more than one base table. You can only update columns that belong to a single base table.
5. To insert a row, scroll down to the end of the rows and insert the new values.

    You can't insert a row if the view references more than one base table.

## Use Transact-SQL

### Update table data through a view

1. In **Object Explorer**, connect to an instance of Database Engine.
2. On the Standard bar, select **New Query**.
3. Copy and paste the following example into the query window and select **Execute**. This example changes the value in the `StartDate` and `EndDate` columns for a specific employee by referencing columns in the view `HumanResources.vEmployeeDepartmentHistory`. This view returns values from two tables. This statement succeeds because the columns being modified are from only one of the base tables.

    ```sql
    USE AdventureWorks2022;
    GO
    
    UPDATE HumanResources.vEmployeeDepartmentHistory
        SET StartDate = '20110203',
            EndDate   = GETDATE()
    WHERE LastName = N'Smith'
          AND FirstName = 'Samantha';
    GO
    ```

For more information, see [UPDATE](../../t-sql/queries/update-transact-sql).

### Insert table data through a view

1. In **Object Explorer**, connect to an instance of Database Engine.
2. On the Standard bar, select **New Query**.
3. Copy and paste the following example into the query window and select **Execute**. The example inserts a new row into the base table `HumanResources.Department` by specifying the relevant columns from the view `HumanResources.vEmployeeDepartmentHistory`. The statement succeeds because only columns from a single base table are specified and the other columns in the base table have default values.

    ```sql
    USE AdventureWorks2022;
    GO
    
    INSERT INTO HumanResources.vEmployeeDepartmentHistory (Department, GroupName)
    VALUES ('MyDepartment', 'MyGroup');
    GO
    ```

For more information, see [INSERT](../../t-sql/statements/insert-transact-sql).