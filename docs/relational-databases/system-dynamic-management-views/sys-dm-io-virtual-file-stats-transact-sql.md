---
title: sys.dm_io_virtual_file_stats (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_io_virtual_file_stats
- sys.dm_io_virtual_file_stats_TSQL
- sys.dm_io_virtual_file_stats
- dm_io_virtual_file_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_virtual_file_stats dynamic management function
ms.assetid: fa3e321f-6fe5-45ff-b397-02a0dd3d6b7d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 524f0d82b5f426ae41169b8358dd8ad8be66da03
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67900292"
---
# <a name="sysdmiovirtualfilestats-transact-sql"></a>sys.dm_io_virtual_file_stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-xxx-md.md)]

  返回数据和日志文件的 I/O 统计信息。 此动态管理视图取代[fn_virtualfilestats](../../relational-databases/system-functions/sys-fn-virtualfilestats-transact-sql.md)函数。  
  
> [!NOTE]  
>  若要调用此项从[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]，使用名称**sys.dm_pdw_nodes_io_virtual_file_stats**。 

## <a name="syntax"></a>语法  
  
```  
-- Syntax for SQL Server and Azure SQL Database

sys.dm_io_virtual_file_stats (   
    { database_id | NULL },  
    { file_id | NULL }  
)  
```  

```  
-- Syntax for Azure SQL Data Warehouse

sys.dm_pdw_nodes_io_virtual_file_stats
```
  
## <a name="arguments"></a>参数  


 *database_id* |为 NULL

 **适用对象：** SQL Server（从 2008 版开始）和 Azure SQL 数据库

 数据库 ID。 *database_id*数据类型为 int，无默认值。 有效的输入包括数据库的 ID 号或 NULL。 如果指定 NULL，则返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的所有数据库。  
  
 内置函数[DB_ID](../../t-sql/functions/db-id-transact-sql.md)可以指定。  
  
*file_id* |为 NULL

**适用对象：** SQL Server（从 2008 版开始）和 Azure SQL 数据库
 
文件的 ID。 *file_id*数据类型为 int，无默认值。 有效输入为文件 ID 号或为 NULL。 如果指定 NULL，则返回数据库中的所有文件。  
  
 内置函数[FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) ，还可以指定和引用当前数据库中的文件。  
  
## <a name="table-returned"></a>返回的表  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|数据库名称。</br></br>对于 SQL 数据仓库，这是数据库的存储在由 pdw_node_id 标识的节点上的名称。 每个节点都有一个具有 13 文件的 tempdb 数据库。 每个节点还具有一个数据库，每个分布区，并且每个分发数据库具有 5 个文件。 例如，如果每个节点包含 4 个分布区，则结果会显示 pdw_node_id 每 20 个分发数据库文件。 
|**database_id**|**smallint**|数据库的 ID。|  
|**file_id**|**smallint**|文件的 ID。|  
|**sample_ms**|**bigint**|自从计算机启动以来的毫秒数。 可以使用此列来比较该函数的不同输出。</br></br>数据类型是**int**为[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]通过 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|**num_of_reads**|**bigint**|对文件发出的读取次数。|  
|**num_of_bytes_read**|**bigint**|在此文件中读取的总字节数。|  
|**io_stall_read_ms**|**bigint**|用户等待文件中发出读取所用的总时间（毫秒）。|  
|**num_of_writes**|**bigint**|在该文件中写入的次数。|  
|**num_of_bytes_written**|**bigint**|写入文件的总字节数。|  
|**io_stall_write_ms**|**bigint**|用户等待在该文件中完成写入所用的总时间（毫秒）。|  
|**io_stall**|**bigint**|用户等待在文件中完成 I/O 操作所用的总时间（毫秒）。|  
|**size_on_disk_bytes**|**bigint**|该文件在磁盘上占用的字节数。 对于稀疏文件，此数字是数据库快照在磁盘上所占用的实际字节数。|  
|**file_handle**|**varbinary**|用于此文件的 Windows 文件句柄。|  
|**io_stall_queued_read_ms**|**bigint**|**不适用于：** :[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]通过[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)]。<br /><br /> 针对读的 IO 资源调控所引入的总 IO 延迟。 不可为 null。 有关详细信息，请参阅[sys.dm_resource_governor_resource_pools &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)。|  
|**io_stall_queued_write_ms**|**bigint**|**不适用于：** :[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]通过[!INCLUDE[ssSQL12](../../includes/sssql11-md.md)]。<br /><br />  针对写的 IO 资源调控所引入的总 IO 延迟。 不可为 null。|
|**pdw_node_id**|**int**|适用范围：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] </br></br>分发节点的标识符。
 
  
## <a name="permissions"></a>权限  
 需要 VIEW SERVER STATE 权限。 有关详细信息，请参阅[动态管理视图和函数&#40;TRANSACT-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)。  
  
## <a name="examples"></a>示例  

### <a name="a-return-statistics-for-a-log-file"></a>A. 返回一个日志文件的统计信息

**适用范围：** SQL Server（从 2008 版开始）和 Azure SQL 数据库

 以下示例将返回有关 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的日志文件的统计信息。  
  
```sql  
SELECT * FROM sys.dm_io_virtual_file_stats(DB_ID(N'AdventureWorks2012'), 2);  
GO  
```  
  
### <a name="b-return-statistics-for-file-in-tempdb"></a>B. 在 tempdb 中返回文件统计的信息

**适用范围：** Azure SQL 数据仓库

```sql
SELECT * FROM sys.dm_pdw_nodes_io_virtual_file_stats 
WHERE database_name = 'tempdb' AND file_id = 2;

```

## <a name="see-also"></a>请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [我 O 相关动态管理视图和函数&#40;Transact SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.master_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
  
  

