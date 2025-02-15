---
title: TRY_CAST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TRY_CAST_TSQL
- TRY_CAST
dev_langs:
- TSQL
helpviewer_keywords:
- TRY_CAST function
ms.assetid: ea3a16de-995b-415c-b5f0-9355cf7bb401
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 313f1bd13cf1b12a0685eeb101998792e0586767
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68098771"
---
# <a name="trycast-transact-sql"></a>TRY_CAST (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]

  返回转换为指定数据类型的值（如果转换成功）；否则返回 Null。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
TRY_CAST ( expression AS data_type [ ( length ) ] )  
```  
  
## <a name="arguments"></a>参数  
 *expression*  
 要强制转换的值。 任何有效的表达式。  
  
 data_type   
 要将 expression 强制转换为的数据类型  。  
  
 *length*  
 指定目标数据类型长度的可选整数。  
  
 可接受值的范围由 data_type 的值确定  。  
  
## <a name="return-types"></a>返回类型  
 返回转换为指定数据类型的值（如果转换成功）；否则返回 Null。  
  
## <a name="remarks"></a>Remarks  
 TRY_CAST 接收传递给它的值，并尝试将该值转换为指定的 data_type   。 如果强制转换成功，TRY_CAST 按指定的 data_type 返回值；如果发生错误，则返回 null   。 但是，如果请求的转换是显式不允许执行的转换，则 TRY_CAST 失败并显示错误  。  
  
 TRY_CAST 不是新的保留关键字，且可用于所有兼容级别  。 当连接到远程服务器时，TRY_CAST 与 TRY_CONVERT 具有相同的语义   。  
  
## <a name="examples"></a>示例  
  
### <a name="a-trycast-returns-null"></a>A. TRY_CAST 返回 Null。  
 下面的示例演示转换失败时 TRY_CAST 返回 Null。  
  
```sql  
SELECT   
    CASE WHEN TRY_CAST('test' AS float) IS NULL   
    THEN 'Cast failed'  
    ELSE 'Cast succeeded'  
END AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
------------  
Cast failed  
  
(1 row(s) affected)  
```  
  
 以下示例演示表达式必须采用所需的格式。  
  
```sql  
SET DATEFORMAT dmy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------  
NULL  
  
(1 row(s) affected)  
```  
  
### <a name="b-trycast-fails-with-an-error"></a>B. TRY_CAST 失败并显示错误  
 下面的示例演示显式不允许转换时 TRY_CAST 返回错误。  
  
```sql  
SELECT TRY_CAST(4 AS xml) AS Result;  
GO  
```  
  
 此语句的结果是一个错误，因为整数无法转换为 xml 数据类型。  
  
```  
Explicit conversion from data type int to xml is not allowed.  
```  
  
### <a name="c-trycast-succeeds"></a>C. TRY_CAST 成功  
 此示例演示表达式必须采用所需的格式。  
  
```  
SET DATEFORMAT mdy;  
SELECT TRY_CAST('12/31/2010' AS datetime2) AS Result;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
----------------------------------  
2010-12-31 00:00:00.0000000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [TRY_CONVERT (Transact-SQL)](../../t-sql/functions/try-convert-transact-sql.md)   
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  
