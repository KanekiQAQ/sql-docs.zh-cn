---
title: 从模型查看器使用钻取 |Microsoft Docs
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9c1d636b4435aca63ffadbef3b45e93f3215a40c
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68209578"
---
# <a name="use-drillthrough-from-the-model-viewers"></a>从模型查看器使用钻取
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  根据模型类型，您可以从数据挖掘设计器的 **“挖掘模型查看器”** 选项卡上的浏览查看器中使用钻取功能，以便浏览在挖掘模型中使用的事例或查看挖掘结构中的其他列。 尽管因为模型中的模式无法直接链接到特定事例，导致许多模型类型不支持钻取功能，但以下模型类型是支持钻取功能的。  
  
 请注意，必须对模型启用钻取，并且您必须拥有适当的权限。 如果模型处于未处理状态，钻取选项也可能会被禁用；而与模型是否以前已处理和具有内容无关。 若要通过使用钻取功能检索模型事例数据，结构和模型的缓存必须是当前的。  
  
### <a name="use-drillthrough-in-the-microsoft-tree-viewer"></a>在 Microsoft 树查看器中使用钻取  
  
1.  在数据挖掘设计器中，选择某一决策树模型，并且选择 **“浏览模型”** 以便在 **“Microsoft 树查看器”** 中打开该模型。 在 SQL Server Management Studio 中，右键单击该模型，然后选择“浏览”   
  
2.  右键单击树图形中的任何节点，然后选择“钻取”  。  
  
3.  选择以下选项之一：**仅模型列**或**模型和结构列**。 如果您没有权限，则某个选项可能不可用。  
  
4.  “钻取”  对话框将打开，并且显示事例数据和/或结构数据。 该对话框的标题栏还包含说明，标识已从其执行钻取查询的节点。  
  
5.  右键单击结果中的任何地方，然后选择“全部复制”  以便将结果保存到剪贴板。  
  
### <a name="use-drillthrough-in-the-microsoft-cluster-viewer"></a>在 Microsoft 分类查看器中使用钻取  
  
1.  在数据挖掘设计器中，选择某一聚类分析模型，并且选择 **“浏览模型”** 以便在 **“Microsoft 分类查看器”** 中打开该模型。 在 SQL Server Management Studio 中，右键单击该模型，然后选择“浏览”  。  
  
2.  对“分类”  选项卡上，右键单击任何节点。  
  
3.  选择**钻取**，然后选择下列选项之一：**仅模型列**或**模型和结构列**。 如果您没有权限，则某个选项可能不可用。  
  
4.  “钻取”  对话框将打开，并且显示事例数据和/或结构数据。 该对话框的标题栏还包含标识事例分类的说明。  
  
5.  右键单击结果中的任何地方，然后选择“全部复制”  以便将结果保存到剪贴板。  
  
### <a name="use-drillthrough-in-the-microsoft-association-rules-viewer"></a>在 Microsoft 关联规则查看器中使用钻取  
  
1.  在数据挖掘设计器中，选择某一关联模型，并且选择 **“浏览模型”** 以便在 **“Microsoft 关联规则查看器”** 中打开该模型。 在 SQL Server Management Studio 中，右键单击该模型，然后选择“浏览”   
  
2.  在“规则”  选项卡上，右键单击表示某一规则的任何行。 在 **“项集”** 选项卡上，单击包含某一项集的任何行。  
  
3.  选择**钻取**，然后选择下列选项之一：**仅模型列**或**模型和结构列**。 如果您没有权限，则某个选项可能不可用。  
  
4.  “钻取”  对话框将打开，并且显示事例数据和/或结构数据。 该对话框的标题栏还包含标识规则名称的说明。  
  
5.  右键单击结果中的任何地方，然后选择“全部复制”  以便将完整事例结果保存到剪贴板。 您还可以选择 **“复制”** 以便只复制所选事例。 如果模型包含嵌套表列，则只粘贴嵌套表列的名称；若要检索每个事例的嵌套表列内的数据值，您必须对模型内容创建查询。  
  
### <a name="use-drillthrough-in-the-microsoft-sequence-cluster-viewer"></a>在 Microsoft 序列分类查看器中使用钻取  
  
1.  在数据挖掘设计器中，选择某一聚类分析模型，并且选择 **“浏览模型”** 以便在 **“Microsoft 分类查看器”** 中打开该模型。 在 SQL Server Management Studio 中，右键单击该模型，然后选择“浏览”  。  
  
2.  对“分类关系图”  选项卡上，右键单击表示某一分类的任何节点。 从 **“分类剖面图”** 选项卡，在分类剖面图中或者表示模型总量的分类中单击任何地方。  
  
3.  选择**钻取**，然后选择下列选项之一：**仅模型列**或**模型和结构列**。 如果您没有权限，则某个选项可能不可用。  
  
4.  “钻取”  对话框将打开，并且显示事例数据和/或结构数据。 该对话框的标题栏还包含标识事例分类的说明。  
  
5.  右键单击结果中的任何地方，然后选择“全部复制”  以便将结果保存到剪贴板。 如果模型包含嵌套表列，则只粘贴嵌套表列的名称；若要检索每个事例的嵌套表列内的数据值，您必须对模型内容创建查询。  
  
## <a name="see-also"></a>请参阅  
 [挖掘模型查看器任务和操作指南](../../analysis-services/data-mining/mining-model-viewer-tasks-and-how-tos.md)   
 [对挖掘模型的钻取功能](../../analysis-services/data-mining/drillthrough-on-mining-models.md)   
 [对挖掘结构的钻取功能](../../analysis-services/data-mining/drillthrough-on-mining-structures.md)  
  
  
