---
title: sys.stats (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.stats
- stats_TSQL
- sys.stats_TSQL
- stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.stats catalog view
ms.assetid: 42605c80-126f-460a-befb-a0b7482fae6a
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1a50bf492641da227f355f1e844a3e9d7156d93e
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68108964"
---
# <a name="sysstats-transact-sql"></a>sys.stats (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的数据库中的表、索引和索引视图对应的每个统计信息对象都包含一行。 每个索引将有对应的统计信息行具有相同名称和 ID (**index_id** = **stats_id**)，但不是每个统计信息行都有对应的索引。  
  
 目录视图[sys.stats_columns](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)提供数据库中每个列的统计信息。 有关统计信息的详细信息，请参阅[统计信息](../../relational-databases/statistics/statistics.md)。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|这些统计信息所属对象的 ID。|  
|**name**|**sysname**|统计信息的名称。 在对象中是唯一的。|  
|**stats_id**|**int**|统计信息 ID。 在对象中是唯一的。<br /><br />如果统计信息对应于索引， *stats_id*值等同于*index_id*中的值[sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)目录视图。|  
|**auto_created**|**bit**|指示统计信息是否由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动创建。<br /><br /> 0 = 统计信息不是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动创建。<br /><br /> 1 = 统计信息由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动创建。|  
|**user_created**|**bit**|指示统计信息是否由用户创建。<br /><br /> 0 = 统计信息不是用户创建的。<br /><br /> 1 = 统计信息是用户创建的。|  
|**no_recompute**|**bit**|指示统计信息已创建了**NORECOMPUTE**选项。<br /><br /> 0 = 统计信息创建时未使用**NORECOMPUTE**选项。<br /><br /> 1 = 使用创建的统计信息**NORECOMPUTE**选项。|  
|**has_filter**|**bit**|0 = 统计信息不具有筛选器并且针对所有行进行计算。<br /><br /> 1 = 统计信息具有筛选器并且仅针对满足筛选器定义的行进行计算。|  
|**filter_definition**|**nvarchar(max)**|包含在筛选统计信息中的行子集的表达式。<br /><br /> NULL = 非筛选的统计信息。|  
|**is_temporary**|**bit**|**适用范围**： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 指示统计信息是否是临时的。 临时统计信息支持 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 辅助数据库（支持只读访问）。<br /><br /> 0 = 统计信息不是临时的。<br /><br /> 1 = 统计信息是临时的。|  
|**is_incremental**|**bit**|**适用范围**： [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 指示统计信息是否作为增量统计信息创建。<br /><br /> 0 = 统计信息不是增量统计信息。<br /><br /> 1 = 统计信息是增量统计信息。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
 下面的示例返回 `HumanResources.Employee` 表的所有统计信息和统计信息列。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT s.name AS statistics_name  
      ,c.name AS column_name  
      ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('HumanResources.Employee');  
```  
  
## <a name="see-also"></a>请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [统计信息](../../relational-databases/statistics/statistics.md)    
 [sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.dm_db_stats_histogram &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)   
 [sys.stats_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-stats-columns-transact-sql.md)
 

 
