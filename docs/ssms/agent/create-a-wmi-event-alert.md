---
title: 创建 WMI 事件警报 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- WMI event alerts [SQL Server Management Studio]
ms.assetid: b8c46db6-408b-484e-98f0-a8af3e7ec763
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 3e8ffcdb3f5a231560aa08d2c238f3a9020e1353
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68267309"
---
# <a name="create-a-wmi-event-alert"></a>创建 WMI 事件警报
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> [Azure SQL 数据库托管实例](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)目前支持大多数但并非所有 SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 数据库托管实例与 SQL Server 之间的 T-SQL 差异](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明如何使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中创建 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 代理警报，以便在出现由 WMI Provider for Server Events 监视的特定 [!INCLUDE[tsql](../../includes/tsql-md.md)]事件时引发警报。  
  
有关使用 WMI 提供程序监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件的详细信息，请参阅 [WMI Provider for Server Events 类和属性](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md)。 有关接收 WMI 事件警报通知的所需权限的信息，请参阅 [为 SQL Server 代理服务选择帐户](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)。 有关 WQL 的详细信息，请参阅 [将 WQL 与 WMI Provider for Server Events 结合使用](../../relational-databases/wmi-provider-server-events/using-wql-with-the-wmi-provider-for-server-events.md)。  
  
**本主题内容**  
  
-   **开始之前：**  
  
    [限制和局限](#Restrictions)  
  
    [安全性](#Security)  
  
-   **若要创建 WMI 事件警报，可使用：**  
  
    [SQL Server Management Studio](#SSMSProcedure)  
  
    [Transact-SQL](#TsqlProcedure)  
  
## <a name="BeforeYouBegin"></a>开始之前  
  
### <a name="Restrictions"></a>限制和局限  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供了一种易用的图形方式来管理整个警报系统，这也是配置警报基础结构的推荐方式。  
  
-   用 **xp_logevent** 生成的事件在 master 数据库中发生。 因此，除非警报的 **xp_logevent** 为 **@database_name** 或 NULL，否则 **@database_name** 不触发警报。  
  
-   仅支持运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的计算机上的 WMI 命名空间。  
  
### <a name="Security"></a>安全性  
  
#### <a name="Permissions"></a>Permissions  
默认情况下，只有 **sysadmin** 固定服务器角色的成员才能执行 **sp_add_alert**。  
  
## <a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-a-wmi-event-alert"></a>创建 WMI 事件警报  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开要创建 WMI 事件警报的服务器。  
  
2.  单击加号以展开 **“SQL Server 代理”** 。  
  
3.  右键单击“警报”  并选择“新建警报”  。  
  
4.  在 **“新建警报”** 对话框的 **“名称”** 框中，输入此警报的名称。  
  
5.  选中 **“启用”** 复选框将运行警报。 默认情况下， **“启用”** 为选中状态。  
  
6.  在 **“类型”** 列表中，选择 **“WMI 事件警报”** 。  
  
7.  在“WMI 事件警报定义”  下的“命名空间”  框中，为标识触发该警报的 WMI 事件的 WMI 查询语言 (WQL) 语句指定 WMI 命名空间。  
  
8.  在 **“查询”** 框中，指定标识该警报所响应事件的 WQL 语句。  
  
9. 单击“确定”  。  
  
## <a name="TsqlProcedure"></a>使用 Transact-SQL  
  
#### <a name="to-create-a-wmi-event-alert"></a>创建 WMI 事件警报  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    -- creates a WMI event alert that retrieves all event properties for any ALTER_TABLE event that occurs on table AdventureWorks2012.Sales.SalesOrderDetail  
    -- This example assumes that the message 54001 already exists.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_alert  
        @name = N'Test Alert 2',  
        @message_id = 54001  
        @notification_message = N'Error 54001 has occurred on the Sales.SalesOrderDetail table on the AdventureWorks2012 database.',  
        @wmi_namespace = '\\.\root\Microsoft\SqlServer\ServerEvents\,  
        @wmi_query = N'SELECT * FROM ALTER_TABLE   
    WHERE DatabaseName = 'AdventureWorks2012' AND SchemaName = 'Sales'   
        AND ObjectType='Table' AND ObjectName = 'SalesOrderDetail'';  
    GO  
    ```  
  
有关详细信息，请参阅 [sp_add_alert (Transact-SQL)](https://msdn.microsoft.com/d9b41853-e22d-4813-a79f-57efb4511f09)。  
  
