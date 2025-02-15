---
title: 数据迁移的监视与故障排除 (Stretch Database) | Microsoft Docs
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, monitoring
- monitoring Stretch Database
ms.assetid: 06950858-8c02-4ec6-9c59-42b787316a2d
author: rothja
ms.author: jroth
ms.openlocfilehash: 6130bd88e93a33c5bcb295e73b752ae1b749ff77
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68136080"
---
# <a name="monitor-and-troubleshoot-data-migration-stretch-database"></a>数据迁移的监视与故障排除 (Stretch Database)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md)]


  在 SQL Server Management Studio 中选择数据库的“任务 | Stretch | 监视”  以监视 Stretch Database 监视器中的数据迁移。  
  
## <a name="check-the-status-of-data-migration-in-the-stretch-database-monitor"></a>检查 Stretch Database 监视器中数据迁移的状态  
 在 SQL Server Management Studio 中选择数据库的“任务 | Stretch | 监视”  以打开 Stretch Database 监视器并监视数据迁移。  
  
-   此监视器的上半部分显示有关已启用拉伸的 SQL Server 数据库和远程 Azure 数据库的常规信息。  
  
-   监视器的下半部分显示数据库中每个已启用拉伸的表的数据迁移状态。  
  
 ![Stretch Database 监视器](../../sql-server/stretch-database/media/stretch-monitor.PNG "Stretch Database 监视器")  
  
##  <a name="Migration"></a> 检查动态管理视图中数据迁移的状态  
 打开动态管理视图 **sys.dm_db_rda_migration_status** 以查看有多少批数据和数据行已迁移。 有关详细信息，请参阅 [sys.dm_db_rda_migration_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md)。  
  
##  <a name="Firewall"></a> 数据迁移故障排除  
 **我的已启用拉伸的表中的行未迁移到 Azure。这是什么问题？**  
 有几个问题可能会影响迁移。 请检查以下事项。  
  
-   检查 SQL Server 计算机的网络连接。  
  
-   确保 Azure 防火墙未阻止你的 SQL Server 连接到远程端点。  
  
-   检查动态管理视图 **sys.dm_db_rda_migration_status** ，了解最新批处理的状态。 如果出现错误，请检查此批处理的 error_number、error_state 和 error_severity 值。  
  
    -   有关视图的详细信息，请参阅 [sys.dm_db_rda_migration_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/stretch-database-sys-dm-db-rda-migration-status.md)。  
  
    -   有关 SQL Server 错误消息内容的详细信息，请参阅 [sys.messages (Transact-SQL)](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)。  
  
 **Azure 防火墙阻止了来自我的本地服务器的连接。**  
 你可能需要在 Azure 服务器的 Azure 防火墙设置中添加一条规则，让 SQL Server 与远程 Azure 服务器进行通信。  
  
## <a name="see-also"></a>另请参阅  
 [对 Stretch Database 进行管理和故障排除](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
  
