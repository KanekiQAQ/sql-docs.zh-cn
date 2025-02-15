---
title: SQL Server 备份到 URL | Microsoft Docs
ms.custom: ''
ms.date: 01/25/2016
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 11be89e9-ff2a-4a94-ab5d-27d8edf9167d
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: d3911ab34a01b2da971aa602df37c8c559ed6390
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62920725"
---
# <a name="sql-server-backup-to-url"></a>SQL Server 备份到 URL
  本主题介绍使用 Windows Azure Blob 存储服务作为备份目标所需的概念、要求和组件。 备份和还原功能与使用磁盘或磁带时相同，或类似但区别不大。 区别均为显著的例外，并且本主题中包括少量代码示例。  
  
## <a name="requirements-components-and-concepts"></a>要求、组件和概念  
 **本节内容：**  
  
-   [安全性](#security)  
  
-   [关键组件和概念简介](#intorkeyconcepts)  
  
-   [Windows Azure Blob 存储服务](#Blob)  
  
-   [SQL Server 组件](#sqlserver)  
  
-   [限制](#limitations)  
  
-   [对备份/还原语句的支持](#Support)  
  
-   [使用 SQL Server Management Studio 中的备份任务](sql-server-backup-to-url.md#BackupTaskSSMS)  
  
-   [使用维护计划向导将 SQL Server 备份到 URL](sql-server-backup-to-url.md#MaintenanceWiz)  
  
-   [使用 SQL Server Management Studio 从 Windows Azure 存储还原](sql-server-backup-to-url.md#RestoreSSMS)  
  
###  <a name="security"></a> 安全性  
 以下是备份到 Windows Azure Blob 存储服务或从该服务还原时的安全注意事项和要求。  
  
-   为 Windows Azure Blob 存储服务创建容器时，我们建议您将访问权限设置为 **“私有”** 。 将访问权限设置为“私有”后，只允许可提供对 Windows Azure 帐户进行身份验证所需的信息的用户或帐户进行访问。  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要求在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 凭据中存储 Windows Azure 帐户名称和访问密钥身份验证信息。 在执行备份或还原操作时，此信息用于对 Windows Azure 帐户进行身份验证。  
  
-   用于发出 BACKUP 或 RESTORE 命令的用户帐户应属于具有“更改任意凭据”  权限的 **db_backup 操作员**数据库角色。  
  
###  <a name="intorkeyconcepts"></a> 关键组件和概念简介  
 以下两节介绍了 Windows Azure Blob 存储服务以及在备份到 Windows Azure Blob 存储服务或从中还原时使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件。 了解这些组件和它们之间的交互对于执行备份到 Windows Azure Blob 存储服务或从中还原很重要。  
  
 首先需要创建 Windows Azure 帐户。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用**Windows Azure 存储帐户名称**并将其**访问密钥**值来写入和读取 blob 存储服务进行身份验证。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 凭据存储此身份验证信息并在备份或还原操作期间使用它。 有关创建存储帐户和执行简单还原的完整演练，请参阅 [将 Windows Azure 存储服务用于 SQL Server 备份和还原的教程](https://go.microsoft.com/fwlink/?LinkId=271615)。  
  
 ![存储帐户映射到 sql 凭据](../../tutorials/media/backuptocloud-storage-credential-mapping.gif "映射到 sql 凭据的存储帐户")  
  
###  <a name="Blob"></a> Windows Azure Blob 存储服务  
 **存储帐户：** 存储帐户是所有存储服务的起始点。 要访问 Windows Azure Blob 存储服务，请首先创建一个 Windows Azure 存储帐户。 需要使用 **storage account name** 和 **access key** 属性来向 Windows Azure Blob 存储服务及其组件进行身份验证。  
  
 **容器：** 容器提供一组 Blob，并且可以存储无限的数量的 Blob。 要将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份写入 Windows Azure Blob 服务，您必须至少创建根容器。  
  
 **Blob：** 任意类型和大小的文件。 Windows Azure Blob 存储服务中可存储两类 blob：块 blob 和页 blob。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份将页 Blob 作为 Blob 类型。 Blob 是可使用以下 URL 格式寻址： https://\<存储帐户 >.blob.core.windows.net/\<容器 > /\<blob >  
  
 ![Azure Blob 存储](../../database-engine/media/backuptocloud-blobarchitecture.gif "Azure Blob 存储")  
  
 有关 Windows Azure Blob 存储服务的详细信息，请参阅 [如何使用 Windows Azure Blob 存储服务](http://www.windowsazure.com/develop/net/how-to-guides/blob-storage/)  
  
 有关页 Blob 的详细信息，请参阅 [了解块 Blob 和页 Blob](https://msdn.microsoft.com/library/windowsazure/ee691964.aspx)  
  
###  <a name="sqlserver"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Components  
 **URL：** URL 指定统一资源标识符 (URI) 来标识唯一备份文件。 URL 用于提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份文件的位置和名称。 在此实现中，唯一有效的 URL 是指向 Windows Azure 存储帐户中页 Blob 的 URL。 该 URL 必须指向实际 Blob，而不仅仅是容器。 如果 Blob 不存在，则创建它。 如果现有的 Blob 是指定，BACKUP 将失败，除非指定了"WITH FORMAT"选项。  
  
> [!WARNING]  
>  如果您选择复制备份文件并将它上载到 Windows Azure Blob 存储服务，则将页 blob 用作您的存储选项。 不支持从块 Blob 进行还原。 从块 blob 类型 RESTORE 将出错并且失败。  
  
 下面是一个示例 URL 值： http[s]://ACCOUNTNAME.Blob.core.windows.net/\<容器 > /\<FILENAME.bak >。 HTTPS 不是必需的，但建议这样做。  
  
 **凭据：** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 凭据是用于存储连接到 SQL Server 外部资源所需的身份验证信息的对象。  在这里， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 备份和还原进程使用凭据对 Windows Azure Blob 存储服务进行身份验证。 凭据存储着存储帐户的名称和存储帐户的 **access key** 值。 创建凭据后，在发出 BACKUP/RESTORE 命令时必须在 WITH CREDENTIAL 选项中指定它。 有关如何查看、 复制或重新生成存储帐户详细信息**访问密钥**，请参阅[存储帐户访问密钥](https://msdn.microsoft.com/library/windowsazure/hh531566.aspx)。  
  
 有关如何创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 凭据的分步说明，请参阅本主题后面的 [创建凭据](#credential) 示例。  
  
 有关凭据的一般信息，请参阅 [凭据](../security/authentication-access/credentials-database-engine.md)  
  
 有关使用凭据的其他示例的信息，请参阅 [创建 SQL 代理的代理](../../ssms/agent/create-a-sql-server-agent-proxy.md)。  
  
###  <a name="limitations"></a> 限制  
  
-   不支持备份到高级存储。  
  
-   支持的最大备份大小为 1 TB。  
  
-   可使用 TSQL、SMO 或 PowerShell cmdlet 发出备份或还原语句。 当前不支持使用 SQL Server Management Studio 备份或还原向导备份到 Windows Azure Blob 存储服务或从该服务还原。  
  
-   不支持创建逻辑设备名称。 因此，不支持使用 sp_dumpdevice 或通过 SQL Server Management Studio 将 URL 添加为备份设备。  
  
-   不支持追加到现有备份 blob。 只能使用 WITH FORMAT 选项覆盖到现有 Blob 的备份。  
  
-   不支持在单个备份操作中备份到多个 blob。 例如，下面的代码将返回错误：  
  
    ```  
    BACKUP DATABASE AdventureWorks2012   
    TO URL = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012_1.bak'   
       URL = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012_2.bak'   
          WITH CREDENTIAL = 'mycredential'   
         ,STATS = 5;  
    GO  
  
    ```  
  
-   不支持使用 `BACKUP` 指定块大小。  
  
-   不支持指定 `MAXTRANSFERSIZE`。  
  
-   不支持指定备份集选项 - `RETAINDAYS` 和 `EXPIREDATE`。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要求备份设备名称最多包含 259 个字符。 对于用于指定 URL“https://.blob.core.windows.net//.bak”所需的元素，BACKUP TO URL 占用 36 个字符，其余 223 个字符将用于帐户、容器和 Blob 名称。  
  
###  <a name="Support"></a> 对备份/还原语句的支持  
  
|||||  
|-|-|-|-|  
|备份/还原语句|支持|Exceptions|注释|  
|BACKUP|???|不支持 BLOCKSIZE 和 MAXTRANSFERSIZE。|要求指定 WITH CREDENTIAL|  
|RESTORE|???||要求指定 WITH CREDENTIAL|  
|RESTORE FILELISTONLY|???||要求指定 WITH CREDENTIAL|  
|RESTORE HEADERONLY|???||要求指定 WITH CREDENTIAL|  
|RESTORE LABELONLY|???||要求指定 WITH CREDENTIAL|  
|RESTORE VERIFYONLY|???||要求指定 WITH CREDENTIAL|  
|RESTORE REWINDONLY|???|||  
  
 有关备份语句的语法和一般信息，请参阅 [BACKUP (Transact-SQL)](/sql/t-sql/statements/backup-transact-sql)。  
  
 有关还原语句的语法和一般信息，请参阅 [RESTORE (Transact-SQL)](/sql/t-sql/statements/restore-statements-transact-sql)。  
  
### <a name="support-for-backup-arguments"></a>对备份参数的支持  
  
|||||  
|-|-|-|-|  
|参数|是否支持|异常|注释|  
|DATABASE|???|||  
|LOG|???|||  
||  
|TO (URL)|???|与 DISK 和 TAPE 不同，URL 不支持指定或创建逻辑名称。|此参数用于指定备份文件的 URL 路径。|  
|MIRROR TO|???|||  
|**WITH 选项：**||||  
|CREDENTIAL|???||仅当使用 BACKUP TO URL 选项备份到 Windows Azure Blob 存储服务时，才支持 WITH CREDENTIAL。|  
|DIFFERENTIAL|???|||  
|COPY_ONLY|???|||  
|COMPRESSION&#124;NO_COMPRESSION|???|||  
|DESCRIPTION|???|||  
|NAME|???|||  
|EXPIREDATE &#124; RETAINDAYS|???|||  
|NOINIT &#124; INIT|???||如果使用，则忽略此选项。<br /><br /> 不能追加到 blob。 要覆盖备份，请使用 FORMAT 参数。|  
|NOSKIP &#124; SKIP|???|||  
|NOFORMAT &#124; FORMAT|???||如果使用，则忽略此选项。<br /><br /> 除非指定 WITH FORMAT，否则对现有 blob 的备份失败。 指定 WITH FORMAT 时，覆盖现有 blob。|  
|MEDIADESCRIPTION|???|||  
|MEDIANAME|???|||  
|BLOCKSIZE|???|||  
|BUFFERCOUNT|???|||  
|MAXTRANSFERSIZE|???|||  
|NO_CHECKSUM &#124; CHECKSUM|???|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|???|||  
|STATS|???|||  
|REWIND &#124; NOREWIND|???|||  
|UNLOAD &#124; NOUNLOAD|???|||  
|NORECOVERY &#124; STANDBY|???|||  
|NO_TRUNCATE|???|||  
  
 有关备份参数的详细信息，请参阅 [BACKUP (Transact-SQL)](/sql/t-sql/statements/backup-transact-sql)。  
  
### <a name="support-for-restore-arguments"></a>对还原参数的支持  
  
|||||  
|-|-|-|-|  
|参数|支持|Exceptions|注释|  
|DATABASE|???|||  
|LOG|???|||  
|FROM (URL)|???||FROM URL 参数用于指定备份文件的 URL 路径。|  
|**WITH Options:**||||  
|CREDENTIAL|???||仅当使用 RESTORE FROM URL 选项从 Windows Azure Blob 存储服务还原时，才支持 WITH CREDENTIAL。|  
|PARTIAL|???|||  
|RECOVERY &#124; NORECOVERY &#124; STANDBY|???|||  
|LOADHISTORY|???|||  
|MOVE|???|||  
|REPLACE|???|||  
|RESTART|???|||  
|RESTRICTED_USER|???|||  
|FILE|???|||  
|PASSWORD|???|||  
|MEDIANAME|???|||  
|MEDIAPASSWORD|???|||  
|BLOCKSIZE|???|||  
|BUFFERCOUNT|???|||  
|MAXTRANSFERSIZE|???|||  
|CHECKSUM &#124; NO_CHECKSUM|???|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|???|||  
|FILESTREAM|???|||  
|STATS|???|||  
|REWIND &#124; NOREWIND|???|||  
|UNLOAD &#124; NOUNLOAD|???|||  
|KEEP_REPLICATION|???|||  
|KEEP_CDC|???|||  
|ENABLE_BROKER &#124; ERROR_BROKER_CONVERSATIONS &#124; NEW_BROKER|???|||  
|STOPAT &#124; STOPATMARK &#124; STOPBEFOREMARK|???|||  
  
 有关还原参数的详细信息，请参阅[RESTORE 参数 (Transact-SQL)](/sql/t-sql/statements/restore-statements-arguments-transact-sql)。  
  
##  <a name="BackupTaskSSMS"></a> 使用 SQL Server Management Studio 中的备份任务  
 SQL Server Management Studio 中的备份任务已经过增强，其中包括 URL 作为一个目标选项以及 SQL 凭据等备份到 Windows Azure 存储所需的其他支持对象。  
  
 以下步骤介绍为了能够备份到 Windows Azure 存储而对“备份数据库”任务作出的更改：  
  
1.  启动 SQL Server Management Studio 并连接到 SQL Server 实例。  选择你想要备份，请右键单击一个的数据库**任务**，然后选择**备份...** .这将打开备份数据库对话框。  
  
2.  在“常规”页上， **“URL”** 用于向 Windows Azure 存储创建备份。 选择此选项后，将看到此页上启用其他选项：  
  
    1.  **文件名：** 备份文件的名称。  
  
    2.  **SQL 凭据：** 可以指定现有的 SQL Server 凭据，也可以创建一个新通过单击**创建**SQL 凭据框旁。  
  
        > [!IMPORTANT]  
        >  单击 **“创建”** 打开的对话框需要管理证书或订阅的发布配置文件。 SQL Server 当前支持发布配置文件版本 2.0。 要下载支持的发布配置文件版本，请参阅 [下载发布配置文件 2.0](https://go.microsoft.com/fwlink/?LinkId=396421)。  
        >   
        >  如果您无权访问管理证书或发布配置文件，可以创建一个 SQL 凭据，方法是使用 Transact-SQL 或 SQL Server Management Studio 指定存储帐户名称和访问密钥信息。 请参阅中的示例代码[创建凭据](#credential)部分，以使用 TRANSACT-SQL 创建凭据。 或者，使用 SQL Server Management Studio，从数据库引擎实例中右键单击 **“安全性”** ，依次选择 **“新建”** 和 **“凭据”** 。 在 **“标识”** 字段中指定存储帐户名称，在 **“密码”** 字段中指定访问密钥。  
  
    3.  **Azure 存储容器：** Windows Azure 存储容器用于存储备份文件的名称。  
  
    4.  **URL 前缀：** 这是自动使用上一步骤中所述的字段中指定的信息生成的。 如果手动编辑此值，则确保它与以前提供的其他信息相匹配。 例如，如果修改存储 URL，则确保设置 SQL 凭据以向同一存储帐户进行身份验证。  
  
 选择 URL 作为目标后，将禁用 **“介质选项”** 页中的某些选项。  以下主题详细介绍“备份数据库”对话框：  
  
 [备份数据库（“常规”页）](../../integration-services/general-page-of-integration-services-designers-options.md)  
  
 [备份数据库（“媒体选项”页）](back-up-database-media-options-page.md)  
  
 [备份数据库（“备份选项”页）](back-up-database-backup-options-page.md)  
  
 [创建凭据 - 向 Azure 存储进行身份验证](create-credential-authenticate-to-azure-storage.md)  
  
##  <a name="MaintenanceWiz"></a> 使用维护计划向导将 SQL Server 备份到 URL  
 与之前介绍的备份任务类似，SQL Server Management Studio 中的维护计划向导已经过增强，其中包括 **URL** 作为一个目标选项以及 SQL 凭据等备份到 Windows Azure 存储所需的其他支持对象。 有关详细信息，请参阅 [Using Maintenance Plan Wizard](../maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure) 中的“定义备份任务”部分  。  
  
##  <a name="RestoreSSMS"></a> 从 Windows Azure 存储使用 SQL Server Management Studio 还原  
 如果要还原数据库，则加入 **URL** 作为要从其进行还原的设备。 以下步骤介绍为了能够从 Windows Azure 存储还原而对还原任务作出的更改：  
  
1.  在 SQL Server Management Studio 中还原任务的 **“常规”** 页中选择 **“设备”** 后，将进入 **“选择备份设备”** 对话框，其中包括 **“URL”** 作为备份介质类型。  
  
2.  选择 **“URL”** 并单击 **“添加”** 时，将打开 **“连接到 Azure 存储”** 对话框。 指定用于向 Windows Azure 存储进行身份验证的 SQL 凭据信息。  
  
3.  然后，SQL Server 使用所提供的 SQL 凭据信息连接到 Windows Azure 存储，然后打开 **“在 Windows Azure 上定位备份文件”** 对话框。 此页上显示位于存储中的备份文件。 选择要用于还原的文件，然后单击 **“确定”** 。 此操作将返回 **“选择备份设备”** 对话框，而单击此对话框上的 **“确定”** 将返回主 **“还原”** 对话框，从中将可完成还原。  有关详细信息，请参阅以下主题：  
  
     [还原数据库（“常规”页）](restore-database-general-page.md)  
  
     [还原数据库（“文件”页）](restore-database-files-page.md)  
  
     [还原数据库（“选项”页）](restore-database-options-page.md)  
  
##  <a name="Examples"></a> 代码示例  
 本节包含以下示例。  
  
-   [创建凭据](#credential)  
  
-   [备份整个数据库](#complete)  
  
-   [备份数据库和日志](#databaselog)  
  
-   [创建主文件组的完整文件备份](#filebackup)  
  
-   [创建主文件组的差异文件备份](#differential)  
  
-   [还原数据库并移动文件](#restoredbwithmove)  
  
-   [使用 STOPAT 还原到时间点](#PITR)  
  
###  <a name="credential"></a> 创建凭据  
 下面的示例创建存储 Windows Azure 存储身份验证信息的凭据。  
  
1.  **Tsql**  
  
    ```  
    IF NOT EXISTS  
    (SELECT * FROM sys.credentials   
    WHERE credential_identity = 'mycredential')  
    CREATE CREDENTIAL mycredential WITH IDENTITY = 'mystorageaccount'  
    ,SECRET = '<storage access key>' ;  
  
    ```  
  
2.  **C#**  
  
    ```  
    // Connect to default sql server instance on local machine  
    Server server = new Server(".");  
    string identity = "mystorageaccount";  
    string secret = "<storage access key>";  
  
    // Create a Credential  
    string credentialName = "mycredential";  
    Credential credential = new Credential(server, credentialName);  
    credential.Create(identity, secret);  
    ```  
  
3.  **PowerShell**  
  
    ```  
    # create variables  
    $storageAccount = "mystorageaccount"  
    $storageKey = "<storage access key>"  
    $secureString = convertto-securestring $storageKey  -asplaintext -force  
    $credentialName = "mycredential"  
  
    $srvPath = "SQLSERVER:\SQL\COMPUTERNAME\INSTANCENAME"  
    # for default instance, the $srvpath variable would be "SQLSERVER:\SQL\COMPUTERNAME\DEFAULT"  
  
    # Create a credential  
     New-SqlCredential -Name $credentialName -Path $srvpath -Identity $storageAccount -Secret $secureString  
  
    ```  
  
###  <a name="complete"></a> 完整数据库备份  
 下面的示例将 AdventureWorks2012 数据库备份到 Windows Azure Blob 存储服务。  
  
1.  **Tsql**  
  
    ```  
    BACKUP DATABASE AdventureWorks2012   
    TO URL = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012.bak'   
          WITH CREDENTIAL = 'mycredential'   
         ,COMPRESSION  
         ,STATS = 5;  
    GO  
  
    ```  
  
1.  **C#**  
  
    ```  
    // Connect to default sql server instance on local machine  
    Server server = new Server(".");  
    string identity = "mystorageaccount";  
  
    string credentialName = "mycredential";  
    string dbName = "AdventureWorks2012";  
    string blobContainerName = "mycontainer";  
  
    // Generate Unique Url  
    string url = String.Format(@"https://{0}.blob.core.windows.net/{1}/{2}-{3}.bak",  
            identity,  
            blobContainerName,  
            dbName,  
            DateTime.Now.ToString("s").Replace(":", "-"));  
  
    // Backup to Url  
    Backup backup = new Backup();  
    backup.CredentialName = credentialName;  
    backup.Database = dbName;  
    backup.CompressionOption = BackupCompressionOptions.On;  
    backup.Devices.AddDevice(url, DeviceType.Url);  
    backup.SqlBackup(server);  
    ```  
  
2.  **PowerShell**  
  
    ```  
    # create variables  
    $backupUrlContainer = "https://mystorageaccount.blob.core.windows.net/mycontainer/"  
    $credentialName = "mycredential"  
    $srvPath = "SQLSERVER:\SQL\COMPUTERNAME\INSTANCENAME"   
    # for default instance, the $srvpath varilable would be "SQLSERVER:\SQL\COMPUTERNAME\DEFAULT"  
  
    # navigate to SQL Server Instance  
    CD $srvPath   
    $backupFile = $backupUrlContainer + "AdventureWorks2012" +  ".bak"  
    Backup-SqlDatabase -Database AdventureWorks2012 -backupFile $backupFile  -SqlCredential $credentialName -CompressionOption On  
  
    ```  
  
###  <a name="databaselog"></a> 备份数据库和日志  
 下面的示例备份 AdventureWorks2012 示例数据库，默认情况下，该数据库使用简单恢复模式。 若要支持日志备份，请将 AdventureWorks2012 数据库改为使用完整恢复模式。 然后，该示例对 Windows Azure Blob 创建完整数据库备份，并在一段更新活动过后备份日志。 此示例将创建具有日期时间戳的备份文件名。  
  
1.  **Tsql**  
  
    ```  
    -- To permit log backups, before the full database backup, modify the database   
    -- to use the full recovery model.  
    USE master;  
    GO  
    ALTER DATABASE AdventureWorks2012  
       SET RECOVERY FULL;  
    GO  
  
    -- Back up the full AdventureWorks2012 database.  
           -- First create a file name for the backup file with DateTime stamp  
  
    DECLARE @Full_Filename AS VARCHAR (300);  
    SET @Full_Filename = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012_Full_'+   
    REPLACE (REPLACE (REPLACE (CONVERT (VARCHAR (40), GETDATE (), 120), '-','_'),':', '_'),' ', '_') + '.bak';   
    --Back up Adventureworks2012 database  
  
    BACKUP DATABASE AdventureWorks2012  
    TO URL =  @Full_Filename  
    WITH CREDENTIAL = 'mycredential';  
    ,COMPRESSION  
    GO  
    -- Back up the AdventureWorks2012 log.  
    DECLARE @Log_Filename AS VARCHAR (300);  
    SET @Log_Filename = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012_Log_'+   
    REPLACE (REPLACE (REPLACE (CONVERT (VARCHAR (40), GETDATE (), 120), '-','_'),':', '_'),' ', '_') + '.trn';  
    BACKUP LOG AdventureWorks2012  
     TO URL = @Log_Filename  
    WITH CREDENTIAL = 'mycredential'  
    ,COMPRESSION;  
    GO  
    ```  
  
2.  **C#**  
  
    ```  
    // Connect to default sql server instance on local machine  
    Server server = new Server(".");  
    string identity = "mystorageaccount";  
  
    string credentialName = "mycredential";  
    string dbName = "AdventureWorks2012";  
    string blobContainerName = "mycontainer";  
  
    // Generate Unique Url for data backup  
    string urlDataBackup = String.Format(@"https://{0}.blob.core.windows.net/{1}/{2}_Data-{3}.bak",  
            identity,  
            blobContainerName,  
            dbName,  
            DateTime.Now.ToString("s").Replace(":", "-"));  
  
    // Backup Database to Url  
    Backup backupData = new Backup();  
    backupData.CredentialName = credentialName;  
    backupData.Database = dbName;  
    backup.CompressionOption = BackupCompressionOptions.On;  
    backupData.Devices.AddDevice(urlDataBackup, DeviceType.Url);  
    backupData.SqlBackup(server);  
  
    // Generate Unique Url for data backup  
    string urlLogBackup = String.Format(@"https://{0}.blob.core.windows.net/{1}/{2}_Log-{3}.bak",  
            identity,  
            blobContainerName,  
            dbName,  
            DateTime.Now.ToString("s").Replace(":", "-"));  
  
    // Backup Database Log to Url  
    Backup backupLog = new Backup();  
    backupLog.CredentialName = credentialName;  
    backupLog.Database = dbName;  
    backup.CompressionOption = BackupCompressionOptions.On;  
    backupLog.Devices.AddDevice(urlLogBackup, DeviceType.Url);  
    backupLog.Action = BackupActionType.Log;  
    backupLog.SqlBackup(server);  
    ```  
  
3.  **PowerShell**  
  
    ```  
  
    #create variables  
    $backupUrlContainer = "https://mystorageaccount.blob.core.windows.net/mycontainer/"  
    $credentialName = "mycredential"  
    $srvPath = "SQLSERVER:\SQL\COMPUTERNAME\INSTANCENAME"  
    # for default instance, the $srvpath variable would be "SQLSERVER:\SQL\COMPUTERNAME\DEFAULT"  
  
    # navigate to theSQL Server Instance  
  
    CD $srvPath   
    #Create a unique file name for the full database backup  
    $backupFile = $backupUrlContainer + "AdventureWorks2012_" + (Get-Date).ToString("s").Replace("-","_").Replace(":", "_").Replace(" ","_").Replace("/", "_") +  ".bak"  
  
    #Backup Database to URL  
  
    Backup-SqlDatabase -Database AdventureWorks2012 -backupFile $backupFile  -SqlCredential $credentialName -CompressionOption On -BackupAction Database    
  
    #Create a unique file name for log backup  
  
    $backupFile = $backupUrlContainer + "AdventureWorks2012_" + (Get-Date).ToString("s").Replace("-","_").Replace(":", "_").Replace(" ","_").Replace("/", "_") +  ".trn"  
  
    #Backup Log to URL  
  
    Backup-SqlDatabase -Database AdventureWorks2012 -backupFile $backupFile  -SqlCredential $credentialName -CompressionOption On -BackupAction Log  
  
    ```  
  
###  <a name="filebackup"></a> 创建主文件组的完整文件备份  
 下面的示例创建主文件组的完整文件备份。  
  
1.  **Tsql**  
  
    ```  
    --Back up the files in Primary:  
    BACKUP DATABASE AdventureWorks2012  
       FILEGROUP = 'Primary'  
       TO URL = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012files.bck'  
       WITH CREDENTIAL = 'mycredential'  
       ,COMPRESSION;  
    GO  
    ```  
  
2.  **C#**  
  
    ```  
    // Connect to default sql server instance on local machine  
    Server server = new Server(".");  
    string identity = "mystorageaccount";  
  
    string credentialName = "mycredential";  
    string dbName = "AdventureWorks2012";  
    string blobContainerName = "mycontainer";  
  
    // Generate Unique Url  
    string url = String.Format(@"https://{0}.blob.core.windows.net/{1}/{2}-{3}.bck",  
            identity,  
            blobContainerName,  
            dbName,  
            DateTime.Now.ToString("s").Replace(":", "-"));  
  
    // Backup to Url  
    Backup backup = new Backup();  
    backup.CredentialName = credentialName;  
    backup.Database = dbName;  
    backup.Action = BackupActionType.Files;  
    backup.DatabaseFileGroups.Add("PRIMARY");  
    backup.CompressionOption = BackupCompressionOptions.On;  
    backup.Devices.AddDevice(url, DeviceType.Url);  
    backup.SqlBackup(server);  
  
    ```  
  
3.  **PowerShell**  
  
    ```  
  
    #create variables  
    $backupUrlContainer = "https://mystorageaccount.blob.core.windows.net/mycontainer/"  
    $credentialName = "mycredential"  
    $srvPath = "SQLSERVER:\SQL\COMPUTERNAME\INSTANCENAME"  
    # for default instance, the $srvpath variable would be "SQLSERVER:\SQL\COMPUTERNAME\DEFAULT"  
  
    # navigate to the SQL Server Instance  
  
    CD $srvPath   
    #Create a unique file name for the file backup  
    $backupFile = $backupUrlContainer + "AdventureWorks2012_" + (Get-Date).ToString("s").Replace("-","_").Replace(":", "_").Replace(" ","_").Replace("/", "_") +  ".bck"  
  
    #Backup Primary File Group to URL  
  
    Backup-SqlDatabase -Database AdventureWorks2012 -backupFile $backupFile  -SqlCredential $credentialName -CompressionOption On -BackupAction Files -DatabaseFileGroup Primary  
  
    ```  
  
###  <a name="differential"></a> 创建主文件组的差异文件备份  
 下面的示例创建主文件组的差异文件备份。  
  
1.  **Tsql**  
  
    ```  
    --Back up the files in Primary:  
    BACKUP DATABASE AdventureWorks2012  
       FILEGROUP = 'Primary'  
       TO URL = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012filesdiff.bck'  
       WITH   
          CREDENTIAL = 'mycredential'  
          ,COMPRESSION  
      ,DIFFERENTIAL;  
    GO  
  
    ```  
  
2.  **C#**  
  
    ```  
    // Connect to default sql server instance on local machine  
    Server server = new Server(".");  
    string identity = "mystorageaccount";  
  
    string credentialName = "mycredential";  
    string dbName = "AdventureWorks2012";  
    string blobContainerName = "mycontainer";  
  
    // Generate Unique Url  
    string url = String.Format(@"https://{0}.blob.core.windows.net/{1}/{2}-{3}.bak",  
            identity,  
            blobContainerName,  
            dbName,  
            DateTime.Now.ToString("s").Replace(":", "-"));  
  
    // Backup to Url  
    Backup backup = new Backup();  
    backup.CredentialName = credentialName;  
    backup.Database = dbName;  
    backup.Action = BackupActionType.Files;  
    backup.DatabaseFileGroups.Add("PRIMARY");  
    backup.Incremental = true;  
    backup.CompressionOption = BackupCompressionOptions.On;  
    backup.Devices.AddDevice(url, DeviceType.Url);  
    backup.SqlBackup(server);  
  
    ```  
  
3.  **PowerShell**  
  
    ```  
  
    #create variables  
    $backupUrlContainer = "https://mystorageaccount.blob.core.windows.net/mycontainer/"  
    $credentialName = "mycredential"  
    $srvPath = "SQLSERVER:\SQL\COMUTERNAME\INSTANCENAME"  
    # for default instance, the $srvpath variable would be "SQLSERVER:\SQL\COMPUTERNAME\DEFAULT"  
  
    # navigate to SQL Server Instance  
  
    CD $srvPath   
  
    #create a unique file name for the full backup  
    $backupdbFile = $backupUrlContainer + "AdventureWorks2012_" + (Get-Date).ToString("s").Replace("-","_").Replace(":", "_").Replace(" ","_").Replace("/", "_") +  ".bak"  
  
    #Create a differential backup of the primary filegroup  
  
    Backup-SqlDatabase -Database AdventureWorks2012 -backupFile $backupFile  -SqlCredential $credentialName -CompressionOption On -BackupAction Files -DatabaseFileGroup Primary -Incremental  
  
    ```  
  
###  <a name="restoredbwithmove"></a> 还原数据库并移动文件  
 要还原完整数据库备份并将还原的数据库移到 C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Data 目录，请使用以下步骤。  
  
1.  **Tsql**  
  
    ```  
    -- Backup the tail of the log first  
  
    DECLARE @Log_Filename AS VARCHAR (300);  
    SET @Log_Filename = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012_Log_'+   
    REPLACE (REPLACE (REPLACE (CONVERT (VARCHAR (40), GETDATE (), 120), '-','_'),':', '_'),' ', '_') + '.trn';  
    BACKUP LOG AdventureWorks2012  
     TO URL = @Log_Filename  
    WITH CREDENTIAL = 'mycredential'  
    ,NORECOVERY;  
    GO  
  
    RESTORE DATABASE AdventureWorks2012 FROM URL = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012.bak'  
    WITH CREDENTIAL = 'mycredential'  
    ,MOVE 'AdventureWorks2012_data' to 'C:\Program Files\Microsoft SQL Server\myinstance\MSSQL\DATA\AdventureWorks2012.mdf'  
    ,MOVE 'AdventureWorks2012_log' to 'C:\Program Files\Microsoft SQL Server\myinstance\MSSQL\DATA\AdventureWorks2012.ldf'  
    ,STATS = 5  
  
    ```  
  
2.  **C#**  
  
    ```  
    // Connect to default sql server instance on local machine  
    Server server = new Server(".");  
    string identity = "mystorageaccount";  
  
    string credentialName = "mycredential";  
    string dbName = "AdventureWorks2012";  
    string blobContainerName = "mycontainer";  
  
    // Generate Unique Url  
    string urlBackupData = String.Format(@"https://{0}.blob.core.windows.net/{1}/{2}-Data{3}.bak",  
            identity,  
            blobContainerName,  
            dbName,  
            DateTime.Now.ToString("s").Replace(":", "-"));  
  
    // Backup to Url  
    Backup backup = new Backup();  
    backup.CredentialName = credentialName;  
    backup.Database = dbName;  
    backup.Devices.AddDevice(urlBackupData, DeviceType.Url);  
    backup.SqlBackup(server);  
  
    // Generate Unique Url for tail log backup  
    string urlTailLogBackup = String.Format(@"https://{0}.blob.core.windows.net/{1}/{2}-TailLog{3}.bak",  
            identity,  
            blobContainerName,  
            dbName,  
            DateTime.Now.ToString("s").Replace(":", "-"));  
  
    // Backup Tail Log to Url  
    Backup backupTailLog = new Backup();  
    backupTailLog.CredentialName = credentialName;  
    backupTailLog.Database = dbName;  
    backupTailLog.Action = BackupActionType.Log;  
    backupTailLog.NoRecovery = true;  
    backupTailLog.Devices.AddDevice(urlTailLogBackup, DeviceType.Url);  
    backupTailLog.SqlBackup(server);  
  
    // Restore a database and move files  
    string newDataFilePath = server.MasterDBLogPath  + @"\" + dbName + DateTime.Now.ToString("s").Replace(":", "-") + ".mdf";  
    string newLogFilePath = server.MasterDBLogPath  + @"\" + dbName + DateTime.Now.ToString("s").Replace(":", "-") + ".ldf";  
  
    Restore restore = new Restore();  
    restore.CredentialName = credentialName;  
    restore.Database = dbName;  
    restore.ReplaceDatabase = true;  
    restore.Devices.AddDevice(urlBackupData, DeviceType.Url);  
    restore.RelocateFiles.Add(new RelocateFile(dbName, newDataFilePath));  
    restore.RelocateFiles.Add(new RelocateFile(dbName+ "_Log", newLogFilePath));  
    restore.SqlRestore(server);  
  
    ```  
  
3.  **PowerShell**  
  
    ```  
  
    #create variables  
    $backupUrlContainer = "https://mystorageaccount.blob.core.windows.net/mycontainer/"  
    $credentialName = "mycredential"  
    $srvPath = "SQLSERVER:\SQL\COMPUTERNAME\INSTNACENAME"  
    # for default instance, the $srvpath variable would be "SQLSERVER:\SQL\COMPUTERNAME\DEFAULT"  
  
    # navigate to SQL Server Instance   
  
    CD $srvPath   
  
    #create a unique file name for the full backup  
    $backupdbFile = $backupUrlContainer + "AdventureWorks2012_" + (Get-Date).ToString("s").Replace("-","_").Replace(":", "_").Replace(" ","_").Replace("/", "_") +  ".bak"  
  
    # Full database backup to URL  
    Backup-SqlDatabase -Database AdventureWorks2012 -backupFile $backupdbFile  -SqlCredential $credentialName -CompressionOption On      
  
    #Create a unique file name for the tail log backup  
    $backuplogFile = $backupUrlContainer + "AdventureWorks2012_" + (Get-Date).ToString("s").Replace("-","_").Replace(":", "_").Replace(" ","_").Replace("/", "_") +  ".trn"  
  
    #Backup tail log to URL  
  
    Backup-SqlDatabase -Database AdventureWorks2012 -backupFile $backupFile  -SqlCredential $credentialName  -BackupAction Log -NoRecovery    
  
    # Restore Database and move files  
  
    $newDataFilePath = New-Object Microsoft.SqlServer.Management.Smo.RelocateFile ("AdventureWorks_Data","C:\Program Files\Microsoft SQL Server\myinstance\MSSQL\DATA\AdventureWorks2012.mdf")  
    $newLogFilePath = New-Object Microsoft.SqlServer.Management.Smo.RelocateFile("AdventureWorks_Log","C:\Program Files\Microsoft SQL Server\myinstance\MSSQL\DATA\AdventureWorks2012.ldf")  
  
    Restore-SqlDatabase -Database AdventureWorks2012 -SqlCredential $credentialName -BackupFile $backupdbFile -RelocateFile @($newDataFilePath,$newLogFilePath)  
  
    ```  
  
###  <a name="PITR"></a> 使用 STOPAT 还原到时间点  
 下面的示例将数据库状态还原到某个时间点并显示一个还原操作。  
  
1.  **Tsql**  
  
    ```  
    RESTORE DATABASE AdventureWorks FROM URL = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012.bak'   
    WITH   
     CREDENTIAL = 'mycredential'  
    ,MOVE 'AdventureWorks2012_data' to 'C:\Program Files\Microsoft SQL Server\myinstance\MSSQL\DATA\AdventureWorks2012.mdf'  
    ,Move 'AdventureWorks2012_log' to 'C:\Program Files\Microsoft SQL Server\myinstance\MSSQL\DATA\AdventureWorks2012.ldf'  
    ,NORECOVERY  
    --,REPLACE  
    ,STATS = 5;  
    GO   
  
    RESTORE LOG AdventureWorks FROM URL = 'https://mystorageaccount.blob.core.windows.net/mycontainer/AdventureWorks2012.trn'   
    WITH CREDENTIAL = 'mycredential'  
    ,RECOVERY   
    ,STOPAT = 'Oct 23, 2012 5:00 PM'   
    GO  
    ```  
  
2.  **C#**  
  
    ```  
    // Connect to default sql server instance on local machine  
    Server server = new Server(".");  
    string identity = "mystorageaccount";  
  
    string credentialName = "mycredential";  
    string dbName = "AdventureWorks2012";  
    string blobContainerName = "mycontainer";  
  
    // Generate Unique Url  
    string urlBackupData = String.Format(@"https://{0}.blob.core.windows.net/{1}/{2}-Data{3}.bak",  
            identity,  
            blobContainerName,  
            dbName,  
            DateTime.Now.ToString("s").Replace(":", "-"));  
  
    // Backup to Url  
    Backup backup = new Backup();  
    backup.CredentialName = credentialName;  
    backup.Database = dbName;  
    backup.Devices.AddDevice(urlBackupData, DeviceType.Url);  
    backup.SqlBackup(server);  
  
    // Generate Unique Url for Tail Log backup  
    string urlTailLogBackup = String.Format(@"https://{0}.blob.core.windows.net/{1}/{2}-TailLog{3}.bak",  
            identity,  
            blobContainerName,  
            dbName,  
            DateTime.Now.ToString("s").Replace(":", "-"));  
  
    // Backup Tail Log to Url  
    Backup backupTailLog = new Backup();  
    backupTailLog.CredentialName = credentialName;  
    backupTailLog.Database = dbName;  
    backupTailLog.Action = BackupActionType.Log;  
    backupTailLog.NoRecovery = true;  
    backupTailLog.Devices.AddDevice(urlTailLogBackup, DeviceType.Url);  
    backupTailLog.SqlBackup(server);  
  
    // Restore a database and move files  
    string newDataFilePath = server.MasterDBLogPath + @"\" + dbName + DateTime.Now.ToString("s").Replace(":", "-") + ".mdf";  
    string newLogFilePath = server.MasterDBLogPath + @"\" + dbName + DateTime.Now.ToString("s").Replace(":", "-") + ".ldf";  
  
    Restore restore = new Restore();  
    restore.CredentialName = credentialName;  
    restore.Database = dbName;  
    restore.ReplaceDatabase = true;  
    restore.NoRecovery = true;  
    restore.Devices.AddDevice(urlBackupData, DeviceType.Url);  
    restore.RelocateFiles.Add(new RelocateFile(dbName, newDataFilePath));  
    restore.RelocateFiles.Add(new RelocateFile(dbName + "_Log", newLogFilePath));  
    restore.SqlRestore(server);  
  
    // Restore transaction Log with stop at   
    Restore restoreLog = new Restore();  
    restoreLog.CredentialName = credentialName;  
    restoreLog.Database = dbName;  
    restoreLog.Action = RestoreActionType.Log;  
    restoreLog.Devices.AddDevice(urlBackupData, DeviceType.Url);  
    restoreLog.ToPointInTime = DateTime.Now.ToString();   
    restoreLog.SqlRestore(server);  
  
    ```  
  
3.  **PowerShell**  
  
    ```  
  
    #create variables  
    $backupUrlContainer = "https://mystorageaccount.blob.core.windows.net/mycontainer/"  
    $credentialName = "mycredential"  
    $srvPath = "SQLSERVER:\SQL\COMPUTERNAME\INSTANCENAME"  
    # for default instance, the $srvpath variable would be "SQLSERVER:\SQL\COMPUTERNAME\DEFAULT"  
  
    # Navigate to SQL Server Instance Directory  
  
    CD $srvPath   
  
    #create a unique file name for the full backup  
    $backupdbFile = $backupUrlContainer + "AdventureWorks2012_" + (Get-Date).ToString("s").Replace("-","_").Replace(":", "_").Replace(" ","_").Replace("/", "_") +  ".bak"  
  
    # Full database backup to URL  
    Backup-SqlDatabase -Database AdventureWorks2012 -backupFile $backupdbFile  -SqlCredential $credentialName -CompressionOption On     
  
    #Create a unique file name for the tail log backup  
    $backuplogFile = $backupUrlContainer + "AdventureWorks2012_" + (Get-Date).ToString("s").Replace("-","_").Replace(":", "_").Replace(" ","_").Replace("/", "_") +  ".trn"  
  
    #Backup tail log to URL  
  
    Backup-SqlDatabase -Database AdventureWorks2012 -backupFile $backupFile  -SqlCredential $credentialName  -BackupAction Log -NoRecovery     
  
    # Restore Database and move files  
  
    $newDataFilePath = New-Object Microsoft.SqlServer.Management.Smo.RelocateFile ("AdventureWorks_Data","C:\Program Files\Microsoft SQL Server\myinstance\MSSQL\DATA\AdventureWorks2012.mdf")  
    $newLogFilePath = New-Object Microsoft.SqlServer.Management.Smo.RelocateFile("AdventureWorks_Log","C:\Program Files\Microsoft SQL Server\myinstance\MSSQL\DATA\AdventureWorks2012.ldf")  
  
    Restore-SqlDatabase -Database AdventureWorks2012 -SqlCredential $credentialName -BackupFile $backupdbFile -RelocateFile @($newDataFilePath,$newLogFilePath) -NoRecovery    
  
    # Restore Transaction log with Stop At:  
    Restore-SqlDatabase -Database AdventureWorks2012 -SqlCredential $credentialName -BackupFile $backuplogFile  -ToPointInTime (Get-Date).ToString()  
  
    ```  
  
## <a name="see-also"></a>请参阅  
 [SQL Server 备份到 URL 最佳实践和故障排除](sql-server-backup-to-url-best-practices-and-troubleshooting.md)   
 [备份和还原系统数据库 (SQL Server)](back-up-and-restore-of-system-databases-sql-server.md)   
 [教程：Windows Azure Blob 存储服务的 SQL Server 备份和还原](../tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service.md)  
  
  
