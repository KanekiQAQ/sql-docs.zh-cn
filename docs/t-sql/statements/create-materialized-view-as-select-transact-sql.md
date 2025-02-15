---
title: CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: data-warehouse
ms.topic: language-reference
f1_keywords:
- CREATE VIEW
- VIEW_TSQL
- VIEW
- CREATE_VIEW_TSQL
- SCHEMABINDING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table creation [SQL Server], CREATE VIEW
- views [SQL Server], creating
- CREATE VIEW statement
- updatable partitioned views
- tables [SQL Server], virtual
- updatable views
- modifying partitioned views
- virtual tables [SQL Server]
- number of columns per view
- partitioned views [SQL Server], creating
- WITH ENCRYPTION clause
- WITH CHECK OPTION clause
- partitioned views [SQL Server], modifying
- partitioned views [SQL Server], replication
- distributed partitioned views [SQL Server]
- views [SQL Server], indexed views
- maximum number of columns per view
ms.assetid: aecc2f73-2ab5-4db9-b1e6-2f9e3c601fb9
author: XiaoyuL-Preview
ms.author: xiaoyul
monikerRange: =azure-sqldw-latest||=sqlallproducts-allversions
ms.openlocfilehash: 076bf71586baa61e8bb77c093cd274eca898bf00
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67912622"
---
# <a name="create-materialized-view-as-select-transact-sql-preview"></a>CREATE MATERIALIZED VIEW AS SELECT (Transact-SQL)（预览）

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md)]

本文说明如何在 Azure SQL 数据仓库中使用 CREATE MATERIALIZED VIEW AS SELECT T-SQL 语句开发解决方案。 此外，本文还提供了代码示例。

具体化视图将保留视图定义查询返回的数据，并自动随着基础表中的数据更改而更新。   它可以提高复杂查询的性能（通常指使用联接和聚合的查询），同时还提供简单的维护操作。   由于具体化视图具有执行计划自动匹配功能，因此无需在查询中引用它，优化器即会考虑将此视图作为替换项。  这样数据工程师即可以将具体化视图作为改进查询响应时间的机制来实现，而不必再更改查询。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
CREATE MATERIALIZED VIEW [ schema_name. ] materialized_view_name
    WITH (  
      <distribution_option>
    )
    AS <select_statement>
[;]

<distribution_option> ::=
    {  
        DISTRIBUTION = HASH ( distribution_column_name )  
      | DISTRIBUTION = ROUND_ROBIN  
    }

<select_statement> ::=
    SELECT select_criteria
