---
title: 主表达式 (XQuery) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- variable references [XQuery]
- primary expressions [XQuery]
- function calls [XQuery]
- expressions [XQuery], primary
- literals [XQuery]
- context item expressions [XQuery]
ms.assetid: d4183c3e-12b5-4ca0-8413-edb0230cb159
author: rothja
ms.author: jroth
ms.openlocfilehash: e8704a01d810477fd0359196cb622984da357cf6
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67946383"
---
# <a name="primary-expressions-xquery"></a>主表达式 (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  XQuery 主表达式包含文字、变量引用、上下文项表达式、构造函数和函数调用。  
  
## <a name="literals"></a>文字  
 XQuery 文字可以是数字或字符串。 字符串可包括预定义的实体引用，实体引用就是字符序列。 这种序列以“and”符为开头，此符号表示单个字符，如果不加此符号则可能具有语法意义。 以下是 XQuery 中的预定义的实体引用。  
  
|实体引用|表示|  
|----------------------|----------------|  
|&lt;|\<|  
|&gt;|>|  
|&amp;|&|  
|&quot;|"|  
|&apos;|”启用|  
  
 字符串还可以包含字符引用，对 Unicode 字符（由其十进制或十六进制码位标识）的 XML 样式的引用。 例如，可以由字符引用，表示欧元符号"&\#8364;"。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用 XML 1.0 版作为分析基础。  
  
### <a name="examples"></a>示例  
 以下示例说明了文字、实体引用以及字符引用的用法。  
  
 此代码将返回错误，因为 `<'` 和 `'>` 字符具有特殊含义。  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <SalaryRange>Salary > 50000 and < 100000</SalaryRange>')  
GO  
```  
  
 如果改为使用实体引用，则查询将起作用。  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <SalaryRange>Salary > 50000 and < 100000</SalaryRange>')  
GO  
```  
  
 下面的示例阐释了使用字符引用来表示欧元符号。  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query(' <a>€12.50</a>')  
```  
  
 结果如下：  
  
 `<a>€12.50</a>`  
  
 在以下示例中，查询被撇号分隔开了。 因此，字符串值中的撇号用两个相邻的撇号来表示。  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query('<a>I don''t know</a>')  
Go  
```  
  
 结果如下：  
  
 `<a>I don't know</a>`  
  
 内置布尔函数**true （)** 并**false （)** ，可以用于表示布尔值，如下面的示例中所示。  
  
```  
DECLARE @var XML  
SET @var = ''  
SELECT @var.query('<a>{true()}</a>')  
GO  
```  
  
 直接的元素构造函数在大括号中指定一个表达式。 在生成的 XML 中，此表达式将被其值替代。  
  
 结果如下：  
  
 `<a>true</a>`  
  
## <a name="variable-references"></a>变量引用  
 XQuery 中的变量引用是以 $ 符号为前缀的 QName。 此实现只支持不带前缀的变量引用。 例如，下面的查询在 FLWOR 表达式中定义了变量 `$i`。  
  
```  
DECLARE @var XML  
SET @var = '<root>1</root>'  
SELECT @var.query('  
 for $i in /root return data($i)')  
GO  
```  
  
 下面的查询将不起作用，因为在变量名称中添加了命名空间前缀。  
  
```  
DECLARE @var XML  
SET @var = '<root>1</root>'  
SELECT @var.query('  
DECLARE namespace x="https://X";  
for $x:i in /root return data($x:i)')  
GO  
```  
  
 可以使用 sql:variable() 扩展函数来引用 SQL 变量，如下面的查询中所示。  
  
```  
DECLARE @price money  
SET @price=2500  
DECLARE @x xml  
SET @x = ''  
SELECT @x.query('<value>{sql:variable("@price") }</value>')  
```  
  
 结果如下：  
  
 `<value>2500</value>`  
  
#### <a name="implementation-limitations"></a>实现限制  
 实现限制如下：  
  
-   不支持带有命名空间前缀的变量。  
  
-   不支持模块导入。  
  
-   不支持外部变量声明。 此解决方案是使用[sql:variable() 函数](../xquery/xquery-extension-functions-sql-variable.md)。  
  
## <a name="context-item-expressions"></a>上下文项表达式  
 上下文项是路径表达式的上下文中当前正在处理的项。 该项在带有文档节点的非空 XML 数据类型实例中进行初始化。 此外可以更改由 nodes （） 方法，在 XPath 表达式的上下文或 [] 谓词。  
  
 上下文项由包含点 (.) 的表达式返回。 例如，以下查询计算每个元素 <`a`> 的属性是否存在`attr`。 如果存在此属性，则返回该元素。 请注意，谓词中的条件指定使用单个句点指定上下文节点。  
  
```  
DECLARE @var XML  
SET @var = '<ROOT>  
<a>1</a>  
<a attr="1">2</a>  
</ROOT>'  
SELECT @var.query('/ROOT[1]/a[./@attr]')  
```  
  
 结果如下：  
  
 `<a attr="1">2</a>`  
  
## <a name="function-calls"></a>函数调用  
 可以调用内置 XQuery 函数和[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]sql:variable() 和 sql: column 函数。 有关实现的函数列表，请参阅[针对 xml 数据类型的 XQuery 函数](../xquery/xquery-functions-against-the-xml-data-type.md)。  
  
#### <a name="implementation-limitations"></a>实现限制  
 实现限制如下：  
  
-   不支持 XQuery prolog 中的函数声明。  
  
-   不支持函数导入。  
  
## <a name="see-also"></a>请参阅  
 [XML 构造&#40;XQuery&#41;](../xquery/xml-construction-xquery.md)  
  
  
