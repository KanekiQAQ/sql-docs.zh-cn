---
title: 属性和属性层次结构 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b858af0f4f3e86e480099a16e2b8db607b8882c3
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62679439"
---
# <a name="attributes-and-attribute-hierarchies"></a>属性和属性层次结构
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  维度是属性的集合，这些属性绑定到数据源视图的表或视图中的一列或多列。  
  
## <a name="key-attribute"></a>键属性  
 每个维度都包含一个键属性。 每个属性绑定到维度表中的一个或多个列。 键属性是维度中标识维度主表中各列（在与事实数据表的外键关系中使用）的属性。 通常，键属性表示维度表中的一个或多个主键列。 您可以对在基础数据源内没有物理主键的数据源视图中的某个表定义逻辑主键。 **有关详细信息**，请参阅[定义逻辑主键在数据源视图中&#40;Analysis Services&#41;](../../analysis-services/multidimensional-models/define-logical-primary-keys-in-a-data-source-view-analysis-services.md)。 当定义键属性时，多维数据集向导和维度向导会尝试使用数据源视图中维度表的主键列。 如果维度表未定义逻辑主键或物理主键，则上述向导可能无法正确地为维度定义键属性。  
  
## <a name="binding-an-attribute-to-columns-in-data-source-view-tables-or-views"></a>将属性绑定到数据源视图表或视图中的列  
 属性将绑定到一个或多个数据源视图表或视图中的列。 属性始终绑定到一个或多个键列上，这些键列将确定属性包含的成员。 默认情况下，这是唯一绑定有属性的列。 还可以针对特定的目的将属性绑定到一个或多个其他列。 例如，特性的**NameColumn**属性决定向用户显示每个特性成员的名称-属性的此属性可以绑定到特定的维度列通过数据源视图，也可以是绑定到数据源视图中的计算列。 有关详细信息，请参阅[维度特性属性参考](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)。  
  
## <a name="attribute-hierarchies"></a>属性层次结构  
 默认情况下，会将属性成员组织到两个级别的层次结构中，其中包含一个叶级别和一个“全部”级别。 “全部”级别包含每个与属性相关的度量值组（其维度是一个成员）中所有度量值的属性成员的聚合值。 但是，如果**IsAggregatable**属性设置为 False，则不创建全部级别。 有关详细信息，请参阅[维度特性属性参考](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)。  
  
 可以并且通常将属性排列到用户定义的层次结构中，通过这些层次结构提供的下钻路径，用户可以浏览与属性相关的度量值组中的数据。 在客户端应用程序中，属性可用于提供分组和约束信息。 属性排列到用户定义层次结构中，定义时，级别之间的关系在多对一的层次结构级别之间的关系或一对一关系 (称为*自然*关系)。 例如，在“日历时间”层次结构中，“日”级别应与“月”级别相关，“月”级别应与“季度”级别相关等等。 通过定义用户定义层次结构中级别之间的关系，Analysis Services 可以定义更有用的聚合来提高查询性能，并且还可以在处理性能过程中节省内存，这对于大型或复杂多维数据集非常重要。 有关详细信息，请参阅[用户层次结构](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies.md)，[创建用户定义层次结构](../../analysis-services/multidimensional-models/user-defined-hierarchies-create.md)，并[定义属性关系](../../analysis-services/multidimensional-models/attribute-relationships-define.md)。  
  
## <a name="attribute-relationships-star-schemas-and-snowflake-schemas"></a>属性关系、星型架构和雪花型架构  
 默认情况下，在星型架构中，所有属性都直接与键属性相关，这使得用户可根据维度中的任意属性层次结构浏览多维数据集中的事实数据。 在雪花型架构中，如果属性的基础表直接链接到事实数据表，则属性会直接链接到键属性；另外，属性还可以使用绑定到基础表（将雪花状表链接到直接链接的表）中键的属性间接地进行链接。  
  
## <a name="see-also"></a>请参阅  
 [创建用户定义层次结构](../../analysis-services/multidimensional-models/user-defined-hierarchies-create.md)   
 [定义属性关系](../../analysis-services/multidimensional-models/attribute-relationships-define.md)   
 [维度特性属性参考](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)  
  
  
