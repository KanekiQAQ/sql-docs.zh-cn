---
title: sp_helparticle (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_helparticle_TSQL
- sp_helparticle
helpviewer_keywords:
- sp_helparticle
ms.assetid: 9c4a1a88-56f1-45a0-890c-941b8e0f0799
author: stevestein
ms.author: sstein
ms.openlocfilehash: cffdecba62283e3fc404c3630866467bc3a2b1c1
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68084991"
---
# <a name="sphelparticle-transact-sql"></a>sp_helparticle (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  显示有关项目的信息。 在发布服务器上对发布数据库执行此存储的过程。 对于 Oracle 发布服务器，此存储过程在分发服务器的任一数据库上执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helparticle [ @publication = ] 'publication'   
    [ , [ @article = ] 'article' ]  
    [ , [ @returnfilter = ] returnfilter ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @found = ] found OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 是发布的名称。 *发布*是**sysname**，无默认值。  
  
`[ @article = ] 'article'` 是发布中的名称。 *文章*是**sysname**，默认值为 **%** 。 如果*一文*是未提供，则返回指定发布的所有项目的信息。  
  
`[ @returnfilter = ] returnfilter` 指定是否应返回筛选子句。 *returnfilter*是**位**，默认值为**1**，表示返回筛选器子句。  
  
`[ @publisher = ] 'publisher'` 指定一个非[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布服务器。 *发布服务器*是**sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  *发布服务器*不应通过请求对项目的信息发布时指定[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布服务器。  
  
`[ @found = ] found OUTPUT` 仅限内部使用。  
  
## <a name="result-sets"></a>结果集  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**文章 id**|**int**|项目的 ID。|  
|**项目名称**|**sysname**|项目的名称。|  
|**基对象**|**nvarchar(257)**|项目或存储过程所表示的基础表的名称。|  
|**目标对象**|**sysname**|目标（订阅）表的名称。|  
|**同步对象**|**nvarchar(257)**|用于定义已发布项目的视图的名称。|  
|**type**|**smallint**|项目的类型：<br /><br /> **1** = 基于日志的。<br /><br /> **3** = 基于日志的具有手动筛选器。<br /><br /> **5** = 具有手动视图并且基于日志。<br /><br /> **7** = 具有手动筛选器和手动视图并且基于日志。<br /><br /> **8** = 存储过程执行。<br /><br /> **24** = 可序列化的存储的过程执行。<br /><br /> **32** = 存储过程 （仅限架构）。<br /><br /> **64** = 视图 （仅限架构）。<br /><br /> **96** = 聚合函数 （仅限架构）。<br /><br /> **128** = 函数 （仅限架构）。<br /><br /> **257** = 基于日志的索引的视图。<br /><br /> **259** = 具有手动筛选器基于日志的索引的视图。<br /><br /> **261** = 具有手动视图并且基于日志的索引的视图。<br /><br /> **263** = 具有手动筛选器基于日志的索引的视图和手动视图。<br /><br /> **320** = 索引视图 （仅限架构）。<br /><br />|  
|**status**|**tinyint**|可以是[& （位与）](../../t-sql/language-elements/bitwise-and-transact-sql.md)的一个或多个项目属性的结果：<br /><br /> **0x00** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> **0x01** = 项目处于活动状态。<br /><br /> **0x08** = 包括 insert 语句中的列名称。<br /><br /> **0x16** = 使用参数化语句。<br /><br /> **0x32** = 使用参数化语句，并在 insert 语句中包括的列名称。|  
|**filter**|**nvarchar(257)**|用于水平筛选表的存储过程。 必须已使用 FOR REPLICATION 子句创建了此存储过程。|  
|**description**|**nvarchar(255)**|项目的说明项。|  
|**insert_command**|**nvarchar(255)**|复制对表项目的插入操作时所使用的复制命令类型。 有关详细信息，请参阅[指定如何传播事务项目的更改](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。|  
|**update_command**|**nvarchar(255)**|复制对表项目的更新操作时所使用的复制命令类型。 有关详细信息，请参阅[指定如何传播事务项目的更改](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。|  
|**delete_command**|**nvarchar(255)**|复制对表项目的删除操作时所使用的复制命令类型。 有关详细信息，请参阅[指定如何传播事务项目的更改](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md)。|  
|**创建脚本路径**|**nvarchar(255)**|用于创建目标表的项目架构脚本的路径和名称。|  
|**垂直分区**|**bit**|是是否垂直分区项目启用了;其中的值**1**表示启用垂直分区。|  
|**pre_creation_cmd**|**tinyint**|DROP TABLE、DELETE TABLE 或 TRUNCATE TABLE 的预创建命令。|  
|**filter_clause**|**ntext**|用于指定水平筛选的 WHERE 子句。|  
|**schema_option**|**binary(8)**|给定项目的架构生成选项位图。 有关完整列表**schema_option**值，请参阅[sp_addarticle &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)。|  
|**dest_owner**|**sysname**|目标对象所有者的名称。|  
|**source_owner**|**sysname**|源对象的所有者。|  
|**unqua_source_object**|**sysname**|源对象的名称，不含所有者名称。|  
|**sync_object_owner**|**sysname**|用于定义已发布项目的视图的所有者。 .|  
|**unqualified_sync_object**|**sysname**|用于定义已发布项目的视图的名称，不含所有者名称。|  
|**filter_owner**|**sysname**|筛选的所有者。|  
|**unqua_filter**|**sysname**|筛选的名称，不含所有者名称。|  
|**auto_identity_range**|**int**|用于表示在创建发布时是否在发布上打开了自动标识范围处理功能的标志。 **1**表示启用自动标识范围;**0**表示处于禁用状态。|  
|**publisher_identity_range**|**int**|如果将项目的范围的发布服务器上的标识范围的大小*identityrangemanagementoption*设置为**自动**或**auto_identity_range**设置为**true**。|  
|**identity_range**|**bigint**|如果将项目的范围的订阅服务器上的标识范围的大小*identityrangemanagementoption*设置为**自动**或**auto_identity_range**设置为**true**。|  
|**threshold**|**bigint**|表示分发代理何时分配新标识范围的百分比值。|  
|**identityrangemanagementoption**|**int**|表示针对项目处理的标识范围管理。|  
|**fire_triggers_on_snapshot**|**bit**|表示应用初始快照时是否执行已复制的用户触发器。<br /><br /> **1** = 执行的用户触发器。<br /><br /> **0** = 不执行触发器的用户。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_helparticle**快照复制和事务复制中使用。  
  
## <a name="permissions"></a>权限  
 只有的成员**sysadmin**固定服务器角色**db_owner**固定的数据库角色或当前发布的发布访问列表才能执行**sp_helparticle**.  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_helptranarticle](../../relational-databases/replication/codesnippet/tsql/sp-helparticle-transact-_1.sql)]  
  
## <a name="see-also"></a>请参阅  
 [查看和修改项目属性](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_changearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
