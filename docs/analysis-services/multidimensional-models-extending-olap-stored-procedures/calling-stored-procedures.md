---
title: 调用存储的过程 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a0bdf517f2845e28a9d3520034d9de16ff65a25e
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68181169"
---
# <a name="calling-stored-procedures"></a>调用存储过程
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  可以在服务器上或从客户端应用程序中调用存储过程。 在任何一种情况下，存储过程都始终运行于服务器上，或者使用服务器的上下文，或者使用数据库的上下文。 执行存储过程时，不需要具备特殊的权限。 存储过程由程序集添加到服务器或数据库上下文后，只要用户的角色允许执行存储过程所执行的操作，则任何用户均可执行该存储过程。  
  
 调用 MDX 中的存储过程是按照与调用内部 MDX 函数相同的方式来完成的。 对于不带参数的存储过程，则使用过程名和一对空括号，如下所示：  
  
```  
MyStoredProcedure()  
```  
  
 如果存储过程有一个或多个参数，就会按顺序提供参数，并用逗号彼此分隔。 下面的示例演示了带有三个参数的示例存储过程：  
  
```  
MyStoredProcedure("Parameter1", 2, 800)  
```  
  
## <a name="calling-stored-procedures-in-mdx-queries"></a>在 MDX 查询中调用存储过程  
 在所有 MDX 查询中，存储过程都必须返回 MDX 表达式所需的正确语法类型。 如果存储过程未返回正确的类型，则会发生 MDX 错误。 以下示例所演示的存储过程将返回数学运算的集合、成员和结果。  
  
### <a name="returning-a-set"></a>返回集合  
 以下示例实现一个名为 MySproc 的存储过程，它将返回一个集合。 在第一个示例中，MySproc 直接在 SELECT 表达式中返回集合。 在后面两个示例中，MySproc 返回的集合是 Crossjoin 和 DrilldownLevel 函数的参数。  
  
```  
SELECT MySetProcedure(a,b,c) ON 0 FROM Sales  
SELECT Crossjoin(MySetProcedure(a,b,c)) ON 0 FROM Sales  
SELECT DrilldownLevel(MySetProcedure(a,b,c)) ON 0 FROM Sales  
```  
  
### <a name="returning-a-member"></a>返回成员  
 以下示例显示返回成员的 MySproc 函数：  
  
```  
SELECT Descendants(MySproc(a,b,c),3) ON 0 FROM Sales  
```  
  
### <a name="returning-the-result-of-a-math-operation"></a>返回数学运算的结果  
  
```  
SELECT Country.Members on 0, MySproc(Measures.Sales) ON 1 FROM Sales  
```  
  
## <a name="calling-stored-procedures-with-the-call-statement"></a>用 Call 语句调用存储过程  
 可以使用 MDX MDX 查询的上下文以外调用存储的过程**调用**语句。  
  
 可以使用此方法实例化存储查询的副作用，或让应用程序获得存储查询的结果。 一个常见用途**调用**语句将使用分析管理对象 (AMO) 来执行没有返回结果的管理功能。 例如，以下命令将调用一个存储过程：  
  
```  
Call MyStoredProcedure(a,b,c)  
```  
  
 唯一受支持的类型从存储过程中返回**调用**语句是一个行集。 行集的序列化是由 XML for Analysis 定义的。 如果中的存储过程**调用**语句返回任何其他类型，将被忽略，不以 xml 格式返回到调用应用程序。 有关 XML for Analysis 行集的详细信息，请参阅“XML for Analysis 架构行集”。  
  
 如果存储过程返回 .NET 行集，[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 将在服务器中将结果转换为 XML for Analysis 行集。 中的存储过程始终返回 XML for Analysis 行集**调用**函数。 如果数据集包含了在 XML for Analysis 行集中无法表达的功能，则会产生失败。  
  
 返回 void 值的函数（例如，Visual Basic 中的子例程）也可与 CALL 关键字一起使用。 例如，如果想要在 MDX 语句中使用 MyVoidFunction() 函数，可采用下面的语法：  
  
```  
CALL(MyVoidFunction)  
```  
  
## <a name="see-also"></a>请参阅  
 [多维模型程序集管理](../../analysis-services/multidimensional-models/multidimensional-model-assemblies-management.md)   
 [定义存储过程](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/defining-stored-procedures.md)  
  
  
