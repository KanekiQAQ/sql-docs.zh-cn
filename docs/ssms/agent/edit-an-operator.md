---
title: 编辑操作员 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, operators
- modifying operators
- jobs [SQL Server Agent], operators
- operators (users) [Database Engine], modifying with Management Studio
ms.assetid: b2ba2168-ca0b-4b59-9007-4e1e4c30679e
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f02134cecf05f5f3bb6a79304972db737eb080dc
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68262443"
---
# <a name="edit-an-operator"></a>编辑运算符
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> [Azure SQL 数据库托管实例](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)目前支持大多数但并非所有 SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 数据库托管实例与 SQL Server 之间的 T-SQL 差异](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中对操作员能否接收通知进行编辑，并编辑他们的电子邮件地址、寻呼地址和 net send 地址。  
  
**本主题内容**  
  
-   **开始之前：**  
  
    [限制和局限](#Restrictions)  
  
    [安全性](#Security)  
  
-   **若要编辑操作员，可使用：**  
  
    [SQL Server Management Studio](#SSMSProcedure)  
  
    [Transact-SQL](#TsqlProcedure)  
  
## <a name="BeforeYouBegin"></a>开始之前  
  
### <a name="Restrictions"></a>限制和局限  
  
-   在 **的未来版本中，将从** 代理中删除寻呼程序和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] net send [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]选项。 请避免在新的开发工作中使用这些功能，并考虑修改当前使用这些功能的应用程序。  
  
-   请注意，若要向操作员发送电子邮件和寻呼通知，必须将 SQL Server 代理配置为使用数据库邮件。 有关详细信息，请参阅 [向操作员分配警报](assign-alerts-to-an-operator.md)。  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 为管理作业提供了一种图形化的简便方法，建议使用此方法来创建和管理作业基础结构。  
  
### <a name="Security"></a>安全性  
  
#### <a name="Permissions"></a>Permissions  
只有 **sysadmin** 固定服务器角色的成员才能编辑操作员。  
  
## <a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-edit-an-operator"></a>编辑操作员  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开包含要编辑的操作员的服务器。  
  
2.  单击加号以展开 **“SQL Server 代理”** 。  
  
3.  单击加号以展开 **“操作员”** 文件夹。  
  
4.  右键单击要编辑的操作员，然后选择“属性”  。  
  
    有关“_operator\_name_ 属性”  对话框包含的可用选项的详细信息，请参阅：  
  
    -   [操作员属性 - 新建操作员（“常规”页）](../../ssms/agent/operator-properties-new-operator-general-page.md)  
  
    -   [操作员属性 - 新建操作员（“通知”页）](../../ssms/agent/operator-properties-new-operator-notifications-page.md)  
  
    -   [操作员属性（“历史记录”页）](../../ssms/agent/operator-properties-history-page.md)  
  
5.  完成后，单击 **“确定”** 。  
  
## <a name="TsqlProcedure"></a>使用 Transact-SQL  
  
#### <a name="to-edit-an-operator"></a>编辑操作员  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    -- updates the operator status to enabled, and sets the days   
    -- (from Monday through Friday, from 8 A.M. through 5 P.M.)
    --  when the operator can be paged.   
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_update_operator   
        @name = N'François Ajenstat',  
        @enabled = 1,  
        @email_address = N'françoisa',  
        @pager_address = N'5551290AW@pager.Adventure-Works.com',  
        @weekday_pager_start_time = 080000,  
        @weekday_pager_end_time = 170000,  
        @pager_days = 64 ;  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_update_operator (Transact-SQL)](https://msdn.microsoft.com/231750a6-4828-4d03-afe6-b91d38c42ed3)。  
  
