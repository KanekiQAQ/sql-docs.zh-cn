---
title: managed_backup.fn_get_health_status (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_get_health_status_TSQL
- smart_admin.fn_get_health_status_TSQL
- smart_admin.fn_get_health_status
- fn_get_health_status
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_get_health_status
- fn_get_health_status
ms.assetid: b376711d-444a-4b5e-b483-8df323b4e31f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b921846b0fc27e59ff0874cdbf0827095bfc7db4
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68140662"
---
# <a name="managedbackupfngethealthstatus-transact-sql"></a>managed_backup.fn_get_health_status (TRANSACT-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  返回 0 行、一行或多行的表，行中是扩展事件在指定的一段时间内报告的错误总数。  
  
 该函数用于报告智能管理下的服务的运行状况状态当前[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]下，智能管理涵盖性支持。 因此，返回的错误与 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 相关。  
  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql  
managed_backup.fn_get_health_status([@begin_time = ] 'time_1' , [ @end_time = ] 'time_2')  
```  
  
##  <a name="Arguments"></a> 参数  
 [@begin_time]  
 计算错误总数的起始时间。  @begin_time参数是 DATETIME。 默认值为 NULL。 当值为 NULL 时，此函数将处理当前时间最长 30 分钟之前报告的事件。  
  
 [ @end_time]  
 计算错误总数的结束时间。 @end_time参数是 DATETIME，默认值为 NULL。 当值为空 NULL 时，此函数将处理截至当前时间的扩展事件。  
  
## <a name="table-returned"></a>返回的表  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|number_of_storage_connectivity_errors|INT|当程序连接到 Windows Azure 存储帐户时的连接错误数。|  
|number_of_sql_errors|INT|当程序连接到 SQL Server Engine 时返回的错误数。|  
|number_of_invalid_credential_errors|INT|当程序尝试使用 SQL 凭据进行身份验证时返回的错误数。|  
|number_of_other_errors|INT|连接、SQL 或凭据以外其他类别中的错误数。|  
|number_of_corrupted_or_deleted_backups|INT|删除或损坏的备份文件数。|  
|number_of_backup_loops|INT|备份代理扫描所有配置了 [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] 的数据库的次数。|  
|number_of_retention_loops|INT|扫描数据库以评估所设置的保持期的次数。|  
  
## <a name="best-practices"></a>最佳实践  
 这些聚合的计数可用于监视系统运行状况。 例如，如果 number_of_retention_loops 列在 30 分钟内为 0，则保持管理可能占用较长时间，甚至没有正常工作。 非零错误列可能表示有问题，应检查扩展事件日志以了解任何问题。 或者，使用存储的过程**managed_backup.sp_get_backup_diagnostics**来获取扩展事件来查找错误的详细信息的列表。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 需要**选择**函数上的权限。  
  
## <a name="examples"></a>示例  
  
-   下例返回其执行之前 30 分钟内的错误总数。  
  
    ```  
    SELECT *  
    FROM managed_backup.fn_get_health_status(NULL, NULL)  
  
    ```  
  
-   下例返回本周的错误总数：  
  
    ```  
    Use msdb  
    Go  
    DECLARE @startofweek datetime  
    DECLARE @endofweek datetime  
    SET @startofweek = DATEADD(Day, 1-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)   
    SET @endofweek = DATEADD(Day, 7-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)  
    SELECT *  
    FROM managed_backup.fn_get_health_status(@startofweek, @endofweek)  
  
    ```  
  
  
