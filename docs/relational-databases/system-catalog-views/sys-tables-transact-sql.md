---
title: sys.tables (Transact SQL) |Microsoft Docs
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- tables_TSQL
- sys.tables_TSQL
- sys.tables
- tables
dev_langs:
- TSQL
helpviewer_keywords:
- sys.tables catalog view
ms.assetid: 8c42eba1-c19f-4045-ac82-b97a5e994090
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6cce3b4f08fcb55530ffd7abf6da3011c325478f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68055378"
---
# <a name="systables-transact-sql"></a>sys.tables (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的每个用户表返回一行。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|\<继承列 >||此视图所继承的列的列表，请参阅[sys.objects &#40;TRANSACT-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。|  
|lob_data_space_id|**int**|对于该表，非零值是存放二进制大型对象 (LOB) 数据的数据空间（文件组或分区方案）的 ID。 LOB 数据类型的示例包括**varbinary （max)** ， **varchar （max)** ， **geography**，或者**xml**。<br /><br /> 0 = 该表没有 LOB 数据。|  
|filestream_data_space_id|**int**|FILESTREAM 文件组或包含 FILESTREAM 文件组的分区方案的数据空间 ID。<br /><br /> 若要报告 FILESTREAM 文件组的名称，执行查询`SELECT FILEGROUP_NAME (filestream_data_space_id) FROM sys.tables`。<br /><br /> sys.tables 可以按 filestream_data_space_id = data_space_id 与下列视图联接。<br /><br /> -sys.filegroups<br /><br /> -sys.partition_schemes<br /><br /> -sys.indexes<br /><br /> -sys.allocation_units<br /><br /> -sys.fulltext_catalogs<br /><br /> -sys.data_spaces<br /><br /> -sys.destination_data_spaces<br /><br /> -sys.master_files<br /><br /> -sys.database_files<br /><br /> -backupfilegroup （按 filegroup_id 联接）|  
|max_column_id_used|**int**|此表曾使用的最大列 ID。|  
|lock_on_bulk_load|**bit**|大容量加载期间将锁定表。 有关详细信息，请参阅 [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)。|  
|uses_ansi_nulls|**bit**|在创建表时，将 SET ANSI_NULLS 数据库选项设置为 ON。|  
|is_replicated|**bit**|1 = 使用快照复制或事务复制发布表。|  
|has_replication_filter|**bit**|1 = 表具有复制筛选器。|  
|is_merge_published|**bit**|1 = 使用合并复制发布表。|  
|is_sync_tran_subscribed|**bit**|1 = 使用立即更新订阅来订阅表。|  
|has_unchecked_assembly_data|**bit**|1 = 表包含依赖于上次 ALTER ASSEMBLY 期间定义发生更改的程序集的持久化数据。 在下一次成功执行 DBCC CHECKDB 或 DBCC CHECKTABLE 后将重置为 0。|  
|text_in_row_limit|**int**|Text in row 允许的最大字节数。<br /><br /> 0 = 未设置 Text in row 选项。 有关详细信息，请参阅 [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)。|  
|large_value_types_out_of_row|**bit**|1 = 在行外存储大值类型。 有关详细信息，请参阅 [sp_tableoption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)。|  
|is_tracked_by_cdc|**bit**|1 = 为表启用变更数据捕获。 有关详细信息，请参阅[sys.sp_cdc_enable_table &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)。|  
|lock_escalation|**tinyint**|表的 LOCK_ESCALATION 选项的值如下：<br /><br /> 0 = TABLE<br /><br /> 1 = DISABLE<br /><br /> 2 = AUTO|  
|lock_escalation_desc|**nvarchar(60)**|表的 lock_escalation 选项的文本说明。 可能的值有：表、 AUTO 和 DISABLE。|  
|is_filetable|**bit**|适用范围：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。<br /><br /> 1 = 表是 FileTable。<br /><br /> 有关 FileTable 的详细信息，请参阅 [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md)。|  
|持续性|**tinyint**|适用范围：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。<br /><br /> 以下列出的是可能的值：<br /><br /> 0 = SCHEMA_AND_DATA<br /><br /> 1 = SCHEMA_ONLY<br /><br /> 值为 0 是默认值。|  
|durability_desc|**nvarchar(60)**|适用范围：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。<br /><br /> 下面是可能的值：<br /><br /> SCHEMA_ONLY<br /><br /> SCHEMA_AND_DATA<br /><br /> SCHEMA_AND_DATA 的值指示表是持久内存中表。 SCHEMA_AND_DATA 是内存优化表的默认值。 SCHEMA_ONLY 的值指示，在重新启动包含内存优化对象的数据库后，表数据将不会持久化。|  
|is_memory_optimized|**bit**|适用范围：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。<br /><br /> 下面是可能的值：<br /><br /> 0 = 非内存优化。<br /><br /> 1 = 内存优化。<br /><br /> 值 0 为默认值。<br /><br /> 内存优化表是内存中用户表，类似于其他用户表的磁盘保留的架构。 内存优化的表可访问从本机编译存储的过程。|  
|temporal_type|**tinyint**|适用范围：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。<br /><br /> 表示表的类型的数值：<br /><br /> 0 = NON_TEMPORAL_TABLE<br /><br /> 1 = HISTORY_TABLE<br /><br /> 2 = SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|temporal_type_desc|**nvarchar(60)**|适用范围：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。<br /><br /> 表的类型的文本说明：<br /><br /> NON_TEMPORAL_TABLE<br /><br /> HISTORY_TABLE<br /><br /> SYSTEM_VERSIONED_TEMPORAL_TABLE|  
|history_table_id|**int**|适用范围：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。<br /><br /> 当 temporal_type IN （2，4） 返回 object_id 的表的维护历史数据，否则返回 NULL。|  
|is_remote_data_archive_enabled|**bit**|**适用于**:[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]通过[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]<br /><br /> 指示是否已启用延伸的表。<br /><br /> 0 = 表不是已启用延伸的。<br /><br /> 1 = 表是已启用延伸的。<br /><br /> 有关详细信息，请参阅 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)。|  
|is_external|**bit**|**适用于**:[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]通过[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]， [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]，和[!INCLUDE[sssdwfull](../../includes/sssdwfull-md.md)]。<br /><br /> 指示表是外部表。<br /><br /> 0 = 表不是外部表。<br /><br /> 1 = 表是外部表。| 
|history_retention_period|**int**|**适用于**： [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]。 <br/><br/>表示与 history_retention_period_unit 指定单位的时态历史记录保持期的持续时间的数值。 |  
|history_retention_period_unit|**int**|**适用于**： [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]。 <br/><br/>表示类型的时态历史记录保持期单位数值。 <br /><br />-1： 无限 <br /><br />3：DAY <br /><br />4：WEEK <br /><br />5：MONTH <br /><br />6:YEAR |  
|history_retention_period_unit_desc|**nvarchar(10)**|**适用于**： [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]。 <br/><br/>时态历史记录保持期单位类型的文本说明。 <br /><br />INFINITE <br /><br />DAY <br /><br />WEEK <br /><br />MONTH <br /><br />YEAR |  
|is_node|**bit**|适用范围：[!INCLUDE[sssql17-md.md](../../includes/sssql17-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。 <br/><br/>1 = 这是一个图形节点表。 <br /><br />0 = 这不是图形节点表。 |  
|is_edge|**bit**|适用范围：[!INCLUDE[sssql17-md.md](../../includes/sssql17-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。 <br/><br/>1 = 这是一个图形边缘表。 <br /><br />0 = 这不是图形边缘表。 |  

## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
 下面的示例返回没有主键的所有用户表。  
  
```  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasPrimaryKey') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
下面的示例演示如何相关的临时数据可以公开。  
   
适用范围：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]  。
  
```  
SELECT T1.object_id, T1.name as TemporalTableName, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,  
T2.name as HistoryTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,  
T1.temporal_type_desc  
FROM sys.tables T1  
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id  
ORDER BY T1.temporal_type desc  
```  

下面的示例演示如何可以公开时态历史记录保持期的信息。  

**适用于**： [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)]。  
  
```  
SELECT DB.is_temporal_history_retention_enabled, SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema, 
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema, T2.name as HistoryTableName,
T1.history_retention_period, T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases where name = DB_NAME()) DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2 
```  
  
## <a name="see-also"></a>请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [DBCC CHECKDB (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)   
 [DBCC CHECKTABLE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
