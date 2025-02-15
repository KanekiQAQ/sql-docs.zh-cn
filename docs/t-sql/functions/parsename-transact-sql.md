---
title: PARSENAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- PARSENAME_TSQL
- PARSENAME
dev_langs:
- TSQL
helpviewer_keywords:
- PARSENAME function
- parsing [SQL Server], PARSENAME function
- names [SQL Server], objects
- objects [SQL Server], names
- part of object names [SQL Server]
ms.assetid: abf34f99-9ee9-460b-85b2-930ca5c4b5ae
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dd4869dc26a85e2fd693e2e77bb3f30074af6668
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67914403"
---
# <a name="parsename-transact-sql"></a>PARSENAME (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all-md](../../includes/tsql-appliesto-ss2012-all-md.md)]

  返回对象名称的指定部分。 可检索的对象部分包括对象名称、所有者名称、数据库名称和服务器名称。  
  
> [!NOTE]  
>  PARSENAME 函数不指示指定名称的对象是否存在。 PARSENAME 仅返回指定对象名称的指定部分。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
PARSENAME ( 'object_name' , object_piece )   
```  
  
## <a name="arguments"></a>参数  
 'object_name'   
 要检索其指定部分的对象的名称。 object_name 为 sysname   。 此参数是可选的限定对象名称。 如果对象名称的所有部分都是限定的，则此名称可包含四部分：服务器名称、数据库名称、所有者名称以及对象名称。  
  
 object_piece   
 要返回的对象部分。 object_piece 的数据类型为 int，可以为下列值   ：  
  
 1 = 对象名称  
  
 2 = 架构名称  
  
 3 = 数据库名称  
  
 4 = 服务器名称  
  
## <a name="return-types"></a>返回类型  
 **sysname**  
  
## <a name="remarks"></a>Remarks  
 如果存在下列条件之一，则 PARSENAME 返回 NULL：  
  
-   object_name 或 object_piece 为 NULL   。  
  
-   发生语法错误。  
  
 请求的对象部分长度为 0，且不是有效的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 标识符。 长度为零的对象的名称将使整个限定名称无效。  
  
## <a name="examples"></a>示例  
 以下示例使用 `PARSENAME` 返回有关 `Person` 数据库中 `AdventureWorks2012` 表的信息。  
  
```  
-- Uses AdventureWorks  
  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 1) AS 'Object Name';  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 2) AS 'Schema Name';  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 3) AS 'Database Name';  
SELECT PARSENAME('AdventureWorksPDW2012.dbo.DimCustomer', 4) AS 'Server Name';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
```
Object Name
------------------------------
DimCustomer

(1 row(s) affected)

Schema Name
------------------------------
dbo

(1 row(s) affected)

Database Name
------------------------------
AdventureWorksPDW2012

(1 row(s) affected)

Server Name
------------------------------
(null)

(1 row(s) affected)
```
  
## <a name="see-also"></a>另请参阅  
 [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-for-transact-sql.md)  
  
  

