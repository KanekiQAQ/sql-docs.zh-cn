---
title: 创建 PowerShell 脚本作业步骤 | Microsoft Docs
ms.custom: ''
ms.date: 01/20/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- PowerShell [SQL Server], job steps
- jobs [SQL Server Agent], PowerShell
- job steps [PowerShell]
- SQL Server Agent jobs, PowerShell steps
ms.assetid: 50afcf84-fae0-4eb5-9b0f-f2cf144c1433
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f281e32dc982e3e7295665adce81740dfff64e24
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68264826"
---
# <a name="create-a-powershell-script-job-step"></a>Create a PowerShell Script Job Step
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> [Azure SQL 数据库托管实例](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)目前支持大多数但并非所有 SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 数据库托管实例与 SQL Server 之间的 T-SQL 差异](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍如何通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中创建和定义执行 PowerShell 脚本的 [!INCLUDE[tsql](../../includes/tsql-md.md)]代理作业步骤。  
  
**本主题内容**  
  
-   **开始之前：**  
  
    [安全性](#Security)  
  
-   **若要创建 PowerShell 脚本作业步骤，请使用：**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server 管理对象](#SMO)  

## <a name="BeforeYouBegin"></a>开始之前  
  
### <a name="Security"></a>安全性  
有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  

[!INCLUDE[Freshness](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

## <a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-a-powershell-script-job-step"></a>创建 PowerShell 脚本作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  展开 **“SQL Server 代理”** ，创建一个新作业或右键单击一个现有作业，再单击 **“属性”** 。 有关创建作业的详细信息，请参阅 [创建作业](../../ssms/agent/create-jobs.md)。  
  
3.  在 **“作业属性”** 对话框中，单击 **“步骤”** 页，再单击 **“新建”** 。  
  
4.  在 **“新建作业步骤”** 对话框中，键入作业的 **“步骤名称”** 。  
  
5.  在 **“类型”** 列表中单击 **PowerShell**。  
  
6.  在 **“运行身份”** 列表中，选择该作业将要使用的代理帐户和凭据。  
  
7.  在 **“命令”** 框中，输入将为该作业步骤执行的 PowerShell 脚本语法。 或者，单击 **“打开”** ，选择包含脚本语法的文件。 有关 PowerShell 脚本的示例，请参阅下面的 **使用 Transact-SQL** 。  
  
8.  单击 **“高级”** 页设置以下作业步骤选项：当该作业步骤成功或失败时将执行的操作、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理应该尝试执行该作业步骤的次数以及重试的时间间隔。  
  
## <a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-create-a-powershell-script-job-step"></a>创建 PowerShell 脚本作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    -- creates a PowerShell job step that finds the processes
    -- that use more than 1000 MB of memory and kills them  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Kills all processes that use more than 1000 MB of memory',  
        @subsystem = N'PowerShell',  
        @command = N'Get-Process | Where-Object { $_.WS -gt 1000MB } | Stop-Process',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_add_jobstep (Transact-SQL)](https://msdn.microsoft.com/97900032-523d-49d6-9865-2734fba1c755)。  
  
## <a name="SMO"></a>使用 SQL Server 管理对象  
**创建 PowerShell 脚本作业步骤**  
  
通过使用所选编程语言（如 Visual Basic、Visual C# 或 PowerShell）来使用 **JobStep** 类。  
  
