---
title: sys.dm_os_memory_cache_entries (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_memory_cache_entries
- sys.dm_os_memory_cache_entries
- dm_os_memory_cache_entries_TSQL
- sys.dm_os_memory_cache_entries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_entries dynamic management view
ms.assetid: dd32be6b-10d1-4059-b4fd-0bf817f40d54
author: stevestein
ms.author: sstein
ms.openlocfilehash: a96d536143c7e636045eb926e9240e047c78070b
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68265804"
---
# <a name="sysdmosmemorycacheentries-transact-sql"></a>sys.dm_os_memory_cache_entries (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中返回有关缓存中所有条目的信息。 使用此视图可对缓存条目进行跟踪，直至它们的关联对象。 还可使用此视图获取有关缓存条目的统计信息。  
  
> [!NOTE]  
>  若要调用此项从[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]或[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]，使用名称**sys.dm_pdw_nodes_os_memory_cache_entries**。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|缓存的地址。 不可为 null。|  
|**name**|**nvarchar(256)**|缓存的名称。 不可为 null。|  
|**type**|**varchar(60)**|缓存类型。 不可为 null。|  
|**entry_address**|**varbinary(8)**|缓存条目的描述符地址。 不可为 null。|  
|**entry_data_address**|**varbinary(8)**|缓存条目中用户数据的地址。<br /><br /> 0x00000000 = 条目数据地址不可用。<br /><br /> 不可为 null。|  
|**in_use_count**|**int**|同时使用此缓存条目的用户数。 不可为 null。|  
|**is_dirty**|**bit**|指示是否将此缓存条目标记为待删除。 1 = 标记为待删除。 不可为 null。|  
|**disk_ios_count**|**int**|创建此条目时引发的 I/O 数。 不可为 null。|  
|**context_switches_count**|**int**|创建此条目时引发的上下文开关数。 不可为 null。|  
|**original_cost**|**int**|此条目的原始开销。 此值是引发的 I/O 数、CPU 指令开销以及条目占用的内存量等的近似值。 开销越大，从缓存中删除此条目的机会越小。 不可为 null。|  
|**current_cost**|**int**|缓存条目的当前开销。 此值将在条目清除过程中更新。 重用条目时，当前开销将重置为原始值。 不可为 null。|  
|**memory_object_address**|**varbinary(8)**|关联内存对象的地址。 可以为 Null。|  
|**pages_allocated_count**|**bigint**|**适用范围**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]。<br /><br /> 存储此缓存条目的 8 KB 页的数目。 不可为 null。|  
|**pages_kb**|**bigint**|**适用范围**： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 此缓存条目使用的内存量 (KB)。  不可为 null。|  
|**entry_data**|**nvarchar(2048)**|缓存条目的序列化表示形式。 此信息与缓存存储相关。 可以为 Null。|  
|**pool_id**|**int**|**适用范围**： [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 与条目关联的资源池 ID。 可以为 Null。<br /><br /> 不是 katmai|  
|**pdw_node_id**|**int**|**适用于**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]， [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 对于此分布的节点标识符。|  
  
## <a name="permissions"></a>权限 

上[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]，需要`VIEW SERVER STATE`权限。   
上[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]高级层，需要`VIEW DATABASE STATE`数据库中的权限。 上[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]标准版和基本层，需要**服务器管理员**或**Azure Active Directory 管理员**帐户。   

## <a name="see-also"></a>请参阅  
 
  [与 SQL Server 操作系统相关的动态管理视图&#40;Transact SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


