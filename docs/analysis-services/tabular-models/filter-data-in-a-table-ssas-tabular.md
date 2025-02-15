---
title: 在 Analysis Services 表格模型表中筛选数据 |Microsoft Docs
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 9b1fcdda5bd49f8056a6035ec10da8fb52f1c5d1
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68162812"
---
# <a name="filter-data-in-a-table"></a>筛选表数据 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  您可以在导入数据时应用筛选器，以便控制加载到表中的行。 在导入数据后，不能删除单独的行。 不过，您可以应用自定义筛选器，以便控制显示行的方式。 不符合筛选条件的行会被隐藏。 您可以基于一列或多列进行筛选。 筛选器是累加式的，这意味着每个附加的筛选器都基于当前筛选器，从而进一步减少数据子集。  
  
> [!NOTE]  
>  筛选器预览窗口限制了显示的不同值的数量。 如果超出限制，则将显示一条消息。  
  
### <a name="to-filter-data-based-on-column-values"></a>基于列值筛选数据  
  
1.  在模型设计器中，选择一个表，然后单击您要基于其进行筛选的列标题中的箭头。  
  
2.  在“自动筛选”菜单中，执行以下操作之一：  
  
    -   在列值的列表中，选择或清除作为筛选依据的一个或多个值，然后单击 **“确定”** 。  
  
         如果值的数目极多，则可能无法在列表中显示各个项。 此时您将看到消息“项过多，无法显示。”  
  
    -   单击“数字筛选器”  或“文本筛选器”  （根据列类型），然后单击比较运算符命令（如“等于”  ），或单击“自定义筛选器”  。 在 **“自定义筛选器”** 对话框中，创建筛选器，然后单击 **“确定”** 。  
  
### <a name="to-clear-a-filter-for-a-column"></a>清除列筛选器  
  
1.  单击您要清除其筛选器的列的标题中的箭头。  
  
2.  单击**清除筛选器从\<列名 >** 。  
  
### <a name="to-clear-all-filters-for-a-table"></a>清除表的所有筛选器  
  
1.  在模型设计器中，选择您要为其清除筛选器的表。  
  
2.  单击 **“列”** 菜单，然后单击 **“清除所有筛选器”** 。  
  
## <a name="see-also"></a>请参阅  
 [筛选器和数据进行排序](http://msdn.microsoft.com/library/55ebd7a6-2458-4398-911f-fcfeb2413f1b)   
 [透视](../../analysis-services/tabular-models/perspectives-ssas-tabular.md)   
 [Roles](../../analysis-services/tabular-models/roles-ssas-tabular.md)  
  
  
