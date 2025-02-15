---
title: 创建 SQL Server 代理主作业 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], master jobs
- jobs [SQL Server Agent], creating
- master SQL Server Agent job [SQL Server]
ms.assetid: c12ab23f-d7ee-43a5-8cd2-0a9121292bcd
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 4582614b4032ae4dbcfffdb46ba37dc22b0ba940
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68264860"
---
# <a name="create-a-sql-server-agent-master-job"></a>创建 SQL Server 代理主作业
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> [Azure SQL 数据库托管实例](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)目前支持大多数但并非所有 SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 数据库托管实例与 SQL Server 之间的 T-SQL 差异](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中创建主 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。  
  
**本主题内容**  
  
-   **开始之前：**  
  
    [限制和局限](#Restrictions)  
  
    [安全性](#Security)  
  
-   **若要创建主 SQL Server 代理作业，可使用：**  
  
    [SQL Server Management Studio](#SSMSProcedure)  
  
    [Transact-SQL](#TsqlProcedure)  
  
## <a name="BeforeYouBegin"></a>开始之前  
  
### <a name="Restrictions"></a>限制和局限  
对主 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业所做更改必须传播到所有涉及的目标服务器。 由于在指定那些目标后，目标服务器才开始下载作业，因此 [!INCLUDE[msCoName](../../includes/msconame_md.md)] 建议在完成特定作业的所有作业步骤和作业计划后，再指定任何目标服务器。 否则，必须通过执行 **sp_post_msx_operation** 存储过程或使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]修改作业来手动请求目标服务器重新下载修改后的作业。 有关详细信息，请参阅 [sp_post_msx_operation (Transact-SQL)](https://msdn.microsoft.com/085deef8-2709-4da9-bb97-9ab32effdacf) 或 [修改作业](../../ssms/agent/modify-a-job.md)。  
  
### <a name="Security"></a>安全性  
  
#### <a name="Permissions"></a>Permissions  
如果分布式作业的步骤与某个代理相关联，则该作业将在目标服务器上该代理帐户的上下文下运行。 请确保满足以下条件，否则与代理关联的作业步骤将不会从主服务器下载到目标服务器上：  
  
-   主服务器注册表子项 **\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<&#42;instance_name&#42;>\SQL Server Agent\AllowDownloadedJobsToMatchProxyName** (REG_DWORD) 设置为 1 (true)。 默认情况下，此子项设置为 0 (False)。  
  
-   目标服务器上已存在与运行作业步骤的主服务器代理帐户同名的代理帐户。  
  
从主服务器将使用代理帐户的作业步骤下载到目标服务器时，如果作业步骤失败，可以检查 **msdb** 数据库中 **sysdownloadlist** 表的 **error_message** 列是否存在以下错误消息：  
  
-   “该作业步骤需要代理帐户，但是目标服务器上禁用了代理匹配功能。” 若要解决此错误，请将 **AllowDownloadedJobsToMatchProxyName** 注册表子项设置为 1。  
  
-   “找不到代理。” 若要解决此错误，请确保目标服务器上已存在与运行作业步骤的主服务器代理帐户同名的代理帐户。  
  
## <a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-a-master-sql-server-agent-job"></a>创建主 SQL Server 代理作业  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开要创建 SQL Server 代理作业的服务器。  
  
2.  单击加号以展开 **“SQL Server 代理”** 。  
  
3.  右键单击“作业”文件夹，然后选择“新建作业…”   。  
  
4.  在 **“新建作业”** 对话框的 **“常规”** 页上，修改作业的常规属性。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“常规”页）](../../ssms/agent/job-properties-new-job-general-page.md)  
  
5.  在 **“步骤”** 页上，组织作业步骤。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“步骤”页）](../../ssms/agent/job-properties-new-job-steps-page.md)  
  
6.  在 **“计划”** 页上，组织作业的计划。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“计划”页）](../../ssms/agent/job-properties-new-job-schedules-page.md)  
  
7.  在 **“警报”** 页上，组织作业的警报。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“警报”页）](../../ssms/agent/job-properties-new-job-alerts-page.md)  
  
8.  在 **“通知”** 页上，设置在作业完成时 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理要执行的操作。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“通知”页）](../../ssms/agent/job-properties-new-job-notifications-page.md)。  
  
9. 在 **“目标”** 页上，管理作业的目标服务器。 有关此页上可用选项的详细信息，请参阅[作业属性 - 新建作业（“目标”页）](../../ssms/agent/job-properties-new-job-targets-page.md)。  
  
10. 完成后，单击 **“确定”** 。  
  
## <a name="TsqlProcedure"></a>使用 Transact-SQL  
  
#### <a name="to-create-a-master-sql-server-agent-job"></a>创建主 SQL Server 代理作业  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    USE msdb ;  
    GO  
    -- Adds a new job executed by the SQLServerAgent service called 'Weekly Sales Data Backup'  
    EXEC dbo.sp_add_job  
        @job_name = N'Weekly Sales Data Backup' ;  
    GO  
    -- Adds a step (operation) to the 'Weekly Sales Data Backup' job.  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    -- Creates a schedule called RunOnce  
    EXEC dbo.sp_add_schedule  
        @schedule_name = N'RunOnce',  
        @freq_type = 1,  
        @active_start_time = 233000 ;  
    USE msdb ;  
    GO  
    -- Sets the 'RunOnce' schedule to the "Weekly Sales Data Backup' Job  
    EXEC sp_attach_schedule  
       @job_name = N'Weekly Sales Data Backup',  
       @schedule_name = N'RunOnce';  
    GO  
    -- assigns the multiserver job Weekly Sales Backups to the server SEATTLE2  
    -- assumes that SEATTLE2 is registered as a target server for the current instance.  
    EXEC dbo.sp_add_jobserver  
        @job_name = N'Weekly Sales Data Backups',  
        @server_name = N'SEATTLE2' ;  
    GO  
    ```  
  
有关详细信息，请参阅：  
  
-   [sp_add_job (Transact-SQL)](https://msdn.microsoft.com/6ca8fe2c-7b1c-4b59-b4c7-e3b7485df274)  
  
-   [sp_add_jobstep (Transact-SQL)](https://msdn.microsoft.com/97900032-523d-49d6-9865-2734fba1c755)  
  
-   [sp_add_schedule (Transact-SQL)](https://msdn.microsoft.com/9060aae3-3ddd-40a5-83bb-3ea7ab1ffbd7)  
  
-   [sp_attach_schedule (Transact-SQL)](https://msdn.microsoft.com/80c80eaf-cf23-4ed8-b8dd-65fe59830dd1)  
  
-   [sp_add_jobserver (Transact-SQL)](https://msdn.microsoft.com/485252cc-0081-490a-9bd1-cbbd68eea286)  
  
