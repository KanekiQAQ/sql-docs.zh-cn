---
title: sys.database_files (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.database_files
- sys.database_files_TSQL
- database_files
- database_files_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_files catalog view
ms.assetid: 0f5b0aac-c17d-4e99-b8f7-d04efc9edf44
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 37ec05a27421b8b55fb0085dbac97ab564bc5bff
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67915097"
---
# <a name="sysdatabasefiles-transact-sql"></a>sys.database_files (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  每个存储在数据库本身中的数据库文件在表中占用一行。 这是一个基于每个数据库的视图。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**file_id**|**int**|数据库内文件的 ID。|  
|**file_guid**|**uniqueidentifier**|文件的 GUID。<br /><br /> NULL = 数据库是从早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 升级的。|  
|**type**|**tinyint**|文件类型：<br /><br /> 0 = 行（包括升级到的或在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中创建的全文目录的文件。）<br /><br /> 1 = 日志<br /><br /> 2 = FILESTREAM<br /><br /> 3 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 4 = 全文（[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]之前的全文目录；升级到的或在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中创建的全文目录将报告文件类型 0。）|  
|**type_desc**|**nvarchar(60)**|文件类型的说明：<br /><br /> ROWS（包括升级到的或在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中创建的全文目录的文件）。<br /><br /> LOG<br /><br /> FILESTREAM<br /><br /> FULLTEXT（[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 之前的全文目录。）|  
|**data_space_id**|**int**|该值可以是 0 或大于 0。 值为 0 表示数据库日志文件，值大于 0 表示存储此数据文件的文件组的 ID。|  
|**名称**|**sysname**|数据库中文件的逻辑名称。|  
|**physical_name**|nvarchar(260) |操作系统文件名。 如果数据库承载由某一 AlwaysOn[可读辅助副本](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)， **physical_name**指示主副本数据库的文件位置。 对于正确的文件位置的可读辅助数据库，查询[sys.sysaltfiles](../../relational-databases/system-compatibility-views/sys-sysaltfiles-transact-sql.md)。|  
|State |**tinyint**|文件状态：<br /><br /> 0 = ONLINE<br /><br /> 1 = RESTORING<br /><br /> 2 = RECOVERING<br /><br /> 3 = RECOVERY_PENDING<br /><br /> 4 = SUSPECT<br /><br /> 5 = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> 6 = OFFLINE<br /><br /> 7 = DEFUNCT|  
|**state_desc**|**nvarchar(60)**|文件状态的说明：<br /><br /> ONLINE<br /><br /> RESTORING<br /><br /> RECOVERING<br /><br /> RECOVERY_PENDING<br /><br /> SUSPECT<br /><br /> OFFLINE<br /><br /> DEFUNCT<br /><br /> 有关详细信息，请参阅[文件状态](../../relational-databases/databases/file-states.md)。|  
|size |**int**|文件的当前大小（以 8 KB 页为单位）。<br /><br /> 0 = 不适用<br /><br /> 对于数据库快照来说，size 表示该快照可以一直用于文件的最大空间。<br /><br /> FILESTREAM 文件组容器大小反映当前使用的容器的大小。|  
|**max_size**|**int**|最大文件大小（以 8 KB 为单位的页数）：<br /><br /> 0 = 不允许增长。<br /><br /> -1 = 文件将一直增长到磁盘充满为止。<br /><br /> 268435456 = 日志文件将增长到最大大小 2 TB。<br /><br /> 对于 FILESTREAM 文件组容器，max_size 反映容器的最大大小。<br /><br /> 请注意不受限制的日志文件大小与升级的数据库将报告为-1 的日志文件的最大大小。|  
|**增长**|**int**|0 = 文件大小固定，不会增长。<br /><br /> > 0 = 文件将自动增长。<br /><br /> is_percent_growth = 0，则增量以 8 KB 页为单位，并舍入到最近的 64 KB。<br /><br /> 如果 is_percent_growth = 1，增量将用整数百分比表示。|  
|**is_media_read_only**|**bit**|1 = 文件位于只读介质上。<br /><br /> 0 = 文件位于读写介质上。|  
|**is_read_only**|**bit**|1 = 文件标记为只读。<br /><br /> 0 = 文件标记为读/写。|  
|**is_sparse**|**bit**|1 = 文件是稀疏文件。<br /><br /> 0 = 文件不是稀疏文件。<br /><br /> 有关详细信息，请参阅[查看数据库快照的稀疏文件大小 (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md)。|  
|**is_percent_growth**|**bit**|1 = 文件的增长以百分比表示。<br /><br /> 0 = 以页数为单位表示绝对增长大小。|  
|**is_name_reserved**|**bit**|1 = 只有在下一次日志备份之后才能重用删除的文件名（name 或 physical_name）。 从数据库删除文件时，逻辑名称将停留在保留状态，直到下一次日志备份。 此列只在完整恢复模式和大容量日志恢复模式下相关。|  
|**create_lsn**|**numeric(25,0)**|文件创建时的日志序列号 (LSN)。|  
|**drop_lsn**|**numeric(25,0)**|文件删除时的 LSN。<br /><br /> 0 = 文件名无法重用。|  
|**read_only_lsn**|**numeric(25,0)**|包含该文件的文件组从可读/写更改为只读（最新更改）时的 LSN。|  
|**read_write_lsn**|**numeric(25,0)**|包含该文件的文件组从只读更改为可读/写（最新更改）时的 LSN。|  
|**differential_base_lsn**|**numeric(25,0)**|差异备份的基准。 在此 LSN 之后更改的数据区将包含在差异备份中。|  
|**differential_base_guid**|**uniqueidentifier**|差异备份所基于的基准备份的唯一标识符。|  
|**differential_base_time**|**datetime**|与 differential_base_lsn 相对应的时间。|  
|**redo_start_lsn**|**numeric(25,0)**|下一次前滚必须开始时的 LSN。<br /><br /> 除非 state = RESTORING 或 state = RECOVERY_PENDING，否则为 NULL。|  
|**redo_start_fork_guid**|**uniqueidentifier**|恢复分叉的唯一标识符。 还原的下一个日志备份的 first_fork_guid 必须与此值匹配。 这表示文件的当前状态。|  
|**redo_target_lsn**|**numeric(25,0)**|对此文件的联机前滚可以停止时的 LSN。<br /><br /> 除非 state = RESTORING 或 state = RECOVERY_PENDING，否则为 NULL。|  
|**redo_target_fork_guid**|**uniqueidentifier**|可以在其上恢复文件的恢复分叉。 与 redo_target_lsn 成对使用。|  
|**backup_lsn**|**numeric(25,0)**|文件的最新数据或差异备份的 LSN。|  
  
> [!NOTE]  
>  在删除或重新生成大型索引时，或者在删除或截断大型表时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将延迟实际页释放及其关联锁，直至事务提交完毕为止。 延迟的删除操作不会立即释放已分配的空间。 因此，在删除或截断大型对象后由 sys.database_files 立即返回的值可能不反映实际可用的磁盘空间。  
  
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  

## <a name="examples"></a>示例  
以下语句返回名称、 文件大小和每个数据库文件的空白空间量。

```
SELECT name, size/128.0 FileSizeInMB,
size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 
   AS EmptySpaceInMB
FROM sys.database_files;
```
有关详细信息时使用[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]，请参阅[Azure SQL 数据库 V12 中确定数据库大小](https://blogs.msdn.microsoft.com/sqlcat/2016/09/21/determining-database-size-in-azure-sql-database-v12/)SQL 客户顾问团队博客上。
  
## <a name="see-also"></a>请参阅  
 [数据库和文件目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [文件状态](../../relational-databases/databases/file-states.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.master_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)   
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [sys.data_spaces (Transact-SQL)](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)  
  
  
