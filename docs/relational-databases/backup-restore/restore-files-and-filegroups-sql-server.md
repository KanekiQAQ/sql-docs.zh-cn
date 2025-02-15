---
title: 还原文件和文件组 (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restorefilesandfilegrps.general.f1
- sql13.swb.bselectfilegrpsfiles.f1
- sql13.swb.restorefilesandfilegrps.options.f1
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], restoring files and filegroups
- restoring [SQL Server], files
ms.assetid: 72603b21-3065-4b56-8b01-11b707911b05
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 81a832b4372dc2b35893c329d0b7ca909224fb9f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68041516"
---
# <a name="restore-files-and-filegroups-sql-server"></a>还原文件和文件组 (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中还原文件和文件组。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
-   [安全性](#Security)  
  
-   **若要还原文件和文件组，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Restrictions"></a> 限制和局限  
  
-   还原文件和文件组的系统管理员必须是唯一一位当前使用要还原的数据库的人。  
  
-   不允许在显式或隐式事务中使用 RESTORE。  
  
-   在简单恢复模式下，文件必须属于只读文件组。  
  
-   在完整恢复模式或大容量日志恢复模式下，必须先备份活动事务日志（称为日志尾部），然后才能还原文件。 有关详细信息，请参阅 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)中还原文件和文件组。  
  
-   若要还原已加密的数据库，您必须有权访问用于对数据库进行加密的证书或非对称密钥。 如果没有证书或非对称密钥，数据库将无法还原。 因此，只要需要该备份，就必须保留用于对数据库加密密钥进行加密的证书。 有关详细信息，请参阅 [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)。  
  
###  <a name="Security"></a> Security  
  
####  <a name="Permissions"></a> 权限  
 如果不存在要还原的数据库，则用户必须有 CREATE DATABASE 权限才能执行 RESTORE。 如果数据库存在，则 RESTORE 权限默认授予 **sysadmin** 和 **dbcreator** 固定服务器角色成员以及数据库的所有者 (**dbo**)（对于 FROM DATABASE_SNAPSHOT 选项，数据库始终存在）。  
  
 RESTORE 权限被授予那些成员身份信息始终可由服务器使用的角色。 因为只有在固定数据库可以访问且没有损坏时（在执行 RESTORE 时并不会总是这样）才能检查固定数据库角色成员身份，所以 **db_owner** 固定数据库角色成员没有 RESTORE 权限。  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-restore-files-and-filegroups"></a>还原文件和文件组  
  
1.  连接到相应的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例之后，在对象资源管理器中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** 。  根据具体的数据库，选择一个用户数据库，或展开“系统数据库”并选择一个系统数据库。  
  
3.  右键单击数据库，指向“任务”  ，再单击“还原”  。  
  
4.  单击 **“文件和文件组”** ，将打开 **“还原文件和文件组”** 对话框。  
  
5.  在 **“常规”** 页上的 **目标数据库** 列表框中，输入要还原的数据库。 您可以输入新的数据库，也可以从下拉列表中选择现有的数据库。 该列表包含了服务器上除系统数据库 **master** 和 **tempdb**之外的所有数据库。  
  
6.  若要指定要还原的备份集的源和位置，请单击以下选项之一：  
  
    -   **源数据库**  
  
         在列表框中输入数据库名称。 此列表仅包含根据 **msdb** 备份历史记录已进行过备份的数据库。  
  
    -   **源设备**  
  
         单击浏览按钮。 在 **“指定备份设备”** 对话框的 **“备份介质类型”** 列表框中，选择列出的设备类型之一。 若要为 **“备份介质”** 列表框选择一个或多个设备，请单击 **“添加”** 。  
  
         将所需设备添加到 **“备份介质”** 列表框后，单击 **“确定”** 返回到 **“常规”** 页。  
  
7.  在 **“选择用于还原的备份集”** 网格中，选择用于还原的备份。 此网格将显示对于指定位置可用的备份。 默认情况下，系统会推荐一个恢复计划。 若要覆盖建议的恢复计划，可以更改网格中的选择。 任何依赖于已取消选择备份的备份，也将被自动取消选择。  
  
    |列标题|值|  
    |-----------------|------------|  
    |**还原**|选中的复选框指示要还原的备份集。|  
    |**名称**|备份集的名称。|  
    |**文件类型**|指定备份中数据的类型：数据、日志，或 Filestream 数据    。 包含在表中的数据备份在 **“数据”** 文件中。 事务日志数据备份在 **“日志”** 文件中。 存储在文件系统上的二进制大型对象 (BLOB) 数据备份在 **Filestream 数据** 文件中。|  
    |**类型**|执行的备份类型：“完整”、“差异”或“事务日志”    。|  
    |**Server**|执行备份操作的数据库引擎实例的名称。|  
    |**文件逻辑名称**|文件的逻辑名称。|  
    |**“数据库”**|备份操作中涉及的数据库的名称。|  
    |**开始日期**|备份操作开始的日期和时间，按客户端的区域设置显示。|  
    |**完成日期**|备份操作完成的日期和时间，按客户端的区域设置显示。|  
    |**大小**|备份集的大小（字节）。|  
    |**用户名**|执行备份操作的用户的名称。|  
  
8.  若要查看或选择高级选项，请在 **“选择页”** 窗格中单击 **“选项”** 。  
  
