---
title: 客户端与服务器端 XML 格式化 (SQLXML 4.0) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- NESTED mode
- FOR XML clause, formatting
- client-side XML formatting
- server-side XPath
- server-side XML formatting
- AUTO mode
- client-side XPath
ms.assetid: f807ab7a-c5f8-4e61-9b00-23aebfabc47e
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d02eeb0ad64a62343fda53a1907cec2b4bb9e850
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68220363"
---
# <a name="client-side-vs-server-side-xml-formatting-sqlxml-40"></a>客户端与服务器端 XML 格式 (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  本主题说明在 SQLXML 中客户端与服务器端 XML 格式的一般差异。  
  
## <a name="multiple-rowset-queries-not-supported-in-client-side-formatting"></a>客户端格式中不支持多行集查询  
 使用客户端 XML 格式时不支持生成多个行集的查询。 例如，假定您有一个虚拟目录，在其中指定了客户端格式。 请考虑此示例模板，其中有两个 SELECT 语句中 **\<sql:query >** 块：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query>  
     SELECT FirstName FROM Person.Contact FOR XML Nested;   
     SELECT LastName FROM Person.Contact FOR XML Nested    
  </sql:query>  
</ROOT>  
```  
  
 您可以在应用程序代码中执行此模板，但会返回错误，因为客户端 XML 格式不支持多个行集的格式。 如果在两个指定的查询分开 **\<sql:query >** 块，将获得所需的结果。  
  
## <a name="timestamp-maps-differently-in-client--vs-server-side-formatting"></a>timestamp 在客户端与。服务器端格式设置  
 在服务器端 XML 格式的数据库列**时间戳**类型映射到 i8 XDR 类型 （当在查询中指定了 XMLDATA 选项）。  
  
 在客户端 XML 格式中的数据库列**时间戳**类型映射到任一**uri**或**bin.base64** XDR 类型 (具体取决于二进制 base64选项指定在查询中）。 **Bin.base64** XDR 类型是使用 updategram 和 bulkload 功能的情况下很有用，因为此类型转换为[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**时间戳**类型。 这样即可成功执行插入、更新或删除操作。  
  
## <a name="deep-variants-are-used-in-server-side-formatting"></a>服务器端 XML 格式使用深层 VARIANT  
 在服务器端 XML 格式中，使用深层类型的 VARIANT 类型。 如果使用客户端 XML 格式，变量将转换为 Unicode 字符串，并且不使用 VARIANT 的子类型。  
  
## <a name="nested-mode-vs-auto-mode"></a>NESTED 的模式与AUTO 模式  
 客户端 FOR XML 的 NESTED 模式类似于服务器端 FOR XML 的 AUTO 模式，不过以下方面除外：  
  
### <a name="when-you-query-views-using-auto-mode-on-the-server-side-the-view-name-is-returned-as-the-element-name-in-the-resulting-xml"></a>使用服务器端的 AUTO 模式查询视图时，在生成的 XML 中将视图名称返回为元素名称。  
 例如，假定 AdventureWorksdatabase 中的 Person.Contact 表，创建以下视图：  
  
```  
CREATE VIEW ContactView AS (SELECT ContactID as CID,  
                               FirstName  as FName,  
                               LastName  as LName  
                        FROM Person.Contact)  
```  
  
 以下模板指定针对 ContactView 视图的查询，同时指定了服务器端 XML 格式：  
  
```  
 <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="0">  
    SELECT *  
    FROM   ContactView  
    FOR XML AUTO  
  </sql:query>  
</ROOT>  
```  
  
 执行该模板时，将返回以下 XML。 （仅部分显示结果。）请注意，元素名称对其执行查询的视图的名称。  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <ContactView CID="1" FName="Gustavo" LName="Achong" />   
  <ContactView CID="2" FName="Catherine" LName="Abel" />   
...  
</ROOT>  
```  
  
 使用对应的 NESTED 模式指定客户端 XML 格式时，在生成的 XML 中将基表名称返回为元素名称。 例如，以下修改后的模板执行同一 SELECT 语句，但 XML 格式设置在客户端上执行 (即**客户端侧 xml**设置为在模板中，则返回 true):  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="1">  
    SELECT *  
    FROM   ContactView  
    FOR XML NESTED  
  </sql:query>  
</ROOT>  
```  
  
 执行此模板将生成以下 XML。 请注意，在此情况下的元素名称就是基表名称。  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Person.Contact CID="1" FName="Gustavo" LName="Achong" />   
  <Person.Contact CID="2" FName="Catherine" LName="Abel" />   
...  
</ROOT>  
```  
  
