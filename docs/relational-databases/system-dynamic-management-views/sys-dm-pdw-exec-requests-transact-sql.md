---
title: sys.dm_pdw_exec_requests (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 390225cc-23e8-4051-a5f6-221e33e4c0b4
author: XiaoyuL-Preview
ms.author: xiaoyul
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 8e6514991c0819342861a50a2a50b37e7d8748cf
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67899400"
---
# <a name="sysdmpdwexecrequests-transact-sql"></a>sys.dm_pdw_exec_requests (Transact-SQL)

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  保存有关的所有请求的信息当前或最近中处于活动状态[!INCLUDE[ssSDW](../../includes/sssdw-md.md)]。 它列出了每个请求/查询对应一行。  
  
|列名|数据类型|描述|范围|  
|-----------------|---------------|-----------------|-----------|  
|request_id|**nvarchar(32)**|此视图的键。 与请求关联的唯一数字 ID。|在系统中的所有请求之间是唯一的。|  
|session_id|**nvarchar(32)**|与在其中运行此查询的会话相关联的唯一数字 ID。 请参阅[sys.dm_pdw_exec_sessions &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md)。||  
|status|**nvarchar(32)**|请求的当前状态。|正在运行、 挂起、 已完成、 已取消、 失败。|  
|submit_time|**datetime**|提交请求执行的时间。|有效**datetime**小于或等于当前时间和 start_time。|  
|start_time|**datetime**|请求执行开始时间。|对于排队的请求; 值为 NULL否则为有效**datetime**小于或等于当前时间。|  
|end_compile_time|**datetime**|引擎完成编译请求的时间。|对于尚未尚未编译; 的请求为 NULL否则为有效**datetime**小于 start_time 且小于或等于当前时间。|
|end_time|**datetime**|时间的请求执行将已完成、 失败或已取消。|对于排队或处于活动状态的请求; 值为 null否则为有效**datetime**小于或等于当前时间。|  
|total_elapsed_time|**int**|启动请求，以毫秒为单位后，在执行过程中已用时间。|介于 0 和 start_time 和 end_time 之间的差异。</br></br> 如果 total_elapsed_time 超出整数的最大值，将持续 total_elapsed_time 可最大值。 这种情况会生成警告"的最大值已超出。"</br></br> 以毫秒为单位的最大值是 24.8 天相同。|  
|标签|**nvarchar(255)**|与 SELECT 查询的一些语句相关联的可选标签字符串。|任何字符串包含 a-z、 A-Z、"0-9、 _。|  
|error_id|**nvarchar(36)**|如果有与请求关联的错误的唯一 ID。|请参阅[sys.dm_pdw_errors &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-errors-transact-sql.md); 如果没有出现任何错误设置为 NULL。|  
|database_id|**int**|显式上下文 (例如，使用 DB_X) 使用的数据库的标识符。|请参阅中的 ID [sys.databases &#40;TRANSACT-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)。|  
|command|**nvarchar(4000)**|包含用户提交的请求的完整文本。|任何有效的查询或请求文本。 长度超过 4000 字节的查询将被截断。|  
|resource_class|**nvarchar(20)**|用于此请求的资源类。 请参阅相关**concurrency_slots_used**中[sys.dm_pdw_resource_waits &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-resource-waits-transact-sql.md)。  资源类的详细信息，请参阅[资源类和工作负荷管理](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management) |静态资源类</br>staticrc10</br>staticrc20</br>staticrc30</br>staticrc40</br>staticrc50</br>staticrc60</br>staticrc70</br>staticrc80</br>            </br>动态资源类</br>SmallRC</br>MediumRC</br>LargeRC</br>XLargeRC|
|importance|**nvarchar(32)**|设置请求的重要性与已提交。 具有重要性较低的请求将保持排队状态处于挂起状态，如果提交较高的重要性请求。  之前已提交的较低重要性请求之前将执行请求的较高的优先级。  重要性的详细信息，请参阅[工作负荷重要性](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-workload-importance)。  |NULL</br>低</br>below_normal</br>正常 （默认值）</br>above_normal</br>高|
|group_name| |保留供内部使用。</br>适用范围：Azure SQL 数据仓库|
|resource_allocation_percentage| |保留供内部使用。</br>适用范围：Azure SQL 数据仓库|
|result_set_cache|**bit**|详细信息是否已完成的查询是结果缓存命中 (1) 还是失败 (0)。|0，1|
||||
  
 此视图按保留的最大行有关的信息，请参阅中的元数据部分[容量限制](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata)主题。   
  
## <a name="permissions"></a>权限

 需要 VIEW SERVER STATE 权限。  
  
## <a name="security"></a>安全性

 sys.dm_pdw_exec_requests 不会筛选查询结果根据特定于数据库的权限。 具有 VIEW SERVER STATE 权限的登录名可以获取结果的所有数据库的查询结果  
  
>[!WARNING]  
>攻击者可以使用 sys.dm_pdw_exec_requests 来检索有关特定数据库对象的信息，通过只需具有 VIEW SERVER STATE 权限以及不具有特定于数据库的权限。  
  
## <a name="see-also"></a>请参阅

 [SQL 数据仓库和并行数据仓库动态管理视图&#40;Transact SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md) 