9. 在 **“还原选项”** 面板中，可以根据您的实际情况选择下列任意选项。  
  
     **还原为文件组**  
     指示要还原整个文件组。  
  
     **覆盖现有数据库**  
     指定还原操作应覆盖所有现有数据库及其相关文件，即使已存在同名的其他数据库或文件。  
  
     选择此选项等效于在 [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE 语句中使用 REPLACE 选项。  
  
     **还原每个备份之前进行提示**  
     在还原每个备份设置前要求您进行确认。  
  
     如果对于不同介质集必须更换磁带，例如在服务器具有一个磁带设备时，此选项非常有用。  
  
     **限制访问还原的数据库**  
     使还原的数据库仅供 **db_owner**、 **dbcreator**或 **sysadmin**的成员使用。  
  
     选择此选项等效于在 [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE 语句中使用 RESTRICTED_USER 选项。  
  
10. 还可以通过在 **“将数据库文件还原为”** 网格中指定每个文件的新还原目标，从而将数据库还原到新的位置。  
  
    |列标题|值|  
    |-----------------|------------|  
    |**原始文件名**|源备份文件的完整路径。|  
    |**文件类型**|指定备份中数据的类型：数据、日志，或 Filestream 数据    。 包含在表中的数据备份在 **“数据”** 文件中。 事务日志数据备份在 **“日志”** 文件中。 存储在文件系统上的二进制大型对象 (BLOB) 数据备份在 **Filestream 数据** 文件中。|  
    |**还原为**|要还原的数据库文件的完整路径。 若要指定新的还原文件，请单击文本框，再编辑建议的路径和文件名。 更改 **“还原为”** 列中的路径或文件名等效于在 [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE 语句中使用 MOVE 选项。|  
  
11. **“恢复状态”** 面板确定还原操作之后的数据库状态。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

     **Leave the database ready for use by rolling back the uncommitted transactions. Additional transaction logs cannot be restored. (RESTORE WITH RECOVERY)**  
     Recovers the database. This is the default behavior. Choose this option only if you are restoring all of the necessary backups now. This option is equivalent to specifying WITH RECOVERY in a [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE statement.  
  
     **Leave the database non-operational, and don't roll back the uncommitted transactions. Additional transaction logs can be restored. (RESTORE WITH NORECOVERY)**  
     Leaves the database in the restoring state. To recover the database, you will need to perform another restore using the preceding RESTORE WITH RECOVERY option (see above). This option is equivalent to specifying WITH NORECOVERY in a [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE statement.  
  
     If you select this option, the **Preserve replication settings** option is unavailable.  
  
     **Leave the database in read-only mode. Roll back the uncommitted transactions, but save the rollback operation in a file so the recovery effects can be undone. (RESTORE WITH STANDBY)**  
     Leaves the database in a standby state. This option is equivalent to specifying WITH STANDBY in a [!INCLUDE[tsql](../../includes/tsql-md.md)] RESTORE statement.  
  
     Choosing this option requires that you specify a standby file.  
  
     **Rollback undo file**  
     Specify a standby file name in the **Rollback undo file** text box. This option is required if you leave the database in read-only mode (RESTORE WITH STANDBY).  
  
##  <a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-restore-files-and-filegroups"></a>还原文件和文件组  
  
1.  执行 RESTORE DATABASE 语句以还原文件和文件组备份，同时指定下列内容：  
  
    -   要还原的数据库的名称。  
  
    -   从中还原完整数据库备份的备份设备。  
  
    -   每个要还原文件的 FILE 子句。  
  
    -   每个要还原文件组的 FILEGROUP 子句。  
  
    -   NORECOVERY 子句。 如果在创建备份之后没有对文件进行修改，则指定 RECOVERY 子句。  
  
2.  如果在创建文件备份之后对文件进行了修改，则执行 RESTORE LOG 语句以应用事务日志备份，同时指定下列内容：  
  
    -   事务日志将应用到的数据库的名称。  
  
    -   要还原的事务日志备份的备份设备。  
  
    -   如果在应用当前事务日志备份之后还要应用其他事务日志备份，则指定 NORECOVERY 子句；否则指定 RECOVERY 子句。  
  
         事务日志备份（如果可用）必须包括截止到日志结尾处的文件和文件组备份（除非已还原所有数据库文件）。  
  
###  <a name="TsqlExample"></a> 示例 (Transact-SQL)  
 以下示例将还原 `MyDatabase` 数据库的文件和文件组。 为了将数据库还原到当前时间，将应用两个事务日志。  
  
```sql  
USE master;  
GO  
-- Restore the files and filesgroups for MyDatabase.  
RESTORE DATABASE MyDatabase  
   FILE = 'MyDatabase_data_1',  
   FILEGROUP = 'new_customers',  
   FILE = 'MyDatabase_data_2',  
   FILEGROUP = 'first_qtr_sales'  
   FROM MyDatabase_1  
   WITH NORECOVERY;  
GO  
-- Apply the first transaction log backup.  
RESTORE LOG MyDatabase  
   FROM MyDatabase_log1  
   WITH NORECOVERY;  
GO  
-- Apply the last transaction log backup.  
RESTORE LOG MyDatabase  
   FROM MyDatabase_log2  
   WITH RECOVERY;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)   
 [备份文件和文件组 (SQL Server)](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [创建完整数据库备份 (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)   
 [备份事务日志 (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)   
 [还原事务日志备份 (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
