---
title: CREATE EXTERNAL RESOURCE POOL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL RESOURCE POOL
- CREATE EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE POOL
- EXTERNAL_RESOURCE_POOL_TSQL
- EXTERNAL RESOURCE
- EXTERNAL_RESOURCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL RESOURCE POOL statement
ms.assetid: 8cc798ad-c395-461c-b7ff-8c561c098808
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: dc83d3f63478001fccaabfd9fa431652434b0c6c
ms.sourcegitcommit: 9062c5e97c4e4af0bbe5be6637cc3872cd1b2320
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68471169"
---
# <a name="create-external-resource-pool-transact-sql"></a>CREATE EXTERNAL RESOURCE POOL (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss-xxxx-xxxx-xxx-md.md)]

创建用于定义外部进程资源的外部池。 资源池表示数据库引擎实例的物理资源（内存和 CPU）的子集。 数据库管理员可以使用资源调控器在多个资源池之间分发服务器资源，最多可为 64 个池。

+ 对于 [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] 中的[!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)]，外部池控制 `rterm.exe`、`BxlServer.exe` 以及它们生成的其他进程。

+ 对于 [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 中的 [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]，外部池控制 `rterm.exe`、`python.exe`、`BxlServer.exe`，以及它们生成的其他进程。

  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。  
  
## <a name="syntax"></a>语法  
  
```  
CREATE EXTERNAL RESOURCE POOL pool_name  
[ WITH (  
    [ MAX_CPU_PERCENT = value ]  
    [ [ , ] AFFINITY CPU =    
            {  
                AUTO   
              | ( <cpu_range_spec> )   
              | NUMANODE = ( <NUMA_node_id> )   
            } ]   
    [ [ , ] MAX_MEMORY_PERCENT = value ]  
    [ [ , ] MAX_PROCESSES = value ]   
    )   
]  
[ ; ]  
  
<CPU_range_spec> ::=    
{ CPU_ID | CPU_ID  TO CPU_ID } [ ,...n ]  
```  
  
## <a name="arguments"></a>参数

pool_name   
外部资源池的用户定义名称。 pool_name  是字母数字，最多可包含 128 个字符。 此参数在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例中必须是唯一的，并且必须符合[标识符](../../relational-databases/databases/database-identifiers.md)的规则。  

MAX_CPU_PERCENT =value   
指定出现 CPU 争用时，外部资源池中的所有请求可以接收的最大平均 CPU 带宽。 value 为整数且默认设置为 100  。 value 的允许范围是 1 到 100  。

AFFINITY {CPU = AUTO | ( \<CPU_range_spec> ) | NUMANODE = (\<NUMA_node_range_spec>)} 将外部资源池附加到特定 CPU。 默认值为 AUTO。

AFFINITY CPU = ( \<CPU_range_spec> ) 将外部资源池映射到给定 CPU_ID 标识的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CPU   。

使用 AFFINITY NUMANODE = ( \<NUMA_node_range_spec> ) 时，外部资源池将关联到与给定 NUMA 节点或一系列节点相对应的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 物理 CPU   。 

MAX_MEMORY_PERCENT =value   
指定此外部资源池中的请求可使用的总服务器内存量。 value 为整数且默认设置为 100  。 value 的允许范围是 1 到 100  。

MAX_PROCESSES =value   
指定外部资源池允许的最大进程数。 指定 0 以便为池设置无限阈值，此阈值之后仅受计算机资源约束。 默认值为 0。

## <a name="remarks"></a>Remarks

执行 [ALTER RESOURCE GOVERNOR RECONFIGURE](../../t-sql/statements/alter-resource-governor-transact-sql.md) 语句时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]实现资源池。

有关资源池的常规信息，请参阅 [Resource Governor 资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)、[sys.resource_governor_external_resource_pools (Transact-SQL)](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md) 和 [sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)。

有关管理用于机器学习的外部资源池的详细信息，请参阅 [SQL Server 中机器学习的资源调控](../../advanced-analytics/r/resource-governance-for-r-services.md)。 

## <a name="permissions"></a>权限

需要 `CONTROL SERVER` 权限。

## <a name="examples"></a>示例

下面的语句定义了一个将 CPU 使用率限制为 75% 的外部池。 该语句还将最大内存定义为计算机上可用内存的 30%。

```sql
CREATE EXTERNAL RESOURCE POOL ep_1
WITH (  
    MAX_CPU_PERCENT = 75
    , AFFINITY CPU = AUTO
    , MAX_MEMORY_PERCENT = 30
);
GO
ALTER RESOURCE GOVERNOR RECONFIGURE;
GO
```
  
## <a name="see-also"></a>另请参阅

[启用了外部脚本的服务器配置选项](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)
[sp_execute_external_script &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
[ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)
[DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)
[CREATE RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-resource-pool-transact-sql.md)
[CREATE WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-group-transact-sql.md)
[资源调控器资源池](../../relational-databases/resource-governor/resource-governor-resource-pool.md)
[sys.resource_governor_external_resource_pools &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-external-resource-pools-transact-sql.md)
[sys.dm_resource_governor_external_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md)
[ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)
