---
title: 在 Analysis Services 中创建计算的列 |Microsoft Docs
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 705428d2c2a6671452a1d95e06e500f4860574e0
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62472299"
---
# <a name="create-a-calculated-column"></a>创建计算列
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  通过计算列，可以将新数据添加到您的模型。 而不是粘贴或导入到列的值，您可以创建用于定义列的行级值的 DAX 公式。 计算列中每行的值在您创建有效公式并单击 Enter 后会进行计算和填充。 然后可以将计算列添加到报告或分析应用程序中，就像添加任何其他数据列一样。 本文介绍如何在模型设计器中使用 DAX 公式栏创建新的计算的列。  
  
#### <a name="to-create-a-new-calculated-column"></a>创建新的计算列  
  
1.  在模型设计器的“数据视图”中，选择要添加计算列的表，单击 **“列”** 菜单，然后单击 **“添加列”**。  
  
     **“添加列”** 将在最右侧的空列之上突出显示，并且光标将移到公式栏。  
  
     若要在两个现有列之间创建新列，请右键单击某个现有列，然后单击“插入列”。  
  
2.  在编辑栏中，执行以下操作之一：  
  
    -   键入等号，后跟公式。  
  
    -   键入等号，后跟 DAX 函数和该函数所需的参数。  
  
    -   单击函数按钮 (**fx**)，在“插入函数”对话框中选择一个类别和函数，然后单击“确定”。 在编辑栏中键入该函数所需的其他参数。  
  
3.  按 Enter 以接受该公式。  
  
     整个列将用该公式填充，将为每一行计算值。  
  
> [!TIP]  
>  可以在具有嵌套函数的现有公式中使用 DAX 公式记忆式键入功能。 刚好在插入点之前的文本将用于显示下拉列表中的值，并且插入点之后的所有文本都保持不变。  
  
## <a name="see-also"></a>请参阅  
 [计算列](../../analysis-services/tabular-models/ssas-calculated-columns.md)   
 [度量值](../../analysis-services/tabular-models/measures-ssas-tabular.md)  
  
  
