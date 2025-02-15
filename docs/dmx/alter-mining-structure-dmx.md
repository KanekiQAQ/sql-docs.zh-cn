---
title: 更改挖掘结构 (DMX) |Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 487fb5c04d623f2a4ef408cf35784dd57b067f4f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67913839"
---
# <a name="alter-mining-structure-dmx"></a>ALTER MINING STRUCTURE (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  创建基于现有挖掘结构的新挖掘模型。  当你使用**ALTER MINING STRUCTURE**语句以创建新的挖掘模型，该结构必须已经存在。 与此相反，当使用该语句中， [CREATE MINING MODEL &#40;DMX&#41;](../dmx/create-mining-model-dmx.md)，创建模型并同时自动生成其基础挖掘结构。  
  
## <a name="syntax"></a>语法  
  
```  
  
ALTER MINING STRUCTURE <structure>  
ADD MINING MODEL <model>  
(  
    <column definition list>  
  [(<nested column definition list>) [WITH FILTER (<nested filter criteria>)]]  
)  
USING <algorithm> [(<parameter list>)]   
[WITH DRILLTHROUGH]  
[,FILTER(<filter criteria>)]  
```  
  
## <a name="arguments"></a>参数  
 *structure*  
 要向其中添加挖掘模型的挖掘结构的名称。  
  
 *model*  
 挖掘模型的唯一名称。  
  
 *列定义列表*  
 列定义的逗号分隔列表。  
  
 *嵌套的列定义列表*  
 嵌套表中列的逗号分隔的列表（如果适用）。  
  
 *嵌套的筛选条件*  
 应用于嵌套表中的列的筛选表达式。  
  
 *算法*  
 数据挖掘算法，如提供程序定义的名称。  
  
> [!NOTE]  
>  可以通过检索当前提供程序支持的算法的列表[DMSCHEMA_MINING_SERVICES 行集](https://docs.microsoft.com/bi-reference/schema-rowsets/data-mining/dmschema-mining-services-rowset)。 若要查看支持的当前实例中的算法[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]，请参阅[数据挖掘属性](../analysis-services/server-properties/data-mining-properties.md)。  
  
 *参数列表*  
 可选。 由提供程序定义的算法所需参数的逗号分隔列表。  
  
 *筛选条件*  
 应用于事例表中的列的筛选表达式。  
  
## <a name="remarks"></a>备注  
 如果挖掘结构中包含组合键，则挖掘模型必须包括该结构中定义的所有键列。  
  
 如果模型不需要可预测列（例如，使用 [!INCLUDE[msCoName](../includes/msconame-md.md)] 聚类分析和 [!INCLUDE[msCoName](../includes/msconame-md.md)] 顺序分析和聚类分析算法生成的模型），则不必在语句中包括列定义。 生成的模型中的所有属性都将被视为输入。  
  
 在中**WITH**子句应用于事例表中，可以指定用于筛选和钻取选项：  
  
-   添加**筛选器**关键字和筛选条件。 筛选器应用于挖掘模型中的事例。  
  
-   添加**钻取**关键字来使挖掘模型的用户能够从模型结果深化到事例数据。 在数据挖掘扩展插件 (DMX) 中，仅当创建模型时才能启用钻取功能。  
  
 若要同时使用事例筛选和钻取，您组合在单个关键字**WITH**子句通过使用在下面的示例所示的语法：  
  
 `WITH DRILLTHROUGH, FILTER(Gender = 'Male')`  
  
## <a name="column-definition-list"></a>列定义列表  
 通过指定包括每一列对应的如下信息的列定义列表，来定义模型的结构：  
  
-   名称（必选）  
  
-   别名（可选）  
  
-   建模标志  
  
-   预测请求，用于向算法指示该列是否包含可预测的值，指示**PREDICT**或**PREDICT_ONLY**子句  
  
 使用以下列定义列表语法来定义单个列：  
  
```  
<structure column name>  [AS <model column name>]  [<modeling flags>]    [<prediction>]  
```  
  
### <a name="column-name-and-alias"></a>列名和别名  
 在列定义列表中使用的列名必须是该列在挖掘结构中使用的名称。 不过，您可以选择定义一个别名来表示挖掘模型中的结构列。 也可以为同一个结构列创建多个列定义，并为该列的每个副本分配一个不同的别名和预测用途。 如果您不定义别名，则默认情况下将使用结构列名。 有关详细信息，请参阅[为模型列创建别名](../analysis-services/data-mining/create-an-alias-for-a-model-column.md)。  
  
 对于嵌套的表的列，指定嵌套表的名称，指定的数据类型**表**，然后提供要包括在模型中，括在括号中的嵌套列的列表。  
  
 可以通过在嵌套表列定义后附上一个筛选条件表达式，来定义应用于嵌套表的筛选表达式。  
  
### <a name="modeling-flags"></a>建模标志  
 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] 支持将下列建模标志用在挖掘模型列中：  
  
> [!NOTE]  
>  NOT_NULL 建模标志应用于挖掘结构列。 有关详细信息，请参阅 [CREATE MINING STRUCTURE (DMX)](../dmx/create-mining-structure-dmx.md)。  
  
