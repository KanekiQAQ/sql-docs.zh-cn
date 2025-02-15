---
title: catalog.add_execution_worker（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: d587cedd-6402-4d5c-9526-7cd25627a037
author: janinezhang
ms.author: janinez
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: 5dc8e4283a8443c41be994745310114587712e3a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68110556"
---
# <a name="catalogaddexecutionworker-ssisdb-database"></a>catalog.add_execution_worker（SSISDB 数据库）

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md](../../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]

将 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Scale Out Worker 添加到 Scale Out 中的一个执行实例。

## <a name="syntax"></a>语法

```sql
catalog.add_execution_worker [@execution_id = ] execution_id, [@workeragent_id = ] workeragent_id
```

## <a name="arguments"></a>参数
[ @execution_id = ] execution_id  
 执行实例的唯一标识符。 execution_id 为 bigint。  
 
[@workeragent_id = ] workeragent_id  
Scale Out Worker 的辅助角色代理 ID。 workeragent_id 为 uniqueIdentifier。

## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 None  

## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对执行实例的 READ 和 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
 
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
 
- 用户不具备适当的权限。

- 执行标识符无效。

- 辅助角色代理 ID 无效。

- 执行不在 Scale Out 中进行。

## <a name="see-also"></a>另请参阅
[在 Scale Out 中执行包](~/integration-services/scale-out/run-packages-in-integration-services-ssis-scale-out.md)。