### <a name="when-you-use-auto-mode-of-the-server-side-for-xml-the-table-aliases-specified-in-the-query-are-returned-as-element-names-in-the-resulting-xml"></a>使用服务器端 FOR XML 的 AUTO 模式时，在生成的 XML 中将查询中指定的表别名返回为元素名称。  
 例如，考虑以下模板：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="0">  
    SELECT FirstName as fname,  
           LastName as lname  
    FROM   Person.Contact C  
    FOR XML AUTO  
  </sql:query>  
</ROOT>  
```  
  
 执行此模板将生成以下 XML：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <C fname="Gustavo" lname="Achong" />   
  <C fname="Catherine" lname="Abel" />   
...  
</ROOT>   
```  
  
 使用客户端 FOR XML 的 NESTED 模式时，在生成的 XML 中将表名返回为元素名称。 （不使用查询中指定的表别名。）例如，考虑以下模板：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="1">  
    SELECT FirstName as fname,  
           LastName as lname  
    FROM   Person.Contact C  
    FOR XML NESTED  
  </sql:query>  
</ROOT>  
```  
  
 执行此模板将生成以下 XML：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Person.Contact fname="Gustavo" lname="Achong" />   
  <Person.Contact fname="Catherine" lname="Abel" />   
...  
</ROOT>  
```  
  
### <a name="if-you-have-a-query-that-returns-columns-as-dbobject-queries-you-cannot-use-aliases-for-these-columns"></a>如果您的查询将列返回为 dbobject 查询，则无法对这些列使用别名。  
 例如，考虑下面的模板，该模板执行的查询返回员工 ID 和照片。  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
<sql:query client-side-xml="1">  
   SELECT ProductPhotoID, LargePhoto as P  
   FROM   Production.ProductPhoto  
   WHERE  ProductPhotoID=5  
   FOR XML NESTED, elements  
</sql:query>  
</ROOT>  
```  
  
 执行此模板将返回作为 dbobject 查询的 Photo 列。 在这个 dbobject 查询中，`@P` 指不存在的列名。  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Production.ProductPhoto>  
    <ProductPhotoID>5</ProductPhotoID>  
    <LargePhoto>dbobject/Production.ProductPhoto[@ProductPhotoID='5']/@P</LargePhoto>  
  </Production.ProductPhoto>  
</ROOT>  
```  
  
 如果在服务器上执行 XML 格式设置 (**客户端侧 xml ="0"** )，可以返回 dbobject 查询中的实际表和列 （即使您已经指定了别名） 返回名称的列使用别名。 例如，以下模板执行一个查询，并在服务器上执行 XML 格式设置 (**客户端侧 xml**未指定选项和**在客户端运行**不选择选项虚拟根目录）。 该查询还指定了 AUTO 模式（而不是客户端 NESTED 模式）。  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
<sql:query   
   SELECT ProductPhotoID, LargePhoto as P  
   FROM   Production.ProductPhoto  
   WHERE  ProductPhotoID=5  
   FOR XML AUTO, elements  
</sql:query>  
</ROOT>  
```  
  
 执行此模板时，将返回以下 XML 文档（请注意，没有在针对 LargePhoto 列的 dbobject 查询中使用别名）：  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Production.ProductPhoto>  
    <ProductPhotoID>5</ProductPhotoID>  
    <LargePhoto>dbobject/Production.ProductPhoto[@ProductPhotoID='5']/@LargePhoto</LargePhoto>  
  </Production.ProductPhoto>  
</ROOT>  
```  
  
### <a name="client-side-vs-server-side-xpath"></a>客户端与服务器端 XPath  
 客户端 XPath 与服务器端 XPath 的工作方式相同，以下几点除外：  
  
-   使用客户端 XPath 查询时所应用的数据转换与使用服务器端 XPath 查询时所应用的数据转换有所不同。 客户端 XPath 使用 CAST 而不是 CONVERT 模式 126。  
  
-   当指定**客户端侧 xml ="0"** (false) 在模板中，则表示请求服务器端 XML 格式。 因此，不能指定 FOR XML NESTED，因为服务器不识别 NESTED 选项。 这将生成一个错误。 必须使用服务器确实可以识别的 AUTO、RAW 或 EXPLICIT 模式。  
  
-   当指定**客户端侧 xml ="1"** (true) 在模板中，则表示请求客户端 XML 格式。 在这种情况下，可以指定 FOR XML NESTED。 尽管在服务器端上进行指定 FOR XML AUTO，如果 XML 格式化**客户端侧 xml ="1"** 模板中指定。  
  
## <a name="see-also"></a>请参阅  
 [FOR XML 安全注意事项&#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/for-xml-security-considerations-sqlxml-4-0.md)   
 [客户端 XML 格式设置&#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/formatting/client-side-xml-formatting-sqlxml-4-0.md)   
 [服务器端 XML 格式设置&#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/formatting/server-side-xml-formatting-sqlxml-4-0.md)  
  
  
