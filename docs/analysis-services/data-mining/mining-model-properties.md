---
title: 挖掘模型属性 |Microsoft Docs
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8ba834b497950357b83a4ec052654b5a4998b928
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68209798"
---
# <a name="mining-model-properties"></a>挖掘模型属性
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  挖掘模型具有以下几种属性：  
  
-   继承自挖掘结构的属性，这些属性定义了该模型使用的数据的数据类型和内容类型；  
  
-   与用于创建挖掘模型的算法相关的属性，包括任何客户参数；  
  
-   在该模型上定义用于定型模型的筛选器的属性。  
  
 挖掘模型的属性最初在创建模型时定义；但是，您稍后可以更改大多数属性，包括算法参数、筛选器和列用法属性。 可以使用数据挖掘设计器的 **“挖掘模型”** 选项卡或使用 AMO 或 XMLA 更改这些属性。  
  
 更改模型的任何属性时都必须重新处理该模型以使更改反映在模型中。 重新处理是必需的，即使更改只调用元数据（如添加列别名或说明）。  
  
## <a name="properties-of-models"></a>模型的属性  
 下表介绍特定于挖掘模型的属性。 此外，您还可以对挖掘模型中的各个列设置属性  
  
|属性|描述|  
|--------------|-----------------|  
|**算法**|设置挖掘模型的算法类型。|  
|**AlgorithmParameters**|设置各个算法类型可用的算法参数的值。|  
|**Filter**|设置筛选器，以筛选用于定型和测试挖掘模型的数据。 筛选器定义与挖掘模型存储在一起，并可在创建预测查询或测试模型的准确性时根据需要使用。<br /><br /> 定型模型时模型筛选器不是可选项。|  
|**名称**|设置挖掘模型的名称。|  
|**AllowDrillThrough**|指定是否为挖掘模型启用钻取。|  
  
## <a name="properties-of-model-columns"></a>模型列的属性  
 可以为挖掘模型中的各个列设置以下特定于数据挖掘的属性。 针对挖掘模型中的各个列，可以将这些属性设置为不同的值。  
  
|属性|描述|  
|--------------|-----------------|  
|**说明**|说明挖掘列的目的。|  
|**名称**|设置挖掘模型列的名称。 可以键入一个新名称，以便为挖掘模型列提供一个别名。|  
|**ModelingFlags**|设置列的任意特定于算法的标志。|  
|**SourceColumnID**|指示模型列所基于的挖掘结构列的名称。<br /><br /> 该属性为只读。|  
|**Usage**|设置挖掘模型如何使用列。|  
  
## <a name="see-also"></a>请参阅  
 [挖掘模型列](../../analysis-services/data-mining/mining-model-columns.md)   
 [挖掘结构 &#40;Analysis Services-数据挖掘&#41;](../../analysis-services/data-mining/mining-structures-analysis-services-data-mining.md)   
 [挖掘模型任务和操作指南](../../analysis-services/data-mining/mining-model-tasks-and-how-tos.md)   
 [更改挖掘模型的属性](../../analysis-services/data-mining/change-the-properties-of-a-mining-model.md)   
 [数据挖掘工具](../../analysis-services/data-mining/data-mining-tools.md)   
 [创建关系挖掘结构](../../analysis-services/data-mining/create-a-relational-mining-structure.md)   
 [为模型列创建别名](../../analysis-services/data-mining/create-an-alias-for-a-model-column.md)  
  
  
