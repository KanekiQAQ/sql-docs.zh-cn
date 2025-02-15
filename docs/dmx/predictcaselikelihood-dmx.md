---
title: PredictCaseLikelihood (DMX) |Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 12eddedce5d00c1bbc9e71995c2c9ceab34386d6
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68074718"
---
# <a name="predictcaselikelihood-dmx"></a>PredictCaseLikelihood (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  此函数返回输入事例适合现有模型的可能性。 仅适用于聚类分析模型。  
  
## <a name="syntax"></a>语法  
  
```  
  
PredictCaseLikelihood([NORMALIZED|NONNORMALIZED])  
```  
  
## <a name="arguments"></a>参数  
 NORMALIZED  
 返回值包含事例在模型中的概率除以事例不在模型中的概率所得的值。  
  
 NONNORMALIZED  
 返回值包含事例的原始概率，即事例属性概率的乘积。  
  
## <a name="applies-to"></a>适用范围  
 通过使用生成的模型[!INCLUDE[msCoName](../includes/msconame-md.md)]聚类分析和[!INCLUDE[msCoName](../includes/msconame-md.md)]序列聚类分析算法。  
  
## <a name="return-type"></a>返回类型  
 介于 0 和 1 之间的双精度浮点数。 该数值越接近 1，则指示事例出现在此模型中的概率越高。 该数值越接近 0，则指示事例越不可能出现在此模型中。  
  
## <a name="remarks"></a>备注  
 默认情况下的结果**PredictCaseLikelihood**规范化函数。 通常，当事例中的属性个数增加，并且任何两个事例的原始概率之间的差异大大缩小时，规范化的值更为有用。  
  
 下面的公式用于计算规范化的值（给定 x 和 y）：  
  
-   x = 事例基于聚类分析模型的可能性  
  
-   y = 边缘事例可能性（计算为事例基于计数定型事例的对数可能性）  
  
-   Z = Exp (log(x)-Log(Y))  
  
 规范化 = (z / (1 + z))  
  
## <a name="examples"></a>示例  
 以下示例返回在基于 [!INCLUDE[ssSampleDBCoShort](../includes/sssampledbcoshort-md.md)] DW 数据库的聚类分析模型中出现指定事例的可能性。  
  
```  
SELECT  
  PredictCaseLikelihood() AS Default_Likelihood,  
  PredictCaseLikelihood(NORMALIZED) AS Normalized_Likelihood,  
  PredictCaseLikelihood(NONNORMALIZED) AS Raw_Likelihood,  
FROM  
  [TM Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
  '2-5 Miles' AS [Commute Distance],  
  'Graduate Degree' AS [Education],  
  0 AS [Number Cars Owned],  
  0 AS [Number Children At Home]) AS t  
```  
  
 预期的结果：  
  
|Default_Likelihood|Normalized_Likelihood|Raw_Likelihood|  
|-------------------------|----------------------------|---------------------|  
|6.30672792729321E-08|6.30672792729321E-08|9.5824454056846E-48|  
  
 上述结果的差异演示了规范化的效果。 原始值**CaseLikelihood**建议用例的概率是大约 20%; 但是，当规范化结果之后，很明显事例的可能性是非常低。  
  
## <a name="see-also"></a>请参阅  
 [数据挖掘算法 &#40;Analysis Services-数据挖掘&#41;](../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)   
 [数据挖掘扩展插件&#40;DMX&#41;函数参考](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [函数&#40;DMX&#41;](../dmx/functions-dmx.md)   
 [通用预测函数&#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)  
  
  
