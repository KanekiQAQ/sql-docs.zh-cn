---
title: 创建 Transact-SQL 作业步骤 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL job step
- job steps [Transact-SQL]
- SQL Server Agent jobs, Transact-SQL step
ms.assetid: 69c571a7-debe-4063-9d38-e4b6a1e8e84c
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 8e3d3e15c20918add5689c8ddbf4272ed41f8fe3
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68267363"
---
# <a name="create-a-transact-sql-job-step"></a>Create a Transact-SQL Job Step
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> [Azure SQL 数据库托管实例](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)目前支持大多数但并非所有 SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 数据库托管实例与 SQL Server 之间的 T-SQL 差异](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、[!INCLUDE[tsql](../../includes/tsql-md.md)] 或 SQL Server 管理对象，在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中创建用于执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本的 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]代理作业步骤。  
  
这些作业步骤脚本可以调用存储过程和扩展存储过程。 一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤可以包含多个批处理和嵌入的 GO 命令。 有关创建作业的详细信息，请参阅 [创建作业](../../ssms/agent/create-jobs.md)。  
  
**本主题内容**  
  
-   **开始之前：**  
  
    [安全性](#Security)  
  
-   **若要创建 Transact-SQL 作业步骤，可使用：**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server 管理对象](#SMO)  
  
## <a name="BeforeYouBegin"></a>开始之前  
  
### <a name="Security"></a>安全性  
有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-a-transact-sql-job-step"></a>创建 Transact-SQL 作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  展开 **“SQL Server 代理”** ，创建一个新作业或右键单击一个现有作业，再单击 **“属性”** 。  
  
3.  在 **“作业属性”** 对话框中，单击 **“步骤”** 页，再单击 **“新建”** 。  
  
4.  在 **“新建作业步骤”** 对话框中，键入作业的 **“步骤名称”** 。  
  
5.  在“类型”  列表中，单击“Transact-SQL 脚本 (TSQL)”  。  
  
6.  在 **“命令”** 框中，键入 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批命令，或者单击 **“打开”** ，选择一个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 文件用作命令。  
  
7.  单击 **“分析”** 检查语法。  
  
8.  如果语法正确，将显示“分析成功”消息。 如果发现错误，更正语法后再继续。  
  
9. 单击 **“高级”** 页设置以下作业步骤选项，例如：当该作业步骤成功或失败时将执行的操作、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理应该尝试执行该作业步骤的次数，以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理将作业步骤输出写入哪个文件或表。 只有 **sysadmin** 固定服务器角色的成员才可以将作业步骤输出写入到操作系统文件中。 所有 SQL Server 代理用户都可以将输出写入表中。  
  
10. 如果您是 **sysadmin** 固定服务器角色的成员，并且希望以其他 SQL 登录身份运行此作业步骤，请从 **“作为以下用户运行”** 列表中选择 SQL 登录名。  
  
## <a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-create-a-transact-sql-job-step"></a>创建 Transact-SQL 作业步骤  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    -- creates a job step that uses Transact-SQL  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_add_jobstep (Transact-SQL)](https://msdn.microsoft.com/97900032-523d-49d6-9865-2734fba1c755)。  
  
## <a name="SMO"></a>使用 SQL Server 管理对象  
**创建 Transact-SQL 作业步骤**  
  
通过使用所选编程语言（如 Visual Basic、Visual C# 或 PowerShell）来使用 **JobStep** 类。  
  
