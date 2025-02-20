---
title: "sys.sp_help_change_feed_settings (Transact-SQL)"
description: "The sys.sp_help_change_feed_settings system stored procedure returns state information for Microsoft Fabric Mirroring."
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: imotiwala
ms.date: 09/24/2024
ms.service: fabric
ms.subservice: system-objects
ms.topic: "reference"
ms.custom:
  - ignite-2024
f1_keywords:
  - "sys.sp_help_change_feed_settings_TSQL"
  - "sys.sp_help_change_feed_settings"
  - "sp_help_change_feed_settings_TSQL"
  - "sp_help_change_feed_settings"
helpviewer_keywords:
  - "sp_help_change_feed_settings"
dev_langs:
  - "TSQL"
monikerRange: ">=sql-server-ver16 || =azuresqldb-current || =fabric || =azure-sqldw-latest"
---
# sys.sp_help_change_feed_settings (Transact-SQL)

[!INCLUDE [sqlserver2022-asdb-asa-fabricmirroredsqldb-fabricsqldb](../../includes/applies-to-version/sqlserver2022-asdb-asa-fabricmirroredsqldb-fabricsqldb.md)]

Provides the provision or deprovision status and information of the [Fabric Mirrored Database](/fabric/database/mirrored-database/overview) feature.

This system stored procedure is used for:

- The Azure Synapse Link feature for SQL Server instances and Azure SQL Database. For more information, see [Manage Azure Synapse Link for SQL Server and Azure SQL Database](../../sql-server/synapse-link/synapse-link-sql-server-change-feed-manage.md).
- The Fabric Mirrored Database feature for Azure SQL Database. For more information, see [Microsoft Fabric mirrored databases](/fabric/database/mirrored-database/overview).
- SQL database in Microsoft Fabric. For more information, see [SQL database in Microsoft Fabric](/fabric/database/sql/overview).

## Syntax

:::image type="icon" source="../../includes/media/topic-link-icon.svg" border="false"::: [Transact-SQL syntax conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

```syntaxsql
EXECUTE sys.sp_help_change_feed_settings;
```

## Arguments

None

## Result set

| Column name | Data type | Description |
| --- | --- | --- |
| `maxtrans` | **int** | Maximum transactions to process in each cycle. The default is 10000. |
| `seqno` | **binary(10)** | Log Sequence Number (LSN) marker to track the last published LSN (log record). |
| `schema_version` | **int** | Tracks current schema version of database. Determines whether a schema needs to be updated or not on startup. |
| `pollinterval` | **int** | The frequency that the log is scanned for any new changes in seconds. |
| `reseed_state` | **tinyint** | **Applies to:** Fabric Mirrored Database only.<br /><br />`0` = Normal.<br />`1` = The database has started the process of reinitializing to Fabric. Transitionary state.<br />`2` = The database is being reinitialized to Fabric and remains in this state until complete. |

## Permissions

A user with [CONTROL database permissions](../security/permissions-database-engine.md), **db_owner** database role membership, or **sysadmin** server role membership can execute this procedure.

## Remarks

If Fabric Mirroring, the source SQL database transaction log is monitored. If the transaction log is observed to be full and the log reuse reason is `REPLICATION`, the database will automatically be reinitialized to Fabric to allow the transaction log to be truncated. During this special reseed state, a database snapshot is sent to Fabric again, then incremental replication resumes. The `reseed_state` column in `sys.sp_help_change_feed_settings` indicates the reseed state.

## Related content

- [sys.sp_help_change_feed (Transact-SQL)](sp-help-change-feed.md)
- [sys.sp_help_change_feed_table (Transact-SQL)](sp-help-change-feed-table.md)
- [sys.sp_help_change_feed_table_groups (Transact-SQL)](sp-help-change-feed-table-groups.md)
- [sys.sp_change_feed_configure_parameters (Transact-SQL)](sp-change-feed-configure-parameters.md)
- [sys.dm_change_feed_log_scan_sessions (Transact-SQL)](../system-dynamic-management-views/sys-dm-change-feed-log-scan-sessions.md)
- [sys.dm_change_feed_errors (Transact-SQL)](../system-dynamic-management-views/sys-dm-change-feed-errors.md)
