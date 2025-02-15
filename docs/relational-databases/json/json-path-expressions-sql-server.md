---
title: JSON 路径表达式 (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 01/23/2017
ms.prod: sql
ms.reviewer: genemi
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- JSON, path expressions
- path expressions (JSON)
ms.assetid: 25ea679c-84cc-4977-867c-2cbe9d192553
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b06468560c612801efbf75a7717399659ec71eab
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68005465"
---
# <a name="json-path-expressions-sql-server"></a>JSON 路径表达式 (SQL Server)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

 使用 JSON 路径表达式可引用 JSON 对象的属性。  
  
 在调用以下函数时，必须提供路径表达式。  
  
-   调用 **OPENJSON** 以创建 JSON 数据的关系视图时。 有关详细信息，请参阅 [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)。  
  
-   调用 **JSON_VALUE** 以从 JSON 文本中提取值。 有关详细信息，请参阅 [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md)。  
  
-   调用 **JSON_QUERY** 以提取 JSON 对象或数组时。 有关详细信息，请参阅 [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)。  
  
-   调用 **JSON_MODIFY** 以更新 JSON 字符串的属性值时。 有关详细信息，请参阅 [JSON_MODIFY &#40;Transact-SQL&#41;](../../t-sql/functions/json-modify-transact-sql.md)。  

## <a name="parts-of-a-path-expression"></a>路径表达式的各部分
 路径表达式由两部分组成。  
  
1.  可选的[路径模式](#PATHMODE)，其值为 lax 或 strict   。  
  
2.  [路径](#PATH) 本身。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="PATHMODE"></a> Path mode  
 在路径表达式的开头，可以选择指定关键字 **lax** 或 **strict**来声明路径模式。 默认值为 **lax**。  
  
-   在 lax 模式下，如果路径表达式包含错误，函数将返回空值  。 例如，如果请求值 $.name，但 JSON 文本不包含 name 键，函数将返回 null，但不会引发错误   。  
  
-   在 strict 模式下，如果路径表达式包含错误，函数将引发错误  。  

以下查询显式指定路径表达式中的 `lax` 模式。

```sql  
DECLARE @json NVARCHAR(MAX)
SET @json=N'{ ... }'

SELECT * FROM OPENJSON(@json, N'lax $.info')
```  
  
##  <a name="PATH"></a> Path  
 在声明可选的路径模式后，请指定路径本身。  
  
-   美元符号 (`$`) 表示上下文项。  
  
-   属性路径是一组路径步幅。 路径步幅可以包含下列元素和运算符。  
  
    -   键名。 例如， `$.name` 和 `$."first name"`。 如果键名以美元符号开头或者包含空格等特殊字符，请为其加上引号。   
  
    -   数组元素。 例如， `$.product[3]`。 数组从零开始。  
  
    -   点运算符 (`.`) 指示对象的成员。 例如，在 `$.people[1].surname` 中，`surname` 是 `people` 的子级。
  
## <a name="examples"></a>示例  
 本部分中的示例引用以下 JSON 文本。  
  
```json  
{
    "people": [{
        "name": "John",
        "surname": "Doe"
    }, {
        "name": "Jane",
        "surname": null,
        "active": true
    }]
}
```  
  
 下表显示了一些路径表达式示例。  
  
|路径表达式|ReplTest1|  
|---------------------|-----------|  
|$.people[0].name|John|  
|$.people[1]|{ "name":"Jane",  "surname": null, "active": true }|  
|$.people[1].surname|null|  
|$|{ "people": [ { "name":"John",  "surname":"Doe" },<br />   { "name":"Jane",  "surname": null, "active": true } ] }|  
  
## <a name="how-built-in-functions-handle-duplicate-paths"></a>内置函数如何处理重复的路径  
 如果 JSON 文本包含重复属性，例如，同一级别上有两个同名的键，JSON_VALUE 和 JSON_QUERY 函数将仅返回第一个与路径匹配的值   。 若要分析包含重复键的 JSON 对象并返回所有值，请使用 OPENJSON，如下面的示例中所示  。  
  
```sql  
DECLARE @json NVARCHAR(MAX)
SET @json=N'{"person":{"info":{"name":"John", "name":"Jack"}}}'

SELECT value
FROM OPENJSON(@json,'$.person.info') 
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>另请参阅  
 [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)   
 [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md)   
 [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)  
  
  
