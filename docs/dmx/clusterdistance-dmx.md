---
title: ClusterDistance (DMX) |Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 1884bf191d842ba136165cf28aa14c23dd82b2e3
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68071074"
---
# <a name="clusterdistance-dmx"></a>ClusterDistance (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  **ClusterDistance**函数返回输入事例的距离，从指定的群集，或如果未指定，输入事例与最可能的分类的距离。  
  
## <a name="syntax"></a>语法  
  
```  
  
ClusterDistance([<ClusterID expression>])  
```  
  
## <a name="applies-to"></a>适用范围  
 只有在基础数据挖掘模型支持聚类分析时，此函数才可用。 该函数可用于任何类型的聚类分析模型 (EM、 K-means，等等)，但结果因算法而异。  
  
## <a name="return-type"></a>返回类型  
 一个标量值。  
  
## <a name="remarks"></a>备注  
 **ClusterDistance**函数返回输入的事例的概率最高的输入事例与群集之间的距离。  
  
 在 K-Means 聚类分析中，由于所有事例只能属于一个分类，并且成员身份权值为 1.0，因此分类距离始终是 0。 但是，在 K-Means 中，假定每个分类有一个中点。 您可以通过查询或浏览挖掘模型内容中的 NODE_DISTRIBUTION 嵌套表来获取中点的值。 有关详细信息，请参阅 [聚类分析模型的挖掘模型内容（Analysis Services - 数据挖掘）](../analysis-services/data-mining/mining-model-content-for-clustering-models-analysis-services-data-mining.md)。  
  
 在默认的 EM 聚类分析方法中，分类中的所有点都被视为具有均等的可能性；因此，分类就没有中点。 值**ClusterDistance**特定用例和特定的群集之间*N*的计算方式如下：  
  
 ClusterDistance(N) =1-(membershipWeight(N))  
  
 或：  
  
 ClusterDistance(N) = 1 ClusterProbability (N))  
  
## <a name="related-prediction-functions"></a>相关预测函数  
 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] 提供了以下可用于查询聚类分析模型的函数：  
  
-   使用[群集&#40;DMX&#41; ](../dmx/cluster-dmx.md)函数返回最可能的分类。  
  
-   使用[ClusterProbability &#40;DMX&#41; ](../dmx/clusterprobability-dmx.md)函数可获取某个事例属于特定分类的概率。 此值用作分类距离的反数。  
  
-   使用[PredictHistogram &#40;DMX&#41; ](../dmx/predicthistogram-dmx.md)函数返回输入事例存在每个模型的群集中的可能性直方图。  
  
-   使用[PredictCaseLikelihood &#40;DMX&#41; ](../dmx/predictcaselikelihood-dmx.md)函数返回从 0 到 1，指示输入的事例可能性的度量值是存在的模型得出学习的算法。  
  
## <a name="example1-obtaining-cluster-distance-to-the-most-likely-cluster"></a>示例 1:获取为最可能的分类的分类距离  
 下面的示例返回从指定事例到该事例最有可能属于的分类的距离。  
  
```  
SELECT  
    ClusterDistance()  
FROM  
    [TM Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
    '2-5 Miles' AS [Commute Distance],  
    'Graduate Degree' AS [Education],  
    0 AS [Number Cars Owned],  
    0 AS [Number Children At Home]) AS t  
```  
  
 示例结果：  
  
|表达式|  
|----------------|  
|0.0477390930705145|  
  
 若要查看是哪个分类，可以替换上一示例中的 `Cluster` 替换为 `ClusterDistance`。  
  
 示例结果：  
  
|$CLUSTER|  
|--------------|  
|分类 6|  
  
## <a name="example2-obtaining-distance-to-a-specified-cluster"></a>示例 2:获取到指定分类的距离  
 下面的语句使用挖掘模型内容架构行集返回挖掘模型中分类的节点 ID 和节点标题的列表。 您然后可以使用节点标题中的分类标识符参数作为**ClusterDistance**函数。  
  
```  
SELECT NODE_UNIQUE_NAME, NODE_CAPTION   
FROM <model>.CONTENT   
WHERE NODE_TYPE = 5  
```  
  
 示例结果：  
  
|NODE_UNIQUE_NAME|NODE_CAPTION|  
|------------------------|-------------------|  
|001|分类 1|  
|002|Cluster 2|  
  
 下面的语句示例返回指定事例到标记为 Cluster 2 的分类的距离。  
  
```  
SELECT  
    ClusterDistance('Cluster 2')  
AS [Cluster 2 Distance]  
FROM [TM Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
    '2-5 Miles' AS [Commute Distance],  
    'Graduate Degree' AS [Education],  
    0 AS [Number Cars Owned],  
    0 AS [Number Children At Home]) AS t  
```  
  
 示例结果：  
  
|Cluster 2 Distance|  
|------------------------|  
|0.97008209236394|  
  
## <a name="see-also"></a>请参阅  
 [群集&#40;DMX&#41;](../dmx/cluster-dmx.md)   
 [数据挖掘扩展插件&#40;DMX&#41;函数参考](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [函数&#40;DMX&#41;](../dmx/functions-dmx.md)   
 [聚类分析模型的挖掘模型内容（Analysis Services - 数据挖掘）](../analysis-services/data-mining/mining-model-content-for-clustering-models-analysis-services-data-mining.md)  
  
  
