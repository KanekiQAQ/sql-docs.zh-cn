---
title: sp_help_job (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_help_job_TSQL
- sp_help_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_job
ms.assetid: 8a8b6104-e0e4-4d07-a2c3-f4243ee0d6fa
author: stevestein
ms.author: sstein
ms.openlocfilehash: a6c2929062451d139cc3452b6bd272dd85bac951
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68054990"
---
# <a name="sphelpjob-transact-sql"></a>sp_help_job (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  返回有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理用来在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中执行自动活动的作业的信息。  
  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_job { [ @job_id = ] job_id  
[ @job_name = ] 'job_name' }   
     [ , [ @job_aspect = ] 'job_aspect' ]   
     [ , [ @job_type = ] 'job_type' ]   
     [ , [ @owner_login_name = ] 'login_name' ]   
     [ , [ @subsystem = ] 'subsystem' ]   
     [ , [ @category_name = ] 'category' ]   
     [ , [ @enabled = ] enabled ]   
     [ , [ @execution_status = ] status ]   
     [ , [ @date_comparator = ] 'date_comparison' ]   
     [ , [ @date_created = ] date_created ]   
     [ , [ @date_last_modified = ] date_modified ]   
     [ , [ @description = ] 'description_pattern' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @job_id = ] job_id` 作业标识号。 *job_id*是**uniqueidentifier**，默认值为 NULL。  
  
`[ @job_name = ] 'job_name'` 作业的名称。 *job_name*是**sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  若要查看特定作业，或者*job_id*或*job_name*必须指定。  同时省略*job_id*并*job_name*返回有关所有作业的信息。
  
`[ @job_aspect = ] 'job_aspect'` 要显示的作业属性。 *job_aspect*是**varchar(9)** ，默认值为 NULL，并且可以是下列值之一。  
  
|值|描述|  
|-----------|-----------------|  
|**ALL**|作业特征信息|  
|**JOB**|作业信息|  
|**计划**|计划信息|  
|**STEPS**|作业步骤信息|  
|**目标**|目标信息|  
  
`[ @job_type = ] 'job_type'` 要在报表中包括的作业类型。 *job_type*是**varchar(12)** ，默认值为 NULL。 *job_type*可以是**本地**或**多服务器**。  
  
`[ @owner_login_name = ] 'login_name'` 作业的所有者的登录名。 *login_name*是**sysname**，默认值为 NULL。  
  
`[ @subsystem = ] 'subsystem'` 子系统的名称。 *子系统*是**nvarchar(40)** ，默认值为 NULL。  
  
`[ @category_name = ] 'category'` 类别的名称。 *类别*是**sysname**，默认值为 NULL。  
  
`[ @enabled = ] enabled` 一个数字，指示是否显示信息启用作业还是为禁用的作业。 *已启用*是**tinyint**，默认值为 NULL。 **1**指示已启用的作业，并**0**指示已禁用的作业。  
  
`[ @execution_status = ] status` 作业的执行状态。 *状态*是**int**，默认值为 NULL，并且可以是下列值之一。  
  
|ReplTest1|Description|  
|-----------|-----------------|  
|**0**|只返回那些空闲的或挂起的作业。|  
|**1**|正在执行。|  
|**2**|正在等待线程。|  
|**3**|在两次重试之间。|  
|**4**|空闲。|  
|**5**|挂起。|  
|**7**|正在执行完成操作。|  
  
`[ @date_comparator = ] 'date_comparison'` 要在比较中使用的比较运算符*date_created*并*date_modified*。 *date_comparison*是**char （1)** ，可以为 = \<，或 >。  
  
`[ @date_created = ] date_created` 创建作业的日期。 *date_created*是**datetime**，默认值为 NULL。  
  
`[ @date_last_modified = ] date_modified` 上次修改作业的日期。 *date_modified*是**datetime**，默认值为 NULL。  
  
`[ @description = ] 'description_pattern'` 作业的说明。 *description_pattern*是**nvarchar(512)** ，默认值为 NULL。 *description_pattern*可以包括模式匹配的 SQL Server 通配符字符。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="result-sets"></a>结果集  
 如果未不指定任何参数， **sp_help_job**返回以下结果集。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|作业的唯一 ID。|  
|**originating_server**|**nvarchar(30)**|作业来自的服务器的名称。|  
|**name**|**sysname**|作业的名称。|  
|**enabled**|**tinyint**|指示是否启用要执行的作业。|  
|**description**|**nvarchar(512)**|对作业的说明。|  
|**start_step_id**|**int**|执行作业的起始步骤的 ID。|  
|**category**|**sysname**|作业类别。|  
|**owner**|**sysname**|作业所有者。|  
|**notify_level_eventlog**|**int**|**位掩码**，该值指示应在什么情况下在 Microsoft Windows 应用程序日志中记录通知事件。 可以是下列值之一：<br /><br /> **0** = 从不<br /><br /> **1** = 当作业成功时<br /><br /> **2** = 当作业失败时<br /><br /> **3** = 当作业完成时 （而不考虑作业结果） 时|  
|**notify_level_email**|**int**|**位掩码**，该值指示在作业完成时应在什么情况下发送一个通知电子邮件。 可能的值均为适用于**notify_level_eventlog**。|  
|**notify_level_netsend**|**int**|**位掩码**，该值指示在作业完成时应在什么情况下发送网络消息。 可能的值均为适用于**notify_level_eventlog**。|  
|**notify_level_page**|**int**|**位掩码**，该值指示在作业完成时应在什么情况下发送一个页面。 可能的值均为适用于**notify_level_eventlog**。|  
|**notify_email_operator**|**sysname**|被通知的操作员的电子邮件名称。|  
|**notify_netsend_operator**|**sysname**|在发送网络消息时所使用的计算机或用户的名称。|  
|**notify_page_operator**|**sysname**|在发送寻呼时所使用的计算机或用户的名称。|  
|**delete_level**|**int**|**位掩码**，该值指示在何种情况下，作业完成时应删除该作业。 可能的值均为适用于**notify_level_eventlog**。|  
|**date_created**|**datetime**|作业的创建日期。|  
|**date_modified**|**datetime**|上次修改作业的日期。|  
|**version_number**|**int**|作业的版本（每次修改作业时都自动对其进行更新）。|  
|**last_run_date**|**int**|作业上一次开始执行的日期。|  
|**last_run_time**|**int**|作业上一次开始执行的时间。|  
|**last_run_outcome**|**int**|作业上一次运行时所得到的结果：<br /><br /> **0** = 失败<br /><br /> **1** = 成功<br /><br /> **3** = 已取消<br /><br /> **5** = 未知|  
|**next_run_date**|**int**|计划作业下一次运行的日期。|  
|**next_run_time**|**int**|计划作业下一次运行的时间。|  
|**next_run_schedule_id**|**int**|下一个运行的计划的标识号。|  
|**current_execution_status**|**int**|当前执行状态。|  
|**current_execution_step**|**sysname**|作业中当前的执行步骤。|  
|**current_retry_attempt**|**int**|如果作业正在运行，并且已经重试过该步骤，那么这就是当前的重试尝试。|  
|**has_step**|**int**|作业具有的作业步骤数。|  
|**has_schedule**|**int**|作业具有的作业计划数。|  
|**has_target**|**int**|作业具有的目标服务器数。|  
|**type**|**int**|作业的类型。<br /><br /> 1 = 本地作业。<br /><br /> **2** = 多服务器作业。<br /><br /> **0** = 作业有没有目标服务器。|  
  
 如果*job_id*或*job_name*指定，则**sp_help_job**返回作业步骤、 作业计划及作业目标服务器的这些额外的结果集。  
  
 下面是针对作业步骤的结果集。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**step_id**|**int**|步骤的唯一（是针对该作业的）标识符。|  
|**step_name**|**sysname**|步骤的名称。|  
|**subsystem**|**nvarchar(40)**|执行步骤命令的子系统。|  
|**command**|**nvarchar(3200)**|执行的命令。|  
|**flag**|**nvarchar(4000)**|**位掩码**的控制步骤行为的值。|  
|**cmdexec_success_code**|**int**|有关**CmdExec**步骤中，这是成功命令的进程退出代码。|  
|**on_success_action**|**nvarchar(4000)**|步骤成功时的操作：<br /><br /> **1** = 成功后退出。<br /><br /> **2** = 失败后退出。<br /><br /> **3** = 转到下一步。<br /><br /> **4** = 转至步骤。|  
|**on_success_step_id**|**int**|如果**on_success_action**是**4**，这表示要执行的下一个步骤。|  
|**on_fail_action**|**nvarchar(4000)**|步骤失败时所采取的操作。 值均为适用于**on_success_action**。|  
|**on_fail_step_id**|**int**|如果**on_fail_action**是**4**，这表示要执行的下一个步骤。|  
|服务器 |**sysname**|保留。|  
|**database_name**|**sysname**|对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 步骤，这是将在其中执行命令的数据库。|  
|**database_user_name**|**sysname**|对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 步骤，这是命令执行时所在的数据库用户上下文。|  
|**retry_attempts**|**int**|在认定步骤已经失败之前，应该对命令进行重试的最大次数（如果命令没有成功）。|  
|**retry_interval**|**int**|两次重试尝试之间的间隔（以分钟为单位）。|  
|**os_run_priority**|**varchar(4000)**|保留。|  
|**output_file_name**|**varchar(200)**|输出应写入到哪个命令文件 ([!INCLUDE[tsql](../../includes/tsql-md.md)]并**CmdExec**仅步骤)。|  
|**last_run_outcome**|**int**|步骤上一次运行的结果：<br /><br /> **0** = 失败<br /><br /> **1** = 成功<br /><br /> **3** = 已取消<br /><br /> **5** = 未知|  
|**last_run_duration**|**int**|步骤上一次运行的持续时间（以秒为单位）。|  
|**last_run_retries**|**int**|步骤上一次运行时，重试命令的次数。|  
|**last_run_date**|**int**|步骤上一次开始执行的日期。|  
|**last_run_time**|**int**|步骤上一次开始执行的时间。|  
|**proxy_id**|**int**|作业步骤的代理。|  
  
 下面是针对作业计划的结果集。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**schedule_id**|**int**|计划的标识符（对所有作业都是唯一的）。|  
|**schedule_name**|**sysname**|计划的名称（只对该作业是唯一的）。|  
|**enabled**|**int**|是否计划处于活动状态 (**1**) 或不 (**0**)。|  
|**freq_type**|**int**|表示何时执行作业的值：<br /><br /> **1** = 一次<br /><br /> **4** = 每日<br /><br /> **8** = 每周<br /><br /> **16** = 每月<br /><br /> **32** = 每月，相对于**freq_interval**<br /><br /> **64** = 时，运行**SQLServerAgent**服务启动。|  
|**freq_interval**|**int**|执行作业的天数。 值对的值取决于**freq_type**。 有关详细信息，请参阅[sp_add_schedule &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_subday_type**|**Int**|单位**freq_subday_interval**。 有关详细信息，请参阅[sp_add_schedule &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_subday_interval**|**int**|数**freq_subday_type**周期每次执行作业之间。 有关详细信息，请参阅[sp_add_schedule &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_relative_interval**|**int**|计划作业的匹配项**freq_interval**中每个月。 有关详细信息，请参阅[sp_add_schedule &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)|  
|**freq_recurrence_factor**|**int**|作业的已计划执行日期之间的间隔月数。|  
|**active_start_date**|**int**|开始执行作业的日期。|  
|**active_end_date**|**int**|结束执行作业的日期。|  
|**active_start_time**|**int**|在开始执行作业时间**active_start_date。**|  
|**active_end_time**|**int**|结束执行上的作业的时间**active_end_date**。|  
|**date_created**|**datetime**|创建计划的日期。|  
|**schedule_description**|**nvarchar(4000)**|对计划的英语说明（如果需要的话）。|  
|**next_run_date**|**int**|计划下一次引发作业运行的日期。|  
|**next_run_time**|**int**|计划下一次引发作业运行的时间。|  
|**schedule_uid**|**uniqueidentifier**|计划的标识符。|  
|**job_count**|**int**|返回引用此计划的作业数。|  
  
 下面是针对作业目标服务器的结果集。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|目标服务器的标识符。|  
|**server_name**|**nvarchar(30)**|目标服务器的计算机名称。|  
|**enlist_date**|**datetime**|将目标服务器登记到主服务器的日期。|  
|**last_poll_date**|**datetime**|目标服务器上一次轮询主服务器的日期。|  
|**last_run_date**|**int**|作业上一次在此目标服务器上开始执行的日期。|  
|**last_run_time**|**int**|作业上一次在这个目标服务器上开始执行的时间。|  
|**last_run_duration**|**int**|作业上一次在这个目标服务器上运行的持续时间。|  
|**last_run_outcome**|**tinyint**|作业上一次在此服务器上运行的结果：<br /><br /> **0** = 失败<br /><br /> **1** = 成功<br /><br /> **3** = 已取消<br /><br /> **5** = 未知|  
|**last_outcome_message**|**nvarchar(1024)**|作业上一次在这个目标服务器上运行时的结果消息。|  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
 成员**SQLAgentUserRole**只能查看他们所拥有的作业。 成员**sysadmin**， **SQLAgentReaderRole**，并**SQLAgentOperatorRole**可以查看所有本地和多服务器作业。  
  
## <a name="examples"></a>示例  
  
### <a name="a-list-information-for-all-jobs"></a>A. 列出所有作业的信息  
 以下示例执行不带参数的 `sp_help_job` 过程，从而为 `msdb` 数据库中当前定义的所有作业返回信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_job ;  
GO  
```  
  
### <a name="b-listing-information-for-jobs-matching-a-specific-criteria"></a>B. 列出与特定条件相匹配的作业的信息  
 以下示例列出属于 `françoisa`（在其中启用和执行作业）的多服务器作业的作业信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_job   
   @job_type = N'MULTI-SERVER',  
   @owner_login_name = N'françoisa',  
   @enabled = 1,  
   @execution_status = 1 ;  
GO  
```  
  
### <a name="c-listing-all-aspects-of-information-for-a-job"></a>C. 列出作业的各个方面的信息  
 以下示例列出 `NightlyBackups` 作业的各个方面的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_job  
    @job_name = N'NightlyBackups',  
    @job_aspect = N'ALL' ;  
GO  
```  
  
## <a name="see-also"></a>请参阅  
 [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)   
 [sp_delete_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [sp_update_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
