---
title: 串联运算符 |Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 235320a65bf04e8b0ce1d7c23da588a4d541a0b3
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67915860"
---
# <a name="concatenation-operators"></a>串联运算符


  串联运算符为加号 (+)。 可以将两个或更多个字符串合并或串联成一个字符串， 还可以串联二进制字符串。  
  
 下列代码是串联运算符的一个示例，在此示例中，用串联运算符将产品名称与产品的唯一名称连接起来：  
  
```  
WITH MEMBER Measures.ProductName AS   
   Product.Product.CurrentMember.Name + " (" + Product.Product.CurrentMember.UniqueName + ")"  
SELECT   
   {Measures.ProductName} ON COLUMNS,  
   Product.Product.Members ON ROWS  
FROM [Adventure Works]  
```  
  
## <a name="language-considerations"></a>语言注意事项  
 当串联中使用的字符串具有相同的排序规则时，生成的串联字符串具有与输入字符串相同的排序规则。 当串联中使用的字符串具有不同的排序规则时，生成的串联字符串的排序规则由排序规则的优先顺序规则确定。 有关详细信息，请参阅[语言和排序规则 (Analysis Services)](../analysis-services/languages-and-collations-analysis-services.md)。  
  
## <a name="see-also"></a>请参阅  
 [MDX 运算符参考&#40;MDX&#41;](../mdx/mdx-operator-reference-mdx.md)   
 [运算符&#40;MDX 语法&#41;](../mdx/operators-mdx-syntax.md)  
  
  
