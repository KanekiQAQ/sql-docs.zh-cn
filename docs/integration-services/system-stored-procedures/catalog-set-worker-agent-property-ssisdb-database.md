---
title: catalog.set_worker_agent_property（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: ddd2a534-6925-4d66-90e7-541c14f41de7
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 986807ab07a563fef1ae8a9231db194a4a5943e7
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68038676"
---
# <a name="catalogsetworkeragentproperty-ssisdb-database"></a>catalog.set_worker_agent_property（SSISDB 数据库）

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

设置 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Scale Out Worker 的属性。

## <a name="syntax"></a>语法

```sql
catalog.set_worker_agent_property [@WorkerAgentId =] WorkerAgentId, [@PropertyName =] PropertyName, [@PropertyValue =] PropertyValue 
```

## <a name="arguments"></a>参数
[@WorkerAgentId =] *WorkerAgentId*  
Scale Out Worker 的 Worker 代理 ID。 WorkerAgentId 为 uniqueidentifier   。

[@PropertyName =] *PropertyName*  
属性的名称。 *PropertyName* 为 **nvarchar(256)** 。

[@PropertyValue =] *PropertyValue*  
属性的值。 *PropertyValue* 为 **nvarchar(max)** 。

## <a name="remarks"></a>Remarks
有效的属性名称为 **DisplayName**、**Description**、**Tags**。

## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 None  

## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格

## <a name="errors-and-warnings"></a>错误和警告
  下面的列表描述了一些可能引发错误或警告的情况：  
  
-   用户没有相应的权限 

-   Worker 代理 ID 无效。

-   属性名称无效。

-   属性值无效。  
