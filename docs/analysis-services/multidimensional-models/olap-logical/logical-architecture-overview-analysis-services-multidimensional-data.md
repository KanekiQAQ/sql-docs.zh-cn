---
title: 逻辑体系结构概述 (Analysis Services-多维数据) |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: aca12552338eb05549199b22af3a63e9f4b4a101
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68208638"
---
# <a name="logical-architecture-overview-analysis-services---multidimensional-data"></a>逻辑体系结构概述（Analysis Services - 多维数据）
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Analysis Services 在服务器部署模式下运行，该模式可确定不同 Analysis Services 模型类型使用的内存体系结构和运行时环境。 服务器模式在安装过程中确定。 **多维和数据挖掘模式**支持传统的 OLAP 和数据挖掘。 **表格模式下**支持表格模型。 **SharePoint 集成模式下**作为安装的 Analysis Services 的实例是指[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]对于 SharePoint，用于加载和查询 Excel 或[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]工作簿内的数据模型。  
  
 本主题介绍 Analysis Services 在多维和数据挖掘模式下操作时的基本体系结构。 有关其他模式的详细信息，请参阅[表格建模](../../../analysis-services/tabular-models/tabular-models-ssas.md)并[比较表格和多维解决方案](../../../analysis-services/comparing-tabular-and-multidimensional-solutions-ssas.md)。  
  
## <a name="basic-architecture"></a>基本体系结构  
 一个 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 实例可包含多个数据库，一个数据库中可同时包含 OLAP 对象和数据挖掘对象。 应用程序可以连接到指定的 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 实例和指定的数据库。 一个服务器计算机可以承载多个 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 实例。 实例[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]名为"\<服务器名 >\\< 实例名\>"。 下图显示了所有提及之间的关系[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]对象。  
  
 ![AMO 运行对象关系](../../../analysis-services/multidimensional-models/olap-logical/media/amo-runningobjects.gif "AMO 运行对象关系")  
  
 基本类是生成多维数据集所需的最小对象集。 此最小对象集可为一个维度、一个度量值组和一个分区。 聚合是可选的。  
  
 维度由属性和层次结构生成。 层次结构由属性的有序集形成，有序集的每个属性都与层次结构中的某一级别相对应。  
  
 多维数据集由维度和度量值组生成。 多维数据集的维度集合中的维度属于数据库的维度集合。 度量值组是同时拥有相同数据源视图和来自多维数据集的相同维度子集的度量值的集合。 度量值组拥有一个或多个分区，以便管理物理数据。 度量值组可有默认聚合设计。 度量值组中的所有分区都可以使用默认聚合设计；当然，每个分区也可以有自己的聚合设计。  
  
 Server 对象  
 在 AMO 中，每个 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 实例都视为不同的服务器对象；每个不同实例都通过不同的连接连接到 <xref:Microsoft.AnalysisServices.Server> 对象。 每个服务器对象都包含一个或多个数据源、数据源视图、数据库对象、程序集和安全角色。  
  
 Dimension 对象  
 每个数据库对象都可包含多个维度对象。 每个维度对象都包含一个或多个属性，这些属性组织为层次结构。  
  
 多维数据集对象  
 每个数据库对象都包含一个或多个多维数据集对象。 多维数据集通过其度量值和维度定义。 多维数据集中的度量值和维度派生自数据源视图中的表和视图，此数据源视图是多维数据集所基于的视图，或者是通过度量值定义和维度定义生成的视图。  
  
## <a name="object-inheritance"></a>对象继承  
 ASSL 对象模型包含有多个重复元素组。 例如，元素组"**维度**包含**层次结构**，"定义维度层次结构的元素。 这两**多维数据集**并**MeasureGroups**包含元素组"**维度**包含**层次结构**。"  
  
 除非显式重写，否则元素将会从更高级别继承这些重复元素组的详细信息。 例如，**翻译**有关**CubeDimension**相同**翻译**与其祖先元素的**多维数据集**。  
  
 显式重写从更高级别对象继承的属性时，对象无需显式重复更高级别对象的整个结构和属性。 而只需显式声明要重写的那些属性。 例如， **CubeDimension**可能会列出仅**层次结构**需要在中禁用**多维数据集**，其可见性需要更改，或其中部分**级别**尚未在提供的详细信息**维度**级别。  
  
 对象上指定的某些属性可以向子对象或后代对象中的同一属性提供在默认值。 例如， **Cube.StorageMode**提供的默认值为**Partition.StorageMode**。 对于继承的默认值，ASSL 向继承的默认值应用以下规则：  
  
-   在 XML 中，如果子对象的属性为空，则该属性的值将默认为继承的值。 但当您从服务器查询值时，服务器将会返回值为空的 XML 元素。  
  
-   无法通过编程方式来确定子对象属性是直接在子对象上设置的，还是直接继承的。  
  
## <a name="example"></a>示例  
 “进口”多维数据集包含“包”和“上一次”两个度量值以及“路线”、“源”和“时间”三个相关维度。  
  
 ![多维数据集示例 1](../../../analysis-services/multidimensional-models/olap-logical/media/cubeintro1.gif "多维数据集示例 1")  
  
 多维数据集周围更小的字母数字值是维度的成员。 示例成员为“陆地”（“路线”维度的成员）、“非洲”（“源”维度的成员）以及“第一季度”（“时间”维度的成员）。  
  
### <a name="measures"></a>度量值组  
 多维数据集单元中的值表示两个度量值：“包”和“上一次”。 包度量值表示导入包的数量并**之和**使用函数来聚合事实数据。 最后一个度量值表示收到的日期，并**最大**使用函数来聚合事实数据。  
  
