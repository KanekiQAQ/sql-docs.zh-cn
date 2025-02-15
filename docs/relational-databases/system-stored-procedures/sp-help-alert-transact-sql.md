---
title: sp_help_alert (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_help_alert
- sp_help_alert_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_alert
ms.assetid: 850cef4e-6348-4439-8e79-fd1bca712091
author: stevestein
ms.author: sstein
ms.openlocfilehash: 39d0c2f6e17f51928de561820f33bc0c34d89a62
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68055242"
---
# <a name="sphelpalert-transact-sql"></a>sp_help_alert (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  报告有关为服务器定义的警报的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_alert [ [ @alert_name = ] 'alert_name' ]   
     [ , [ @order_by = ] 'order_by' ]   
     [ , [ @alert_id = ] alert_id ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @legacy_format = ] legacy_format ]  
```  
  
## <a name="arguments"></a>参数  
`[ @alert_name = ] 'alert_name'` 警报的名称。 *alert_name*是**nvarchar （128)** 。 如果*alert_name*是未指定，则返回有关所有警报的信息。  
  
`[ @order_by = ] 'order_by'` 要用于生成的结果排序顺序。 *order_by*是**sysname**，默认值为 N '*名称*。  
  
`[ @alert_id = ] alert_id` 报告有关信息警报的标识号。 *alert_id*是**int**，默认值为 NULL。  
  
`[ @category_name = ] 'category'` 警报类别。 *类别*是**sysname**，默认值为 NULL。  
  
`[ @legacy_format = ] legacy_format` 要生成早期结果集。 *legacy_format*是**位**，默认值为**0**。 当*legacy_format*是**1**， **sp_help_alert**返回的结果集将返回**sp_help_alert**在 Microsoft SQL Server 2000。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="result-sets"></a>结果集  
 当 **@legacy_format** 是**0**， **sp_help_alert**生成以下结果集。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**id**|**int**|系统分配的唯一整数标识符。|  
|**名称**|**sysname**|警报名称 (例如，Demo:完整**msdb**日志)。|  
|**event_source**|**nvarchar(100)**|事件源。 将始终**MSSQLServer**有关[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 版|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|定义警报的消息错误号。 (通常与中的错误号相对应**sysmessages**表)。 如果使用严重性定义警报， **message_id**是**0**或为 NULL。|  
|severity |**int**|严重级别 (从**9**通过**25**， **110**， **120**， **130**，或**140**)它定义警报。|  
|**enabled**|**tinyint**|警报当前是否已启用的状态 (**1**) 或不 (**0**)。 不发送未启用的警报。|  
|**delay_between_responses**|**int**|警报两次响应之间的等待间隔（秒）。|  
|**last_occurrence_date**|**int**|上次出现警报的日期。|  
|**last_occurrence_time**|**int**|上次出现警报的时间。|  
|**last_response_date**|**int**|响应警报的最后一个日期**SQLServerAgent**服务。|  
|**last_response_time**|**int**|响应的时间警报上次**SQLServerAgent**服务。|  
|**notification_message**|**nvarchar(512)**|作为电子邮件或寻呼通知的一部分发送给操作员的其他可选消息。|  
|**include_event_description**|**tinyint**|Microsoft Windows 应用程序日志中的 SQL Server 错误说明是否应成为通知消息的一部分。|  
|**database_name**|**sysname**|必须出现错误才能使警报得以激发的数据库。 如果数据库名称为 NULL，那么无论错误出现在哪里都激发警报。|  
|**event_description_keyword**|**nvarchar(100)**|Windows 应用程序日志中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误的说明，该说明必须类似于所提供的字符序列。|  
|**occurrence_count**|**int**|警报出现的次数。|  
|**count_reset_date**|**int**|日期**occurrence_count**上次重置。|  
|**count_reset_time**|**int**|时间**occurrence_count**上次重置。|  
|**job_id**|**uniqueidentifier**|为了响应警报而执行的作业的标识号。|  
|**job_name**|**sysname**|为了响应警报而执行的作业的名称。|  
|**has_notification**|**int**|如果将这个警报通知给一个或多个操作员，则为非零。 值为一个或多个以下值 （或运算组合在一起）：<br /><br /> **1**= 有电子邮件通知<br /><br /> **2**= 有寻呼通知<br /><br /> **4**= 已**网络发送**通知。|  
|**flag**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**performance_condition**|**nvarchar(512)**|如果**类型**是**2**，此列显示性能条件的定义; 否则，该列为 NULL。|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0，将始终为“[Uncategorized]”。|  
|**wmi_namespace**|**sysname**|如果**类型**是**3**，此列显示 WMI 事件的命名空间。|  
|**wmi_query**|**nvarchar(512)**|如果**类型**是**3**，此列显示 WMI 事件查询。|  
|**type**|**int**|事件类型：<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]事件警报<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]性能警报<br /><br /> **3** = WMI 事件警报|  
  
 当 **@legacy_format** 是**1**， **sp_help_alert**生成以下结果集。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**id**|**int**|系统分配的唯一整数标识符。|  
|**名称**|**sysname**|警报名称 (例如，Demo:完整**msdb**日志)。|  
|**event_source**|**nvarchar(100)**|事件源。 将始终**MSSQLServer**为[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]7.0 版|  
|**event_category_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**event_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**message_id**|**int**|定义警报的消息错误号。 (通常与中的错误号相对应**sysmessages**表)。 如果使用严重性定义警报， **message_id**是**0**或为 NULL。|  
|severity |**int**|严重级别 (从**9**通过**25**， **110**， **120**， **130**，或 1**40**)它定义警报。|  
|**enabled**|**tinyint**|警报当前是否已启用的状态 (**1**) 或不 (**0**)。 不发送未启用的警报。|  
|**delay_between_responses**|**int**|警报两次响应之间的等待间隔（秒）。|  
|**last_occurrence_date**|**int**|上次出现警报的日期。|  
|**last_occurrence_time**|**int**|上次出现警报的时间。|  
|**last_response_date**|**int**|响应警报的最后一个日期**SQLServerAgent**服务。|  
|**last_response_time**|**int**|响应的时间警报上次**SQLServerAgent**服务。|  
|**notification_message**|**nvarchar(512)**|作为电子邮件或寻呼通知的一部分发送给操作员的其他可选消息。|  
|**include_event_description**|**tinyint**|Windows 应用程序日志中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误的说明是否应包括为通知消息的一部分。|  
|**database_name**|**sysname**|必须出现错误才能使警报得以激发的数据库。 如果数据库名称为 NULL，那么无论错误出现在哪里都激发警报。|  
|**event_description_keyword**|**nvarchar(100)**|Windows 应用程序日志中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误的说明，该说明必须类似于所提供的字符序列。|  
|**occurrence_count**|**int**|警报出现的次数。|  
|**count_reset_date**|**int**|日期**occurrence_count**上次重置。|  
|**count_reset_time**|**int**|时间**occurrence_count**上次重置。|  
|**job_id**|**uniqueidentifier**|作业标识号。|  
|**job_name**|**sysname**|为了响应警报而执行的按需作业。|  
|**has_notification**|**int**|如果将这个警报通知给一个或多个操作员，则为非零。 该值是下列值中的一个或多个（用 OR 连起来）：<br /><br /> **1**= 有电子邮件通知<br /><br /> **2**= 有寻呼通知<br /><br /> **4**= 已**网络发送**通知。|  
|**flag**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]。|  
|**performance_condition**|**nvarchar(512)**|如果**类型**是**2**，此列显示性能条件的定义。 如果**类型**是**3**，此列显示 WMI 事件查询。 否则，该列为 NULL。|  
|**category_name**|**sysname**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] 将始终为 **[未分类]** 为[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]7.0。|  
|**type**|**int**|警报类型：<br /><br /> **1**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]事件警报<br /><br /> **2**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]性能警报<br /><br /> **3** = WMI 事件警报|  
  
## <a name="remarks"></a>备注  
 **sp_help_alert**必须从运行**msdb**数据库。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 **msdb** 数据库中的 **SQLAgentOperatorRole** 固定数据库角色的权限。  
  
 有关详细信息**SQLAgentOperatorRole**，请参阅[SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
## <a name="examples"></a>示例  
 以下示例报告有关 `Demo: Sev. 25 Errors` 警报的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC sp_help_alert @alert_name = 'Demo: Sev. 25 Errors';  
GO  
```  
  
## <a name="see-also"></a>请参阅  
 [sp_add_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sp_update_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-alert-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
