---
title: PredictProbability (DMX) |Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: e3a948ce783593ce649ad161d0db73a22a909885
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68041726"
---
# <a name="predictprobability-dmx"></a>PredictProbability (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  返回指定状态的概率。  
  
## <a name="syntax"></a>语法  
  
```  
  
PredictProbability(<scalar column reference>, [<predicted state>])  
```  
  
## <a name="applies-to"></a>适用范围  
 标量列。  
  
## <a name="return-type"></a>返回类型  
 一个标量值。  
  
## <a name="remarks"></a>备注  
 如果省略预测状态，则将使用概率最大的状态，不包括“缺失状态”存储桶。 若要包括缺少状态存储桶，设置\<预测状态 > 到**INCLUDE_NULL**。 若要返回缺少状态的概率，设置\<预测状态 > 为 NULL。  
  
> [!NOTE]  
>  某些挖掘模型不提供概率值，因此不能使用此函数。 此外，任何特定目标值的概率值都以不同方式计算，或可能有不同的解释，具体取决于所查询的模型类型。 有关如何为特定模型类型计算概率的详细信息，请参阅中的单个算法主题[挖掘模型内容&#40;Analysis Services-数据挖掘&#41;](../analysis-services/data-mining/mining-model-content-analysis-services-data-mining.md)。  
  
## <a name="examples"></a>示例  
 以下示例根据 TM Decision Tree 挖掘模型，使用自然预测联接确定某个人是否可能购买自行车，并且还确定该预测的概率。 在此示例中，有两个 PredictProbability 函数，分别用于每个可能的值。 如果省略此参数，则函数将返回最可能的值的概率。  
  
```  
SELECT  
  [Bike Buyer],  
  PredictProbability([Bike Buyer], 1) AS [Bike Buyer = Yes],  
  PredictProbability([Bike Buyer], 0) AS [Bike Buyer = No]  
FROM [TM Decision Tree]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
  '2-5 Miles' AS [Commute Distance],  
  'Graduate Degree' AS [Education],  
  0 AS [Number Cars Owned],  
  0 AS [Number Children At Home]) AS t  
```  
  
 示例结果：  
  
|Bike Buyer|Bike Buyer = Yes|Bike Buyer = No|  
|----------------|-----------------------|----------------------|  
|1|0.867074195848097|0.132755556974282|  
  
## <a name="see-also"></a>请参阅  
 [数据挖掘扩展插件&#40;DMX&#41;函数参考](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [函数&#40;DMX&#41;](../dmx/functions-dmx.md)   
 [通用预测函数&#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)  
  
  
