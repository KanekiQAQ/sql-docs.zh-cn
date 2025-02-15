---
title: 仅复制备份 (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2018
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- copy-only backups [SQL Server]
- COPY_ONLY option [BACKUP statement]
- backups [SQL Server], copy-only backups
ms.assetid: f82d6918-a5a7-4af8-868e-4247f5b00c52
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017
ms.openlocfilehash: 51d267f26d0750c069a82c825c8ffa0cd751c8ff
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68076094"
---
# <a name="copy-only-backups-sql-server"></a>仅复制备份 (SQL Server)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]
  *仅复制备份*是[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]独立于常规备份序列[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的备份。 通常，进行备份会更改数据库并影响其后备份的还原方式。 但是，有时在不影响数据库总体备份和还原过程的情况下，为特殊目的而进行备份还是有用的。 仅复制备份就是用于此目的。  
  
 仅复制备份的类型如下所示：  
  
-   仅复制完整备份（所有恢复模式）  
  
     仅复制备份不能用作差异基准或差异备份，并且不影响差异基准。  
  
     还原仅复制完整备份与还原任何其他完整备份相同。  
  
-   仅复制日志备份（仅限于完整恢复模式和大容量日志恢复模式）  
  
     仅复制日志备份保留当前日志存档点，因此，不影响常规日志备份的序列。 通常不必进行仅复制日志备份。 相反，您可以创建新的常规日志备份（使用 WITH NORECOVERY），然后将该备份与还原序列所需的任何以前的日志备份一起使用。 但是，仅复制日志备份有时可用于执行联机还原。 关于这方面的示例，请参阅[示例：读/写文件的联机还原（完整恢复模式）](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)。  
  
     事务日志从不在仅复制备份后出现截断。  
  
 仅复制备份记录在 **backupset** 表的 [is_copy_only](../../relational-databases/system-tables/backupset-transact-sql.md) 列中。  
  
## <a name="to-create-a-copy-only-backup"></a>创建仅复制备份  
 您可以通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、 [!INCLUDE[tsql](../../includes/tsql-md.md)]或 PowerShell 创建仅复制备份。  

### <a name="examples"></a>示例  
###  <a name="SSMSProcedure"></a> A.  使用 SQL Server Management Studio  
在此示例中，`Sales` 数据库的仅复制备份将备份到磁盘的默认备份位置。

[!INCLUDE[Freshness](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

1.  在“对象资源管理器”  中，连接到一个 SQL Server 数据库引擎实例，然后展开该实例。

2.  展开“数据库”  ，右键单击 `Sales`，然后指向“任务”  ，再单击“备份...”  。

3.  在“常规”  页的“源”  部分中，选中“仅复制备份”  复选框。

4.  单击“确定”  。

  
###  <a name="TsqlProcedure"></a>B.  使用 Transact-SQL  
此示例利用 COPY_ONLY 参数为 `Sales` 数据库创建仅复制备份。  同时还创建事务日志的仅复制备份。

```sql
BACKUP DATABASE Sales
TO DISK = 'E:\BAK\Sales_Copy.bak'
WITH COPY_ONLY;

BACKUP LOG Sales
TO DISK = 'E:\BAK\Sales_LogCopy.trn'
WITH COPY_ONLY;
```
  
> [!NOTE]  
> 使用 DIFFERENTIAL 选项指定时，COPY_ONLY 不起作用。  

  
###  <a name="PowerShellProcedure"></a>C.  使用 PowerShell  
此示例利用 -CopyOnly 参数为 `Sales` 数据库创建仅复制备份。  
```powershell
Backup-SqlDatabase -ServerInstance 'SalesServer' -Database 'Sales' -BackupFile 'E:\BAK\Sales_Copy.bak' -CopyOnly
```  
  
##  <a name="RelatedTasks"></a> 相关任务  
 **创建完整备份或日志备份**  
  
-   [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
 **查看仅复制备份**  
  
-   [backupset (Transact-SQL)](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../relational-databases/scripting/sql-server-powershell-provider.md)  
  
  
## <a name="see-also"></a>另请参阅  
 [备份概述 (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [通过备份和还原来复制数据库](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
 [还原和恢复概述 (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)  
[BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)  
[Backup-SqlDatabase](/powershell/module/sqlserver/backup-sqldatabase)

  
