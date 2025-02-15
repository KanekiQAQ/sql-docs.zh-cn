---
title: 属性关系 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f9eddac84f4db92770daa2bbf32b51597f84eb3f
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68209293"
---
# <a name="attribute-relationships"></a>的维度设计器中，可以在“维度结构”视图的
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  在中[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]，维度内的属性始终直接或间接与相关的键属性。 当您基于星型架构（在该架构中，所有维度属性都派生自同一关系表）定义维度时，维度的键属性和每个非键属性之间会自动定义属性关系。 当您基于雪花架构（在该架构中，维度属性派生自多个相关的表）定义维度时，会自动按如下方式定义属性关系：  
  
-   在键属性与绑定到主维度表中各列的每个非键属性之间定义。  
  
-   在键属性与绑定到辅助表中链接基础维度表的外键的属性之间定义。  
  
-   在绑定到辅助表中外键的属性与绑定到辅助表中各列的每个非键属性之间定义。  
  
 但是，由于多种原因您可能需要更改这些默认的属性关系。 例如，您可能需要定义自然层次结构、自定义排序顺序或基于非键属性的维度粒度。 有关详细信息，请参阅[维度特性属性参考](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)。  
  
> [!NOTE]  
>  特性关系在多维表达式 (MDX) 中称作成员属性。  
  
## <a name="natural-hierarchy-relationships"></a>自然层次结构关系  
 当用户定义层次结构中包含的每个属性都与其下直接属性具有一对多关系时，层次结构就是自然层次结构。 例如，请参考基于具有八列的关系源表的“客户”维度：  
  
-   CustomerKey  
  
-   CustomerName  
  
-   年龄  
  
-   性别  
  
-   Email  
  
-   City  
  
-   Country  
  
-   地区  
  
 相应的 Analysis Services 维度具有七个属性：  
  
-   客户（基于 CustomerKey，其中 CustomerName 提供成员名称）  
  
-   年龄、性别、电子邮件、市县、地区、国家(地区)  
  
 通过在某级别的属性和此级别下面的级别的属性之间创建属性关系，强制执行表示自然层次结构的关系。 对于 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]，这可指定自然关系和潜在聚合。 在“客户”维度中，“国家(地区)”、“区域”、“市县”和“客户”属性具有自然层次结构。 通过添加下列属性关系来说明 `{Country, Region, City, Customer}` 的自然层次结构：  
  
-   与“区域”属性具有属性关系的“国家(地区)”属性。  
  
-   与“市县”属性具有属性关系的“区域”属性。  
  
-   与“客户”属性具有属性关系的“市县”属性。  
  
 对于导航多维数据集中的数据，还可以创建不表示自然层次结构数据中的用户定义层次结构 (称为*即席*或*reporting*层次结构)。 例如，您可以基于 `{Age, Gender}` 创建用户定义层次结构。 用户不会看到任何区别在于两个层次结构的行为方式，尽管自然层次结构可以利用聚合和索引结构-向用户隐藏的源数据中的自然关系该帐户。  
  
 **SourceAttribute**级别的属性将确定哪些属性用于说明该级别。 **KeyColumns**特性的属性提供成员的数据源视图中指定的列。 **NameColumn**属性可以指定不同的名称列的成员。  
  
 若要使用用户定义层次结构中定义的级别[!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]，则**维度设计器**允许你的数据源视图中包含的相关表中选择维度属性、 维度表中的列多维数据集。 有关创建用户定义的层次结构的详细信息，请参阅[创建用户定义层次结构](../../analysis-services/multidimensional-models/user-defined-hierarchies-create.md)。  
  
 在 Analysis Services 中，通常对成员的内容进行假定。 叶成员没有后代，并且包含派生自基础数据源的数据。 非叶成员则具有后代，并且包含派生自对子成员执行的聚合的数据。 在聚合级别中，各成员基于其从属级别的聚合。 因此，当**IsAggregatable**属性设置为**False**级别源特性，没有可聚合属性应添加为级别上面的级别。  
  
## <a name="defining-an-attribute-relationship"></a>定义属性关系  
 创建属性关系时的主要约束是确保，对于属性关系引用的属性，属性关系所属的属性中的任何成员的值不超过一个。 例如，如果您在“市县”属性与“省市自治区”属性之间定义了关系，则每个市县只能与单个省市自治区相关。  
  
## <a name="attribute-relationship-queries"></a>属性关系查询  
 可以使用 MDX 查询从特性关系，成员属性的窗体中的数据检索与**属性**MDX 关键字**选择**语句。 有关如何使用 MDX 检索成员属性的详细信息，请参阅[使用成员属性&#40;MDX&#41;](../../analysis-services/multidimensional-models/mdx/mdx-member-properties.md)。  
  
## <a name="see-also"></a>请参阅  
 [属性和属性层次结构](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/attributes-and-attribute-hierarchies.md)   
 [维度特性属性参考](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)   
 [用户层次结构](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies.md)   
 [用户层次结构属性](../../analysis-services/multidimensional-models-olap-logical-dimension-objects/user-hierarchies-properties.md)  
  
  
