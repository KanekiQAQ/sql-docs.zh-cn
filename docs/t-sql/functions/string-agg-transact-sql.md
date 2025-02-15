---
title: STRING_AGG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- STRING_AGG
- STRING_AGG_TSQL
helpviewer_keywords:
- STRING_AGG function
ms.assetid: 8860ef3f-142f-4cca-aa64-87a123e91206
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2017||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3af59410ed151e54a5cc7ea7a546f8979a318693
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67906868"
---
# <a name="stringagg-transact-sql"></a>STRING_AGG (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2017-asdb-asdw-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-asdw-xxx-md.md)]

串联字符串表达式的值，并在其间放置分隔符值。 不能在字符串末尾添加分隔符。
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
STRING_AGG ( expression, separator ) [ <order_clause> ]

<order_clause> ::=   
    WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )   
```

## <a name="arguments"></a>参数

*expression*  
是任何类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 串联期间，表达式被转换为 `NVARCHAR` 或 `VARCHAR` 类型。 非字符串类型被转换为 `NVARCHAR` 类型。

separator   
是 `NVARCHAR` 或 `VARCHAR` 类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)，用作串联字符串的分隔符。 可以是文本或变量。 

<order_clause>   
使用 `WITHIN GROUP` 子句有选择性地指定串联结果的顺序：

```
WITHIN GROUP ( ORDER BY <order_by_expression_list> [ ASC | DESC ] )
```   
<order_by_expression_list>   
 
  可用于对结果进行排序的一系列非常量[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 每个查询只允许使用一个 `order_by_expression`。 默认的排序顺序为升序。   
  
## <a name="return-types"></a>返回类型

返回类型取决于第一个参数（表达式）。 如果输入参数是字符串类型（`NVARCHAR``VARCHAR`，则结果类型与输入类型相同。 下表列出了自动转换：  

|输入表达式类型 |结果 | 
|-------|-------|
|NVARCHAR(MAX) |NVARCHAR(MAX) |
|VARCHAR(MAX) |VARCHAR(MAX) |
|NVARCHAR(1...4000) |NVARCHAR(4000) |
|VARCHAR(1...8000) |VARCHAR(8000) |
|int, bigint, smallint, tinyint, numeric, float, real, bit, decimal, smallmoney, money, datetime, datetime2, |NVARCHAR(4000) |

## <a name="remarks"></a>Remarks

`STRING_AGG` 是一个聚合函数，用于提取行中的所有表达式，并将这些表达式串联成一个字符串。 表达式值隐式转换为字符串类型，然后串联在一起。 隐式转换为字符串的过程遵循现有的数据类型转换规则。 有关数据类型转换的详细信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)。 

如果输入表达式的类型为 `VARCHAR`，则分隔符的类型不能是 `NVARCHAR`。 

null 值会被忽略，且不会添加相应的分隔符。 若要为 null 值返回占位符，请使用 `ISNULL` 函数，如示例 B 中所示。

`STRING_AGG` 适用于任何兼容级别。

## <a name="examples"></a>示例

### <a name="a-generate-list-of-names-separated-in-new-lines"></a>A. 生成以新行分隔的姓名列表

下面的示例在一个结果单元格中生成姓名列表，并将其以回车符分隔。
```sql
SELECT STRING_AGG (FirstName, CHAR(13)) AS csv 
FROM Person.Person; 
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|csv | 
|--- |
|Syed <br />Catherine <br />Kim <br />Kim <br />Kim <br />Hazem <br />... | 

结果中不返回 `name` 单元格中的 `NULL` 值。   
> [!NOTE]  
>  如果使用 Management Studio 查询编辑器，“结果显示为网格”选项无法实现回车符  。 可切换到“结果显示为文本”，以便正确查看结果集  。   

### <a name="b-generate-list-of-names-separated-with-comma-without-null-values"></a>B. 生成使用逗号分隔且不带 NULL 值的姓名列表

下面的示例在一个结果单元格中返回以逗号分隔的姓名，并使用“N/A”替换 null 值。  
```sql
SELECT STRING_AGG ( ISNULL(FirstName,'N/A'), ',') AS csv 
FROM Person.Person; 
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|Csv | 
|--- |
|John,N/A,Mike,Peter,N/A,N/A,Alice,Bob |  

### <a name="c-generate-comma-separated-values"></a>C. 生成采用逗号分隔的值

```sql
SELECT 
STRING_AGG(CONCAT(FirstName, ' ', LastName, ' (', ModifiedDate, ')'), CHAR(13)) 
  AS names 
FROM Person.Person; 
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|姓名 |
|--- |
|Ken Sánchez (Feb  8 2003 12:00AM) <br />Terri Duffy (Feb 24 2002 12:00AM) <br />Roberto Tamburello (Dec  5 2001 12:00AM) <br />Rob Walters (Dec 29 2001 12:00AM) <br />... |

> [!NOTE]  
>  如果使用 Management Studio 查询编辑器，“结果显示为网格”选项无法实现回车符  。 可切换到“结果显示为文本”，以便正确查看结果集  。   

### <a name="d-return-news-articles-with-related-tags"></a>D. 返回带有相关标记的新闻文章 
文章及其标记被分隔到不同的表中。 开发人员想在返回时将每篇文章及其所有相关标记作为一行。 请使用以下查询：

```sql
SELECT a.articleId, title, STRING_AGG (tag, ',') as tags 
FROM dbo.Article AS a       
LEFT JOIN dbo.ArticleTag AS t 
    ON a.ArticleId = t.ArticleId 
GROUP BY a.articleId, title;
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|articleId |title |标记 |
|--- |--- |--- |
|172 |参选方民调结果不相上下 |政治,民意调查,市参议会 | 
|176 |新高速公路有望减少交通拥塞 |NULL |
|177 |狗继续比猫更受人喜爱 |民意调查,动物| 

### <a name="e-generate-list-of-emails-per-towns"></a>E. 生成按城市分类的电子邮件列表

下面的查询用于查找员工的电子邮件地址，并将结果按城市分类：

```sql
SELECT town, STRING_AGG (email, ';') AS emails 
FROM dbo.Employee 
GROUP BY town; 
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|城市 |电子邮件 |
|--- |--- |
|Seattle |syed0@adventure-works.com;catherine0@adventure-works.com;kim2@adventure-works.com |
|LA |sam1@adventure-works.com;hazem0@adventure-works.com |

可以直接使用在电子邮件列中返回的电子邮件向在某些特定城市工作的人们发送电子邮件。 

### <a name="f-generate-a-sorted-list-of-emails-per-towns"></a>F. 生成按城市分类并经过排序的电子邮件列表   
与上一个示例类似，下面的查询用于查找员工的电子邮件地址，将结果按城市分类，并按字母顺序对电子邮件排序：   
```sql
SELECT town, 
    STRING_AGG (email, ';') WITHIN GROUP (ORDER BY email ASC) AS emails 
FROM dbo.Employee 
GROUP BY town; 
```
   
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]

|城市 |电子邮件 |
|--- |--- |
|Seattle |catherine0@adventure-works.com;kim2@adventure-works.com;syed0@adventure-works.com |
|LA |hazem0@adventure-works.com;sam1@adventure-works.com |

## <a name="see-also"></a>另请参阅
 
 [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS (Transact-SQL)](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE (Transact-SQL)](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE (Transact-SQL)](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE (Transact-SQL)](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_ESCAPE (Transact-SQL)](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF (Transact-SQL)](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE (Transact-SQL)](../../t-sql/functions/translate-transact-sql.md)  
 [聚合函数 (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)  
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  

