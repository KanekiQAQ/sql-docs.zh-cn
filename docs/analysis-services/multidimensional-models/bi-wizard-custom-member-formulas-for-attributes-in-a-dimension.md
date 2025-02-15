---
title: 维度中的设置的属性的自定义成员公式 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 29b4d83956e6963aa7df0b4a1afd11edda6c83c4
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68179106"
---
# <a name="bi-wizard---custom-member-formulas-for-attributes-in-a-dimension"></a>BI 向导 - 维度中特性的自定义成员公式
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  通过在多维数据集或维度中添加自定义成员公式增强功能，可以使用多维表达式 (MDX) 表达式的结果替换与维度成员关联的默认聚合。 （此增强功能将设置维度中的指定特性的 **CustomRollupColumn** 属性。）  
  
> [!NOTE]  
>  自定义成员公式只对基于现有数据源的维度可用。 对于不使用数据源所创建的维度，则必须运行架构生成向导，以便在添加自定义成员公式之前创建数据源视图。  
  
 若要添加自定义成员公式，请使用商业智能向导，并在 **“选择增强功能”** 页上选择 **“创建自定义成员公式”** 选项。 然后，此向导将指引您完成相应的步骤，以选择要应用自定义成员公式的维度，并启用自定义成员公式。  
  
## <a name="selecting-a-dimension"></a>选择维度  
 在向导的第一个 **“创建自定义成员公式”** 页中，指定希望应用自定义成员公式的维度。 添加到此所选维度中的自定义成员公式增强功能将使维度发生更改。 所有包含选定维度的多维数据集都将继承这些更改。  
  
## <a name="enabling-a-custom-member-formula"></a>启用自定义成员公式  
 在第二个 **“创建自定义成员公式”** 页中，将包含自定义成员公式的源列与维度中的一个或多个属性关联。 在 **“属性”** 列中，选中要与自定义成员公式列关联的属性旁边的复选框。 选择每个属性之后，向导将显示 **“选择列”** 对话框。 在此对话框中，单击包含公式的维度表中的列。 如果在关闭“选择列”对话框之后希望更改选择，请单击希望更改的“源列”单元，再单击省略号 ( **...** ) 以再次打开“选择列”对话框。     
  
## <a name="see-also"></a>请参阅  
 [使用商业智能向导增强维度](http://msdn.microsoft.com/library/12d995d1-75ca-4890-bf4b-a2656910b8d0)  
  
  
