---
title: 使用 AlwaysOn 策略查看可用性组 (SQL Server) 的运行状况 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 6f1bcbc3-1220-4071-8e53-4b957f5d3089
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 12a183718ee13915fd6236caf943fea03819e723
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62812970"
---
# <a name="use-alwayson-policies-to-view-the-health-of-an-availability-group-sql-server"></a>使用 AlwaysOn 策略查看可用性组的运行状况 (SQL Server)
  本主题说明在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 中如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]或 PowerShell 通过 AlwaysOn 策略确定 AlwaysOn 可用性组的运行状况。 有关 AlwaysOn 基于策略的管理的信息，请参阅[针对 AlwaysOn 可用性组 (SQL Server) 运行问题的 AlwaysOn 策略](always-on-policies-for-operational-issues-always-on-availability.md)。  
  
> [!IMPORTANT]  
>  对于 AlwaysOn 策略，类别名称用作 ID。 更改 AlwaysOn 类别的名称将会破坏其运行状况评价功能。 因此，任何时候都不应修改 AlwaysOn 类别的名称。  
  

  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Security"></a> 安全性  
  
####  <a name="Permissions"></a> Permissions  
 需要 CONNECT、VIEW SERVER STATE 和 VIEW ANY DEFINITION 权限。  
  
##  <a name="SSMSProcedure"></a> 使用 AlwaysOn 仪表板  
 **打开 AlwaysOn 仪表板**  
  
1.  在对象资源管理器中，连接到承载可用性副本之一的服务器实例。 若要查看有关可用性组中所有可用性副本的信息，请使用承载主副本的服务器实例。  
  
2.  单击服务器名称以展开服务器树。  
  
3.  展开 **“AlwaysOn 高可用性”** 节点。  
  
     右键单击“可用性组”节点或展开此节点，然后右键单击特定的可用性组。  
  
4.  选择 **“显示面板”** 命令。  
  
 有关如何使用 AlwaysOn 面板的信息，请参阅[使用 AlwaysOn 面板 (SQL Server Management Studio)](use-the-always-on-dashboard-sql-server-management-studio.md)。  
  
##  <a name="PowerShellProcedure"></a> 使用 PowerShell  
 **使用 AlwaysOn 策略查看可用性组的运行状况**  
  
1.  将默认值 (`cd`) 设置为承载其中一个可用性副本的服务器实例。 若要查看有关可用性组中所有可用性副本的信息，请使用承载主副本的服务器实例。  
  
