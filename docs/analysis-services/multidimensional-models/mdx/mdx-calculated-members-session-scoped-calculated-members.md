---
title: 创建会话作用域的计算成员 (MDX) |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 476c62ef2aa4f0aad3d65cd2b78f27fc9ae6fd7c
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68176583"
---
# <a name="mdx-calculated-members---session-scoped-calculated-members"></a>MDX 计算成员-会话作用域的计算成员
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  若要创建在整个多维表达式 (MDX) 会话中都可用的计算成员，请使用 [CREATE MEMBER](../../../mdx/mdx-data-definition-create-member.md) 语句。 直到 MDX 会话关闭才会删除使用 CREATE MEMBER 语句创建的计算成员。  
  
 如本主题中所介绍，CREATE MEMBER 语句的语法很直观且易于使用。  
  
> [!NOTE]  
>  有关计算成员的详细信息，请参阅[在 MDX 中生成计算成员 (MDX)](../../../analysis-services/multidimensional-models/mdx/mdx-calculated-members-building-calculated-members.md)。  
  
## <a name="create-member-syntax"></a>CREATE MEMBER 语法  
 使用下列语法将 CREATE MEMBER 语句添加到 MDX 语句中：  
  
```  
CREATE [SESSION] MEMBER [<cube-name>.]<fully-qualified-member-name> AS <expression> [,<property-definition-list>]  
<cube name> ::= CURRENTCUBE | <Cube Name>  
<property-definition-list> ::= <property-definition>  
  | <property-definition>, <property-definition-list>  
<property-definition> ::= <property-identifier> = <property-value>  
<property-identifier> ::= VISIBLE | SOLVEORDER | SOLVE_ORDER | FORMAT_STRING | NON_EMPTY_BEHAVIOR <ole db member properties>  
```  
  
 在 CREATE MEMBER 语句的语法中， `fully-qualified-member-name` 值是计算成员的完全限定名。 完全限定名包含计算成员关联的维度或级别。 计算表达式值后， `expression` 值将返回计算成员的值。  
  
## <a name="create-member-example"></a>CREATE MEMBER 示例  
 下面的示例使用 CREATE MEMBER 语句创建 `LastFourStores` 计算成员。 此计算成员返回最后四家商店售出的部件总和，并且在多维数据集的整个会话中都可用。  
  
```  
Create Session Member [Store].[Measures].LastFourStores as   
sum(([Stores].[ByLocation].Lag(3) :  
[Stores].[ByLocation].NextMember), [Measures].[Units Sold])  
```  
  
## <a name="see-also"></a>请参阅  
 [创建查询作用域的计算成员 (MDX)](../../../analysis-services/multidimensional-models/mdx/mdx-calculated-members-query-scoped-calculated-members.md)  
  
  
