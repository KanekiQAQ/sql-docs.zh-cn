---
title: Analysis Services 数据挖掘属性 |Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: bd1440e5ce0649d31f0ae6c0577c61e9e081a800
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62509919"
---
# <a name="data-mining-properties"></a>数据挖掘属性
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]

  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 支持下表中列出的数据挖掘服务器属性。 有关更多服务器属性以及如何设置这些属性的详细信息，请参阅 [Analysis Services 中的服务器属性](../../analysis-services/server-properties/server-properties-in-analysis-services.md)。  
  
 **适用范围：** 仅限多维服务器模式  
  
## <a name="non-specific-category"></a>非特定类别  
 **AllowSessionMiningModels**  
 一个布尔值属性，指示是否可以创建会话挖掘模型。  
  
 此属性的默认值为 False，指示不能创建会话挖掘模型。  
  
 **AllowAdHocOpenRowsetQueries**  
 布尔值属性，指示是否允许即席打开行集查询。  
  
 此属性的默认值为 False，指示在会话期间不允许打开行集查询。  
  
 **AllowedProvidersInOpenRowset**  
 字符串属性，标识在打开的行集中允许存在的提供程序，由以逗号/分号分隔的提供程序 ProgID 列表组成，或者为 [全部]。  
  
 **MaxConcurrentPredictionQueries**  
 有符号 32 位整数属性，用于定义并发预测查询的最大数目。  
  
## <a name="algorithms-category"></a>算法类别  
 **Microsoft_Association_Rules\ Enabled**  
 布尔值属性，指示是否启用 Microsoft_Association_Rules 算法。  
  
 **Microsoft_Clustering\ Enabled**  
 布尔值属性，指示是否启用 Microsoft_Clustering 算法。  
  
 **Microsoft_Decision_Trees\ Enabled**  
 布尔值属性，指示是否启用 Microsoft_DecisionTrees 算法。  
  
 **Microsoft_Naive_Bayes\ Enabled**  
 布尔值属性，指示是否启用 Microsoft_ Naive_Bayes 算法。  
  
 **Microsoft_Neural_Network\ Enabled**  
 布尔值属性，指示是否启用 Microsoft_Neural_Network 算法。  
  
 **Microsoft_Sequence_Clustering\ Enabled**  
 布尔值属性，指示是否启用 Microsoft_Sequence_Clustering 算法。  
  
 **Microsoft_Time_Series\ Enabled**  
 布尔值属性，指示是否启用 Microsoft_Time_Series 算法。  
  
 **Microsoft_Linear_Regression\ Enabled**  
 布尔值属性，指示是否启用 Microsoft_Linear_Regression 算法。  
  
 **Microsoft_Logistic_Regression\ Enabled**  
 布尔值属性，指示是否启用 Microsoft_Logistic_Regression 算法。  
  
> [!NOTE]  
>  除了可定义服务器上可用的数据挖掘服务的属性外，还有定义特定算法的行为的数据挖掘属性。 您可在创建单个数据挖掘模型时，对这些属性进行配置（不在服务器级别）。 有关详细信息，请参阅[数据挖掘算法（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)。  
  
## <a name="see-also"></a>请参阅  
 [物理体系结构（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/physical-architecture-analysis-services-data-mining.md)   
 [Analysis Services 中的服务器属性](../../analysis-services/server-properties/server-properties-in-analysis-services.md)   
 [确定 Analysis Services 实例的服务器模式](../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md)  
  
  
