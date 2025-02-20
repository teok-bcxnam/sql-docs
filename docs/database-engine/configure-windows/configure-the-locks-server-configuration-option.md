---
title: "Server configuration: locks"
description: Learn about the locks option. See how to use it to limit the amount of memory that the SQL Server Database Engine uses for locks.
author: rwestMSFT
ms.author: randolphwest
ms.date: 10/18/2024
ms.service: sql
ms.subservice: configuration
ms.topic: conceptual
helpviewer_keywords:
  - "locks option [SQL Server]"
---
# Server configuration: locks

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

This article describes how to configure the `locks` server configuration option in [!INCLUDE [ssnoversion](../../includes/ssnoversion-md.md)] by using [!INCLUDE [ssManStudioFull](../../includes/ssmanstudiofull-md.md)] or [!INCLUDE [tsql](../../includes/tsql-md.md)]. The `locks` option sets the maximum number of available locks, which limits the amount of memory the [!INCLUDE [ssDEnoversion](../../includes/ssdenoversion-md.md)] uses for them. The default setting is 0, which allows the [!INCLUDE [ssDE](../../includes/ssde-md.md)] to allocate and deallocate lock structures dynamically, based on changing system requirements.

> [!IMPORTANT]  
> [!INCLUDE [ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]

## Recommendations

This option is an advanced option and should be changed only by an experienced database administrator or certified [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] professional.

When the server is started with `locks` set to `0`, the lock manager acquires sufficient memory from the [!INCLUDE [ssDE](../../includes/ssde-md.md)] for an initial pool of 2,500 lock structures. As the lock pool is exhausted, more memory is acquired for the pool.

Generally, if more memory is required for the lock pool than is available in the [!INCLUDE [ssDE](../../includes/ssde-md.md)] memory pool, and more computer memory is available (the `max server memory` threshold hasn't been reached), the [!INCLUDE [ssDE](../../includes/ssde-md.md)] allocates memory dynamically to satisfy the request for locks. However, if allocating that memory would cause paging at the operating system level (for example, if another application is running on the same computer as an instance of [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] and using that memory), more lock space isn't allocated. The dynamic lock pool doesn't acquire more than 60 percent of the memory allocated to the [!INCLUDE [ssDE](../../includes/ssde-md.md)]. After the lock pool reaches 60 percent of the memory acquired by an instance of the [!INCLUDE [ssDE](../../includes/ssde-md.md)], or no more memory is available on the computer, further requests for locks generate an error.

Allowing [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] to use locks dynamically is the recommended configuration. However, you can set `locks` and override the ability of [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] to allocate lock resources dynamically. When `locks` is set to a value other than `0`, the [!INCLUDE [ssDE](../../includes/ssde-md.md)] can't allocate more locks than the value specified in `locks`. Increase this value if [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] displays a message that you exceeded the number of available locks. Because each lock consumes memory (96 bytes per lock), increasing this value can require increasing the amount of memory dedicated to the server.

The `locks` option also affects when lock escalation occurs. When `locks` is set to `0`, lock escalation occurs when the memory used by the current lock structures reaches 40 percent of the [!INCLUDE [ssDE](../../includes/ssde-md.md)] memory pool. When `locks` isn't set to `0`, lock escalation occurs when the number of locks reaches 40 percent of the value specified for `locks`.

## Permissions

Execute permissions on `sp_configure` with no parameters or with only the first parameter are granted to all users by default. To execute `sp_configure` with both parameters to change a configuration option or to run the `RECONFIGURE` statement, a user must be granted the `ALTER SETTINGS` server-level permission. The `ALTER SETTINGS` permission is implicitly held by the **sysadmin** and **serveradmin** fixed server roles.

<a id="SSMSProcedure"></a>

## Use SQL Server Management Studio

1. In Object Explorer, right-click a server and select **Properties**.

1. Select the **Advanced** node.

1. Under **Parallelism**, type the desired value for the `locks` option.

   Use the `locks` option to set the maximum number of available locks, which limits the amount of memory [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] uses for them.

<a id="TsqlProcedure"></a>

## Use Transact-SQL

1. Connect to the [!INCLUDE [ssDE](../../includes/ssde-md.md)].

1. From the Standard bar, select **New Query**.

1. Copy and paste the following example into the query window and select **Execute**. This example shows how to use [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) to set the value of the `locks` option to set the number of locks available for all users to `20000`.

   ```sql
   USE master;
   GO

   EXECUTE sp_configure 'show advanced options', 1;
   GO

   RECONFIGURE;
   GO

   EXECUTE sp_configure 'locks', 20000;
   GO

   RECONFIGURE;
   GO

   EXECUTE sp_configure 'show advanced options', 0;
   GO

   RECONFIGURE;
   GO
   ```

For more information, see [Server configuration options](server-configuration-options-sql-server.md).

<a id="FollowUp"></a>

## Follow up: After you configure the locks option

The server must be restarted before the setting can take effect.

## Related content

- [RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)
- [Server configuration options](server-configuration-options-sql-server.md)
- [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
