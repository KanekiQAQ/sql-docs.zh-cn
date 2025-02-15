---
title: 使用严重级别创建警报 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server], creating
- SQL Server Agent, alerts
- severity levels [SQL Server]
- alerts [SQL Server], severity levels
ms.assetid: a1fd71bf-5bf9-4ce2-9a1d-032576a4a6e9
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a87769e03e9661f9020b61ec4ea9a34b64084668
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68267287"
---
# <a name="create-an-alert-using-severity-level"></a>Create an Alert Using Severity Level
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> [Azure SQL 数据库托管实例](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance)目前支持大多数但并非所有 SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 数据库托管实例与 SQL Server 之间的 T-SQL 差异](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主题说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中创建一个将在发生特定严重级别的事件时引发的 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理警报。  
  
**本主题内容**  
  
-   **开始之前：**  
  
    [限制和局限](#Restrictions)  
  
    [Security](#Security)  
  
-   **若要使用严重级别创建警报，可使用：**  
  
    [SQL Server Management Studio](#SSMSProcedure)  
  
    [Transact-SQL](#TsqlProcedure)  
  
## <a name="BeforeYouBegin"></a>开始之前  
  
### <a name="Restrictions"></a>限制和局限  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供了一种易用的图形方式来管理整个警报系统，这也是配置警报基础结构的推荐方式。  
  
-   用 **xp_logevent** 生成的事件在 master 数据库中发生。 因此，除非警报的 **xp_logevent** 为 **@database_name** 或 NULL，否则 **@database_name** 不触发警报。  
  
-   如果严重级别在 19 到 25 之间，就会向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 应用程序日志发送 [!INCLUDE[msCoName](../../includes/msconame_md.md)] 消息，并触发一个警报。 对于严重级别小于 19 的事件，只有在使用 **sp_altermessage**、RAISERROR WITH LOG 或 **xp_logevent** 强制这些事件写入 Windows 应用程序日志时，才会触发警报。  
  
### <a name="Security"></a>安全性  
  
#### <a name="Permissions"></a>Permissions  
默认情况下，只有 **sysadmin** 固定服务器角色的成员才能执行 **sp_add_alert**。  
  
## <a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-an-alert-using-severity-level"></a>使用严重级别创建警报  
  
1.  在 **“对象资源管理器”** 中，单击加号以展开要使用严重级别创建警报的服务器。  
  
2.  单击加号以展开 **“SQL Server 代理”** 。  
  
3.  右键单击“警报”  并选择“新建警报”  。  
  
4.  在 **“新建警报”** 对话框的 **“名称”** 框中，输入此警报的名称。  
  
5.  在 **“类型”** 列表中，选择 **“SQL Server 事件警报”** 。  
  
6.  在 **“事件警报定义”** 下的 **“数据库名称”** 列表中，选择一个数据库以将警报限制到特定数据库。  
  
7.  在 **“将根据以下条件触发警报”** 下，单击 **“严重性”** ，然后选择将引发警报的特定严重性。  
  
8.  选中与 **“当消息包含以下内容时触发警报”** 复选框以将警报限制到特定的字符序列，然后在 **“消息正文”** 中输入关键字或字符串。 最大字符数为 100。  
  
9. 单击“确定”  。  
  
## <a name="TsqlProcedure"></a>使用 Transact-SQL  
  
#### <a name="to-create-an-alert-using-severity-level"></a>使用严重级别创建警报  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    -- Adds an alert (Test Alert) that notifies the
    -- Alert Operator via email when an error with a 
    -- severity of 23 is detected.
    
    -- Assumes that the Alert Operator already exists 
    -- and that database mail is configured.
    
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_alert @name=N'Test Alert', 
      @message_id = 0, 
      @severity = 23, 
      @enabled = 1, 
      @include_event_description_in = 1
    ;
    GO
    
    EXEC dbo.sp_add_notification @alert_name=N'Test Alert',
      @operator_name=N'Alert Operator',
      @notification_method=1
    ;
    GO

    ```  
  
有关详细信息，请参阅 [sp_add_alert (Transact-SQL)](https://msdn.microsoft.com/d9b41853-e22d-4813-a79f-57efb4511f09)。  
  
