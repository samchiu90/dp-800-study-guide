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
title: Primary and foreign key constraints - SQL Server | Microsoft Learn
canonicalUrl: https://learn.microsoft.com/en-us/sql/relational-databases/tables/primary-and-foreign-key-constraints?view=sql-server-ver17
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
description: Learn about primary and foreign key constraints, important objects used to enforce data integrity in database tables.
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: randolphwest
ms.date: 2024-07-19T00:00:00.0000000Z
ms.service: sql
ms.subservice: table-view-index
ms.topic: concept-article
ms.custom:
- ignite-2025
locale: en-us
document_id: 6691b257-bd0b-bbaa-40fc-07521f9f70b6
document_version_independent_id: af1dbed3-99b4-11ef-15e7-687c6e7535d7
updated_at: 2026-08-24T17:40:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/sql-docs-pr/blob/live/docs/relational-databases/tables/primary-and-foreign-key-constraints.md
gitcommit: https://github.com/MicrosoftDocs/sql-docs-pr/blob/2b1560ace793f3b5b44b879c6d0b915584c1e5e2/docs/relational-databases/tables/primary-and-foreign-key-constraints.md
git_commit_id: 2b1560ace793f3b5b44b879c6d0b915584c1e5e2
default_moniker: sql-server-ver17
site_name: Docs
depot_name: SQL.sql-content
page_type: conceptual
toc_rel: ../../toc.json
pdf_url_template: https://learn.microsoft.com/pdfstore/en-us/SQL.sql-content/{branchName}{pdfName}
word_count: 1824
asset_id: relational-databases/tables/primary-and-foreign-key-constraints
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
source_path: docs/relational-databases/tables/primary-and-foreign-key-constraints.md
cmProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/cbe4ca68-43ac-4375-aba5-5945a6394c20
- https://authoring-docs-microsoft.poolparty.biz/devrel/6ab7faaf-d791-4a26-96a2-3b11738538e7
spProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/ced846cc-6a3c-4c8f-9dfb-3de0e90e2742
- https://authoring-docs-microsoft.poolparty.biz/devrel/302e28b0-1f09-4811-9a9b-2a72e0770581
platformId: 178b825b-82c3-6d53-efb8-c866574ba8d5
---

# Primary and foreign key constraints - SQL Server | Microsoft Learn

