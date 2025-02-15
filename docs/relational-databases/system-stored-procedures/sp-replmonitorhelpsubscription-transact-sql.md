---
title: sp_replmonitorhelpsubscription (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_replmonitorhelpsubscription_TSQL
- sp_replmonitorhelpsubscription
helpviewer_keywords:
- sp_replmonitorhelpsubscription
ms.assetid: a681b2db-c82d-4624-a10c-396afb0ac42f
author: stevestein
ms.author: sstein
ms.openlocfilehash: 8e8f76699e35c71e7bbf85b972cd76eb0cb3c289
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67950547"
---
# <a name="spreplmonitorhelpsubscription-transact-sql"></a>sp_replmonitorhelpsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  返回发布服务器上属于一个或多个发布的订阅的当前状态信息，并为每个返回的订阅返回一行。 在分发服务器的分发数据库上执行此存储过程，用于监视复制。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replmonitorhelpsubscription [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @mode = ] mode ]  
    [ , [ @topnum = ] topnum ]   
    [ , [ @exclude_anonymous = ] exclude_anonymous ]   
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 是正监视其状态的发布服务器的名称。 *发布服务器*是**sysname**，默认值为 NULL。 如果**null**，为所有使用分发服务器的发布服务器返回的信息。  
  
`[ @publisher_db = ] 'publisher_db'` 是已发布数据库的名称。 *publisher_db*是**sysname**，默认值为 NULL。 如果为 NULL，则返回发布服务器上所有已发布数据库的信息。  
  
`[ @publication = ] 'publication'` 正在监视的发布的名称。 *发布*是**sysname**，默认值为 NULL。  
  
`[ @publication_type = ] publication_type` 如果发布的类型。 *publication_type*是**int**，可以是下列值之一。  
  
|ReplTest1|Description|  
|-----------|-----------------|  
|**0**|事务发布。|  
|**1**|快照发布。|  
|**2**|合并发布。|  
|NULL（默认值）|由复制来确定发布类型。|  
  
`[ @mode = ] mode` 返回订阅时要使用的筛选模式监视的信息。 *模式*是**int**，可以是下列值之一。  
  
|ReplTest1|描述|  
|-----------|-----------------|  
|**0** （默认值）|返回所有订阅。|  
|**1**|只返回带错误的订阅。|  
|**2**|只返回已生成在达到阈值度量指标时发出的警告的订阅。|  
|**3**|只返回带错误或已生成在达到阈值度量指标时发出的警告的订阅。|  
|**4**|返回前 25 个坏的情况下执行的订阅。|  
|**5**|返回执行情况最差的 50 个订阅。|  
|**6**|只返回当前同步的订阅。|  
|**7**|只返回当前不同步的订阅。|  
  
`[ @topnum = ] topnum` 将结果设置为仅指定的返回数据顶部的订阅数限制。 *topnum*是**int**，无默认值。  
  
`[ @exclude_anonymous = ] exclude_anonymous` 如果结果集中排除匿名请求订阅。 *exclude_anonymous*是**位**，默认值为**0**; 值为**1**表示排除匿名订阅，值为**0**意味着它们包括在内。  
  
`[ @refreshpolicy = ] refreshpolicy` 仅限内部使用。  
  
## <a name="result-sets"></a>结果集  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**status**|**int**|检查与该发布关联的所有复制代理的状态，并返回按以下顺序找到的最高级状态：<br /><br /> **6** = 失败<br /><br /> **5** = 正在重试<br /><br /> **2** = 已停止<br /><br /> **4** = 空闲<br /><br /> **3** = 正在进行<br /><br /> **1** = 开始|  
|**警告**|**int**|由属于该发布的订阅所生成的最大阈值警告，可以是下列一个或多个值进行逻辑或运算的结果。<br /><br /> **1** = 到期-在保留期阈值内尚未同步对事务发布的订阅。<br /><br /> **2** = latency-将数据从事务发布服务器复制到订阅服务器所用的时间超过阈值，以秒为单位。<br /><br /> **4** = mergeexpiration – 对合并发布的订阅尚未在保持期阈值内进行同步。<br /><br /> **8** = mergefastrunduration-完成的合并订阅同步所花费的时间超过阈值，以秒为单位，通过快速网络连接。<br /><br /> **16** = mergeslowrunduration-完成的合并订阅同步所花费的时间超过阈值，以秒为单位，通过慢速或拨号网络连接。<br /><br /> **32** = mergefastrunspeed-传送速率的行合并订阅的同步过程未能保持阈值速率，单位为每秒，行通过快速网络连接。<br /><br /> **64** = mergeslowrunspeed-传送速率的行合并订阅的同步过程未能保持阈值速率，单位为每秒，行通过慢速或拨号网络连接。|  
|**订阅服务器**|**sysname**|订阅服务器的名称。|  
|**subscriber_db**|**sysname**|用于订阅的数据库的名称。|  
|**publisher_db**|**sysname**|发布数据库的名称。|  
|**publication**|**sysname**|发布的名称。|  
|**publication_type**|**int**|发布的类型，可以是下列值之一：<br /><br /> **0** = 事务发布<br /><br /> **1** = 快照发布<br /><br /> **2** = 合并发布|  
|**subtype**|**int**|订阅类型，可以是下列值之一：<br /><br /> **0** = 推送<br /><br /> **1** = 请求<br /><br /> **2** = 匿名|  
|**延迟**|**int**|在事务发布中，由日志读取器代理或分发代理传播的数据更改的最长滞后时间（以秒为单位）。|  
|**latencythreshold**|**int**|事务发布的最长滞后时间，高于此时间即产生警告。|  
|**agentnotrunning**|**int**|代理未运行的时间长度，以小时为单位。|  
|**agentnotrunningthreshold**|**int**|产生警告前代理未运行的时间长度，以小时为单位。|  
|**timetoexpiration**|**int**|订阅未同步时订阅过期前的时间长度，以小时为单位。|  
|**expirationthreshold**|**int**|订阅过期导致警告产生前的时间，以小时为单位。|  
|**last_distsync**|**datetime**|分发代理上次运行的日期时间。|  
|**distribution_agentname**|**sysname**|事务发布订阅的分发代理作业的名称。|  
|**mergeagentname**|**sysname**|合并发布订阅的合并代理作业的名称。|  
|**mergesubscriptionfriendlyname**|**sysname**|为订阅指定的友好名称。|  
|**mergeagentlocation**|**sysname**|运行合并代理的服务器的名称。|  
|**mergeconnectiontype**|**int**|将订阅同步到合并发布时使用的连接，可以是下列值之一：<br /><br /> **1** = 局域网 (LAN)<br /><br /> **2** = 拨号网络连接<br /><br /> **3** = web 同步。|  
|**mergePerformance**|**int**|订阅的上次同步相对于其所有同步而言的性能，用上次同步的传递速率除以之前所有传递速率的平均值。|  
|**mergerunspeed**|**float**|订阅的上次同步的传递速率。|  
|**mergerunduration**|**int**|完成订阅的上次同步的时间长度。|  
|**monitorranking**|**int**|用于对结果集中的订阅进行排序的排名值，可以是下列值之一：<br /><br /> 对于事务发布：<br /><br /> **60** = 错误<br /><br /> **56** = 警告： 关键的性能<br /><br /> **52** = 警告： 即将过期或已过期<br /><br /> **50** = 警告： 订阅未初始化<br /><br /> **40** = 正在重试失败的命令<br /><br /> **30** = 未运行 （成功）<br /><br /> **20** = 正在运行 （正在启动、 正在运行或空闲）<br /><br /> 对于合并发布：<br /><br /> **60** = 错误<br /><br /> **56** = 警告： 关键的性能<br /><br /> **54** = 警告： 长时间运行的合并<br /><br /> **52** = 警告： 即将到期<br /><br /> **50** = 警告： 订阅未初始化<br /><br /> **40** = 正在重试失败的命令<br /><br /> **30** = 正在运行 （正在启动、 正在运行或空闲）<br /><br /> **20** = 未运行 （成功）|  
|**distributionagentjobid**|**binary(16)**|事务发布订阅的分发代理作业的 ID。|  
|**mergeagentjobid**|**binary(16)**|合并发布订阅的合并代理作业的 ID。|  
|**distributionagentid**|**int**|订阅的分发代理作业的 ID。|  
|**distributionagentprofileid**|**int**|分发代理使用的代理配置文件的 ID。|  
|**mergeagentid**|**int**|订阅的合并代理作业的 ID。|  
|**mergeagentprofileid**|**int**|合并代理使用的代理配置文件的 ID。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_replmonitorhelpsubscription**用于所有类型的复制。  
  
 **sp_replmonitorhelpsubscription**结果集基于订阅的值确定的状态的严重性进行排序*monitorranking*。 例如，处于错误状态的所有订阅的各行排在处于警告状态的订阅的各行之上。  
  
## <a name="permissions"></a>权限  
 只有的成员**db_owner**或**replmonitor**固定的数据库角色的分发数据库才能执行**sp_replmonitorhelpsubscription**。  
  
## <a name="see-also"></a>请参阅  
 [以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
