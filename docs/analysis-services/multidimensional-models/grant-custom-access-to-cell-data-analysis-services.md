---
title: 授予对单元数据 (Analysis Services) 的自定义访问权限 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 42348298676334a84d9c4d3664aec2eeda4feed6
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68177714"
---
# <a name="grant-custom-access-to-cell-data-analysis-services"></a>授予单元数据的自定义访问权限 (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  单元安全性用于允许或拒绝对多维数据集中度量值数据的访问。 下图显示了作为其角色仅允许访问特定度量值的用户进行连接时，数据透视表中允许和拒绝的度量值的组合。 此示例中， **分销商销售额** 和 **分销商总产品成本** 是通过此角色仅可访问的度量值。 所有其他度量值均被隐式拒绝（以下的下一节“允许访问特定度量值”中提供了用于获得此结果的步骤。）  
  
 ![数据透视表显示允许和拒绝单元格](../../analysis-services/multidimensional-models/media/ssas-permscellsallowed.png "数据透视表显示允许和拒绝单元格")  
  
 单元权限适用于单元内的数据，而不适用于其元数据。 请注意单元如何在查询结果中仍然可见，其值显示为 **#N/A** ，而非实际单元值。 除非客户端应用程序将该值转换，或者通过设置连接字符串中的 Secured Cell Value 属性指定了另一个值，否则在单元中将出现 **#N/A** 值。  
  
 若要完全隐藏单元格，则必须对限制成员维度、 维度属性和维度属性成员的可查看。 有关详细信息，请参阅 [授予对维度数据的自定义访问权限 (Analysis Services)](../../analysis-services/multidimensional-models/grant-custom-access-to-dimension-data-analysis-services.md)。  
  
 作为管理员，你可以指定角色成员是否具有对多维数据集内单元的读取、有条件读取或读/写权限。 授予对单元的权限是允许的最低级别的安全性，因此在开始应用此级别的权限之前，请谨记以下几点：  
  
-   单元级安全性无法扩展已限制在较高级别的权限。 示例：如果一个角色拒绝访问维度数据，则单元级安全性无法重写拒绝集。 另一示例：假如一个角色拥有对多维数据集的“读取”  权限和对单元的“读/写”  权限，那么单元数据权限将不会是“读/写”  ，而是“读取”  。  
  
-   通常需要在同一角色的维度成员和单元之间协调自定义权限。 例如，假如想拒绝访问不同分销商组合的多个与折扣相关的度量值。 已知 **分销商** 为维度数据， **折扣额** 为度量值，你将需要在同一角色内对两个度量值（使用本主题中的说明）的权限，以及对维度成员的权限进行结合。 有关设置维度权限的详细信息，请参阅 [授予对维度数据的自定义访问权限 (Analysis Services)](../../analysis-services/multidimensional-models/grant-custom-access-to-dimension-data-analysis-services.md) 。  
  
 单元级安全性通过 MDX 表达式指定。 由于一个单元就是一个元组（即跨潜在的多个维度和度量值的交点），因此有必要使用 MDX 来标识特定单元。  
  
## <a name="allow-access-to-specific-measures"></a>允许对特定度量值的访问  
 你可以使用单元安全性显式地选择哪些度量值可访问。 一旦专门标识了允许访问哪些成员，则所有其他度量值均变为不可访问。 这可能是通过 MDX 脚本实施的最简单的方案，如以下步骤所示。  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例，选择一个数据库，打开“角色”  文件夹，然后单击一个数据库角色（或创建一个新数据库角色）。 应该已指定成员身份，且该角色应具有对多维数据集的 **Read** 权限。 有关详细信息，请参阅 [授予多维数据集或模型权限 (Analysis Services)](../../analysis-services/multidimensional-models/grant-cube-or-model-permissions-analysis-services.md) 。  
  
2.  在“单元数据”  中，检查多维数据集选择，确保选择正确，然后选择“启用读取权限”  。  
  
     如果只选择了该复选框，而不提供一个 MDX 表达式，则其效果就与拒绝访问多维数据集中的所有单元一样。 这是因为每当 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 解析多维数据集单元的一个子集时，默认的允许集为空集。  
  
3.  输入以下 MDX 表达式。  
  
    ```  
    (Measures.CurrentMember IS [Measures].[Reseller Sales Amount]) OR (Measures.CurrentMember IS [Measures].[Reseller Total Product Cost])  
    ```  
  
     此表达式显式地标识了哪些度量值对用户可见。 通过此角色连接的用户将无法访问其他度量值。 请注意，[CurrentMember (MDX)](../../mdx/currentmember-mdx.md) 设置上下文，随后为允许的度量值。 该表达式的效果是，如果当前成员包括 **分销商销售额** 或 **分销商总产品成本**，则显示值。 否则，拒绝访问。 该表达式含有多个部分，每个部分由圆括号括起来。 **OR** 运算符用于指定多个度量值。  
  
