---
title: CREATE MEASURE 语句 (MDX) |Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: e9eb05d633dcbe660c34793fa39c10d788574700
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68098385"
---
# <a name="mdx-data-definition---create-measure"></a>MDX 数据定义 - CREATE MEASURE


  在表格模型中创建度量值。  
  
## <a name="syntax"></a>语法  
  
```  
  
CREATE MEASURE Table_Name[Measure_Name] = DAX_Expression  
[; CREATE MEASURE ...n]  
  
```  
  
## <a name="arguments"></a>参数  
 *Table_Name*  
 一个有效的字符串，用于提供要创建度量值的表的名称。  
  
 *Measure_Name*  
 提供度量值名称的有效字符串。  
  
 *DAX_Expression*  
 返回单个标量值的有效的 DAX 表达式。  
  
## <a name="remarks"></a>备注  
 *Measure_Name*必须将方括号括起来。  
  
 CREATE MEASURE 语句只能在 MDX 脚本定义; 使用请参阅[MdxScript 元素&#40;ASSL&#41;](https://docs.microsoft.com/bi-reference/assl/objects/mdxscript-element-assl)。  
  
 还可以定义供一个查询使用的计算成员。 若要定义只供一个查询使用的计算成员，请在 SELECT 语句中使用 WITH 子句。 有关详细信息，请参阅[在 MDX 中生成度量值](../analysis-services/multidimensional-models/mdx/mdx-building-measures.md)。  
  
## <a name="see-also"></a>请参阅  
 [MDX 数据定义语句&#40;MDX&#41;](../mdx/mdx-data-definition-statements-mdx.md)  
  
  
