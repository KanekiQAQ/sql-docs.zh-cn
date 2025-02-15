---
title: sp_repladdcolumn (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_repladdcolumn_TSQL
- sp_repladdcolumn
helpviewer_keywords:
- sp_repladdcolumn
ms.assetid: d6220f9f-c738-4f9c-bcf8-419994e86c81
author: stevestein
ms.author: sstein
ms.openlocfilehash: 1b01a48e15c06f021b41b3bded35a0cd2739313c
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68006916"
---
# <a name="sprepladdcolumn-transact-sql"></a>sp_repladdcolumn (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  将列添加到已发布的现有表项目中。 允许将新列添加到发布该表的所有发布服务器，或者只将新列添加到发布该表的特定发布中。 在发布服务器上对发布数据库执行此存储的过程。  
  
> [!IMPORTANT]
>  已不推荐使用此存储过程，支持它是为了能够向后兼容。 只应与[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]发布服务器和[!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]重新发布订阅服务器。 不应对列将此过程与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本中引入的数据类型一起使用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_repladdcolumn [ @source_object = ] 'source_object', [ @column = ] 'column' ]  
    [ , [ @typetext = ] 'typetext' ]  
    [ , [ @publication_to_add = ] 'publication_to_add' ]  
    [ , [ @from_agent = ] from_agent ]  
    [ , [ @schema_change_script = ] 'schema_change_script' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>参数  
 [ @source_object =] '*source_object*  
 包含要添加的新列的表项目的名称。 *source_object*是**nvarchar (358**)，无默认值。  
  
 [ @column =] '*列*  
 表中要为复制添加的列的名称。 *列*是**sysname**，无默认值。  
  
 [ @typetext =] '*typetext*  
 要添加的列的定义。 *typetext*是**nvarchar(3000)** ，无默认值。 例如，如果要添加 order_filled 列，并且它是单字符字段、 不为 NULL，且默认值为**N**，则 order_filled 将为*列*参数，而的定义列中， **char （1) NOT NULL CONSTRAINT constraint_name DEFAULT 'n'** 会*typetext*参数值。  
  
 [ @publication_to_add =] '*publication_to_add*  
 要向其中添加新列的发布的名称。 *publication_to_add*是**nvarchar(4000)** ，默认值为**所有**。 如果**所有**，然后将影响所有包含此表的发布。 如果*publication_to_add*指定，则仅向该发布已添加的新列。  
  
 [ @from_agent =] *from_agent*  
 表示是否由复制代理执行该存储过程。 *from_agent*是**int**，默认值为**0**，其中的值**1**由复制代理，然后在要执行此存储的过程时使用每个其他情况下的默认值**0**应使用。  
  
 [ @schema_change_script =] '*schema_change_script*  
 指定用于修改系统生成的自定义存储过程的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 脚本的名称和路径。 *schema_change_script*是**nvarchar(4000)** ，默认值为 NULL。 复制允许用户定义的自定义存储过程替换事务复制中使用的一个或多个默认过程。 *schema_change_script*后架构更改对使用 sp_repladdcolumn 时，复制的表项目，并可用于执行下列操作之一，将执行：  
  
-   如果自定义存储的过程自动重新生成， *schema_change_script*可用于删除这些自定义存储的过程并将它们替换为用户定义自定义存储过程支持新的架构。  
  
-   如果不自动重新生成自定义存储的过程，则*schema_change_script*可用于重新生成这些存储的过程或创建用户定义的自定义存储过程。  
  
 [ @force_invalidate_snapshot =] *force_invalidate_snapshot*  
 启用或禁用使快照失效的功能。 *force_invalidate_snapshot*是**位**，默认值为**1**。  
  
 **1**指定对项目的更改可能导致快照无效，如果是这种情况的值**1**提供了新快照的权限。  
  
 **0**指定对项目的更改不会导致快照无效。  
  
 [ @force_reinit_subscription =] *force_reinit_subscription*  
 启用或禁用使订阅重新初始化的功能。 *force_reinit_subscription*是**位**默认值为**0**。  
  
 **0**指定对项目的更改不会导致重新初始化订阅。  
  
 **1**指定对项目的更改可能导致订阅重新初始化，如果是这种情况的值**1**使订阅重新初始化发生的权限。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="permissions"></a>权限  
 只有 sysadmin 固定服务器角色或 db_owner 固定数据库角色的成员才能执行 sp_repladdcolumn。  
  
## <a name="see-also"></a>请参阅  
 [SQL Server 复制中已弃用的功能](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
