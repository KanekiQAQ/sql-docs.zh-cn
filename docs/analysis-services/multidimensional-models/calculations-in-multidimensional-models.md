---
title: 多维模型中的计算 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c72d43c95013074051356c690ac2d7abf0a575e0
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62990513"
---
# <a name="calculations-in-multidimensional-models"></a>多维模型中的计算
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  可以使用多维数据集设计器的“计算”选项卡创建计算成员、命名集和其他多维表达式 (MDX) 计算。   
  
 **“计算”** 选项卡有以下三个窗格：  
  
-   **“脚本组织程序”** 窗格列出了多维数据集中的计算。 使用此窗格可以创建、组织和选择计算，以便进行编辑。  
  
-   **“计算工具”** 窗格提供了创建计算所需的元数据、函数和模板。  
  
-   “计算表达式”窗格支持窗体视图和脚本视图。  
  
  
## <a name="creating-a-new-calculation"></a>创建新计算  
 若要创建新计算，请在多维数据集设计器的 **“计算”** 选项卡的 **“多维数据集”** 菜单中，单击 **“新建计算成员”** 、 **“新建命名集”** 或 **“新建脚本命令”** ，具体取决于希望创建的计算的类型。 还可以在工具栏上单击任何相应的按钮，或在“脚本组织程序”窗格中右键单击任何位置，再在快捷菜单中单击一个命令。  此操作会在 **“脚本组织程序”** 窗格中添加新的计算，并在“计算表达式”窗格内的计算窗体中显示它的字段。 如果创建新脚本，则此操作将在“计算表达式”窗格中打开“脚本”视图。 有关生成这三种计算类型的详细信息，请参阅 [创建计算成员](../../analysis-services/multidimensional-models/create-calculated-members.md)、 [创建命名集](../../analysis-services/multidimensional-models/create-named-sets.md)和 [定义赋值和其他脚本命令](../../analysis-services/multidimensional-models/define-assignments-and-other-script-commands.md)。  
  
## <a name="editing-scripts"></a>编辑脚本  
 可以在 **“计算”** 选项卡的“计算表达式”窗格中编辑脚本。“计算表达式”窗格有两个视图：脚本视图和窗体视图。 窗体视图显示单个命令的表达式和属性。 编辑 MDX 脚本时，表达式框将充满整个窗体视图。  
  
 脚本视图提供了用来编辑脚本的代码编辑器。 当“计算表达式”窗格在脚本视图中时， **“脚本组织程序”** 窗格将隐藏。 脚本视图提供了彩色编码、括号匹配、自动完成和 MDX 代码区域。 MDX 代码区域可以折叠或展开，以便于进行编辑。  
  
 通过单击 **“多维数据集”** 菜单，再指向 **“计算显示位置”** ，再单击 **“脚本”** 或 **“窗体”** ，可以在窗体视图和脚本视图之间进行切换。 还可以在工具栏上单击 **“窗体视图”** 或 **“脚本视图”** 。  
  
## <a name="changing-solve-order"></a>更改求解顺序  
 将按照计算在 **“脚本组织程序”** 窗格中的列出的顺序来对计算求解。 通过右键单击任何计算，再在快捷菜单上单击“上移”或“下移”，可以让计算重新排序。   还可以单击计算，再单击工具栏上的 **“上移”** 或 **“下移”** 按钮。  
  
 对于计算单元和计算成员，可以手动覆盖此顺序。 有关传递顺序和求解次序的详细信息，请参阅[理解传递顺序和求解次序 (MDX)](../../analysis-services/multidimensional-models/mdx/mdx-data-manipulation-understanding-pass-order-and-solve-order.md)。  
  
## <a name="deleting-a-calculation"></a>删除计算  
 若要删除现有计算，请在 **“计算”** 选项卡中的 **“脚本组织程序”** 窗格中选择要删除的计算，再在 **“编辑”** 菜单中单击 **“删除”** ，或在工具栏上单击 **“删除”** 图标。 还可以在“脚本组织程序”窗格中右键单击计算，并从快捷菜单中选择“删除”。    
  
  
