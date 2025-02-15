---
title: sys.resource_governor_resource_pools (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.resource_governor_resource_pools
- resource_governor_resource_pools
- sys.resource_governor_resource_pools_TSQL
- resource_governor_resource_pools_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.resource_governor_resource_pools catalog view
ms.assetid: 56793e9c-aa90-452e-88c6-d9b799239888
author: stevestein
ms.author: sstein
ms.openlocfilehash: 06ed4b4820ce7a6e6483df6efd1de2e8fbadf954
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67904458"
---
# <a name="sysresourcegovernorresourcepools-transact-sql"></a>sys.resource_governor_resource_pools (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的存储资源池配置。 视图的每一行都确定了一个池的配置。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|资源池的唯一 ID。 不可为 null。|  
|name|**sysname**|资源池的名称。 不可为 null。|  
|min_cpu_percent|**int**|出现 CPU 争用时资源池中的所有请求可获得的有保证的平均 CPU 带宽。 不可为 null。|  
|max_cpu_percent|**int**|出现 CPU 争用时资源池中的所有请求可获得的最大 CPU 带宽。 不可为 null。|  
|min_memory_percent|**int**|资源池中的所有请求可获得的有保证的内存量。 不与其他资源池共享这部分内存。 不可为 null。|  
|max_memory_percent|**int**|此资源池中的请求可使用的总服务器内存量的百分比。 不可为 null。 有效的最大值取决于池的最小值。 例如，可将 max_memory_percent 设置为 100，但有效的最大值会更低一些。|  
|cap_cpu_percent|**int**|**适用范围**： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 资源池中的所有请求都将收到的 CPU 带宽硬性上限。 将 CPU 最大带宽限制为指定的级别。 允许的值范围为 1 到 100。|  
|min_iops_per_volume|**int**|**适用范围**： [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 针对该池的每个卷设置的每秒最小 I/O 操作数 (IOPS)。 0 = 不预留。 不可为 null。|  
|max_iops_per_volume|**int**|**适用范围**： [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 针对该池的每个卷设置的每秒最大 I/O 操作数 (IOPS)。 0 = 无限制。 不可为 null。|  
  
## <a name="remarks"></a>备注  
 目录视图显示存储的元数据。 若要查看内存中的配置，请使用对应的动态管理视图[sys.dm_resource_governor_resource_pools &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 若要查看内容，则需要拥有 VIEW ANY DEFINITION 权限；若要更改内容，则需要拥有 CONTROL SERVER 权限。  
  
## <a name="see-also"></a>请参阅  
 [资源调控器目录视图&#40;Transact SQL&#41;](../../relational-databases/system-catalog-views/resource-governor-catalog-views-transact-sql.md)   
 [sys.dm_resource_governor_resource_pools &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)   
 [资源调控器](../../relational-databases/resource-governor/resource-governor.md)   
 [sys.resource_governor_external_resource_pools &#40;TRANSACT-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)  
  
  