### <a name="dimensions"></a>维度  
 “路线”维度表示进口货物到达目的地的方式。 该维度的成员包括“陆地”、“非陆地”、“航空”、“海路”、“公路”或“铁路”。 “源”维度表示进口货物的原产地，如“非洲”或“亚洲”。 “时间”维度表示一年的四个季度以及上半年和下半年。  
  
### <a name="aggregates"></a>聚合  
 多维数据集的业务用户可以确定每个维度的每个成员的所有度量值，而不用考虑该成员在维度中的级别，因为 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 将根据需要在更高级别聚合值。 例如，可以通过在下图中所示的时间维度中使用的日历时间层次结构根据标准日历层次结构聚合在上图中的度量值。  
  
 ![按时间维度组织的度量值的关系图](../../../analysis-services/multidimensional-models/olap-logical/media/cubeintro2.gif "按时间维度组织的度量值的关系图")  
  
 除了使用单个维度来聚合度量值以外，还可以使用不同维度的成员的组合来聚合度量值。 这样业务用户就可以同时对多个维度中的度量值进行评估。 例如，如果业务用户要分析各个季度通过航空运输从东半球和西半球进口的货物，则该业务用户可以对多维数据集发出查询来检索以下数据集。  
  
||||包|||上一次|||  
|-|-|-|--------------|-|-|----------|-|-|  
||||全部源|东半球|西半球|全部源|东半球|西半球|  
|所有时间|||25110|6547|18563|99/12/29|99/12/22|99/12/29|  
||上半年||11173|2977|8196|99/06/28|99/06/20|99/06/28|  
|||第一季度|5108|1452|3656|99/03/30|99/03/19|99/03/30|  
|||第二季度|6065|1525|4540|99/06/28|99/06/20|99/06/28|  
||下半年||13937|3570|10367|99/12/29|99/12/22|99/12/29|  
|||第三季度|6119|1444|4675|99/09/30|99/09/18|99/09/30|  
|||第四季度|7818|2126|5692|99/12/29|99/12/22|99/12/29|  
  
 定义多维数据集之后，可以创建新的聚合，也可以更改现有聚合以设置一些选项，比如是在处理期间预先计算聚合，还是在查询时进行计算。 **相关的主题：** [聚合和聚合设计](../../../analysis-services/multidimensional-models-olap-logical-cube-objects/aggregations-and-aggregation-designs.md)。  
  
### <a name="mapping-measures-attributes-and-hierarchies"></a>映射度量值、属性和层次结构  
 示例多维数据集中的度量值、属性和层次结构派生自多维数据集事实数据表和维度表中的以下各列。  
  
|度量值或属性（级别）|Members|源表|源列|示例列值|  
|------------------------------------|-------------|------------------|-------------------|-------------------------|  
|“包”度量值|不适用|ImportsFactTable|包|12|  
|“上一次”度量值|不适用|ImportsFactTable|上一次|99/05/03|  
|“路线”维度中的“路线类别”级别|非陆地、陆地|RouteDimensionTable|Route_Category|非陆地|  
|“路线”维度中的“路线”属性|航空、海路、公路、铁路|RouteDimensionTable|路由|海路|  
|“源”维度中的“半球”属性|东半球、西半球|SourceDimensionTable|半球|东半球|  
|“源”维度中的“洲”属性|非洲、亚洲、澳大利亚、欧洲、北 北美洲、南 美洲|SourceDimensionTable|洲|Europe|  
|“时间”维度中的“半年”属性|上半年、下半年|TimeDimensionTable|半年|下半年|  
|“时间”维度中的“季度”属性|第一季度、第二季度、第三季度、第四季度|TimeDimensionTable|季度|第三季度|  
  
 一个多维数据集单元中的数据通常派生自事实数据表中的多个行。 例如的空运成员、 非洲成员和第一季度成员的交集处的多维数据集单元包含一个值，通过聚合中的以下行**ImportsFactTable**事实数据表。  
  
|||||||  
|-|-|-|-|-|-|  
|Import_ReceiptKey|RouteKey|SourceKey|TimeKey|包|上一次|  
|3516987|1|6|1|15|99/01/10|  
|3554790|1|6|1|40|99/01/19|  
|3572673|1|6|1|34|99/01/27|  
|3600974|1|6|1|45|99/02/02|  
|3645541|1|6|1|20|99/02/09|  
|3674906|1|6|1|36|99/02/17|  
  
 在上表中，每个行都有相同的值**RouteKey**， **SourceKey**，并**TimeKey** ，该值指示这些行参与到同一多维数据集单元格的列。  
  
 这里显示的示例提供了一个非常简单的多维数据集，该多维数据集仅有一个度量值组，并且所有维度表均与事实数据表以星型架构联接。 另一个常见的架构为雪花型架构，在该架构中，一个或多个维度表联接到其他维度表，而不是直接联接到事实数据表。 **相关的主题：** [维度&#40;Analysis Services-多维数据&#41;](../../../analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-analysis-services-multidimensional-data.md)。  
  
 此处显示的示例仅包含一个事实数据表。 如果多维数据集具有多个事实数据表，则会将每个事实数据表中的度量值组织到度量值组中，并且通过已定义的维度关系使每个度量值组都与一组特定的维度相关。 这些关系是通过指定数据源视图中的参与表以及关系的粒度来定义的。 **相关的主题：** [维度关系](../../../analysis-services/multidimensional-models-olap-logical-cube-objects/dimension-relationships.md)。  
  
## <a name="see-also"></a>请参阅  
 [多维模型数据库](../../../analysis-services/multidimensional-models/multidimensional-model-databases-ssas.md)  
  
  
