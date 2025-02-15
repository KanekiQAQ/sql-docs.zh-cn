---
title: 维度对象 (Analysis Services-多维数据) |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b296da3faed422aea0da42d01e0a6ece7333e39c
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68209276"
---
# <a name="dimension-objects-analysis-services---multidimensional-data"></a>维度对象（Analysis Services - 多维数据）
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  简单 <xref:Microsoft.AnalysisServices.Dimension> 对象由基本信息、属性和层次结构组成。 基本信息包括维度的名称、维度的类型、数据源和存储模式等。 属性可定义维度中的实际数据。 属性可不必属于层次结构，但层次结构却要由属性生成。 层次结构不但可创建级别的有序列表，还可定义用户浏览维度的方式。  
  
## <a name="in-this-section"></a>本节内容  
 下列主题提供有关如何设计和实现维度对象的详细信息。  
  
|主题|描述|  
|-----------|-----------------|  
|[维度（Analysis Services - 多维数据）](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-analysis-services-multidimensional-data.md)|在中[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]，维度是多维数据集的一个基本组件。 维度可将和相关领域（如客户、商店或雇员）关联的数据与用户组织起来。|  
|[属性和属性层次结构](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md)|维度是属性的集合，这些属性绑定到数据源视图的表或视图中的一列或多列。|  
|[属性关系](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attribute-relationships.md)|在中[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]，维度内的属性始终直接或间接与相关的键属性。 当您基于星型架构（在该架构中，所有维度属性都派生自同一关系表）定义维度时，维度的键属性和每个非键属性之间会自动定义属性关系。 当您基于雪花架构（在该架构中，维度属性派生自多个相关的表）定义维度时，会自动按如下方式定义属性关系：<br /><br /> 在键属性与绑定到主维度表中各列的每个非键属性之间定义。<br /><br /> 在键属性与绑定到辅助表中链接基础维度表的外键的属性之间定义。<br /><br /> 在绑定到辅助表中外键的属性与绑定到辅助表中各列的每个非键属性之间定义。|  
  
  