2.  使用以下 cmdlet：  
  
     `Test-SqlAvailabilityGroup`  
     通过评估 SQL Server 基于策略的管理 (PBM) 策略来评估可用性组的运行状况。 您必须具有 CONNECT、VIEW SERVER STATE 和 VIEW ANY DEFINITION 权限才能执行此 cmdlet。  
  
     例如，以下命令显示服务器实例 `Computer\Instance`上运行状况状态为“Error”的所有可用性组。  
  
    ```powershell
    Get-ChildItem SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups |
        Test-SqlAvailabilityGroup |
        Where-Object { $_.HealthState -eq "Error" }  
    ```  
  
     `Test-SqlAvailabilityReplica`  
     通过评估 SQL Server 基于策略的管理 (PBM) 策略来评估可用性副本的运行状况。 您必须具有 CONNECT、VIEW SERVER STATE 和 VIEW ANY DEFINITION 权限才能执行此 cmdlet。  
  
     例如，以下命令评估可用性组 `MyReplica` 中名为 `MyAg` 的可用性副本的运行状况并输出简短摘要。  
  
    ```powershell
    Test-SqlAvailabilityReplica `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
     `Test-SqlDatabaseReplicaState`  
     通过评估 SQL Server 基于策略的管理 (PBM) 策略来评估所有联接的可用性副本的可用性数据库的运行状况。  
  
     例如，以下命令评估可用性组 `MyAg` 中所有可用性数据库的运行状况并输出各数据库的简短摘要。  
  
    ```powershell
    Get-ChildItem SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\DatabaseReplicaStates |
        Test-SqlDatabaseReplicaState  
    ```  
  
     这些 cmdlet 接受以下选项：  
  
    |Option|Description|  
    |------------|-----------------|  
    |`AllowUserPolicies`|运行在 AlwaysOn 策略类别中找到的用户策略。|  
    |`InputObject`|对象的集合，表示可用性组、可用性副本或可用性数据库状态（取决于正在使用的 cmdlet）。 此 cmdlet 将计算指定对象的运行状况。|  
    |`NoRefresh`|如果设置了此参数，cmdlet 将不会手动刷新由 `-Path` 或 `-InputObject` 参数指定的对象。|  
    |`Path`|指向可用性组、一个或多个可用性副本或可用性数据库的数据库副本群集状态的路径（具体取决于正在使用的 cmdlet）。 这是一个可选参数。 如果未指定，此参数的值默认为当前的工作位置。|  
    |`ShowPolicyDetails`|显示由 cmdlet 执行的每个策略评估的结果。 对于每个策略评估，cmdlet 都输出一个对象，该对象具有用于描述评估结果的字段（是否传递了策略、策略名称和类别等等）。|  
  
     例如，以下 `Test-SqlAvailabilityGroup` 命令指定 `-ShowPolicyDetails` 参数，以为在名为 `MyAg` 的可用性组上执行的每个基于策略的管理 (PBM) 策略显示此 cmdlet 执行的每个策略评估结果。  
  
    ```powershell
    Test-SqlAvailabilityGroup `   
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\AgName `  
    -ShowPolicyDetails  
    ```  
  
    > [!NOTE]  
    >  若要查看 cmdlet 的语法，请在 `Get-Help` PowerShell 环境中使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cmdlet。 有关详细信息，请参阅 [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)。  
  
 **设置和使用 SQL Server PowerShell 提供程序**  
  
-   [SQL Server PowerShell 提供程序](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
##  <a name="RelatedContent"></a> 相关内容  
 **SQL Server AlwaysOn 团队博客监视 AlwaysOn 运行状况使用 PowerShell:**  
  
-   [第 1 部分：Basic Cmdlet Overview](https://blogs.msdn.com/b/sqlalwayson/archive/2012/02/13/monitoring-alwayson-health-with-powershell-part-1.aspx)（使用 PowerShell 监视 Always On 运行状况 - 第 1 部分：基本 cmdlet 概述）  
  
-   [第 2 部分：Advanced Cmdlet Usage](https://blogs.msdn.com/b/sqlalwayson/archive/2012/02/13/monitoring-alwayson-health-with-powershell-part-2.aspx)（使用 PowerShell 监视 Always On 运行状况 - 第 2 部分：高级 cmdlet 用法）  
  
-   [第 3 部分：A Simple Monitoring Application](https://blogs.msdn.com/b/sqlalwayson/archive/2012/02/15/monitoring-alwayson-health-with-powershell-part-3.aspx)（使用 PowerShell 监视 Always On 运行状况 - 第 3 部分：简单的监视应用程序）  
  
-   [第 4 部分：Integration with SQL Server Agent](https://blogs.msdn.com/b/sqlalwayson/archive/2012/02/15/the-always-on-health-model-part-4.aspx)（使用 PowerShell 监视 Always On 运行状况 - 第 4 部分：与 SQL Server 代理集成）  
  
## <a name="see-also"></a>请参阅  
 [AlwaysOn 可用性组概述&#40;SQL Server&#41;](overview-of-always-on-availability-groups-sql-server.md)   
 [管理可用性组 (SQL Server)](administration-of-an-availability-group-sql-server.md)   
 [监视可用性组 (SQL Server)](monitoring-of-availability-groups-sql-server.md)   
 [针对运行问题的 AlwaysOn 可用性组 (SQL Server) 的 AlwaysOn 策略](always-on-policies-for-operational-issues-always-on-availability.md) 
  
  
