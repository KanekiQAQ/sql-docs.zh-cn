---
title: 创建操作员 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, operators
- jobs [SQL Server Agent], notification options
- operators (users) [Database Engine], creating with Management Studio
- SQL Server Agent jobs, notification options
- jobs [SQL Server Agent], operators
- notifications [SQL Server], job status
ms.assetid: 1359d790-5905-4927-a208-e7155e7768a2
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 666e3925d9b9272951442c4a8e7f89968b7ded16
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68267268"
---
# <a name="create-an-operator"></a>创建操作员
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> [Azure SQL 数据库托管实例](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)目前支持大多数但并非所有 SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 数据库托管实例与 SQL Server 之间的 T-SQL 差异](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题介绍了如何通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中对用户进行配置以接收有关 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的通知。  
  
**本主题内容**  
  
-   **开始之前：**  
  
    [限制和局限](#Restrictions)  
  
    [安全性](#Security)  
  
-   **若要创建操作员，请使用：**  
  
    [SQL Server Management Studio](#SSMSProcedure)  
  
    [Transact-SQL](#TsqlProcedure)  
  
## <a name="BeforeYouBegin"></a>开始之前  
  
### <a name="Restrictions"></a>限制和局限  
  
-   在 **的未来版本中，将从** 代理中删除寻呼程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] net send [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]选项。 请避免在新的开发工作中使用这些功能，并考虑修改当前使用这些功能的应用程序。  
  
-   请注意，若要向操作员发送电子邮件和寻呼通知，必须将 SQL Server 代理配置为使用数据库邮件。 有关详细信息，请参阅 [向操作员分配警报](assign-alerts-to-an-operator.md)。  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 为管理作业提供了一种图形化的简便方法，建议使用此方法来创建和管理作业基础结构。  
  
### <a name="Security"></a>安全性  
  
#### <a name="Permissions"></a>Permissions  
只有 **sysadmin** 固定服务器角色的成员才能创建操作员。  
  
## <a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-an-operator"></a>创建操作员  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开要创建 SQL Server 代理操作员的服务器。  
  
2.  单击加号以展开 **“SQL Server 代理”** 。  
  
3.  右键单击“操作员”  文件夹，然后选择“新建操作员”  。  
  
    在 **“新建操作员”** 对话框的 **“常规”** 页上提供以下选项：  
  
    **名称**  
    更改操作员的名称。  
  
    **已启用**  
    启用操作员。 在未启用时，不会向操作员发送通知。  
  
    **电子邮件名称**  
    指定操作员的电子邮件地址。  
  
    **Net send 地址**  
    指定用于 **net send**的地址。  
  
    **寻呼电子邮件名称**  
    指定用于操作员的寻呼程序的电子邮件地址。  
  
    **寻呼值班计划**  
    设置寻呼程序处于活动状态的时间。  
  
    **星期一 - 星期日**  
    选择寻呼程序在一周中的哪些天处于活动状态。  
  
    **工作日开始**  
    选择一天之中的特定时间， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在该时间之后才可向寻呼程序发送消息。  
  
    **工作日结束**  
    选择一天之中的特定时间， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在该时间之后不再向寻呼程序发送消息。  
  
    在 **“新建操作员”** 对话框的 **“通知”** 页上提供以下选项：  
  
    **警报**  
    查看实例中的警报。  
  
    **作业**  
    查看实例中的作业。  
  
    **警报列表**  
    列出实例中的警报。  
  
    **作业列表**  
    列出实例中的作业。  
  
    **电子邮件**  
    使用电子邮件通知此操作员。  
  
    **寻呼程序**  
    通过将电子邮件发送到寻呼地址来通知此操作员。  
  
    **Net send**  
    使用 **net send**通知此操作员。  
  
4.  在完成了新操作员的创建后，单击 **“确定”** 。  
  
## <a name="TsqlProcedure"></a>使用 Transact-SQL  
  
#### <a name="to-create-an-operator"></a>创建操作员  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    -- sets up the operator information for user 'danwi.'
    -- The operator is enabled.   
    -- SQL Server Agent sends notifications by pager 
    -- from Monday through Friday from 8 A.M. to 5 P.M.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_operator  
        @name = N'Dan Wilson',  
        @enabled = 1,  
        @email_address = N'danwi',  
        @pager_address = N'5551290AW@pager.Adventure-Works.com',  
        @weekday_pager_start_time = 080000,  
        @weekday_pager_end_time = 170000,  
        @pager_days = 62 ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_add_operator (Transact-SQL)](https://msdn.microsoft.com/817cd98a-4dff-4ed8-a546-f336c144d1e0)。  
  
