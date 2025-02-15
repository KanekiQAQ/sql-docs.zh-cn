---
title: sp_helpdistributor_properties (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_helpdistributor_properties_TSQL
- sp_helpdistributor_properties
helpviewer_keywords:
- sp_helpdistributor_properties
ms.assetid: ee267724-3244-49eb-84c9-f38dbefdd639
author: stevestein
ms.author: sstein
ms.openlocfilehash: 61d11dd443e68d743b30cee890d33e4852c99b39
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67902911"
---
# <a name="sphelpdistributorproperties-transact-sql"></a>sp_helpdistributor_properties (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  返回分发服务器属性。 此存储过程在分发服务器上对分发数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpdistributor_properties   
```  
  
## <a name="result-set"></a>结果集  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**heartbeat_interval**|**int**|在不记录进度消息的情况下，代理可以运行的最大分钟数。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_helpdistributor_properties**用于所有类型的复制。  
  
## <a name="permissions"></a>权限  
 只有的成员**sysadmin**固定的服务器角色的成员**db_owner**或**replmonitor**固定的数据库角色上的分发数据库中的用户使用此分发服务器的发布的发布访问列表 (PAL) 可以执行**sp_helpdistributor_properties**。  
  
## <a name="see-also"></a>请参阅  
 [sp_changedistributor_property &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)  
  
  
