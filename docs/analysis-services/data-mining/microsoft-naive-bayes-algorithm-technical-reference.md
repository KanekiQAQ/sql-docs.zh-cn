---
title: Microsoft Naive Bayes 算法技术参考 |Microsoft Docs
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4e571e817caf566a919b5cce453566f4ace2a649
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68209829"
---
# <a name="microsoft-naive-bayes-algorithm-technical-reference"></a>Microsoft Naive Bayes 算法技术参考
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes 算法是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 提供的一种用于预测性建模的分类算法。 该算法计算输入列与可预测列之间的条件概率，并假定列相互独立。 由于此独立性假设，所以取名为 Naive Bayes。  
  
## <a name="implementation-of-the-microsoft-naive-bayes-algorithm"></a>Microsoft Naive Bayes 算法的实现  
 和其他 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 算法相比，此算法所需运算量较少，因而有助于快速生成挖掘模型，从而发现输入列与可预测列之间的关系。 此算法会考虑每对输入属性值和输出属性值。  
  
 不在此文档; 的作用域贝叶斯定理的数学性质的说明有关详细信息，请参阅标题为的 Microsoft research[学习 Bayesian 网络：知识和统计数据的组合](http://go.microsoft.com/fwlink/?LinkId=207029)。  
  
 有关如何调整所有模型中的概率以解释可能的缺失值的说明，请参阅[缺失值（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/missing-values-analysis-services-data-mining.md)。  
  
### <a name="feature-selection"></a>功能选择  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes 算法会自动执行功能选择，以限制生成模型时要考虑的值的数量。 有关详细信息，请参阅[功能选择（数据挖掘）](../../analysis-services/data-mining/feature-selection-data-mining.md)。  
  
|算法|分析方法|注释|  
|---------------|------------------------|--------------|  
|Naive Bayes|Shannon 平均信息量<br /><br /> Bayesian with K2 Prior<br /><br /> 使用统一先验的 Bayesian Dirichlet（默认）|Naive Bayes 仅接受离散或离散化的属性，因此它不能使用兴趣性分数。|  
  
 该算法的设计目的是最小化处理时间，并高效地选择最为重要的属性；但您可以通过设置下面的参数来控制其所用的数据：  
  
-   若要限制用作输入的值的数量，请降低 MAXIMUM_INPUT_ATTRIBUTES 的值。  
  
-   若要限制模型要分析的属性的数量，请降低 MAXIMUM_OUTPUT_ATTRIBUTES 的值。  
  
-   若要限制要为属性考虑的值的数量，请降低 MINIMUM_STATES 的值。  
  
## <a name="customizing-the-naive-bayes-algorithm"></a>自定义 Naive Bayes 算法  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes 算法支持多个参数，这些参数可影响所生成的挖掘模型的行为、性能以及准确性。 您还可以对模型列设置建模标志来控制数据的处理方式，或者对挖掘结构设置标志来指定如何处理缺失值或 Null 值。  
  
### <a name="setting-algorithm-parameters"></a>设置算法参数  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes 算法支持多个参数，这些参数可影响所生成的挖掘模型的性能和准确性。 下表对各参数进行了说明：  
  
 *MAXIMUM_INPUT_ATTRIBUTES*  
 指定算法在调用功能选择之前可以处理的最大输入属性数。 如果将此值设置为 0，则为输入属性禁用功能选择。  
  
 默认值为 255。  
  
 *MAXIMUM_OUTPUT_ATTRIBUTES*  
 指定算法在调用功能选择之前可以处理的最大输出属性数。 如果将此值设置为 0，则为输出属性禁用功能选择。  
  
 默认值为 255。  
  
 *MINIMUM_DEPENDENCY_PROBABILITY*  
 指定输入属性和输出属性之间的最小依赖关系概率。 该值用于限制算法生成的内容大小。 此属性可以设置为 0 到 1 之间的值。 较大的值减少模型内容中的特性数。  
  
 默认值为 0.5。  
  
 *MAXIMUM_STATES*  
 指定算法支持的最大属性状态数。 如果属性具有的状态数大于最大状态数，算法将使用该属性的最常见状态，并将剩余状态视为不存在。  
  
 默认值为 100。  
  
### <a name="modeling-flags"></a>建模标志  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 决策树算法支持下列建模标志。 创建挖掘结构或挖掘模型时，定义建模标志以指定分析期间如何处理每列中的值。 有关详细信息，请参阅[建模标志（数据挖掘）](../../analysis-services/data-mining/modeling-flags-data-mining.md)。  
  
|建模标志|描述|  
|-------------------|-----------------|  
|MODEL_EXISTENCE_ONLY|意味着，该列将被视为具有两个可能状态：Missing 和 Existing。 Null 表示缺失值。<br /><br /> 适用于挖掘模型列。|  
|NOT NULL|指示该列不能包含 Null。 如果 Analysis Services 在模型定型过程中遇到 Null 值，将会导致错误。<br /><br /> 适用于挖掘结构列。|  
  
## <a name="requirements"></a>要求  
 一个 Naive Bayes 树模型必须包含一个键列、至少一个可预测列和至少一个输入列。 所有属性均不能为连续属性；如果数据包含连续数值数据，则将会被忽略或离散化。  
  
### <a name="input-and-predictable-columns"></a>输入列和可预测列  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes 算法支持下表中列出的特定输入列和可预测列。 有关内容类型在用于挖掘模型中时的含义的详细信息，请参阅[内容类型（数据挖掘）](../../analysis-services/data-mining/content-types-data-mining.md)。  
  
|“列”|内容类型|  
|------------|-------------------|  
|输入属性|Cyclical、Discrete、Discretized、Key、Table 和 Ordered|  
|可预测属性|Cyclical、Discrete、Discretized、Table 和 Ordered|  
  
> [!NOTE]  
>  支持 Cyclical 和 Ordered 内容类型，但算法会将它们视为离散值，不会进行特殊处理。  
  
## <a name="see-also"></a>请参阅  
 [Microsoft Naive Bayes 算法](../../analysis-services/data-mining/microsoft-naive-bayes-algorithm.md)   
 [Naive Bayes 模型查询示例](../../analysis-services/data-mining/naive-bayes-model-query-examples.md)   
 [Naive Bayes 模型的挖掘模型内容（Analysis Services - 数据挖掘）](../../analysis-services/data-mining/mining-model-content-for-naive-bayes-models-analysis-services-data-mining.md)  
  
  
