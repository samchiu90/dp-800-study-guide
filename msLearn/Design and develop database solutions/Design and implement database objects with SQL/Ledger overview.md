---
layout: Conceptual
monikers:
- azuresqldb-current
- azuresqldb-mi-current
- sql-server-linux-ver16
- sql-server-linux-ver17
- sql-server-ver16
- sql-server-ver17
defaultMoniker: sql-server-ver17
versioningType: Ranged
title: Ledger Overview - SQL Server | Microsoft Learn
canonicalUrl: https://learn.microsoft.com/en-us/sql/relational-databases/security/ledger/ledger-overview?view=sql-server-ver17
config_moniker_range: =azuresqldb-current || =azuresqldb-mi-current || =azure-sqldw-latest || >=aps-pdw-2016 || >=sql-server-2017 || >=sql-server-linux-2017 || =fabric || =fabric-sqldb
uhfHeaderId: MSDocsHeader-DocsSQL
toc_preview: true
feedback_system: Standard
feedback_product_url: https://feedback.azure.com/d365community/forum/04fe6ee0-3b25-ec11-b6e6-000d3a4f0da0
feedback_help_link_url: https://learn.microsoft.com/answers/tags/191/sql-server
feedback_help_link_type: get-help-at-qna
recommendations: true
breadcrumb_path: ../../../breadcrumb/toc.json
ms.update-cycle: 1825-days
description: Learn the basics of the ledger feature.
author: VanMSFT
ms.author: vanto
ms.reviewer: mathoma
ms.date: 2025-08-07T00:00:00.0000000Z
ms.service: azure-sql-database
ms.subservice: security
ms.topic: overview
ms.custom:
- ignite-2023
locale: en-us
document_id: 0c29298a-7abc-7d20-e18f-96df1f9d432f
document_version_independent_id: 0c29298a-7abc-7d20-e18f-96df1f9d432f
updated_at: 2026-03-02T18:58:00.0000000Z
original_content_git_url: https://github.com/MicrosoftDocs/sql-docs-pr/blob/live/docs/relational-databases/security/ledger/ledger-overview.md
gitcommit: https://github.com/MicrosoftDocs/sql-docs-pr/blob/ba4d03d6fa2622f1df55f724139dddad5e556af4/docs/relational-databases/security/ledger/ledger-overview.md
git_commit_id: ba4d03d6fa2622f1df55f724139dddad5e556af4
default_moniker: sql-server-ver17
site_name: Docs
depot_name: SQL.sql-content
page_type: conceptual
toc_rel: ../../../toc.json
pdf_url_template: https://learn.microsoft.com/pdfstore/en-us/SQL.sql-content/{branchName}{pdfName}
word_count: 1588
asset_id: relational-databases/security/ledger/ledger-overview
moniker_range_name: 058c3b7062a1f01a48b691b30f36c8b2
monikers:
- azuresqldb-current
- azuresqldb-mi-current
- sql-server-linux-ver16
- sql-server-linux-ver17
- sql-server-ver16
- sql-server-ver17
item_type: Content
source_path: docs/relational-databases/security/ledger/ledger-overview.md
cmProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/cbe4ca68-43ac-4375-aba5-5945a6394c20
- https://authoring-docs-microsoft.poolparty.biz/devrel/6ab7faaf-d791-4a26-96a2-3b11738538e7
- https://authoring-docs-microsoft.poolparty.biz/devrel/97159432-14a9-4307-a469-d2f2c75f0e33
spProducts:
- https://authoring-docs-microsoft.poolparty.biz/devrel/ced846cc-6a3c-4c8f-9dfb-3de0e90e2742
- https://authoring-docs-microsoft.poolparty.biz/devrel/302e28b0-1f09-4811-9a9b-2a72e0770581
- https://authoring-docs-microsoft.poolparty.biz/devrel/50565c62-5f6b-4687-be38-323113c72c2e
platformId: 3bc3e3d0-f153-7a22-a24f-8a7e8195b904
---

# Ledger Overview - SQL Server | Microsoft Learn

