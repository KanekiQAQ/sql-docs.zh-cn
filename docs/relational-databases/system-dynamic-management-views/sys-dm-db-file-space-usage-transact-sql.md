---
title: sys.dm_db_file_space_usage (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_file_space_usage
- sys.dm_db_file_space_usage_TSQL
- sys.dm_db_file_space_usage
- dm_db_file_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_file_space_usage dynamic management view
ms.assetid: 148a5276-a8d5-49d2-8146-3c63d24c2144
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 99d5ebc844c2cee8f92266cf5ea18b82666ea203
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68264469"
---
# <a name="sysdmdbfilespaceusage-transact-sql"></a>sys.dm_db_file_space_usage (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  返回数据库中每个文件的空间使用信息。  
  
> [!NOTE]  
>  若要调用此项从[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]或[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]，使用名称**sys.dm_pdw_nodes_db_file_space_usage**。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|数据库 ID。|  
|file_id|**smallint**|文件 ID。<br /><br /> file_id 映射到中的 file_id [sys.dm_io_virtual_file_stats](../../relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql.md)和到中的 fileid [sys.sysfiles](../../relational-databases/system-compatibility-views/sys-sysfiles-transact-sql.md)。|  
|filegroup_id|**smallint**|**适用范围**： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 文件组 ID。|  
|total_page_count|**bigint**|**适用范围**： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 文件中的总页数。|  
|allocated_extent_page_count|**bigint**|**适用范围**： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 文件的已分配区中的总页数。|  
|unallocated_extent_page_count|**bigint**|文件的未分配区中的总页数。<br /><br /> 不包括已分配区中的未使用页。|  
|version_store_reserved_page_count|**bigint**|为版本存储分配的统一区中的总页数。 永远不会从混合区分配版本存储页。<br /><br /> 不包括 IAM 页，因为 IAM 页始终从混合区进行分配。 如果 PFS 页是从统一区分配的，则包括 PFS 页。<br /><br /> 有关详细信息，请参阅 [sys.dm_tran_version_store (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md)。|  
|user_object_reserved_page_count|**bigint**|从统一区为数据库中的用户对象分配的总页数。 计数中包括已分配区中未使用的页。<br /><br /> 不包括 IAM 页，因为 IAM 页始终从混合区进行分配。 如果 PFS 页是从统一区分配的，则包括 PFS 页。<br /><br /> 可以使用中的 total_pages 列[sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)目录视图以返回用户对象中的每个分配单元的保留的页计数。 但请注意，total_pages 列包括 IAM 页。|  
|internal_object_reserved_page_count|**bigint**|从统一区为文件中的内部对象分配的总页数。 计数中包括已分配区中未使用的页。<br /><br /> 不包括 IAM 页，因为 IAM 页始终从混合区进行分配。 如果 PFS 页是从统一区分配的，则包括 PFS 页。<br /><br /> 不存在可返回每个内部对象的页计数的目录视图或动态管理对象。|  
|mixed_extent_page_count|**bigint**|文件的已分配混合区中的已分配和未分配总页数。 混合区包含分配给不同对象的页。 此计数包含文件中的所有 IAM 页。|
|modified_extent_page_count|**bigint**|**适用对象**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br />中修改的页的总数分配自上次完整数据库备份后的文件区块。 修改后的页计数可用于跟踪自上次完整备份，以确定是否需要差异备份后的数据库中的差异更改量。|
|pdw_node_id|**int**|**适用于**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]， [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 对于此分布的节点标识符。|  
|distribution_id|**int**|**适用于**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]， [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 与分发关联的唯一数字 id。|  
  
## <a name="remarks"></a>备注  
 页计数始终为区级计数。 所以，页计数的值始终为八的倍数。 包含全局分配映射表 (GAM) 和共享全局分配映射表 (SGAM) 分配页的区是已分配的统一区。 它们不包含在上文所述的页计数中。 有关页和区的详细信息，请参阅[页和区体系结构指南](../../relational-databases/pages-and-extents-architecture-guide.md)。 
  
 当前版本存储区的内容位于[sys.dm_tran_version_store](../../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md)。 在文件级而不是会话级和任务级跟踪版本存储页，因为它们是全局资源。 会话会生成版本，但在会话结束时不能删除版本。 版本存储清除必须考虑需要访问特定版本的运行时间最长的事务。 可以查看中的 elapsed_time_seconds 列来发现与版本存储清除相关的最长运行事务[sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md)。  
  
 mixed_extent_page_count 列频繁更改可能指示大量使用 SGAM 页。 如出现此情况，您会看到多个 PAGELATCH_UP 等待，且正在等待 SGAM 页资源。 有关详细信息，请参阅[sys.dm_os_waiting_tasks &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md)， [sys.dm_os_wait_stats &#40;-&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)，并[sys.dm_os_latch_统计信息&#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)。  
  
## <a name="user-objects"></a>用户对象  
 用户对象页计数器中包括下列对象：  
  
-   用户定义的表和索引  
  
-   系统表和索引  
  
-   全局临时表和索引  
  
-   局部临时表和索引  
  
-   表变量  
  
-   表值函数中返回的表  
  
## <a name="internal-objects"></a>内部对象  
 内部对象只包含在 tempdb 中。 内部对象页计数器中包括下列对象：  
  
-   用于游标或假脱机操作以及临时大型对象 (LOB) 存储的工作表  
  
-   用于哈希联接等操作的工作文件  
  
-   排序段  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|若要|关系|  
|----------|--------|------------------|  
|sys.dm_db_file_space_usage.database_id、file_id|sys.dm_io_virtual_file_stats.database_id、file_id|一对一|  
  
## <a name="permissions"></a>权限

上[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]，需要`VIEW SERVER STATE`权限。   
上[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]高级层，需要`VIEW DATABASE STATE`数据库中的权限。 上[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]标准版和基本层，需要**服务器管理员**或**Azure Active Directory 管理员**帐户。   

## <a name="examples"></a>示例  
  
### <a name="determing-the-amount-of-free-space-in-tempdb"></a>确定 tempdb 中的可用空间量  
 下面的查询返回兆字节 (MB) 可用中的所有文件中的可用页数和总可用空间总数**tempdb**。  
  
```sql
USE tempdb;  
GO  
SELECT SUM(unallocated_extent_page_count) AS [free pages],   
(SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]  
FROM sys.dm_db_file_space_usage;  
```  

### <a name="determining-the-amount-of-space-used-by-user-objects"></a>确定用户对象使用的空间量  
 下面的查询将返回 tempdb 中用户对象使用的总页数和总空间量。  
  
```sql  
USE tempdb;  
GO  
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],  
(SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]  
FROM sys.dm_db_file_space_usage;
```  
  
## <a name="see-also"></a>请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图&#40;Transact SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_db_task_space_usage &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)   
 [sys.dm_db_session_space_usage (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)  
