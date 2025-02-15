---
title: sp_add_schedule (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_add_schedule_TSQL
- sp_add_schedule
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_schedule
ms.assetid: 9060aae3-3ddd-40a5-83bb-3ea7ab1ffbd7
author: stevestein
ms.author: sstein
ms.openlocfilehash: 438fe71bcc32c63f97aea95c7105399c2ff8a479
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68088508"
---
# <a name="spaddschedule-transact-sql"></a>sp_add_schedule (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  创建一个可由任意数量的作业使用的计划。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_add_schedule [ @schedule_name = ] 'schedule_name'   
    [ , [ @enabled = ] enabled ]  
    [ , [ @freq_type = ] freq_type ]  
    [ , [ @freq_interval = ] freq_interval ]   
    [ , [ @freq_subday_type = ] freq_subday_type ]   
    [ , [ @freq_subday_interval = ] freq_subday_interval ]   
    [ , [ @freq_relative_interval = ] freq_relative_interval ]   
    [ , [ @freq_recurrence_factor = ] freq_recurrence_factor ]   
    [ , [ @active_start_date = ] active_start_date ]   
    [ , [ @active_end_date = ] active_end_date ]   
    [ , [ @active_start_time = ] active_start_time ]   
    [ , [ @active_end_time = ] active_end_time ]   
    [ , [ @owner_login_name = ] 'owner_login_name' ]  
    [ , [ @schedule_uid = ] schedule_uid OUTPUT ]  
    [ , [ @schedule_id = ] schedule_id OUTPUT ]  
    [ , [ @originating_server = ] server_name ] /* internal */  
```  
  
## <a name="arguments"></a>参数  
`[ @schedule_name = ] 'schedule_name'` 计划的名称。 *schedule_name*是**sysname**，无默认值。  
  
`[ @enabled = ] enabled` 指示计划的当前状态。 *已启用*是**tinyint**，默认值为**1** （启用）。 如果**0**，不启用计划。 如果不启用计划，则作业不会按此计划运行。  
  
`[ @freq_type = ] freq_type` 指示作业时要执行的值。 *freq_type*是**int**，默认值为**0**，可以是下列值之一。  
  
|ReplTest1|Description|  
|-----------|-----------------|  
|**1**|一次|  
|**4**|每天|  
|**8**|每周|  
|**16**|每月|  
|**32**|每月，相对于*freq_interval*|  
|**64**|SQLServerAgent 服务启动时运行|  
|**128**|计算机空闲时运行|  
  
`[ @freq_interval = ] freq_interval` 执行作业的天数。 *freq_interval*是**int**，默认值为**1**，并取决于值*freq_type*。  
  
|值*freq_type*|在影响*freq_interval*|  
|---------------------------|--------------------------------|  
|**1** （一次）|*freq_interval*是未使用。|  
|**4** （每日）|每个*freq_interval*天。|  
|**8** （每周）|*freq_interval*是一个或多个以下 （与 OR 逻辑运算符结合使用）：<br /><br /> **1** = 星期日<br /><br /> **2** = 星期一<br /><br /> **4** = 星期二<br /><br /> **8** = 星期三<br /><br /> **16** = 星期四<br /><br /> **32** = 星期五<br /><br /> **64** = 星期六|  
|**16** （每月）|上*freq_interval*天的月份。|  
|**32** （每月相对）|*freq_interval*是以下之一：<br /><br /> **1** = 星期日<br /><br /> **2** = 星期一<br /><br /> **3** = 星期二<br /><br /> **4** = 星期三<br /><br /> **5** = 星期四<br /><br /> **6** = 星期五<br /><br /> **7** = 星期六<br /><br /> **8** = 天<br /><br /> **9** = 工作日<br /><br /> **10** = 休息日|  
|**64** （SQLServerAgent 服务启动时）|*freq_interval*是未使用。|  
|**128**|*freq_interval*是未使用。|  
  
`[ @freq_subday_type = ] freq_subday_type` 指定的单位*freq_subday_interval*。 *freq_subday_type*是**int**，默认值为**0**，可以是下列值之一。  
  
|ReplTest1|说明（单位）|  
|-----------|--------------------------|  
|**0x1**|在指定的时间|  
|**0x2**|Seconds|  
|**0x4**|Minutes|  
|**0x8**|Hours|  
  
`[ @freq_subday_interval = ] freq_subday_interval` 数*freq_subday_type*周期每次执行作业之间。 *freq_subday_interval*是**int**，默认值为**0**。 注意:间隔应大于 10 秒。 *freq_subday_interval*在这些情况下忽略其中*freq_subday_type*等于**1**。  
  
`[ @freq_relative_interval = ] freq_relative_interval` 作业的匹配项*freq_interval*中每个月中，如果*freq_interval*为 32 （每月相对）。 *freq_relative_interval*是**int**，默认值为**0**，可以是下列值之一。 *freq_relative_interval*在这些情况下忽略其中*freq_type*不等于 32。  
  
|ReplTest1|说明（单位）|  
|-----------|--------------------------|  
|**1**|第一个|  
|**2**|第二个|  
|**4**|第三个|  
|**8**|第四个|  
|**16**|上一次|  
  
`[ @freq_recurrence_factor = ] freq_recurrence_factor` 周数或计划作业的执行之间的月数。 *freq_recurrence_factor*时才使用*freq_type*是**8**， **16**，或**32**。 *freq_recurrence_factor*是**int**，默认值为**0**。  
  
`[ @active_start_date = ] active_start_date` 可以开始执行作业的日期。 *active_start_date*是**int**，默认值为 NULL，表示今天的日期。 日期的格式为 YYYYMMDD。 如果*active_start_date*不为 NULL，日期必须大于或等于 19900101。  
  
 创建了计划后，请检查其开始日期，确认该日期是否正确。 详细信息，请参阅"计划开始日期"一节中[创建和计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)。  
  
 对于每周或每月时间表，如果活动开始日期是在过去，则代理将忽略该日期，改用当前日期。 在使用 sp_add_schedule 创建 SQL 代理计划时，有一个选项可用来指定作为作业执行开始日期的参数 active_start_date。 如果计划类型是每周或每月并且 active_start_date 参数设置为过去中的某个日期，则忽略该 active_start_date 参数并且将当前日期用于 active_start_date。  
  
`[ @active_end_date = ] active_end_date` 可以在其停止执行作业的日期。 *active_end_date*是**int**，默认值为**99991231**，这指示年 12 月 31 日到 9999。 其格式为 YYYYMMDD。  
  
`[ @active_start_time = ] active_start_time` 之间的任何日期时间*active_start_date*并*active_end_date*开始执行作业。 *active_start_time*是**int**，默认值为**000000**，指示上午 12:00:00 并且必须使用 HHMMSS 格式输入。  
  
`[ @active_end_time = ] active_end_time` 之间的任何日期时间*active_start_date*并*active_end_date*结束执行作业。 *active_end_time*是**int**，默认值为**235959**，指示 11:59:59 PM 并且必须使用 HHMMSS 格式输入。  
  
`[ @owner_login_name = ] 'owner_login_name'` 拥有计划的服务器主体的名称。 *owner_login_name*是**sysname**，默认值为 NULL，指示计划是否归创建者。  
  
`[ @schedule_uid = ] _schedule_uidOUTPUT` 计划的的唯一标识符。 *schedule_uid*类型的变量**uniqueidentifier**。  
  
`[ @schedule_id = ] _schedule_idOUTPUT` 计划的标识符。 *schedule_id*类型的变量**int**。  
  
`[ @originating_server = ] server_name` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 为管理作业提供了一种图形化的简便方法，建议使用此方法来创建和管理作业基础结构。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-schedule"></a>A. 创建计划  
 以下示例将创建一个名为 `RunOnce` 的计划： 此计划在创建当天的 `23:30` 运行一次。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_schedule  
    @schedule_name = N'RunOnce',  
    @freq_type = 1,  
    @active_start_time = 233000 ;  
  
GO  
```  
  
### <a name="b-creating-a-schedule-attaching-the-schedule-to-multiple-jobs"></a>B. 创建计划并将计划附加到多个作业  
 以下示例将创建一个名为 `NightlyJobs` 的计划： 使用此计划的作业每天在服务器上的时间为 `01:00` 时执行。 该示例将计划附加到作业 `BackupDatabase` 和作业 `RunReports`。  
  
> [!NOTE]  
>  此示例假定作业 `BackupDatabase` 和作业 `RunReports` 已存在。  
  
```  
USE msdb ;  
GO  
  
EXEC sp_add_schedule  
    @schedule_name = N'NightlyJobs' ,  
    @freq_type = 4,  
    @freq_interval = 1,  
    @active_start_time = 010000 ;  
GO  
  
EXEC sp_attach_schedule  
   @job_name = N'BackupDatabase',  
   @schedule_name = N'NightlyJobs' ;  
GO  
  
EXEC sp_attach_schedule  
   @job_name = N'RunReports',  
   @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
## <a name="see-also"></a>请参阅  
 [创建并将计划附加到作业](../../ssms/agent/create-and-attach-schedules-to-jobs.md)   
 [计划作业](../../ssms/agent/schedule-a-job.md)   
 [创建计划](../../ssms/agent/create-a-schedule.md)   
 [SQL Server 代理存储过程&#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_jobschedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md)   
 [sp_update_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
  
