---
title: DB_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DB_ID_TSQL
- DB_ID
dev_langs:
- TSQL
helpviewer_keywords:
- viewing database ID numbers
- IDs [SQL Server], databases
- database IDs [SQL Server]
- identification numbers [SQL Server], databases
- displaying database ID numbers
- DB_ID function
ms.assetid: 7b3aef89-a6fd-4144-b468-bf87ebf381b8
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d2dc84cd224b8ea1863fd67561fdcf20a37c4544
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68119015"
---
# <a name="dbid-transact-sql"></a>DB_ID (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

此函数返回指定数据库的数据库标识 (ID) 号。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```sql
DB_ID ( [ 'database_name' ] )   
```  
  
## <a name="arguments"></a>参数  
'database_name'   
将返回其数据库 ID 号 `DB_ID` 的数据库的名称。 如果对 `DB_ID` 的调用省略 database_name，则 `DB_ID` 返回当前数据库的 ID  。
  
## <a name="return-types"></a>返回类型
**int**

## <a name="remarks"></a>Remarks
`DB_ID` 仅可用于返回 Azure SQL 数据库中当前数据库的数据库标识符。 如果指定的数据库名称不是当前数据库，则返回 NULL。
  
## <a name="permissions"></a>权限  
如果 `DB_ID` 的调用方不具有特定的非 master 或非 tempdb 数据库，则至少需要 `ALTER ANY DATABASE` 或 `VIEW ANY DATABASE` 服务器级权限才能看到相应的 `DB_ID` 行   。 对于 master 数据库，`DB_ID` 至少需要 `CREATE DATABASE` 权限  。 调用方连接的数据库将始终出现在 sys.databases 中  。
  
> [!IMPORTANT]  
>  默认情况下，公共角色具有 `VIEW ANY DATABASE` 权限，允许所有登录名查看数据库信息。 若要防止登录名检测数据库，则需 `REVOKE` 公共登录名的 `VIEW ANY DATABASE` 权限或 `DENY` 个人登录名的 `VIEW ANY DATABASE` 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-database-id-of-the-current-database"></a>A. 返回当前数据库的数据库 ID  
此示例返回当前数据库的数据库 ID。
  
```sql
SELECT DB_ID() AS [Database ID];  
GO  
```  
  
### <a name="b-returning-the-database-id-of-a-specified-database"></a>B. 返回指定数据库的数据库 ID  
此示例返回 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的数据库 ID。
  
```sql
SELECT DB_ID(N'AdventureWorks2008R2') AS [Database ID];  
GO  
```  
  
### <a name="c-using-dbid-to-specify-the-value-of-a-system-function-parameter"></a>C. 使用 DB_ID 来指定系统函数参数的值  
此示例使用 `DB_ID` 返回系统函数 `sys.dm_db_index_operational_stats` 中 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的数据库 ID。 此函数将数据库 ID 作为第一个参数。
  
```sql
DECLARE @db_id int;  
DECLARE @object_id int;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
IF @db_id IS NULL   
  BEGIN;  
    PRINT N'Invalid database';  
  END;  
ELSE IF @object_id IS NULL  
  BEGIN;  
    PRINT N'Invalid object';  
  END;  
ELSE  
  BEGIN;  
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);  
  END;  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-return-the-id-of-the-current-database"></a>D. 返回当前数据库的 ID  
此示例返回当前数据库的数据库 ID。
  
```sql
SELECT DB_ID();  
```  
  
### <a name="e-return-the-id-of-a-named-database"></a>E. 返回命名数据库的 ID。  
此示例返回 AdventureWorksDW2012 数据库的数据库 ID。
  
```sql
SELECT DB_ID('AdventureWorksPDW2012');  
```  
  
## <a name="see-also"></a>另请参阅
[DB_NAME (Transact-SQL)](../../t-sql/functions/db-name-transact-sql.md)  
[元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)  
[sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
[sys.dm_db_index_operational_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
  
  

