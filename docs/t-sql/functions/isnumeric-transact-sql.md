---
title: ISNUMERIC (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ISNUMERIC
- ISNUMERIC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], valid numeric type
- numeric data
- ISNUMERIC function
- verifying valid numeric type
- valid numeric type [SQL Server]
- checking valid numeric type
ms.assetid: 7aa816de-529a-4f6c-a99f-4d5a9ef599eb
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4771c6d611292157caf8247288f36648c8ad5179
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68109390"
---
# <a name="isnumeric-transact-sql"></a>ISNUMERIC (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  确定表达式是否为有效的数值类型。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
ISNUMERIC ( expression )  
```  
  
## <a name="arguments"></a>参数  
 *expression*  
 是要计算的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>Remarks  
 当输入表达式的计算结果为有效的 numeric 数据类型时，ISNUMERIC 返回 1；否则返回 0。 有效的 [numeric 数据类型](../../t-sql/data-types/numeric-types.md)包括以下类型：  

|||
|-|-|
| [精确数字](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md) | **bigint**、**int**、**smallint**、**tinyint**、**bit** |
| [固定精度](../../t-sql/data-types/decimal-and-numeric-transact-sql.md) | **decimal**、**numeric** |
| [近似](../../t-sql/data-types/float-and-real-transact-sql.md) | **float**、**real** |
| [货币值](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) | **money**、 **smallmoney** |

  
> [!NOTE]  
>  对于不是数字的字符（如加号 (+)、减号 (-)）和有效货币符号（如美元符号 ($)）字符，ISNUMERIC 将返回 1。 有关货币符号的完整列表，请参阅 [money 和 smallmoney (Transact-SQL)](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)。  
  
## <a name="examples"></a>示例  
 以下示例使用 `ISNUMERIC` 返回所有非数值的邮政编码。  
  
```  
USE AdventureWorks2012;  
GO  
SELECT City, PostalCode  
FROM Person.Address   
WHERE ISNUMERIC(PostalCode)<> 1;  
GO  
```  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例使用 `ISNUMERIC` 返回所有非数值的邮政编码。  
  
```  
USE master;  
GO  
SELECT name, isnumeric(name) AS IsNameANumber, database_id, isnumeric(database_id) AS IsIdANumber   
FROM sys.databases;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-for-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
  
  