**Applies to:**![](../../../includes/media/yes-icon.svg) SQL Server 2022 (16.x) and later versions ![](../../../includes/media/yes-icon.svg)[Azure SQL Database](../../../sql-server/sql-docs-navigation-guide#applies-to)![](../../../includes/media/yes-icon.svg)[Azure SQL Managed Instance](../../../sql-server/sql-docs-navigation-guide#applies-to)

Establishing trust around the integrity of data stored in database systems has been a longstanding problem for all organizations that manage financial, medical, or other sensitive data. The ledger feature provides tamper-evidence capabilities in your database. You can cryptographically attest to other parties, such as auditors or other business parties, that your data hasn't been tampered with.

Ledger helps protect data from any attacker or high-privileged user, including database administrators (DBAs), system administrators, and cloud administrators. As with a traditional ledger, the feature preserves historical data. If a row is updated in the database, its previous value is maintained and protected in a history table. Ledger provides a chronicle of all changes made to the database over time.

Ledger and the historical data are managed transparently, offering protection without any application changes. The feature maintains historical data in a relational form to support SQL queries for auditing, forensics, and other purposes. It provides guarantees of cryptographic data integrity while maintaining the power, flexibility, and performance of the SQL database.

![Diagram of the ledger table architecture.](media/ledger/ledger-table-architecture.png)

## Use cases for ledger

Let's go over some advantages for using ledger.

### Streamlining audits

Any production system's value is based on the ability to trust the data that the system is consuming and producing. If a malicious user has tampered with the data in your database, that can have disastrous results in the business processes relying on that data.

Maintaining trust in your data requires a combination of enabling the proper security controls to reduce potential attacks, backup and restore practices, and thorough disaster recovery procedures. Audits by external parties ensure that these practices are put in place.

Audit processes are highly time-intensive activities. Auditing requires on-site inspection of implemented practices such as reviewing audit logs, inspecting authentication, and inspecting access controls. Although these manual processes can expose potential gaps in security, they can't provide attestable proof that the data hasn't been maliciously altered.

Ledger provides the cryptographic proof of data integrity to auditors. This proof can help streamline the auditing process. It also provides nonrepudiation regarding the integrity of the system's data.

### Multiple-party business processes

In some systems, such as supply-chain management systems, multiple organizations must share state from a business process with one another. These systems struggle with the challenge of how to share and trust data. Many organizations are turning to traditional blockchains, such as Ethereum or Hyperledger Fabric, to digitally transform their multiple-party business processes.

Blockchain is a great solution for multiple-party networks where trust is low between parties that participate on the network. Many of these networks are fundamentally centralized solutions where trust is important, but a fully decentralized infrastructure is a heavyweight solution.

Ledger provides a solution for these networks. Participants can verify the integrity of the centrally housed data, without the complexity and performance implications that network consensus introduces in a blockchain network.

#### Customer success

- Learn how [Lenovo is reinforcing customer trust using ledger in Azure SQL Database](https://videos.microsoft.com/customer-stories/watch/xEenNHQerYdRyYqwdYLyXi).

### Trusted off-chain storage for blockchain

When a blockchain network is necessary for a multiple-party business process, the ability to query the data on the blockchain without sacrificing performance is a challenge.

Typical patterns for solving this problem involve replicating data from the blockchain to an off-chain store, such as a database. But after the data is replicated to the database from the blockchain, the data integrity guarantees that a blockchain offer is lost. Ledger provides data integrity for off-chain storage of blockchain networks, which helps ensure complete data trust through the entire system.

## How it works

Any rows modified by a transaction in a ledger table are cryptographically SHA-256 hashed using a Merkle tree data structure that creates a root hash representing all rows in the transaction. The transactions that the database processes are then also SHA-256 hashed together through a Merkle tree data structure. The result is a root hash that forms a block. The block is then SHA-256 hashed through the root hash of the block, along with the root hash of the previous block as input to the hash function. That hashing forms a blockchain.

The root hashes in the [database ledger](ledger-database-ledger), also called Database digests, contain the cryptographically hashed transactions and represent the state of the database. They can be periodically generated and stored outside the database in tamper-proof storage, such as [Azure Blob Storage configured with immutability policies](/en-us/azure/storage/blobs/immutable-storage-overview), [Azure Confidential Ledger](/en-us/azure/confidential-ledger/index) or on-premises [Write Once Read Many (WORM) storage devices](https://wikipedia.org/wiki/Write_once_read_many). Database digests are later used to verify the integrity of the database by comparing the value of the hash in the digest against the calculated hashes in database.

Ledger functionality is introduced to tables in two forms:

- Updatable ledger tables, which allow you to update and delete rows in your tables.
- Append-only ledger tables, which only allow insertions to your tables.

Both updatable ledger tables and append-only ledger tables provide tamper-evidence and digital forensics capabilities.

### Updatable ledger tables

[Updatable ledger tables](ledger-updatable-ledger-tables) are ideal for application patterns that expect to issue updates and deletions to tables in your database, such as system of record (SOR) applications. Existing data patterns for your application don't need to change to enable ledger functionality.

Updatable ledger tables track the history of changes to any rows in your database when transactions that perform updates or deletions occur. An updatable ledger table is a system-versioned table that contains a reference to another table with a mirrored schema.

The other table is called the *history table*. The system uses this table to automatically store the previous version of the row each time a row in the ledger table is updated or deleted. The history table is automatically created when you create an updatable ledger table.

The values in the updatable ledger table and its corresponding history table provide a chronicle of the values of your database over time. A system-generated ledger view joins the updatable ledger table and the history table so that you can easily query this chronicle of your database.

For more information on updatable ledger tables, see [Create and use updatable ledger tables](ledger-how-to-updatable-ledger-tables).

### Append-only ledger tables

[Append-only ledger tables](ledger-append-only-ledger-tables) are ideal for application patterns that are insert-only, such as security information and event management (SIEM) applications. Append-only ledger tables block updates and deletions at the API level. This blocking provides more tampering protection from privileged users such as system administrators and DBAs.

Because only insertions are allowed into the system, append-only ledger tables don't have a corresponding history table because there's no history to capture. As with updatable ledger tables, a ledger view provides insights into the transaction that inserted rows into the append-only table, and the user that performed the insertion.

For more information on append-only ledger tables, see [Create and use append-only ledger tables](ledger-how-to-append-only-ledger-tables).

### Ledger database

Ledger databases provide an easy solution for applications that require the integrity of all data to be protected for the entire lifetime of the database. A ledger database can only contain ledger tables. Creating regular tables (that are not ledger tables) is not supported. Each table is, by default, created as an [Updatable ledger table](ledger-updatable-ledger-tables) with default settings, which makes creating such tables very easy. You configure a database as a ledger database at creation. Once created, a ledger database cannot be converted to a regular database. For more information, see [Configure a ledger database](ledger-how-to-configure-ledger-database).

### Database digests

The hash of the latest block in the database ledger is called the [database digest](ledger-digest-management). It represents the state of all ledger tables in the database at the time that the block was generated.

When a block is formed, its associated database digest is published and stored outside the database in tamper-proof storage. Because database digests represent the state of the database at the time that they were generated, protecting the digests from tampering is paramount. An attacker who has access to modify the digests would be able to:

1. Tamper with the data in the database.
2. Generate the hashes that represent the database with those changes.
3. Modify the digests to represent the updated hash of the transactions in the block.

Ledger provides the ability to automatically generate and store the database digests in [immutable storage](/en-us/azure/storage/blobs/immutable-storage-overview) or [Azure Confidential Ledger](/en-us/azure/confidential-ledger/index), to prevent tampering. Alternatively, users can manually generate database digests and store them in the location of their choice. Database digests are used for later verifying that the data stored in ledger tables hasn't been tampered with.

### Ledger verification

The ledger feature doesn't allow modifying the content of ledger system views, append-only tables and history tables. However, an attacker or system administrator who has control of the machine can bypass all system checks and directly tamper with the data. For example, an attacker or system administrator can edit the database files in storage. Ledger can't prevent such attacks but guarantees that any tampering will be detected when the ledger data is verified.

The [ledger verification](ledger-database-verification) process takes as input one or more previously generated database digests and recomputes the hashes stored in the database ledger based on the current state of the ledger tables. If the computed hashes don't match the input digests, the verification fails, indicating that the data has been tampered with. Ledger then reports all inconsistencies that it has detected.