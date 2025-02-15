---
title: SQL Server 备份到 URL 最佳做法和故障排除 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: de676bea-cec7-479d-891a-39ac8b85664f
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 7f652d512f27b935b158a71a80b61c43ac6b7183
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "65619584"
---
# <a name="sql-server-backup-to-url-best-practices-and-troubleshooting"></a>SQL Server 备份到 URL 最佳实践和故障排除
  本主题介绍 SQL Server 备份和还原到 Windows Azure Blob 服务的最佳做法和故障排除提示。  
  
 有关将 Windows Azure Blob 存储服务用于 SQL Server 备份或还原操作的详细信息，请参阅：  
  
-   [使用 Windows Azure Blob 存储服务进行 SQL Server 备份和还原](sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)  
  
-   [教程：Windows Azure Blob 存储服务的 SQL Server 备份和还原](../tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)  
  
## <a name="managing-backups"></a>管理备份  
 下表列出了管理备份的一般建议：  
  
-   建议为每个备份使用唯一文件名以防止意外覆盖 blob。  
  
-   创建容器时，建议将访问级别设置为 **“私有”** ，这样只有可以提供所需的身份验证信息的用户或帐户可以在容器中读取或写入 blob。  
  
-   对于在 Windows Azure 虚拟机中运行的 SQL Server 实例上的 SQL Server 数据库，请使用虚拟机所在区域中的存储帐户来避免区域之间的数据传输成本。 使用同一区域还可以确保备份和还原操作具有最佳性能。  
  
-   失败的备份活动可能导致无效的备份文件。 我们建议定期标识失败的备份和删除 blob 文件。 有关详细信息，请参阅 [Deleting Backup Blob Files with Active Leases](deleting-backup-blob-files-with-active-leases.md)  
  
-   在备份期间使用 `WITH COMPRESSION` 选项可以最大程度降低存储成本和存储事务成本。 它也会减少完成备份过程所需的时间。  
  
## <a name="handling-large-files"></a>处理大型文件  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份操作使用多个线程来优化与 Windows Azure Blob 存储服务的数据传输。  但是性能取决于各种因素，如 ISV 带宽和数据库的大小。 如果您计划从内部 SQL Server 数据库备份大型数据库或文件组，建议您首先执行某些吞吐量测试。 [Windows Azure 存储 SLA](https://go.microsoft.com/fwlink/?LinkId=271619)具有您需要考虑的 blob 的最大处理时间。  
  
-   使用在**管理备份**部分中建议的 `WITH COMPRESSION` 选项，在备份大型文件时这一点非常重要。  
  
## <a name="troubleshooting-backup-to-or-restore-from-url"></a>备份到 URL 或从 URL 还原故障排除  
 以下内容提供了在备份到 Windows Azure Blob 存储服务或从中还原时出现问题的一些快速解决方法。  
  
 若要避免由于不受支持的选项或限制导致的错误，查看限制列表以及备份和还原命令中的信息的支持[SQL Server 备份和还原使用 Windows Azure Blob 存储服务](sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)一文。  
  
 **身份验证错误：**  
  
-   WITH CREDENTIAL 是一个新选项，在备份到 Windows Azure Blob 存储服务或从中还原时需要该选项。 与凭据有关的失败可能包括：  
  
     `BACKUP` 或 `RESTORE` 命令中指定的凭据不存在。 要避免此问题，如果备份语句中没有指定凭据，可以使用 T-SQL 语句来创建凭据。 以下是您可以使用的一个示例：  
  
    ```  
    IF NOT EXISTS  
    (SELECT * FROM sys.credentials   
    WHERE credential_identity = 'mycredential')  
    CREATE CREDENTIAL <credential name> WITH IDENTITY = 'mystorageaccount'  
    ,SECRET = '<storage access key> ;  
  
    ```  
  
-   凭据存在但是用于运行备份命令的登录帐户没有访问凭据的权限。 使用角色为 **db_backupoperator** 且拥有 **更改任意凭据** 权限的登录帐户。  
  
-   验证存储帐户名称和密钥值。 在凭据中存储的信息必须与您在备份和还原操作中使用的 Windows Azure 存储帐户的属性值匹配。  
  
 **备份错误/失败：**  
  
-   并行备份到同一 blob 导致一个备份失败，发生 **“初始化失败”** 错误。  
  
-   使用以下错误日志帮助解决备份错误：  
  
    -   设置跟踪标志 3051 以启用记录到具有以下格式的特定错误日志：  
  
         BackupToUrl-\<instname>-\<dbname>-action-\<PID>.log 其中 \<action> 是以下值之一：  
  
        -   `DB`  
  
        -   `FILELISTONLY`  
  
        -   `LABELONLY`  
  
        -   `HEADERONLY`  
  
        -   `VERIFYONLY`  
  
    -   此外可以通过查看 Windows 事件日志的名称 SQLBackupToUrl 位于应用程序日志中找到信息。  
  
