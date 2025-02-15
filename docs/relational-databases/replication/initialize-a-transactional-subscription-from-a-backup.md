---
title: 从备份初始化事务订阅 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- manual subscription initialization [SQL Server replication]
- subscriptions [SQL Server replication], initializing
- initializing subscriptions [SQL Server replication], without snapshots
- transactional replication, backup and restore
- backups [SQL Server replication], transactional replication
ms.assetid: d0637fc4-27cc-4046-98ea-dc86b7a3bd75
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3e0848a5452ad7cdb189b00c5ed90a6ad1cb8a1a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68127865"
---
# <a name="initialize-a-transactional-subscription-from-a-backup"></a>从备份初始化事务订阅
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  虽然通常使用快照来初始化对事务发布的订阅，但也可以使用复制存储过程通过备份初始化订阅。 有关详细信息，请参阅 [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。  
  
### <a name="to-initialize-a-transactional-subscriber-from-a-backup"></a>从备份初始化事务订阅服务器  
  
1.  对于现有的发布，请通过在发布服务器上对发布数据库执行 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) 来确保该发布支持从备份进行初始化操作。 请注意结果集中 **allow_initialize_from_backup** 的值。  
  
    -   如果值为 **1**，则该发布支持此功能。  
  
    -   如果值为 **0**，则在发布服务器上对发布数据库执行 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)。 将 **allow_initialize_from_backup** 的值指定为 **@property** ，并将 **@value** 的值指定为 **@value** 。  
  
2.  对于新发布，在发布服务器上对发布数据库执行 [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)。 将 **allow_initialize_from_backup** 的值指定为 **true**。 有关详细信息，请参阅 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)。  
  
    > [!WARNING]  
    >  为了避免丢失订阅服务器数据，在将 **sp_addpublication** 与 `@allow_initialize_from_backup = N'true'`一起使用时，始终使用 `@immediate_sync = N'true'`。  
  
3.  使用 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md) 语句创建发布数据库的备份。  
  
4.  使用 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md) 语句在订阅服务器上还原备份。  
  
5.  在发布服务器上对发布数据库执行存储过程 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)。 指定下列参数：  
  
    -   **@sync_type** - 值为 **initialize with backup**。  
  
    -   **@backupdevicetype** - 备份设备的类型： **logical** （默认）、 **disk**或 **tape**。  
  
    -   **@backupdevicename** - 用于还原的逻辑或物理备份设备。  
  
         对于逻辑设备，指定使用 **sp_addumpdevice** 创建该设备时指定的备份设备的名称。  
  
         对于物理设备，指定完整的路径和文件名，比如 `DISK = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\BACKUP\Mybackup.dat'` 或 `TAPE = '\\.\TAPE0'`。  
  
    -   （可选） **@password** - 创建备份集时提供的密码。  
  
    -   （可选） **@mediapassword** - 对介质集设置格式时提供的密码。  
  
    -   （可选） **@fileidhint** - 要还原的备份集的标识符。 例如，指定为 **1** 表示备份介质中的第一个备份集，而指定为 **2** 则表示第二个备份集。  
  
    -   （对于磁带设备是可选的） **@unload** - 如果完成还原后应从驱动器卸载磁带，则将值指定为 **1** （默认），如果不用卸载磁带，则将值指定为 **0** 。  
  
6.  （可选）对于请求订阅，在订阅服务器上对订阅数据库执行 [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md) 和 [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)。 有关详细信息，请参阅 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)。  
  
7.  （可选）启动分发代理。 有关详细信息，请参阅 [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) 或 [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md)。  
  
## <a name="see-also"></a>另请参阅  
 [通过备份和还原来复制数据库](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
  
  
