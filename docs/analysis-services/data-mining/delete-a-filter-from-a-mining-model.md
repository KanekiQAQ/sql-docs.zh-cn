---
title: 从挖掘模型中删除筛选器 |Microsoft Docs
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 66293cf1be2ced3106e2966a930639c12330a660
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68183330"
---
# <a name="delete-a-filter-from-a-mining-model"></a>从挖掘模型中删除筛选器
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  在挖掘模型上创建筛选器时，可以在数据源视图中的数据子集上创建模型。 对于在原始数据子集上测试模型的准确性，筛选器也很有用。  
  
 不过，若要再次查看全部事例，则必须删除筛选器。 此过程介绍如何删除筛选器条件或完全删除筛选器。  
  
### <a name="to-delete-a-condition-from-a-filter-on-a-mining-model"></a>从挖掘模型筛选器中删除条件  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]的解决方案资源管理器中，单击包含要筛选的挖掘模型的挖掘结构。  
  
2.  单击 **“挖掘模型”** 选项卡。  
  
3.  选择模型，然后右键单击打开快捷菜单。  
  
     或  
  
     选择该模型。 在 **“挖掘模型”** 菜单上，选择 **“设置模型筛选器”** 。  
  
4.  在“模型筛选器”  对话框中，右键单击网格中包含要删除的条件的行。  
  
5.  选择“删除”  。  
  
### <a name="to-clear-the-filter-on-a-mining-model-in-the-filter-editor-dialog-box"></a>在“筛选器编辑器”对话框中清除挖掘模型筛选器  
  
-   在“筛选器编辑器”  对话框中，右键单击网格中的任意行，然后选择“全部删除”  。  
  
## <a name="working-with-model-filters-using-the-properties-window"></a>通过“属性”窗口处理模型筛选器  
 若要删除整个筛选器，无需打开筛选器编辑器对话框。 您所创建的筛选条件可在挖掘模型的 **Filter** 属性中找到。  
  
> [!NOTE]  
>  可以在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中查看挖掘模型的属性，但不能在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中查看。  
  
#### <a name="to-clear-the-filter-on-a-mining-model-in-solution-explorer"></a>在解决方案资源管理器中清除挖掘模型筛选器  
  
1.  在解决方案资源管理器中，单击包含筛选器的挖掘模型。  
  
2.  在“属性”  窗口中，右键单击“筛选器”  属性中的筛选器文本，然后选择“全选”  。  
  
3.  按 Backspace 或 Delete 键。  
  
## <a name="see-also"></a>请参阅  
 [从挖掘模型钻取到事例数据](../../analysis-services/data-mining/drill-through-to-case-data-from-a-mining-model.md)   
 [挖掘模型任务和操作指南](../../analysis-services/data-mining/mining-model-tasks-and-how-tos.md)   
 [挖掘模型的筛选器（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining.md)  
  
  