-   从压缩备份中还原时，您可能看到以下错误：  
  
    -   **发生 SqlException 3284。严重性：16 状态：5**  
        **在设备上的消息文件标记 'https://mystorage.blob.core.windows.net/mycontainer/TestDbBackupSetNumber2_0.bak ' 未对齐。用于创建备份集的相同块大小重新发布 Restore 语句：'65536' 看起来像一个可能值。**  
  
         要修复此错误，请重新发布指定了 `BACKUP` 的 `BLOCKSIZE = 65536` 语句。  
  
-   由于 blob 具有活动租约，备份期间出错：失败的备份活动可能导致 blob 产生活动租约。  
  
     如果重新尝试执行备份语句，备份操作可能失败，出现类似于以下的错误：  
  
     **“备份到 URL”收到来自远程端点的异常。异常消息：远程服务器返回了错误：(412) 当前 blob 上的租约，则在请求中未指定任何租约 ID**。  
  
     如果尝试对具有活动租约的备份 blob 文件执行还原语句，则还原操作失败，出现类似于以下的错误：  
  
     **异常消息：远程服务器返回了错误：(409) 冲突...**  
  
     发生这种错误时，需要删除 blob 文件。 有关此情形和如何解决此问题的详细信息，请参阅 [Deleting Backup Blob Files with Active Leases](deleting-backup-blob-files-with-active-leases.md)  
  
## <a name="proxy-errors"></a>代理错误  
 如果您使用代理服务器访问 Internet，可能会发现以下问题：  
  
 **代理服务器限制连接：**  
  
 代理服务器可能具有限制每分钟连接次数的设置。 “备份到 URL”进程是一个多线程进程，因此可能超过此限制。 如果出现此情况，代理服务器将终止连接。 若要解决此问题，请更改代理设置，使 SQL Server 不使用该代理。   下面是一些您可能在错误日志中看到的类型或错误消息的示例：  
  
-   尽情涂鸦"http://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak "失败：备份到 URL 收到来自远程终结点的异常。 异常消息：无法从传输连接读取数据：连接已关闭。  
  
-   文件“http://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak: ”上发生不可恢复的 I/O 错误。无法从远程终结点收集错误。  
  
     消息 3013，级别 16，状态 1，第 2 行  
  
     备份数据库异常终止。  
  
-   Backupiorequest:: Reportioerror 备份设备上写入失败 http://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak 。 操作系统错误，“备份到 URL”收到来自远程端点的异常。 异常消息：无法从传输连接读取数据：连接已关闭。  
  
 如果使用跟踪标志 3051 打开详细日志记录，您还可能在日志中看到以下消息：  
  
 HTTP 状态代码 502，HTTP 状态消息代理错误 (每分钟的 HTTP 请求数超出配置限制。 请与 ISA Server 管理员联系。  )  
  
 **未选择默认代理设置：**  
  
 有时，可能由于没有选择默认设置而导致如下代理身份验证错误：文件“http://storageaccount.blob.core.windows.net/container/BackupAzurefile.bak: ”上发生不可恢复的 I/O 错误。备份到 URL”收到来自远程终结点的异常 *。异常消息：远程服务器返回了错误：(407)* **需要代理身份验证**。  
  
 若要解决此问题，请使用以下步骤创建一个配置文件，以允许“备份到 URL”进程使用默认代理设置：  
  
1.  使用以下 xml 创建一个名为 BackuptoURL.exe.config 的配置文件：  
  
    ```  
    <?xml version ="1.0"?>  
    <configuration>   
                    <system.net>   
                                    <defaultProxy enabled="true" useDefaultCredentials="true">   
                                                    <proxy usesystemdefault="true" />   
                                    </defaultProxy>   
                    </system.net>  
    </configuration>  
  
    ```  
  
2.  将该配置文件置于 SQL Server 实例的 Binn 文件夹中。 例如，如果我的 SQL Server 安装在计算机的 C 驱动器上，配置文件将置于以下位置：*C:\Program Files\Microsoft SQL Server\MSSQL12。\<实例名 > \MSSQL\Binn*。  
  
## <a name="troubleshooting-sql-server-managed-backup-to-windows-azure"></a>对 SQL Server 托管备份到 Windows Azure 进行故障排除  
 由于 SQL Server 托管备份构建在“备份到 URL”上，因此上面各节中介绍的故障排除提示适用于使用 SQL Server 托管备份的数据库或实例。  中详细介绍了有关 SQL Server 托管备份到 Windows Azure 进行故障排除信息[故障排除 SQL Server 托管备份到 Windows Azure](sql-server-managed-backup-to-microsoft-azure.md)。  
  
## <a name="see-also"></a>请参阅  
 [从 Windows Azure 中存储的备份还原](restoring-from-backups-stored-in-microsoft-azure.md)  
  
  
