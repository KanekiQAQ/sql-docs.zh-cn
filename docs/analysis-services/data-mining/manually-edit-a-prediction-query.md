---
title: 手动编辑预测查询 |Microsoft Docs
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: aba181ab73ab730869eaa7930591cf21a947d20c
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62679408"
---
# <a name="manually-edit-a-prediction-query"></a>手动编辑预测查询
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  在使用预测查询生成器设计查询后，可通过在数据挖掘设计器的 **“挖掘模型预测”** 选项卡上切换到“查询文本”视图来修改查询。 屏幕的底部会出现文本编辑器，显示查询生成器创建的查询。  
  
 切换到“查询文本”视图对向查询中添加内容非常有用。 例如，可以添加 WHERE 子句或 ORDER BY 子句。  
  
 使用预测查询生成器中的网格可插入对象和列的名称，并设置单个预测函数的语法，然后切换到手动编辑模式以更改参数值。  
  
> [!NOTE]  
>  如果从“查询文本”视图切换回“设计”视图，则会丢失在“查询文本”视图中所做的任何更改    。  
  
### <a name="modify-a-query"></a>修改查询  
  
1.  在 **中数据挖掘设计器的** “挖掘模型预测” [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]选项卡上，单击 **SQL**。  
  
     屏幕底部的网格将替换为包含查询的文本编辑器。 可以在此编辑器中键入对查询的更改。  
  
2.  若要运行查询，请在 **“挖掘模型”** 菜单上选择 **“结果”** ，或者单击此按钮切换到查询结果。  
  
    > [!NOTE]  
    >  如果您创建的查询无效，则“结果”窗口将不会显示错误，也不会显示任何结果。 单击 **“设计”** 按钮，或者从 **“挖掘模型”** 菜单中选择 **“设计”** 或 **“查询”** 以解决问题并再次运行查询。  
  
## <a name="see-also"></a>请参阅  
 [数据挖掘查询](../../analysis-services/data-mining/data-mining-queries.md)   
 [预测查询生成器（数据挖掘）](http://msdn.microsoft.com/library/12900d49-db88-48bb-a5f4-0a9a172bc126)   
 [第 6 课：创建和使用预测&#40;数据挖掘基础教程&#41;](http://msdn.microsoft.com/library/b213cb58-2c40-4c89-b08b-d3c36a4afad3)  
  
  
