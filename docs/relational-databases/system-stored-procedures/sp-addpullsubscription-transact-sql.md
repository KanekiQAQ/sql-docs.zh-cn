---
title: sp_addpullsubscription (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_addpullsubscription
- sp_addpullsubscription_TSQL
helpviewer_keywords:
- sp_addpullsubscription
ms.assetid: 0f4bbedc-0c1c-414a-b82a-6fd47f0a6a7f
author: stevestein
ms.author: sstein
ms.openlocfilehash: ea87c5e83b5be3945469ddb0e32c9f8158a5e116
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68022445"
---
# <a name="spaddpullsubscription-transact-sql"></a>sp_addpullsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  将请求订阅添加到快照或事务发布中。 此存储过程在订阅服务器上要创建请求订阅的数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addpullsubscription [ @publisher= ] 'publisher'  
    [ , [ @publisher_db= ] 'publisher_db' ]  
        , [ @publication= ] 'publication'  
    [ , [ @independent_agent= ] 'independent_agent' ]  
    [ , [ @subscription_type= ] 'subscription_type' ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @update_mode= ] 'update_mode' ]  
    [ , [ @immediate_sync = ] immediate_sync ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 是发布服务器的名称。 *发布服务器*是**sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 是发布服务器数据库的名称。 *publisher_db*是**sysname**，默认值为 NULL。 *publisher_db* Oracle 发布服务器忽略。  
  
`[ @publication = ] 'publication'` 是发布的名称。 *发布*是**sysname**，无默认值。  
  
`[ @independent_agent = ] 'independent_agent'` 指定是否有用于此发布的独立分发代理。 *independent_agent*是**nvarchar(5)** ，默认值为 TRUE。 如果 **，则返回 true**，没有用于此发布的独立分发代理。 如果**false**，每个发布服务器数据库/订阅服务器数据库对一个分发代理。 *independent_agent*是发布的属性，必须具有相同的值，因为它具有发布服务器上。  
  
`[ @subscription_type = ] 'subscription_type'` 是订阅的类型。 *subscription_type*是**nvarchar(9)** ，默认值为**匿名**。 必须指定的值**拉取**有关*subscription_type*，除非你想要创建订阅，而无需在发布服务器注册此订阅。 在这种情况下，必须指定的值**匿名**。 如果在订阅配置期间无法建立与发布服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接，则匿名订阅是必需的。  
  
`[ @description = ] 'description'` 是发布的说明。 *描述*是**nvarchar(100)** ，默认值为 NULL。  
  
`[ @update_mode = ] 'update_mode'` 已更新的类型。 *update_mode*是**nvarchar(30)** ，可以是下列值之一。  
  
|ReplTest1|描述|  
|-----------|-----------------|  
|**只读**（默认值）|该订阅是只读的。 在订阅服务器上所做的任何更改不会发送回发布服务器。 应在订阅服务器不进行更新时使用。|  
|**synctran**|支持立即更新订阅。|  
|**排队的 tran**|支持订阅进行排队更新。 可以在订阅服务器上进行数据修改，将其存储在队列中，然后传播到发布服务器。|  
|**failover**|将排队更新作为故障转移的情况下启用用于即时更新的订阅。 可以在订阅服务器上进行数据修改并立即传播到发布服务器。 如果发布服务器与订阅服务器未连接在一起，则可以将在订阅服务器上所做的数据修改存储在队列中，直到订阅服务器与发布服务器重新连接在一起。|  
|**排队故障转移**|支持将订阅作为排队更新订阅，并允许更改为立即更新模式。 在订阅服务器和发布服务器之间建立连接之前，可以在订阅服务器上修改数据，并将数据修改存储在队列中。 建立起持续连接后，即可将更新模式更改为立即更新。 *对 Oracle 发布服务器不支持*。|  
  
`[ @immediate_sync = ] immediate_sync` 同步文件是否是创建或重新创建每次快照代理运行的时。 *immediate_sync*是**位**默认值为 1，并且必须设置为相同的值*immediate_sync*中**sp_addpublication**。*immediate_sync*是发布的属性，必须具有相同的值，因为它具有发布服务器上。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_addpullsubscription**快照复制和事务复制中使用。  
  
> [!IMPORTANT]  
>  对于在队列中排队的更新订阅，请将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 验证用于与订阅服务器的连接，并为每个订阅服务器连接指定一个不同的帐户。 创建支持排队更新的请求订阅时，复制始终将连接设置为使用 Windows 身份验证（对于请求订阅，复制无法访问使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证所需的订阅服务器中的元数据）。 在这种情况下，应执行[sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md)若要更改要使用的连接[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]身份验证后配置订阅。  
  
 如果[MSreplication_subscriptions &#40;TRANSACT-SQL&#41; ](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md)表不在订阅服务器上，存在**sp_addpullsubscription**创建它。 它还将添加到行[MSreplication_subscriptions &#40;TRANSACT-SQL&#41; ](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md)表。 对于请求订阅[sp_addsubscription &#40;TRANSACT-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)应在发布服务器首次调用。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-t_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有的成员**sysadmin**固定的服务器角色或**db_owner**固定的数据库角色可以执行**sp_addpullsubscription**。  
  
## <a name="see-also"></a>请参阅  
 [创建请求订阅](../../relational-databases/replication/create-a-pull-subscription.md)   
 [创建事务发布的可更新订阅](../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md)[订阅发布](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)   
 [sp_change_subscription_properties &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