|||  
|-|-|  
|术语|定义|  
|**REGRESSOR**|指示该算法可以在回归算法的回归公式中使用指定列。|  
|**MODEL_EXISTENCE_ONLY**|指示该属性列的值没有该属性的存在重要。|  
  
 可以为一个列定义多个建模标志。 有关如何使用建模标志的详细信息，请参阅[建模标志&#40;DMX&#41;](../dmx/modeling-flags-dmx.md)。  
  
### <a name="prediction-clause"></a>预测子句  
 预测子句说明使用预测列的方式。 下表将列出可能的子句。  
  
|||  
|-|-|  
|**PREDICT**|该列可以由模型预测，并且它的值可用作输入以预测其他可预测列的值。|  
|**PREDICT_ONLY**|此列可以由模型预测，但其值不可用于输入事例来预测其他可预测列的值。|  
  
## <a name="filter-criteria-expressions"></a>筛选条件表达式  
 可以定义限制在挖掘模型中使用的事例的筛选器。 此筛选器可应用于事例表中的列和/或嵌套表中的行。  
  
 筛选条件表达式是简化的 DMX 谓词，与 WHERE 子句相似。 筛选表达式仅限于使用基本数学运算符、标量和列名的公式。 但 EXISTS 运算符是个例外，如果为子查询至少返回一行，则它的计算结果为 true。 可以通过使用常用逻辑运算符组合谓词：AND、 OR 和 NOT。  
  
 有关与挖掘模型一起使用的筛选器的详细信息，请参阅[挖掘模型的筛选器&#40;Analysis Services-数据挖掘&#41;](../analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining.md)。  
  
> [!NOTE]  
>  筛选器中的列必须是挖掘结构列。 不能对模型列或别名列创建筛选器。  
  
 有关 DMX 运算符和语法的详细信息，请参阅[挖掘模型列](../analysis-services/data-mining/mining-model-columns.md)。  
  
## <a name="parameter-definition-list"></a>参数定义列表  
 可以通过向参数列表中添加算法参数来调整模型的性能和功能。 可使用的参数取决于您在 USING 子句中指定的算法。 有关与每种算法关联的参数的列表，请参阅[数据挖掘算法&#40;Analysis Services-数据挖掘&#41;](../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)。  
  
 参数列表语法如下：  
  
```  
[<parameter> = <value>, <parameter> = <value>,...]  
```  
  
## <a name="example-1-add-a-model-to-a-structure"></a>示例 1：向结构中添加模型  
 下面的示例添加 Naive Bayes 挖掘模型与**New Mailing**最大属性状态数为 50 的挖掘结构和限制。  
  
```  
ALTER MINING STRUCTURE [New Mailing]  
ADD MINING MODEL [Naive Bayes]  
(  
    CustomerKey,   
    Gender,  
    [Number Cars Owned],  
    [Bike Buyer] PREDICT  
)  
USING Microsoft_Naive_Bayes (MAXIMUM_STATES = 50)  
```  
  
## <a name="example-2-add-a-filtered-model-to-a-structure"></a>示例 2：向结构中添加筛选后的模型  
 下面的示例添加挖掘模型`Naive Bayes Women`到**New Mailing**挖掘结构。 新模型有着与示例 1 中添加的挖掘模型相同的基本结构；但是，此模型将挖掘结构中的事例限定为 50 岁以上的女性客户。  
  
```  
ALTER MINING STRUCTURE [New Mailing]  
ADD MINING MODEL [Naive Bayes Women]  
(  
    CustomerKey,   
    Gender,  
    [Number Cars Owned],  
    [Bike Buyer] PREDICT  
)  
USING Microsoft_Naive_Bayes  
WITH FILTER([Gender] = 'F' AND [Age] >50)  
```  
  
## <a name="example-3-add-a-filtered-model-to-a-structure-with-a-nested-table"></a>示例 3:将筛选后的模型添加到具有嵌套表的结构  
 下面的示例将一个挖掘模型添加到市场篮挖掘结构的修改后的版本中。 在示例中使用的挖掘结构进行了修改，以添加**区域**列中，其中包含客户区域属性，和一个**Income Group**列，通过对客户收入进行分类值**高**，**中等**，或**低**。  
  
 挖掘结构还包括一个嵌套表，其中列出客户已购买的商品。  
  
 因为挖掘结构包含嵌套表，所以可以对事例表和/或嵌套表定义筛选器。 本示例结合使用事例筛选器和嵌套行筛选器，将事例限定为购买过 road 轮胎型号之一的欧洲高收入客户。  
  
```  
ALTER MINING STRUCTURE [Market Basket with Region and Income]  
ADD MINING MODEL [Decision Trees]  
(  
    CustomerKey,   
    Region,  
    [Income Group],  
    [Product] PREDICT (Model)   
WITH FILTER (EXISTS (SELECT * FROM [v Assoc Seq Line Items] WHERE   
 [Model] = 'HL Road Tire' OR  
 [Model] = 'LL Road Tire' OR  
 [Model] = 'ML Road Tire' )  
)  
) WITH FILTER ([Income Group] = 'High' AND [Region] = 'Europe')  
USING Microsoft_Decision Trees  
```  
  
## <a name="see-also"></a>请参阅  
 [数据挖掘扩展插件&#40;DMX&#41;数据定义语句](../dmx/dmx-statements-data-definition.md)   
 [数据挖掘扩展插件&#40;DMX&#41;数据操作语句](../dmx/dmx-statements-data-manipulation.md)   
 [数据挖掘扩展插件 (DMX) 语句引用](../dmx/data-mining-extensions-dmx-statements.md)  
  
  
