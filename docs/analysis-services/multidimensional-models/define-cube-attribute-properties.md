---
title: 定义多维数据集特性属性 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f7e2ab2374955710452f1ba1cba91e3a4d8ff8c1
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "63025437"
---
# <a name="define-cube-attribute-properties"></a>定义多维数据集特性属性
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  通过使用多维数据集特性属性，您可以为基于同一数据库维度的多维数据集维度中的维度特性指定唯一的设置。 下表介绍了多维数据集特性的属性。  
  
|属性|Description|  
|--------------|-----------------|  
|**AggregationUsage**|指定聚合设计向导将如何设计此属性的聚合。 默认值是 **Default**秒。 此属性可以有下列值：<br /><br /> **Default**：<br />                    聚合设计向导根据属性类型应用默认规则（对于键为 Full，对于其他为 Unrestricted）。<br /><br /> **无**：<br />                    多维数据集的所有聚合都不应包括此属性。<br /><br /> **Unrestricted**：<br />                    对聚合设计向导不作限制<br /><br /> **Full**：<br />                    多维数据集的每个聚合都必须包含此属性。|  
|**AttributeHierarchyEnabled**|标识是否对此多维数据集维度启用属性层次结构。 这可以对特定多维数据集或维度角色禁用属性层次结构。 如果禁用了基础属性层次结构，则此设置不起作用。 默认值为 **True**。|  
|**OptimizedState**|指示是否对此多维数据集维度优化属性层次结构。 这可以对特定多维数据集或维度角色优化属性层次结构。 如果未优化基础属性层次结构，则此设置不起作用。 默认值是 **FullyOptimized**秒。 此属性可以有下列值：<br /><br /> **FullyOptimized**：实例为层次结构生成索引以提高查询性能。 这是默认值。<br /><br /> **NotOptimized**：<br />                    实例未生成其他索引。|  
|**AttributeHierarchyVisible**|指示属性层次结构在此多维数据集维度上是否可见。 这可以使属性层次结构在特定多维数据集或维度角色上可见。 如果基础属性层次结构不可见，则此设置不起作用。 默认值是 **True**秒。|  
|**AttributeID**|包含属性的唯一标识符 (ID)。|  
  
## <a name="see-also"></a>请参阅  
 [定义多维数据集维度属性](../../analysis-services/multidimensional-models/define-cube-dimension-properties.md)   
 [定义多维数据集层次结构属性](../../analysis-services/multidimensional-models/define-cube-hierarchy-properties.md)  
  
  