```  
  
## <a name="arguments"></a>参数

schema_name      
 视图所属架构的名称。  
  
materialized_view_name     
视图名称。 视图名称必须符合有关标识符的规则。 可以选择是否指定视图所有者名称。  

分布选项       
仅支持 HASH 和 ROUND_ROBIN 分步。

select_statement     
具体化视图定义中的 SELECT 列表需要至少满足以下两个条件之一：
- SELECT 列表包含聚合函数。
- 具体化视图定义使用了 GROUP BY，并且 SELECT 列表包括 GROUP BY 中的所有列。  

具体化视图定义的 SELECT 列表必须包含聚合函数。  支持的聚合包括 MAX、MIN、AVG、COUNT、COUNT_BIG、SUM、VAR、STDEV。

具体化视图定义的 SELECT 列表使用 MIN/MAX 聚合时，以下要求适用：
 
- 必须使用 FOR_APPEND。  例如：
  ```sql 
  CREATE MATRIALIZED VIEW mv_test2  
  WITH (distribution = hash(i_category_id), FOR_APPEND)  
  AS
  SELECT MAX(i.i_rec_start_date) as max_i_rec_start_date, MIN(i.i_rec_end_date) as min_i_rec_end_date, i.i_item_sk, i.i_item_id, i.i_category_id
  FROM syntheticworkload.item i  
  GROUP BY i.i_item_sk, i.i_item_id, i.i_category_id
  ```

- 引用的基表出现 UPDATE 或 DELETE 时，将禁用具体化视图。  此限制不适用于 INSERT。  若要重新启用具体化视图，请运行 ALTER MATERIALIZED INDEX 和 REBUILD。
  
## <a name="remarks"></a>Remarks

Azure 数据仓库的具体化视图与 SQL Server 的索引视图非常相似。  除了具体化视图支持聚合函数外，它与索引视图适用的限制几乎相同（请参阅[创建索引视图](/sql/relational-databases/views/create-indexed-views)了解详细信息）。   以下是关于具体化视图的其他注意事项。  
 
具体化视图仅支持 CLUSTERED COLUMNSTORE INDEX。 
 
可以通过 DROP VIEW 删除具体化视图。  可以使用 ALTER MATERIALIZED VIEW 禁用或重新生成具体化视图。   
 
可以在已分区表上创建具体化视图。  具体化视图引用的表支持 SPLIT/MERGE 操作。  具体化视图引用的表不支持 SWITCH。 若尝试执行此操作，用户将看到错误 `Msg 106104, Level 16, State 1, Line 9`
 
具体化视图引用的表不支持 ALTER TABLE SWITCH。 请先禁用或删除具体化视图，然后再使用 ALTER TABLE SWITCH。 在以下应用场景中，需要向具体化视图添加新列，才能创建具体化视图：

|应用场景|要添加到具体化视图的新列|注释|  
|-----------------|---------------|-----------------|
|COUNT_BIG() | 具体化视图定义的 SELECT 列表缺少它 |COUNT_BIG (*) |通过具体化视图创建自动添加。  不需要任何用户操作。|
|由用户在具体化视图定义的 SELECT 列表中指定 SUM(a)，其中“a”是可为 null 的表达式 |COUNT_BIG (a) |用户需要手动将表达式“a”添加到具体化视图定义中。|
|由用户在具体化视图定义的 SELECT 列表中指定 AVG(a)，其中“a”是表达式。|SUM(a), COUNT_BIG(a)|通过具体化视图创建自动添加。  不需要任何用户操作。|
|由用户在具体化视图定义的 SELECT 列表中指定 STDEV(a)，其中“a”是表达式。|SUM(a),  
COUNT_BIG(a) SUM(square(a))|通过具体化视图创建自动添加。  不需要任何用户操作。 |
| | | |

创建后，SQL Server Management Studio 中的 Azure SQL 数据仓库实例的视图文件夹将显示具体化实体。

用户可以运行 [SP_SPACEUSED](/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql?view=azure-sqldw-latest) 和 [DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql?view=azure-sqldw-latest) 来确定具体化视图占用的空间。  

SQL Server Management Studio 中的 EXPLAIN 计划和图形估计执行计划可以显示查询优化器是否考虑将具体化视图用于查询执行。 SQL Server Management Studio 中的图形估计执行计划可以显示查询优化器是否考虑将具体化视图用于查询执行。

若要了解 SQL 语句是否可以从新的具体化视图受益，请运行 `EXPLAIN` 命令和 `WITH_RECOMMENDATIONS`。  有关详细信息，请参阅 [EXPLAIN (Transact-SQL)](/sql/t-sql/queries/explain-transact-sql?view=azure-sqldw-latest)。

## <a name="permissions"></a>权限

要求在数据库中具有 CREATE VIEW 权限，并具有在其中创建视图的架构的 ALTER 权限。 
  
## <a name="see-also"></a>另请参阅

[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](/sql/t-sql/statements/alter-materialized-view-transact-sql?view=azure-sqldw-latest)   
[EXPLAIN &#40;Transact-SQL&#41;](/sql/t-sql/queries/explain-transact-sql?view=azure-sqldw-latest)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](/sql/relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql?view=azure-sqldw-latest)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](/sql/relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql?view=azure-sqldw-latest)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](/sql/relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql?view=azure-sqldw-latest)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](/sql/t-sql/database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql?view=azure-sqldw-latest)   
[SQL 数据仓库和并行数据仓库目录视图](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Azure SQL 数据仓库支持的系统视图](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Azure SQL 数据仓库支持的 T-SQL 语句](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)
