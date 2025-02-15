---
title: sp_helpqreader_agent (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_helpqreader_agent_TSQL
- sp_helpqreader_agent
helpviewer_keywords:
- sp_helpqreader_agent
ms.assetid: 8e74e1aa-e95b-4183-8017-bf123439b08d
author: stevestein
ms.author: sstein
ms.openlocfilehash: 229442fed0defba9ebe39822a6184ba3b5d35644
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68137560"
---
# <a name="sphelpqreaderagent-transact-sql"></a>sp_helpqreader_agent (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  返回队列读取器代理的属性。 此存储过程在分发服务器的分发数据库或发布服务器的任意数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpqreader_agent [ [ @frompublisher = ] frompublisher ]  
```  
  
## <a name="arguments"></a>参数  
`[ @frompublisher = ] frompublisher` 指定在发布服务器或分发服务器上是否调用该存储的过程。 *frompublisher*为 bit，默认值为 0。 **1**表示从发布服务器中，调用存储的过程和**0**表示从分发服务器上调用存储的过程。  
  
## <a name="result-sets"></a>结果集  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**id**|**int**|代理的 ID。|  
|**name**|**nvarchar(100)**|代理的名称。|  
|**job_id**|**uniqueidentifier**|代理作业的唯一 ID。|  
|**job_login**|**nvarchar(512)**|是运行分发代理的 Windows 帐户的格式返回*域*\\*用户名*。|  
|**job_password**|**sysname**|出于安全原因，值为 **\*\*\*\*\*\*\*\*\*\*** 始终返回。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_helpqreader_agent**事务复制中使用。  
  
## <a name="permissions"></a>权限  
 时的值*frompublisher*是**1**，只有的成员**sysadmin**固定的服务器角色的发布服务器或成员**db_owner**上对发布数据库的固定的数据库角色可以执行**sp_helpqreader_agent**。 否则为只有的成员**sysadmin**固定的服务器角色的成员的分发服务器**db_owner**上的分发数据库的固定的数据库角色可以执行**sp_helpqreader_代理**。  
  
## <a name="see-also"></a>请参阅  
 [允许更新事务发布的订阅](../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)  
  
  
