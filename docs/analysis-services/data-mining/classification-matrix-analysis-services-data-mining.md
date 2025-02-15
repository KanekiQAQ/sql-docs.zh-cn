---
title: 分类矩阵 (Analysis Services-数据挖掘) |Microsoft Docs
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 1b6e9f3ccb71c0b3a45101cd1da660e6bc1af133
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62724782"
---
# <a name="classification-matrix-analysis-services---data-mining"></a>分类矩阵（Analysis Services - 数据挖掘）
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  “分类矩阵”  通过确定预测值是否与实际值匹配，将模型中的所有事例分为不同的类别。 然后会对每个类别中的所有事例进行计数，并在矩阵中显示总计。 分类矩阵是评估统计模型的标准工具，有时被称为“混淆矩阵”  。  
  
 在选择 **“分类矩阵”** 选项为每个指定的预测状态将实际值与预测值比较时创建的图表。 矩阵的行表示模型的预测值，而列则表示实际值。 分析中使用的类别为  ,  ,  “假负”和   
  
 分类矩阵是评估预测结果的重要工具，因为它使得结果更易于理解并说明错误预测的影响。 通过查看此矩阵中每个单元的金额和百分比，可以快速查看模型做出准确预测的频率。  
  
 本节说明如何创建分类矩阵以及如何解释结果。  
  
## <a name="understanding-the-classification-matrix"></a>理解分类矩阵  
 考虑作为数据挖掘基础教程的一部分而创建的模型。 [TM_DecisionTree] 模型用于帮助创建目标邮递活动，可利用该模型来预测哪些客户最有可能购买自行车。 为了测试此模型是否有效，您使用一个结果属性值 [Bike Buyer] 已知的数据集。 通常，将使用在创建用于定型模型的挖掘结构时留出的测试数据集。  
  
 只有两个可能的结果：是（客户可能购买自行车）和否（客户可能不购买自行车）。 因此，所得的分类矩阵相对比较简单。  
  
## <a name="interpreting-the-results"></a>解释结果  
 下表显示 TM_DecisionTree 模型的分类矩阵。 请记住对于这个可预测属性，0 表示“否”，1 表示“是”。  
  
|预测|0（实际）|1（实际）|  
|---------------|------------------|------------------|  
|0|362|144|  
|1|121|373|  
  
 包含值 362 的第一个结果单元格指示值 0 为“  真正”的次数。 因为 0 指示客户未购买自行车，因此该统计信息指出模型在 362 个事例中对非自行车购买者预测出了正确的值。  
  
 直接位于该单元格下方的单元格包含值 121，其指示“  假正”的数目，或者模型预测出某人会购买自行车而实际上该人却未购买的次数。  
  
 包含值 144 的单元格指示值 1 为“  假正”的次数。 因为 1 表示客户确实购买了自行车，因此该统计信息指出在 144 个事例中，该模型预测出某人不会购买自行车，而实际上却正相反。  
  
 最后，包含值 373 的单元格指示目标值 1 为“真正”的次数。 换言之，在 373 个事例中，该模型正确预测出某人会购买自行车。  
  
 将对角线上相邻的单元格中的值相加，根据得出的结果可以确定该模型的总体准确性。 一条对角线指示准确预测的总数，另一条对角线指示错误预测的总数。  
  
### <a name="using-multiple-predictable-values"></a>使用多个可预测值  
 [Bike Buyer] 事例由于只有两个可能值，因此特别容易解释。 如果可预测属性具有多个可能值，则分类矩阵会针对每个可能的实际值添加一个新列，然后为每个预测值统计匹配的数目。 下表显示有关另一个模型的结果，该模型中有三个可能值（0、1 和 2）。  
  
|预测|0（实际）|1（实际）|2（实际值）|  
|---------------|------------------|------------------|------------------|  
|0|111|3|5|  
|1|2|123|17|  
|2|19|0|20|  
  
 尽管添加更多的列会使报表看起来更复杂，但如果希望评估做出错误预测的累计成本，则这些附加详细信息会非常有帮助。 若要在对角线上求和或者比较不同行组合的结果，可以单击 **“分类矩阵”** 选项卡中提供的 **“复制”** 按钮，然后将该报表粘贴到 Excel 中。 也可使用同时支持 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本的客户端，如 Excel 数据挖掘客户端，直接在包括计数和百分比的 Excel 中创建分类报表。 有关详细信息，请参阅 [SQL Server Data Mining](http://go.microsoft.com/fwlink/?LinkID=77733)（SQL Server 数据挖掘）。  
  
## <a name="restrictions-on-the-classification-matrix"></a>分类矩阵的限制  
 分类矩阵仅可与离散可预测属性结合使用。  
  
 尽管在 **“挖掘准确性图表”** 设计器的 **“输入选择”** 选项卡上选择模型时可以添加多个模型， **“分类矩阵”** 选项卡将为每个模型显示单独的矩阵。  
  
## <a name="related-content"></a>相关内容  
 下列主题包含有关如何生成和使用分类矩阵和其他图表的详细信息。  
  
|主题|链接|  
|------------|-----------|  
|提供如何创建目标邮递模型的提升图的演练。|[数据挖掘基础教程](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c)<br /><br /> [测试提升图的准确性（数据挖掘基础教程）](http://msdn.microsoft.com/library/822d414b-4a39-473f-80c3-53476e30655a)|  
|说明相关的图表类型。|[提升图（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/lift-chart-analysis-services-data-mining.md)<br /><br /> [利润图（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/profit-chart-analysis-services-data-mining.md)<br /><br /> [散点图（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/scatter-plot-analysis-services-data-mining.md)|  
|说明如何将交叉验证用于挖掘模型和挖掘结构。|[交叉验证（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md)|  
|说明用于创建提升图和其他准确性图表的步骤。|[测试和验证任务和操作指南（数据挖掘）](../../analysis-services/data-mining/testing-and-validation-tasks-and-how-tos-data-mining.md)|  
  
## <a name="see-also"></a>请参阅  
 [测试和验证（数据挖掘）](../../analysis-services/data-mining/testing-and-validation-data-mining.md)  
  
  
