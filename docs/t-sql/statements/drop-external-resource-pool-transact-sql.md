---
title: DROP EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP EXTERNAL RESOURCE POOL
- DROP_EXTERNAL_RESOURCE_POOL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL RESOURCE POOL statement
ms.assetid: e2fa01bd-96ff-4ea9-bb08-6cb6b6adf68c
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: a7311828e42ffbd73e53e82ab3f87fb6e7b0925d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68086680"
---
# <a name="drop-external-resource-pool-transact-sql"></a>DROP EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  删除用于定义外部进程资源的 Resource Governor 外部资源池。 对于 R Services，外部池控制 `rterm.exe`、`BxlServer.exe` 及其生成的其他进程。 外部资源池使用 [CREATE EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-external-resource-pool-transact-sql.md) 创建，使用 [ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md) 修改。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。  
  
## <a name="syntax"></a>语法  
  
```  
DROP EXTERNAL RESOURCE POOL pool_name  
```  
  
## <a name="arguments"></a>参数  
 pool_name   
 要删除的外部资源池的名称。  
  
## <a name="remarks"></a>Remarks  
 如果外部资源池包含工作负荷组，则不能删除该资源池。  
  
 不能删除资源调控器默认池或内部池。  
  
 重新配置  
  
 建议您在熟悉资源调控器状态之后再执行 DDL 语句。 有关详细信息，请参阅 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)。  
  
## <a name="permissions"></a>权限  
 需要 `CONTROL SERVER` 权限。  
  
## <a name="examples"></a>示例  
 以下示例会删除名为 `ex_pool` 的外部资源池。  
  
```  
DROP EXTERNAL RESOURCE POOL ex_pool;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [external scripts enabled 服务器配置选项](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)   
 [SQL Server R Services](../../advanced-analytics/r-services/sql-server-r-services.md)   
 [SQL Server R Services 的已知问题](../../advanced-analytics/r-services/known-issues-for-sql-server-r-services.md)   
 [CREATE EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/create-external-resource-pool-transact-sql.md)   
 [ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)   
 [DROP WORKLOAD GROUP (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)  
  
  
