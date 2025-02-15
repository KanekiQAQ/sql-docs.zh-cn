---
title: sp_reinitsubscription (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_reinitsubscription
- sp_reinitsubscription_TSQL
helpviewer_keywords:
- sp_reinitsubscription
ms.assetid: d56ae218-6128-4ff9-b06c-749914505c7b
author: stevestein
ms.author: sstein
ms.openlocfilehash: 85911e4ce5652d15b99be4d87f75a04214a7f854
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68075644"
---
# <a name="spreinitsubscription-transact-sql"></a>sp_reinitsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  将标记为要重新初始化订阅。 此存储过程在发布服务器的推送订阅中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_reinitsubscription [ [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
        , [ @subscriber = ] 'subscriber'  
    [ , [ @destination_db = ] 'destination_db']  
    [ , [ @for_schema_change = ] 'for_schema_change']  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @ignore_distributor_failure = ] ignore_distributor_failure ]   
    [ , [ @invalidate_snapshot = ] invalidate_snapshot ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 是发布的名称。 *发布*是**sysname**，使用默认值为 all。  
  
`[ @article = ] 'article'` 是的名称。 *文章*是**sysname**，使用默认值为 all。 对于立即更新的发布，*一文*必须是**所有**; 否则为该存储的过程将跳过发布并报告错误。  
  
`[ @subscriber = ] 'subscriber'` 是订阅服务器的名称。 *订阅服务器上*是**sysname**，无默认值。  
  
`[ @destination_db = ] 'destination_db'` 是目标数据库的名称。 *destination_db*是**sysname**，使用默认值为 all。  
  
`[ @for_schema_change = ] 'for_schema_change'` 指示是否要重新初始化对发布数据库的架构更改导致。 *for_schema_change*是**位**，默认值为 0。 如果**0**，重新激活允许即时更新的发布的活动订阅，只要整个发布而不只是一些其文章中，被重新初始化。 这意味着，架构更改导致启动重新初始化。 如果**1**，活动的订阅不会重新激活，直到运行快照代理。  
  
`[ @publisher = ] 'publisher'` 指定一个非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布服务器。 *发布服务器*是**sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  *发布服务器*不应为[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布服务器。  
  
`[ @ignore_distributor_failure = ] ignore_distributor_failure` 即使分发服务器上不存在或处于脱机状态，则可以重新初始化。 *ignore_distributor_failure*是**位**，默认值为 0。 如果**0**，如果分发服务器上不存在或处于脱机状态，重新初始化会失败。  
  
`[ @invalidate_snapshot = ] invalidate_snapshot` 使现有的发布快照无效。 *invalidate_snapshot*是**位**，默认值为 0。 如果**1**，为发布生成新的快照。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_reinitsubscription**事务复制中使用。  
  
 **sp_reinitsubscription**不支持对等事务复制。  
  
 对于自动应用初始快照和发布不允许可更新订阅的订阅，执行此存储过程之后必须运行快照代理，以便准备架构和大容量复制程序文件，并使分发代理随后能够重新同步订阅。  
  
 对于自动应用初始快照和发布允许可更新订阅的订阅，分发代理使用先前由快照代理创建的最新架构和大容量复制程序文件重新同步订阅。 分发代理重新同步订阅，用户执行后立即**sp_reinitsubscription**，如果分发代理未忙碌; 否则为同步可能会在消息间隔 （指定分发代理命令提示符参数：**MessageInterval**)。  
  
 **sp_reinitsubscription**不起作用的订阅，手动应用初始快照。  
  
 若要重新同步发布的匿名订阅，请传入**所有**或 NULL 作为*订阅服务器*。  
  
 事务复制支持项目级的订阅重新初始化。 当项目被标记为要重新初始化后，在下一个同步过程中将在订阅服务器上重新应用项目快照。 但是，如果有相关的项目也由同一个订阅服务器订阅，那么在项目上重新应用快照可能会失败，除非发布中的相关项目在某些情况下也自动重新初始化：  
  
-   如果项目上的预创建命令是“删除”，则项目基对象上的架构绑定视图和架构绑定存储过程也将被标记为要重新初始化。  
  
-   如果项目上的架构选项包括在主键上创建声明引用完整性脚本，则基表同重新初始化项目的基表有外键关系的项目也将被标记为要重新初始化。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_reinittranpushsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitsubscription-tr_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有的成员**sysadmin**固定的服务器角色的成员**db_owner**固定的数据库角色或订阅创建者才能执行**sp_reinitsubscription**.  
  
## <a name="see-also"></a>请参阅  
 [重新初始化订阅](../../relational-databases/replication/reinitialize-a-subscription.md)   
 [重新初始化订阅](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