## <a name="deny-access-to-specific-measures"></a>拒绝对特定度量值的访问  
 以下 MDX 表达式（也在“创建角色”   | “单元数据”   | “允许读取多维数据集内容”  中指定）具有相反的效果，将导致某些度量值不可访问。 此示例中，使用 **NOT** 和 **AND** 运算符使 **折扣金额** 和 **折扣率** 不可访问。 所有其他度量值将对通过此角色连接的用户可见。  
  
```  
(NOT Measures.CurrentMember IS [Measures].[Discount Amount]) AND (NOT Measures.CurrentMember IS [Measures].[Discount Percentage])  
```  
  
 在下图的 Excel 中，单元安全性显而易见：  
  
 ![Excel 列显示为不可用的单元格](../../analysis-services/multidimensional-models/media/ssas-permscellshidemeasure.png "Excel 显示为不可用的单元格的列")  
  
## <a name="set-read-permissions-on-calculated-measures"></a>设置对计算度量值的“读取”权限  
 对计算度量值的权限可独立于其组成部分进行设置。 如果想协调计算度量值和其依赖度量值之间的权限，请跳转到下一节“有条件读取”。  
  
 要了解读取权限是如何对一个计算度量值产生作用的，请考虑 AdventureWorks 中的 **分销商毛利润** 。 它派生于 **分销商销售额** 和 **分销商总产品成本** 度量值。 只要一个角色具有对 **分销商毛利润** 单元的“读取”权限，此度量值就可查看，即使对其他度量值的权限被明确拒绝。 作为演示，请将以下 MDX 表达式复制到“创建角色”   | “单元数据”   | “允许读取多维数据集内容”  中。  
  
```  
(NOT Measures.CurrentMember IS [Measures].[Reseller Sales Amount])  
AND (NOT Measures.CurrentMember IS [Measures].[Reseller Total Product Cost])  
```  
  
 在 Excel 中，使用当前角色连接到多维数据集，并选择全部三个度量值以查看单元安全性的影响。 请注意，拒绝集中的度量值是无法访问的，但计算度量值对用户可见。  
  
 ![Excel 表与可用和不可用 cellls](../../analysis-services/multidimensional-models/media/ssas-permscalculatedcells.png "可用和不可用 cellls 与 Excel 表")  
  
## <a name="set-read-contingent-permissions-on-calculated-measures"></a>设置对计算度量值的“有条件读取”权限  
 单元安全性为设置对参与计算的相关单元的权限提供了一个替代，即“有条件读取”。 再次考虑 **分销商毛利润** 示例。 输入前一节提供的同一 MDX 表达式时，此次将其置于“创建角色”   | “单元数据”  对话框（在“允许根据单元安全性读取单元内容”  下方的文本区域内），在 Excel 中查看时，其结果显而易见。 由于 **分销商毛利润** 视 **分销商销售额** 和 **分销商总产品成本**而定，现无法访问毛利润，因为无法访问其组成部分。  
  
> [!NOTE]  
>  在同一角色内同时设置对一个单元的“读取”和“有条件读取”权限将会发生什么？ 此角色将提供对单元的“读取”权限，而非“有条件读取”权限。  
  
 回顾之前章节的内容可知只要选择“启用有条件读取权限”  复选框，无需提供任何 MDX 表达式，即可拒绝对多维数据集内所有单元的访问。 这是因为每当 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 解析多维数据集单元的一个子集时，默认的允许集为空集。  
  
## <a name="set-readwrite-permissions-on-a-cell"></a>设置对单元的“读/写”权限  
 对单元的“读/写”权限用于启用回写（假设成员具有对多维数据集本身的“读/写”权限）。 授予的单元级权限不能大于授予的多维数据集级权限。 有关详细信息，请参阅 [Set Partition Writeback](../../analysis-services/multidimensional-models/set-partition-writeback.md) 。  
  
## <a name="see-also"></a>请参阅  
 [MDX 生成器（Analysis Services -多维数据）](http://msdn.microsoft.com/library/fecbf093-65ea-4e1b-b637-f04876f1cb0f)   
 [基本 MDX 脚本 (MDX)](../../analysis-services/multidimensional-models/mdx/the-basic-mdx-script-mdx.md)   
 [授予处理权限 (Analysis Services)](../../analysis-services/multidimensional-models/grant-process-permissions-analysis-services.md)   
 [授予维度的权限 (Analysis Services)](../../analysis-services/multidimensional-models/grant-permissions-on-a-dimension-analysis-services.md)   
 [授予对维度数据的自定义访问权限 (Analysis Services)](../../analysis-services/multidimensional-models/grant-custom-access-to-dimension-data-analysis-services.md)   
 [授予多维数据集或模型权限 (Analysis Services)](../../analysis-services/multidimensional-models/grant-cube-or-model-permissions-analysis-services.md)  
  
  
