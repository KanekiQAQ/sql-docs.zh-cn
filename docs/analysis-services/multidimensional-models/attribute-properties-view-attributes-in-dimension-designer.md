---
title: 在维度设计器中查看属性 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c8e0535a1df60b4a4e1550e2b49a02e7a60e2c04
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68179392"
---
# <a name="attribute-properties---view-attributes-in-dimension-designer"></a>特性属性 - 在维度设计器中查看特性
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  属性是针对维度对象创建的。 可以使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]中的维度设计器来查看和配置属性。 维度设计器的 **“维度结构”** 选项卡的 **“属性”** 窗格中列出了维度中的各属性。 使用该窗格可以添加、删除或配置属性。 您还可以选择属性以将其用作新建层次结构中的某一级别，或添加为现有层次结构中的某一级别。  
  
 若要查看维度中的属性，请打开该维度的维度设计器。 该设计器的 **“维度结构”** 选项卡的 **“属性”**  窗格中显示了维度中的各属性。 可以通过指向 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 的“维度”菜单中的“属性显示方式”，再单击以下命令之一，在列表、树或网格视图之间进行切换   。  
  
1.  在“列表”中显示属性。  以列表格式显示属性。 右键单击要从列表中删除的属性以重命名该属性，或更改其用法。 使用该视图可以生成层次结构。 特性信息和成员属性不可见。  
  
2.  在“树”中显示属性。  以树格式显示属性，其中维度作为树中的顶级节点。 展开属性可以查看其属性关系，还可以通过执行下列操作创建新的属性关系：  
  
    -   在 **“属性”** 窗口中单击要查看其属性的维度、特性或成员属性。  
  
    -   右键单击要从列表中删除的特性或成员属性以重命名该特性或成员属性，或更改其用法。  
  
     使用该视图可以查看和创建成员属性。 您还可以使用该视图生成层次结构。  
  
3.  在“网格”中显示属性。  以网格格式显示属性。 该网格显示下列各列：  
  
    -   **名称** ，显示 **“名称”** 属性的值。 键入其他名称可以更改该设置。  
  
    -   **用法** ，指定该属性是常规属性、键属性、父属性还是 AccountType 属性。 单击该列中的值便可选择其他设置。  
  
    -   **类型** ，指定该属性的商业智能类别。 单击该单元便可选择其他设置。  
  
    -   **键列** ，显示特性的 **KeyColumn** 属性的 OLE DB 数据类型。 该列无法更改。  
  
    -   **名称列** ，指示特性的 **NameColumn** 属性设置是否与 **KeyColumn** 属性设置为同一列。 该列无法更改。  
  
     单击网格中的任意行便可查看该特性的属性。  
  
     使用该视图可以创建和配置属性。  
  
 在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，下表中显示的图标根据属性的用法对属性进行标记。  
  
|图标|属性用法|  
|----------|---------------------|  
|![属性图标](../../analysis-services/multidimensional-models/media/as-icon-attribute.gif "属性图标")|常规或 AccountType|  
|![键属性图标](../../analysis-services/multidimensional-models/media/as-icon-key-attribute.gif "键属性图标")|键|  
|![父属性图标](../../analysis-services/multidimensional-models/media/as-icon-parent-attribute.gif "父属性图标")|Parent|  
  
## <a name="see-also"></a>请参阅  
 [维度特性属性参考](../../analysis-services/multidimensional-models/dimension-attribute-properties-reference.md)  
  
  
