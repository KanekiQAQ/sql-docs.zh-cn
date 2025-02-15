---
title: sp_helpsubscriberinfo (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_helpsubscriberinfo
- sp_helpsubscriberinfo_TSQL
helpviewer_keywords:
- sp_helpsubscriberinfo
ms.assetid: fbabe1ec-57cf-425c-bae7-af7f5d3198fd
author: stevestein
ms.author: sstein
ms.openlocfilehash: ab6168730f8c2f5be3db951ed595db4d377b996d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68048368"
---
# <a name="sphelpsubscriberinfo-transact-sql"></a>sp_helpsubscriberinfo (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  显示有关订阅服务器的信息。 此存储过程在发布服务器的任何数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpsubscriberinfo [ [ @subscriber =] 'subscriber']  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @subscriber = ] 'subscriber'` 是订阅服务器的名称。 *订阅服务器上*是**sysname**，默认值为 **%** ，表示返回所有信息。  
  
`[ @publisher = ] 'publisher'` 是发布服务器的名称。 *发布服务器*是**sysname**，默认值为当前服务器的名称。  
  
> [!NOTE]  
>  *发布服务器*应未指定，除非它是 Oracle 发布服务器。  
  
## <a name="result-sets"></a>结果集  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**订阅服务器**|**sysname**|订阅服务器的名称。|  
|**type**|**tinyint**|订阅服务器的类型：<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]数据库**1** = ODBC 数据源|  
|**login**|**sysname**|用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录 ID。|  
|**password**|**sysname**|密码[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]身份验证。|  
|**commit_batch_size**|**int**|不提供支持。|  
|**status_batch_size**|**int**|不提供支持。|  
|**flush_frequency**|**int**|不提供支持。|  
|**frequency_type**|**int**|分发代理的运行频率：<br /><br /> **1** = 一次<br /><br /> **2** = 按需<br /><br /> **4** = 每日<br /><br /> **8** = 每周<br /><br /> **16** = 每月<br /><br /> **32** = 每月相对<br /><br /> **64** = 自动启动<br /><br /> **128** = 重复执行|  
|**frequency_interval**|**int**|应用于设置频率的值*frequency_type*。|  
|**frequency_relative_interval**|**int**|分发代理的日期时使用*frequency_type*设置为**32** （每月相对）：<br /><br /> **1** = 第一次<br /><br /> **2** = 第二个<br /><br /> **4** = 第三次<br /><br /> **8** = 第四次<br /><br /> **16** = 最后一次|  
|**frequency_recurrence_factor**|**int**|使用的重复因子*frequency_type*。|  
|**frequency_subday**|**int**|在定义的周期内的重新调度频率：<br /><br /> **1** = 一次<br /><br /> **2** = 第二个<br /><br /> **4** = 分钟<br /><br /> **8** = 小时|  
|**frequency_subday_interval**|**int**|时间间隔*frequency_subday*。|  
|**active_start_time_of_day**|**int**|第一次调度分发代理的时间，格式为 HHMMSS。|  
|**active_end_time_of_day**|**int**|分发代理停止时的时间安排，格式为 HHMMSS。|  
|**active_start_date**|**int**|第一次调度分发代理的日期，格式为 YYYYMMDD。|  
|**active_end_date**|**int**|停止调度分发代理的日期，格式为 YYYYMMDD。|  
|**retryattempt**|**int**|不提供支持。|  
|**retrydelay**|**int**|不提供支持。|  
|**description**|**nvarchar(255)**|对订阅服务器的文本说明。|  
|**security_mode**|**int**|实现的安全模式：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]身份验证<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 身份验证|  
|**frequency_type2**|**int**|合并代理的运行频率：<br /><br /> **1** = 一次<br /><br /> **2** = 按需<br /><br /> **4** = 每日<br /><br /> **8** = 每周<br /><br /> **16** = 每月<br /><br /> **32** = 每月相对<br /><br /> **64** = 自动启动<br /><br /> **128** = 重复执行|  
|**frequency_interval2**|**int**|应用于设置频率的值*frequency_type*。|  
|**frequency_relative_interval2**|**int**|合并代理的日期时使用*frequency_type*设置为 32 （每月相对）：<br /><br /> **1** = 第一次<br /><br /> **2** = 第二个<br /><br /> **4** = 第三次<br /><br /> **8** = 第四次<br /><br /> **16** = 最后一次|  
|**frequency_recurrence_factor2**|**int**|使用的重复因子*frequency_type * *。*|  
|**frequency_subday2**|**int**|在定义的周期内的重新调度频率：<br /><br /> **1** = 一次<br /><br /> **2** = 第二个<br /><br /> **4** = 分钟<br /><br /> **8** = 小时|  
|**frequency_subday_interval2**|**int**|时间间隔*frequency_subday*。|  
|**active_start_time_of_day2**|**int**|第一次调度合并代理的时间，格式为 HHMMSS。|  
|**active_end_time_of_day2**|**int**|停止调度合并代理的时间，格式为 HHMMSS。|  
|**active_start_date2**|**int**|第一次调度合并代理的日期，格式为 YYYYMMDD。|  
|**active_end_date2**|**int**|停止调度合并代理的日期，格式为 YYYYMMDD。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_helpsubscriberinfo**快照复制、 事务复制和合并复制中使用。  
  
## <a name="permissions"></a>权限  
 只有的成员**sysadmin**固定服务器角色**db_owner**固定的数据库角色或发布的发布访问列表才能执行**sp_helpsubscriberinfo**.  
  
## <a name="see-also"></a>请参阅  
 [sp_adddistpublisher &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addpullsubscription &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_changesubscriber &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropsubscriber &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpdistributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
