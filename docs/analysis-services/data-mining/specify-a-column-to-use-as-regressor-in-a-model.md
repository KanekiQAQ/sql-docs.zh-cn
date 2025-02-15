---
title: 使用某个列指定模型中的回归量 |Microsoft Docs
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0cfbe8f17c14518b2acf41bdb9ecf64b1679ffb2
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68182340"
---
# <a name="specify-a-column-to-use-as-regressor-in-a-model"></a>在模型中指定用作回归量的列
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  线性回归模型将可预测属性的值表示为结合了各种输入的公式的结果，这样，数据将尽可能接近地适合于估计的回归线。 该算法只接受数值作为输入，并且自动检测提供最佳调整的输入。  
  
 不过，您可以通过将 FORCE_REGRESSOR 参数添加到该模型并指定要使用的回归量，指定要作为回归量包括的列。 您最好在属性有意义的情况下（甚至在结果太小以致模型无法检测到时）或者您要确保属性包括在公式中时执行此操作。  
  
 下面的过程介绍的是如何使用 [神经网络教程](http://msdn.microsoft.com/library/42c3701a-1fd2-44ff-b7de-377345bbbd6b)使用的示例数据创建简单线形回归模型。 该模型不一定强健，但演示了如何使用数据挖掘设计器自定义线性回归模型。  
  
### <a name="how-to-create-a-simple-linear-regression-model"></a>如何创建简单线性回归模型  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]的 **解决方案资源管理器**中，展开 **“挖掘结构”** 。  
  
2.  双击 Call Center.dmm 以在设计器中将其打开。  
  
3.  从 **“挖掘模型”** 菜单中，选择 **“新建挖掘模型”** 。  
  
4.  对于算法，选择 **“Microsoft 线性回归”** 。 对于名称，键入“呼叫中心回归”。   
  
5.  在 **“挖掘模型”** 选项卡中，按如下所示更改列用法。 不在以下列表中的所有列都应设置为 **“忽略”** （如果尚未设置它们）。  
  
     FactCallCenterID**键**  
  
     ServiceGrade**仅预测**  
  
     Total Operators**输入**  
  
     AverageTimePerIssue**输入**  
  
6.  从 **“挖掘模型”** 菜单上，选择 **“设置模型参数”** 。  
  
7.  对于参数 FORCE_REGRESSOR，在“值”列中，键入用方括号括起来的列名称，各名称之间用逗号分隔，如下所示：   
  
    ```  
    [Average Time Per Issue],[Total Operators]  
    ```  
  
    > [!NOTE]  
    >  算法将自动检测哪些列是最佳回归量。 在您想要确保某一列包括在最终的公式中时，只需强制回归量。  
  
8.  从 **“挖掘模型”** 菜单中，选择 **“处理模型”** 。  
  
     在查看器中，该模型将表示为包含递归公式的单个节点。 您可以在 **“挖掘图例”** 中查看该公式，或者可以通过使用查询提取公式的系数。  
  
## <a name="see-also"></a>请参阅  
 [Microsoft 线性回归算法](../../analysis-services/data-mining/microsoft-linear-regression-algorithm.md)   
 [数据挖掘查询](../../analysis-services/data-mining/data-mining-queries.md)   
 [Microsoft 线性回归算法技术参考](../../analysis-services/data-mining/microsoft-linear-regression-algorithm-technical-reference.md)   
 [线性回归模型的挖掘模型内容（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/mining-model-content-for-linear-regression-models-analysis-services-data-mining.md)  
  
  
