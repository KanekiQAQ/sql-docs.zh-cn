---
title: 创建命名的集 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4eb82cba133f572e996f460be04661bfe511492e
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "63020065"
---
# <a name="create-named-sets"></a>创建命名集
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  命名集是为重复使用而创建的一组维度成员或集表达式，例如用于多维表达式 (MDX) 查询中。 可以通过组合多维数据集数据、算数运算符、数字和函数来创建命名集。 例如，可以创建一个名为“前十位工厂”的命名集，它包含“工厂”维度中具有最高的“产量”度量值的十个成员。 然后最终用户可以在查询中使用“前十位工厂”。 例如，最终用户可以将“前十位工厂”放置在一个坐标轴上，将包含“产量”的“度量值”维度放置在另一个坐标轴上。 有关详细信息，请参阅[多维模型中的计算](../../analysis-services/multidimensional-models/calculations-in-multidimensional-models.md)和[在 MDX 中生成命名集 (MDX)](../../analysis-services/multidimensional-models/mdx/mdx-named-sets-building-named-sets.md)。  
  
 若要创建命名集，请使用多维数据集设计器的 **“计算”** 选项卡上的 **“新建命名集”** 命令。 可以在 **“计算”** 选项卡工具栏上的 **“多维数据集”** 菜单中调用该命令。 该命令显示了一个窗体，可指定下列命名集选项：  
  
 **名称**  
 选择命名集的名称。 最终用户浏览多维数据集时将看到该名称。  
  
 **表达式**  
 指定生成命名集的表达式。 该表达式可以用 MDX 编写。 该表达式可以包含下列任何内容：  
  
-   表示多维数据集的维度、级别、度量值等组件的数据表达式。  
  
-   算术运算符。  
  
-   数字。  
  
-   函数。  
  
 您可以将多维数据集组件从 **“计算工具”** 窗格的 **“元数据”** 选项卡中拖动或复制到 **“命名集窗体编辑器”** 窗格的 **“表达式”** 框中。 您可以将函数从 **“计算工具”** 窗格的 **“函数”** 选项卡中拖动或复制到 **“命名集窗体编辑器”** 窗格的 **“表达式”** 框中。  
  
> [!IMPORTANT]  
>  如果通过显式命名集的成员创建的集表达式，将成员的列表括在大括号中 ({})。  
  
## <a name="see-also"></a>请参阅  
 [多维模型中的计算](../../analysis-services/multidimensional-models/calculations-in-multidimensional-models.md)  
  
  
