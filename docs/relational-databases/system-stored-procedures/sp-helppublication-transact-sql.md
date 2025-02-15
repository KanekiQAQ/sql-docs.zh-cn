---
title: sp_helppublication (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_helppublication_TSQL
- sp_helppublication
helpviewer_keywords:
- sp_helppublication
ms.assetid: e801c3f0-dcbd-4b4a-b254-949a05f63518
author: stevestein
ms.author: sstein
ms.openlocfilehash: 18fc2e1dfadff4e276cd40ff6d64a0aa2fc9a06e
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68137574"
---
# <a name="sphelppublication-transact-sql"></a>sp_helppublication (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  返回有关发布的信息。 有关[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布，在发布服务器上对发布数据库执行此存储的过程。 对于 Oracle 发布，此存储过程在分发服务器上对任何数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helppublication [ [ @publication = ] 'publication' ]  
    [ , [ @found=] found OUTPUT]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 是要查看的名称。 *发布*数据类型为 sysname，默认值为 **%** ，表示返回有关所有发布的信息。  
  
`[ @found = ] 'found' OUTPUT` 是一个标志，指示返回行。 *找到*是**int**而且是 OUTPUT 参数，默认值为**23456**。 **1**指示已找到发布。 **0**指示找不到该发布。  
  
`[ @publisher = ] 'publisher'` 指定一个非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布服务器。 *发布服务器*数据类型为 sysname，默认值为 NULL。  
  
> [!NOTE]  
>  *发布服务器*请求中的发布信息时不应指定[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布服务器。  
  
## <a name="result-sets"></a>结果集  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|pubid|**int**|发布的的 ID。|  
|name|**sysname**|发布的名称。|  
|restricted|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|status|**tinyint**|发布的当前状态。<br /><br /> **0** = 非活动状态。<br /><br /> **1** = 活动。|  
|任务 (task)||用于保持向后兼容性。|  
|replication frequency|**tinyint**|复制频率的类型：<br /><br /> **0** = 事务<br /><br /> **1** = 快照|  
|synchronization method|**tinyint**|同步模式：<br /><br /> **0** = 本机大容量复制程序 (**bcp**实用程序)<br /><br /> **1** = 字符大容量复制<br /><br /> **3** = 并发，这意味着该本机大容量复制 (**bcp**实用程序) 使用，但在快照期间不锁定表<br /><br /> **4** = Concurrent_c，表示使用字符大容量复制但不是在快照期间锁定表|  
|description|**nvarchar(255)**|发布的的可选说明。|  
|immediate_sync|**bit**|表示是否在每次快照代理运行时创建或重新创建同步文件。|  
|enabled_for_internet|**bit**|表示是否通过文件传输协议 (FTP) 和其他服务将发布的同步文件在 Internet 上公开。|  
|allow_push|**bit**|表示是否允许对发布使用推送订阅。|  
|allow_pull|**bit**|表示是否允许对发布使用请求订阅。|  
|allow_anonymous|**bit**|表示是否允许对发布使用匿名订阅。|  
|independent_agent|**bit**|表示是否有用于该发布的独立分发代理。|  
|immediate_sync_ready|**bit**|表示快照代理是否生成了准备由新订阅使用的快照。 只有当发布被设置为始终有可用于新订阅或重新初始化订阅的快照，才定义此参数。|  
|allow_sync_tran|**bit**|表示是否允许对发布使用立即更新订阅。|  
|autogen_sync_procs|**bit**|表示是否自动生成存储过程以支持立即更新订阅。|  
|snapshot_jobid|**binary(16)**|已计划任务 ID。|  
|retention|**int**|为给定的发布保存的更改量（小时）。|  
|has subscription|**bit**|表示发布是否具有活动订阅。 **1**表示发布具有活动订阅，并**0**意味着发布具有任何订阅。|  
|allow_queued_tran|**bit**|指定是否已启用在订阅服务器上禁用更改排队直到这些更改可以应用到发布服务器。 如果**0**，则不在订阅服务器上的更改进行排队。|  
|snapshot_in_defaultfolder|**bit**|指定是否在默认文件夹中存储快照文件。 如果**0**，快照文件存储在由指定的备用位置*alternate_snapshot_folder*。 如果**1**，可以在默认文件夹中找到快照文件。|  
|alt_snapshot_folder|**nvarchar(255)**|指定快照的备用文件夹的位置。|  
|pre_snapshot_script|**nvarchar(255)**|指定一个指向 **.sql**文件位置。 分发代理将在运行任何复制的对象时在订阅服务器上应用快照之前运行快照前脚本。|  
|post_snapshot_script|**nvarchar(255)**|指定一个指向 **.sql**文件位置。 分发代理将在初始同步过程中已应用所有其他复制的对象脚本和数据之后才运行快照后脚本。|  
|compress_snapshot|**bit**|指定写入到的快照*alt_snapshot_folder*位置是将压缩成[!INCLUDE[msCoName](../../includes/msconame-md.md)]CAB 格式。 **0**指定不压缩快照。|  
|ftp_address|**sysname**|分发服务器的 FTP 服务的网络地址。 指定供订阅服务器的分发代理或合并代理拾取的发布快照文件的位置。|  
|ftp_port|**int**|分发服务器的 FTP 服务的端口号。|  
|ftp_subdirectory|**nvarchar(255)**|指定供订阅服务器的分发代理或合并代理拾取的快照文件的位置（如果发布支持使用 FTP 传播快照）。|  
|ftp_login|**sysname**|用于连接到 FTP 服务的用户名。|  
|allow_dts|**bit**|指定发布允许数据转换。 **0**指定不允许 DTS 转换。|  
|allow_subscription_copy|**bit**|指定是否已启用复制订阅该发布的订阅数据库的功能。 **0**表示不允许复制。|  
|centralized_conflicts|**bit**|指定冲突记录是否存储在发布服务器上：<br /><br /> **0** = 均存储的冲突记录的发布服务器和订阅服务器导致冲突。<br /><br /> **1** = 的冲突记录存储在发布服务器。|  
|conflict_retention|**int**|指定冲突保持期（天）。|  
|conflict_policy|**int**|指定使用排队更新订阅服务器选项时遵循的冲突解决策略。 可以是下列值之一：<br /><br /> **1** = 发布服务器入选冲突。<br /><br /> **2** = 订阅服务器入选冲突。<br /><br /> **3** = 重新初始化订阅。|  
|queue_type||指定所使用的队列类型。 可以是下列值之一：<br /><br /> **msmq** = 使用[!INCLUDE[msCoName](../../includes/msconame-md.md)]消息队列来存储事务。<br /><br /> **sql** = 使用[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]来存储事务。<br /><br /> 注意:已不支持消息队列。|  
|backward_comp_level||数据库兼容级别，可以为下列值之一：<br /><br /> **90** = [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]<br /><br /> **100** = [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
|publish_to_AD|**bit**|指定是否在发布该发布[!INCLUDE[msCoName](../../includes/msconame-md.md)]Active Directory 批处理。 值为**1**指示它已发布，并将值**0**指示未发布。|  
|allow_initialize_from_backup|**bit**|指示订阅服务器是否能够从备份而不是从初始快照来初始化对此发布的订阅。 **1**意味着可以从备份初始化订阅并**0**表示不能。 有关详细信息，请参阅[初始化事务订阅不使用快照](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)不使用快照的事务订阅服务器。|  
|replicate_ddl|**int**|指示发布是否支持架构复制。 **1**指示复制在发布服务器上执行数据定义语言 (DDL) 语句是的并**0**指示不复制 DDL 语句。 有关详细信息，请参阅[对发布数据库进行架构更改](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)。|  
|enabled_for_p2p|**int**|表示发布是否可用于对等复制拓扑。 **1**指示发布支持对等复制。 有关详细信息，请参阅 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)。|  
|publish_local_changes_only|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|enabled_for_het_sub|**int**|指定发布是否支持非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 订阅服务器。 值为**1**表示非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]支持订阅服务器。 值为**0**表示仅[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]支持订阅服务器。 有关详细信息，请参阅 [Non-SQL Server Subscribers](../../relational-databases/replication/non-sql/non-sql-server-subscribers.md)。|  
|enabled_for_p2p_conflictdetection|**int**|指定分发代理是否为针对对等复制启用的发布检测冲突。 值为**1**意味着在检测到冲突。 有关详细信息，请参阅 [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)。|  
|originator_id|**int**|指定对等拓扑中某个节点的 ID。 此 ID 用于进行冲突检测，如果**enabled_for_p2p_conflictdetection**设置为**1**。 有关已经使用过的 ID 的列表，请查询 [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md) 系统表。|  
|p2p_continue_onconflict|**int**|指定检测到冲突时分发代理是否继续处理更改。 值为**1**意味着代理继续处理更改。<br /><br /> **\*\* 谨慎\* \*** 我们建议你使用的默认值**0**。 如果此选项设置为**1**，分发代理尝试应用来自具有最高的原始发件人 ID 的节点的冲突行收敛拓扑中的数据 此方法不保证将会收敛。 您应确保检测到冲突之后拓扑保持一致。 有关详细信息，请参阅 [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)中的“处理冲突”。|  
|allow_partition_switch|**int**|指定是否更改表...可以对已发布数据库执行 SWITCH 语句。 有关详细信息，请参阅[复制已分区表和索引](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md)。|  
|replicate_partition_switch|**int**|指定是否更改表...对已发布数据库执行的 SWITCH 语句应复制到订阅服务器。 此选项才有效才*allow_partition_switch*设置为**1**。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 sp_helppublication 用于快照复制和事务复制。  
  
 sp_helppublication 将返回执行此过程的用户所拥有的所有发布的信息。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_helppublication](../../relational-databases/replication/codesnippet/tsql/sp-helppublication-trans_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有发布服务器上的 sysadmin 固定服务器角色成员或发布数据库上的 db_owner 固定数据库角色成员，或者发布访问列表 (PAL) 中的用户才能执行 sp_helppublication。  
  
 对于非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布服务器上，唯一的 sysadmin 固定服务器角色成员在分发服务器或 db_owner 固定的数据库角色的成员的分发数据库或者 PAL 中的用户才能执行 sp_helppublication。  
  
## <a name="see-also"></a>请参阅  
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addpublication &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_droppublication &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
