---
title: 查看作业活动 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- viewing job activity
- jobs [SQL Server Agent], viewing
- SQL Server Agent jobs, viewing
- displaying job activity
ms.assetid: 5c284e5e-7775-435d-ac49-f3f12a27ddc7
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ad5825e8c3e04803942827d98b9905e34f5c529c
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68260782"
---
# <a name="view-job-activity"></a>View Job Activity
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> [Azure SQL 数据库托管实例](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)目前支持大多数但并非所有 SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 数据库托管实例与 SQL Server 之间的 T-SQL 差异](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍了如何使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中查看 [!INCLUDE[tsql](../../includes/tsql-md.md)]代理作业的运行时状态。  
  
[!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务启动后，将创建一个新的会话，并且 **sysjobactivity** 数据库的 **sysjobactivity** 表由所有现有的已定义作业填充。 此表记录当前作业活动和状态。 您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理中的作业活动监视器查看作业的当前状态。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务意外终止，您可以查看 **sysjobactivity** 表以查明服务终止时正在执行哪些作业。  
  
**本主题内容**  
  
-   **开始之前：**  
  
    [安全性](#Security)  
  
-   **若要查看作业活动，请使用：**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
## <a name="before-you-begin"></a>开始之前  
  
### <a name="Security"></a>Security  
有关详细信息，请参阅[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)。  
  
## <a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-view-job-activity"></a>查看作业活动  
  
1.  在 **对象资源管理器**中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的实例，然后展开该实例。  
  
2.  展开 **“SQL Server 代理”** 。  
  
3.  右键单击“作业活动监视器”  ，然后单击“查看作业活动”  。  
  
4.  在 **作业活动监视器**中，可以查看为此服务器定义的每个作业的详细信息。  
  
5.  右键单击一个作业以启动、停止、启用或禁用该作业，按照作业活动监视器中的显示刷新状态，删除该作业，或者查看其历史记录或属性。  若要启动、停止、启用、禁用或刷新多个作业，请在作业活动监视器中选择多个行，然后右键单击所选内容。  
  
6.  若要更新作业活动监视器，请单击 **“刷新”** 。 若要查看较少的行，请单击 **“筛选”** ，然后输入筛选参数。  
  
## <a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-view-job-activity"></a>查看作业活动  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    -- lists activity for all jobs that the current user has permission to view.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_help_jobactivity ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_help_jobactivity (Transact-SQL)](https://msdn.microsoft.com/d344864f-b4d3-46b1-8933-b81dec71f511)。  
  