**Applies to:**![](../../includes/media/yes-icon.svg) SQL Server 2016 (13.x) and later versions ![](../../includes/media/yes-icon.svg)[Azure SQL Database](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[Azure SQL Managed Instance](../../sql-server/sql-docs-navigation-guide#applies-to)![](../../includes/media/yes-icon.svg)[SQL database in Microsoft Fabric](../../sql-server/sql-docs-navigation-guide#applies-to)

Primary keys and foreign keys are two types of constraints that can be used to enforce data integrity in SQL Server tables. These are important database objects.

## Primary key constraints

A table typically has a column or combination of columns that contain values that uniquely identify each row in the table. This column, or columns, is called the primary key (PK) of the table and enforces the entity integrity of the table. Because primary key constraints guarantee unique data, they're frequently defined on an identity column.

When you specify a primary key constraint for a table, the Database Engine enforces data uniqueness by automatically creating a unique index for the primary key columns. This index also permits fast access to data when the primary key is used in queries. If a primary key constraint is defined on more than one column, values can be duplicated within one column, but each combination of values from all the columns in the primary key constraint definition must be unique.

As shown in the following illustration, the `ProductID` and `VendorID` columns in the `Purchasing.ProductVendor` table form a composite primary key constraint for this table. This makes sure that every row in the `ProductVendor` table has a unique combination of `ProductID` and `VendorID`. This prevents the insertion of duplicate rows.

![Diagram of rows in a table for a composite PRIMARY KEY constraint.](media/primary-and-foreign-key-constraints/composite-primary-key.gif)

- A table can contain only one primary key constraint.
- A primary key can't exceed 32 columns and a total key length of 900 bytes.
- The index generated by a primary key constraint can't cause the number of indexes on the table to exceed 999 nonclustered indexes and 1 clustered index.
- If clustered or nonclustered isn't specified for a primary key constraint, clustered is used if there's no clustered index on the table.
- All columns defined within a primary key constraint must be defined as not null. If nullability isn't specified, all columns participating in a primary key constraint have their nullability set to not null.
- If a primary key is defined on a CLR user-defined type column, the implementation of the type must support binary ordering.

## Foreign key constraints

A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables to control the data that can be stored in the foreign key table. In a foreign key reference, a link is created between two tables when the column or columns that hold the primary key value for one table are referenced by the column or columns in another table. This column becomes a foreign key in the second table.

For example, the `Sales.SalesOrderHeader` table has a foreign key link to the `Sales.SalesPerson` table because there's a logical relationship between sales orders and salespeople. The `SalesPersonID` column in the `SalesOrderHeader` table matches the primary key column of the `SalesPerson` table. The `SalesPersonID` column in the `SalesOrderHeader` table is the foreign key to the `SalesPerson` table. By creating this foreign key relationship, a value for `SalesPersonID` can't be inserted into the `SalesOrderHeader` table if it doesn't already exist in the `SalesPerson` table.

A table can reference a maximum of 253 other tables and columns as foreign keys (outgoing references). SQL Server 2016 (13.x) increases the limit for the number of other tables and columns that can reference columns in a single table (incoming references), from 253 to 10,000. (Requires at least 130 compatibility level.) The increase has the following restrictions:

- Greater than 253 foreign key references are only supported for `DELETE` DML operations. `UPDATE` and `MERGE` operations aren't supported.
- A table with a foreign key reference to itself is still limited to 253 foreign key references.
- Greater than 253 foreign key references aren't currently available for columnstore indexes, memory-optimized tables, Stretch Database, or partitioned foreign key tables.

    Important

    Stretch Database is deprecated in SQL Server 2022 (16.x) and Azure SQL Database. This feature will be removed in a future version of the Database Engine. Avoid using this feature in new development work, and plan to modify applications that currently use this feature.

## Indexes on foreign key constraints

Unlike primary key constraints, creating a foreign key constraint doesn't automatically create a corresponding index. However, manually creating an index on a foreign key is often useful for the following reasons:

- Foreign key columns are frequently used in join criteria when the data from related tables is combined in queries by matching the column or columns in the foreign key constraint of one table with the primary or unique key column or columns in the other table. An index enables the Database Engine to quickly find related data in the foreign key table. However, creating this index isn't required. Data from two related tables can be combined even if no primary key or foreign key constraints are defined between the tables, but a foreign key relationship between two tables indicates that the two tables have been optimized to be combined in a query that uses the keys as its criteria.
- Changes to primary key constraints are checked with foreign key constraints in related tables.

## Referential integrity

Although the main purpose of a foreign key constraint is to control the data that can be stored in the foreign key table, it also controls changes to data in the primary key table. For example, if the row for a salesperson is deleted from the `Sales.SalesPerson` table, and the salesperson's ID is used for sales orders in the `Sales.SalesOrderHeader` table, the relational integrity between the two tables is broken; the deleted salesperson's sales orders are orphaned in the `SalesOrderHeader` table without a link to the data in the `SalesPerson` table.

A foreign key constraint prevents this situation. The constraint enforces referential integrity by guaranteeing that changes can't be made to data in the primary key table if those changes invalidate the link to data in the foreign key table. If an attempt is made to delete the row in a primary key table or to change a primary key value, the action fails when the deleted or changed primary key value corresponds to a value in the foreign key constraint of another table. To successfully change or delete a row in a foreign key constraint, you must first either delete the foreign key data in the foreign key table or change the foreign key data in the foreign key table, which links the foreign key to different primary key data.

### Cascading referential integrity

By using cascading referential integrity constraints, you can define the actions that the Database Engine takes when a user tries to delete or update a key to which existing foreign keys point. The following cascading actions can be defined.

- `NO ACTION`

    The Database Engine raises an error and the delete or update action on the row in the parent table is rolled back.
- `CASCADE`

    Corresponding rows are updated or deleted in the referencing table when that row is updated or deleted in the parent table. `CASCADE` can't be specified if a **timestamp** column is part of either the foreign key or the referenced key. `ON DELETE CASCADE` can't be specified for a table that has an `INSTEAD OF DELETE` trigger. `ON UPDATE CASCADE` can't be specified for tables that have `INSTEAD OF UPDATE` triggers.
- `SET NULL`

    All the values that make up the foreign key are set to `NULL` when the corresponding row in the parent table is updated or deleted. For this constraint to execute, the foreign key columns must be nullable. Can't be specified for tables that have `INSTEAD OF UPDATE` triggers.
- `SET DEFAULT`

    All the values that make up the foreign key are set to their default values if the corresponding row in the parent table is updated or deleted. For this constraint to execute, all foreign key columns must have default definitions. If a column is nullable, and there's no explicit default value set, `NULL` becomes the implicit default value of the column. Can't be specified for tables that have `INSTEAD OF UPDATE` triggers.

`CASCADE`, `SET NULL`, `SET DEFAULT`, and `NO ACTION` can be combined on tables that have referential relationships with each other. If the Database Engine encounters `NO ACTION`, it stops and rolls back related `CASCADE`, `SET NULL`, and `SET DEFAULT` actions. When a `DELETE` statement causes a combination of `CASCADE`, `SET NULL`, `SET DEFAULT`, or `NO ACTION` actions, all the `CASCADE`, `SET NULL`, and `SET DEFAULT` actions are applied before the Database Engine checks for any `NO ACTION`.

### Triggers and cascading referential actions

Cascading referential actions fire the `AFTER UPDATE` or `AFTER DELETE` triggers in the following manner:

- All the cascading referential actions directly caused by the original `DELETE` or `UPDATE` are performed first.
- If there are any `AFTER` triggers defined on the affected tables, these triggers fire after all cascading actions are performed. These triggers fire in opposite order of the cascading action. If there are multiple triggers on a single table, they fire in random order, unless there's a dedicated first or last trigger for the table. This order is as specified by using [sp_settriggerorder](../system-stored-procedures/sp-settriggerorder-transact-sql).
- If multiple cascading chains originate from the table that was the direct target of an `UPDATE` or `DELETE` action, the order in which these chains fire their respective triggers is unspecified. However, one chain always fires all its triggers before another chain starts firing.
- An `AFTER` trigger on the table that is the direct target of an `UPDATE` or `DELETE` action fires regardless of whether any rows are affected. There are no other tables affected by cascading in this case.
- If any one of the previous triggers performs `UPDATE` or `DELETE` operations on other tables, these actions can start secondary cascading chains. These secondary chains are processed for each `UPDATE` or `DELETE` operation at a time after all triggers on all primary chains fire. This process can be recursively repeated for subsequent `UPDATE` or `DELETE` operations.
- Performing `CREATE`, `ALTER`, `DELETE`, or other data definition language (DDL) operations inside the triggers can cause DDL triggers to fire. This might subsequently perform DELETE or UPDATE operations that start additional cascading chains and triggers.
- If an error is generated inside any particular cascading referential action chain, an error is raised, no `AFTER` triggers are fired in that chain, and the DELETE or UPDATE operation that created the chain is rolled back.
- A table that has an `INSTEAD OF` trigger can't also have a `REFERENCES` clause that specifies a cascading action. However, an `AFTER` trigger on a table targeted by a cascading action can execute an `INSERT`, `UPDATE`, or `DELETE` statement on another table or view that fires an `INSTEAD OF` trigger defined on that object.