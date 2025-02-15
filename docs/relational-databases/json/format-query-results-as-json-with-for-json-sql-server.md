---
title: 使用 FOR JSON 将查询结果格式化为 JSON (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/06/2019
ms.prod: sql
ms.reviewer: genemi
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON
- JSON, exporting
- exporting JSON
ms.assetid: 15b56365-58c2-496c-9d4b-aa2600eab09a
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ccdd995900f10b8d0a83f10795659e1a5050995
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67909416"
---
# <a name="format-query-results-as-json-with-for-json-sql-server"></a>使用 FOR JSON 将查询结果格式化为 JSON (SQL Server)

[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

通过将 **FOR JSON** 子句添加到 **SELECT** 语句中，将查询结果格式化为 JSON，或者将 SQL Server 中的数据导出为 JSON。 使用 FOR JSON 子句，通过委托从应用到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 JSON 输出格式来简化客户端应用程序  。
  
 使用 FOR JSON 子句时，可以显式指定 JSON 输出的结构，或让 SELECT 语句的结构来决定输出  。  
  
-   使用 FOR JSON PATH 来保持对 JSON 输出格式的完全控制  。 你可以创建包装对象并嵌套复杂属性。  
  
-   使用 FOR JSON AUTO 来根据 SELECT 语句的结构自动格式化 JSON 输出  。  
  
下面是带有 **FOR JSON** 子句的 **SELECT** 语句及其输出的示例。
  
 ![对于 JSON](../../relational-databases/json/media/jsonslides2forjson.png)
  
## <a name="option-1---you-control-output-with-for-json-path"></a>选项 1 - 使用 FOR JSON PATH 控制输出
在 PATH 模式下，可以使用点语法来设置嵌套的输出格式，例如 `'Item.UnitPrice'`  。  

以下是一个使用带有 **FOR JSON** 子句的 **对于 JSON** 模式。 下面的示例使用 **ROOT** 选项指定一个已命名根元素。 
  
 ![FOR JSON 输出流的关系图](../../relational-databases/json/media/forjson-example1.png) 

### <a name="more-info-about-for-json-path"></a>有关 FOR JSON PATH 的详细信息
有关详细信息和示例，请参阅[在 PATH 模式下格式化嵌套的 JSON 输出 (SQL Server)](../../relational-databases/json/format-nested-json-output-with-path-mode-sql-server.md)。

有关语法和用法的详细信息，请参阅 [FOR 子句 (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md)。  

## <a name="option-2---select-statement-controls-output-with-for-json-auto"></a>选项 2 - SELECT 语句使用 FOR JSON AUTO 控制输出
在 **AUTO** 模式下，SELECT 语句的结构决定 JSON 输出的格式。

默认情况下，输出中不包括 **null** 值。 你可以使用 **INCLUDE_NULL_VALUES** 来更改此行为。  

以下是一个使用带有 **FOR JSON** 子句的 **AUTO** 模式的示例查询。

```sql  
SELECT name, surname  
FROM emp  
FOR JSON AUTO;
```  

下面是返回的 JSON。

```json  
[{
    "name": "John"
}, {
    "name": "Jane",
    "surname": "Doe"
}]
```

### <a name="2b---example-with-join-and-null"></a>2.b - JOIN 和 NULL 示例

以下 `SELECT...FOR JSON AUTO` 示例显示当 `JOIN` 表中的数据之间存在 1 对多关系时显示的 JSON 结果。

还说明了返回的 JSON 中缺少 NULL 值。 但是，可以使用 `FOR` 子句上的 `INCLUDE_NULL_VALUES` 关键字替代此默认行为。

```sql
go

DROP TABLE IF EXISTS #tabStudent;
DROP TABLE IF EXISTS #tabClass;

go

CREATE TABLE #tabClass
(
   ClassGuid   uniqueIdentifier  not null  default newid(),
   ClassName   nvarchar(32)      not null
);

CREATE TABLE #tabStudent
(
   StudentGuid   uniqueIdentifier  not null  default newid(),
   StudentName   nvarchar(32)      not null,
   ClassGuid     uniqueIdentifier      null   -- Foreign key.
);

go

INSERT INTO #tabClass
      (ClassGuid, ClassName)
   VALUES
      ('DE807673-ECFC-4850-930D-A86F921DE438', 'Algebra Math'),
      ('C55C6819-E744-4797-AC56-FF8A729A7F5C', 'Calculus Math'),
      ('98509D36-A2C8-4A65-A310-E744F5621C83', 'Art Painting')
;

INSERT INTO #tabStudent
      (StudentName, ClassGuid)
   VALUES
      ('Alice Apple', 'DE807673-ECFC-4850-930D-A86F921DE438'),
      ('Alice Apple', 'C55C6819-E744-4797-AC56-FF8A729A7F5C'),
      ('Betty Boot' , 'C55C6819-E744-4797-AC56-FF8A729A7F5C'),
      ('Betty Boot' , '98509D36-A2C8-4A65-A310-E744F5621C83'),
      ('Carla Cap'  , null)
;

go

SELECT
      c.ClassName,
      s.StudentName
   from
                       #tabClass   as c
      RIGHT OUTER JOIN #tabStudent as s ON s.ClassGuid = c.ClassGuid
   --where
   --   c.ClassName LIKE '%Math%'
   order by
      c.ClassName,
      s.StudentName
   FOR
      JSON AUTO
      --, INCLUDE_NULL_VALUES
;

go

DROP TABLE IF EXISTS #tabStudent;
DROP TABLE IF EXISTS #tabClass;

go
```

接下来是上述 SELECT 输出的 JSON。

```json
JSON_F52E2B61-18A1-11d1-B105-00805F49916B

[
   {"s":[{"StudentName":"Carla Cap"}]},
   {"ClassName":"Algebra Math","s":[{"StudentName":"Alice Apple"}]},
   {"ClassName":"Art Painting","s":[{"StudentName":"Betty Boot"}]},
   {"ClassName":"Calculus Math","s":[{"StudentName":"Alice Apple"},{"StudentName":"Betty Boot"}]}
]
```

### <a name="more-info-about-for-json-auto"></a>有关 FOR JSON AUTO 的详细信息

有关详细信息和示例，请参阅[在 AUTO 模式下自动格式化 JSON 输出 (SQL Server)](../../relational-databases/json/format-json-output-automatically-with-auto-mode-sql-server.md)。

有关语法和用法的详细信息，请参阅 [FOR 子句 (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md)。  
  
## <a name="control-other-json-output-options"></a>控制其他 JSON 输出选项  
使用以下附加选项控制 FOR JSON 子句的输出  。  
  
-   ROOT  。 若要将单个顶层元素添加到 JSON 输出中，请指定 **ROOT** 选项。 如果没有指定此选项，JSON 输出不会包括根元素。 有关详细信息，请参阅 [使用 ROOT 选项将根节点添加到 JSON 输出中 (SQL Server)](../../relational-databases/json/add-a-root-node-to-json-output-with-the-root-option-sql-server.md)。  
  
-   INCLUDE_NULL_VALUES  。 若要在 JSON 输出中包含 null 值，请指定 **INCLUDE_NULL_VALUES** 选项。 如果没有指定此选项，输出不会在查询结果中包括 NULL 值的 JSON 属性。 有关详细信息，请参阅[使用 INCLUDE_NULL_VALUES 选项将 NULL 值包含在 JSON 输出中 (SQL Server)](../../relational-databases/json/include-null-values-in-json-include-null-values-option.md)。   

-   WITHOUT_ARRAY_WRAPPER  。 若要删除默认括住 **FOR JSON** 子句的 JSON 输出的方括号，请指定 **WITHOUT_ARRAY_WRAPPER** 选项。 使用此选项可以生成单个 JSON 对象作为单行结果中的输出。 如果不指定此选项，JSON 输出将格式化为数组 - 即括在方括号内。 有关详细信息，请参阅 [使用 WITHOUT_ARRAY_WRAPPER 选项从 JSON 输出中删除方括号 (SQL Server)](../../relational-databases/json/remove-square-brackets-from-json-without-array-wrapper-option.md)。 
   
## <a name="output-of-the-for-json-clause"></a>FOR JSON 子句的输出  
FOR JSON 子句的输出具有以下特征  ：  
  
1.  结果集包含单个列。
    -   一个小结果集可包含单个列。
    -   一个大结果集可将长 JSON 字符串拆分到多行中。
        -   默认情况下，输出设置为“以网格显示结果”时，SQL Server Management Studio (SSMS) 会将结果连接到单个行中  。 SSMS 状态栏会显示实际行数。
        -   其他客户端应用程序可能需要使用代码，通过串联多个行的内容来将较长的结果重新组合为单个有效的 JSON 字符串。 有关 C# 应用程序中此代码的示例，请参阅[在 C# 客户端应用中使用 FOR JSON 输出](../../relational-databases/json/use-for-json-output-in-sql-server-and-in-client-apps-sql-server.md#use-for-json-output-in-a-c-client-app)。
  
     ![FOR JSON 输出示例](../../relational-databases/json/media/forjson-example2.png)  
  
2.  结果会格式化为 JSON 对象数组。  
  
    -   JSON 数组中的元素数量等于 SELECT 语句结果中的行数（应用 FOR JSON 子句前）。 
  
    -   SELECT 语句结果中的每一行（应用 FOR JSON 子句前）将成为数组中的单独 JSON 对象。  
  
    -   SELECT 语句结果中的每一列（应用 FOR JSON 子句前）将成为 JSON 对象的属性。  
  
3.  列的名称及其值都会根据 JSON 语法进行转义。 有关详细信息，请参阅 [FOR JSON 如何转义特殊字符和控制字符 (SQL Server)](../../relational-databases/json/how-for-json-escapes-special-characters-and-control-characters-sql-server.md)。

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

### <a name="example"></a>示例
下面是演示 FOR JSON 子句如何格式化 JSON 输出的示例  。  
  
**查询结果**  
  
|||||  
|-|-|-|-|  
|**指向**|**B**|**C**|**D**|  
|10|11|12|X|  
|20|21|22|是|  
|30|31|32|Z|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
 **JSON 输出**  
  
```json  
[{
    "A": 10,
    "B": 11,
    "C": 12,
    "D": "X"
}, {
    "A": 20,
    "B": 21,
    "C": 22,
    "D": "Y"
}, {
    "A": 30,
    "B": 31,
    "C": 32,
    "D": "Z"
}] 
```  

 有关 FOR JSON 子句的输出内容的详细信息，请参阅以下主题  。  

-   [FOR JSON 如何将 SQL Server 数据类型转换为 JSON 数据类型 (SQL Server)](../../relational-databases/json/how-for-json-converts-sql-server-data-types-to-json-data-types-sql-server.md)  
    **FOR JSON** 子句使用本主题中所述的规则在 JSON 输出中将 SQL 数据类型转换为 JSON 类型。  

-   [FOR JSON 如何转义特殊字符和控制字符 (SQL Server)](../../relational-databases/json/how-for-json-escapes-special-characters-and-control-characters-sql-server.md)  
    **FOR JSON** 子句按照本主题所述的方式在 JSON 输出中转义特殊字符和表示控制字符。  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>另请参阅  
 [FOR 子句 (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md)   
 [在 SQL Server 和客户端应用中使用 FOR JSON 输出 (SQL Server)](../../relational-databases/json/use-for-json-output-in-sql-server-and-in-client-apps-sql-server.md)  
  
  
