---
title: 设计聚合 (XMLA) |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b9f681b3c99bd0e8351a844f28b16be6249de199
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "63288496"
---
# <a name="designing-aggregations-xmla"></a>设计聚合 (XMLA)
  聚合设计与特定度量值组的分区相关联，以确保分区在存储聚合时使用相同的结构。 对分区使用相同的存储结构可让你可以轻松地定义可使用更高版本合并的分区[MergePartitions](https://docs.microsoft.com/bi-reference/xmla/xml-elements-commands/mergepartitions-element-xmla)命令。 有关聚合设计的详细信息，请参阅[聚合和聚合设计](../../analysis-services/multidimensional-models-olap-logical-cube-objects/aggregations-and-aggregation-designs.md)。  
  
 若要定义的聚合设计的聚合，可以使用[DesignAggregations](https://docs.microsoft.com/bi-reference/xmla/xml-elements-commands/designaggregations-element-xmla)命令 XML for Analysis (XMLA) 中。 **DesignAggregations**命令的属性可以标识要用作引用以及如何控制设计过程中基于该引用的聚合设计。 使用**DesignAggregations**命令和其属性，您可以以迭代方式或成批设计聚合，然后查看生成的设计统计信息来评估设计过程。  
  
## <a name="specifying-an-aggregation-design"></a>指定聚合设计  
 [对象](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/object-element-xmla)的属性**DesignAggregations**命令必须包含对现有聚合设计的对象引用。 该对象引用包含数据库标识符、多维数据集标识符、度量值组标识符和聚合设计标识符。 如果该聚合设计不存在，则会出现错误。  
  
## <a name="controlling-the-design-process"></a>控制设计过程  
 可以使用的以下属性**DesignAggregations**命令来控制用于聚合设计定义聚合的算法：  
  
-   [步骤](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/steps-element-xmla)属性确定多少次迭代**DesignAggregations**控制权返还给客户端应用程序之前，应该执行命令。  
  
-   [时间](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/time-element-xmla)属性确定的毫秒数**DesignAggregations**控制权返还给客户端应用程序之前，应该执行命令。  
  
-   [优化](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/optimization-element-xmla)属性确定的性能提高百分比估计的值**DesignAggregations**命令应尝试获得。 如果要以迭代方式设计聚合，只需在第一条命令中发送此属性。  
  
-   [存储](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/storage-element-xmla)属性确定磁盘存储，在使用的字节数的估计的量**DesignAggregations**命令。 如果要以迭代方式设计聚合，只需在第一条命令中发送此属性。  
  
-   [具体化](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/materialize-element-xmla)属性确定是否**DesignAggregations**命令应创建在设计过程中定义的聚合。 如果要以迭代方式设计聚合，则应在准备好保存所设计的聚合之前，将此属性设置为 false。 如果设置为 true，则当前设计过程将会终止，并会将所定义的聚合添加到指定的聚合设计中。  
  
## <a name="specifying-queries"></a>指定查询  
 DesignAggregations 命令通过包含一个或多个支持基于使用情况的优化命令**查询**中的元素[查询](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/queries-element-xmla)属性。 **查询**属性可以包含一个或多个[查询](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/query-element-xmla)元素。 如果**查询**属性不包含任何**查询**中指定的元素，聚合设计**对象**元素使用包含的默认结构已将组常规聚合。 此组常规聚合设计以满足中指定的条件**优化**并**存储**的属性**DesignAggregations**命令。  
  
 每个 **Query** 元素表示一个目标查询，设计进程使用这些查询定义以最常用的查询为目标的聚合。 您可以指定自己的目标查询，也可以使用查询日志中 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例存储的信息来检索最常用查询的相关信息。 基于使用情况的优化向导，使用查询日志来检索目标查询基于时间、 使用情况或指定的用户，会在发送时**DesignAggregations**命令。 有关详细信息，请参阅[基于使用情况的优化向导的 F1 帮助](http://msdn.microsoft.com/library/e5f5a938-ae7c-4f4e-9416-a7f94ac82763)。  
  
 如果您迭代式地设计聚合，则只需要在第一个 **DesignAggregations** 命令中传递目标查询，这是因为 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例存储这些目标查询，并在执行后续 **DesignAggregations** 命令时使用这些查询。 在迭代进程的第一个 **DesignAggregations** 命令中传递目标查询后，任何在 **DesignAggregations** 属性中包含目标查询的后续 **Queries** 命令都会生成错误。  
  
 **Query** 元素包含一个以逗号分隔的值，该值包含以下参数：  
  
 *Frequency*,*Dataset*[,*Dataset*...]  
  
 *频率*  
 与该查询此前执行的次数对应的加权系数。 如果 **Query** 元素表示一个新查询，则 *Frequency* 值表示设计进程用于评估该查询的加权系数。 在设计进程中，随着频率值变大，该查询的权重也会增加。  
  
 *数据集*  
 一个数字字符串，该字符串指定维度中的哪些属性要包括在该查询中。 此字符串的字符个数必须与维度中的属性个数相同。 零 (0) 指示指定序号位置的属性不包括在指定维度的查询中，而 1 指示指定序号位置的属性包括在指定维度的查询中。  
  
 例如，字符串“011”所指的查询涉及具有三个属性的维度，其中第二个和第三个属性包括在该查询中。  
  
> [!NOTE]  
>  某些属性与数据集无关。 有关排除的属性的详细信息，请参阅[查询元素&#40;XMLA&#41;](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/query-element-xmla)。  
  
 包含聚合设计的度量值组中的每个维度都由 *Query* 元素中的 **Dataset** 值表示。 *Dataset* 值的顺序必须与包括在度量值组中的维度的顺序一致。  
  
## <a name="designing-aggregations-using-iterative-or-batch-processes"></a>使用迭代过程或批处理过程设计聚合  
 可以使用**DesignAggregations**命令作为一个迭代过程或批处理过程，具体取决于所需的设计过程的交互性的一部分。  
  
### <a name="designing-aggregations-using-an-iterative-process"></a>使用迭代过程设计聚合  
 若要以迭代方式设计聚合，发送多个**DesignAggregations**命令提供对设计过程进行精细控制。 聚合设计向导使用同样的方法精确控制设计过程。 有关详细信息，请参阅[聚合设计向导 F1 帮助](http://msdn.microsoft.com/library/39e23cf1-6405-4fb6-bc14-ba103314362d)。  
  
> [!NOTE]  
>  以迭代方式设计聚合需要显式会话。 有关显式会话的详细信息，请参阅[管理连接和会话&#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/managing-connections-and-sessions-xmla.md)。  
  
 若要开始迭代过程，第一次发送**DesignAggregations**命令包含以下信息：  
  
-   **存储**并**优化**属性值在整个设计过程的目标。  
  
-   **步骤**并**时间**的设计过程的第一步是有限的属性值。  
  
-   如果您希望基于使用情况的优化**查询**查询上整个设计过程目标的属性，其中包含目标。  
  
-   **具体化**属性设置为 false。 将此属性设置为 false 指示当命令完成时，设计过程不会将所定义的聚合保存到聚合设计中。  
  
 当第一个**DesignAggregations**命令执行完毕后，该命令将返回一个包含设计统计信息行集。 您可以对这些设计统计信息进行评估，以确定设计过程是应继续还是已完成。 如果该进程也应继续，则然后发送另一个**DesignAggregations**包含的命令**步骤**并**时间**与其设计的此步骤中处理的值仅限。 您要对结果统计信息进行评估，然后确定设计过程是否应继续。 发送的此迭代过程**DesignAggregations**命令和评估结果会一直持续达到目标并具有一组适当的聚合定义。  
  
 你已达到所需的聚合集后，发送最后一个**DesignAggregations**命令。 最后一条**DesignAggregations**命令应具有其**步骤**属性设置为 1 并且其**具体化**属性设置为 true。 通过使用这些设置，此最后一条**DesignAggregations**命令完成设计过程，并将定义的聚合保存到聚合设计。  
  
### <a name="designing-aggregations-using-a-batch-process"></a>使用批处理过程设计聚合  
 您还可以通过将发送一个设计批处理过程中的聚合**DesignAggregations**包含的命令**步骤**，**时间**，**存储**，并**优化**属性值的目标和有限的整个设计过程。 如果您希望基于使用情况的优化，在设计过程目标的目标查询，也应在包含**查询**属性。 此外，请确保**具体化**属性被设置为 true，因此，设计过程中将所定义的聚合保存到聚合设计时在该命令完成。  
  
 您可以在隐式或显式会话中使用批处理过程来设计聚合。 有关隐式和显式会话的详细信息，请参阅[管理连接和会话&#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/managing-connections-and-sessions-xmla.md)。  
  
## <a name="returning-design-statistics"></a>返回设计统计信息  
 当**DesignAggregations**命令将控制权返还给客户端应用程序，该命令将返回包含该命令的设计统计信息的单个行的行集。 下表列出了该行集包含的列。  
  
|“列”|数据类型|Description|  
|------------|---------------|-----------------|  
|步骤|Integer|在将控制权返还给客户端应用程序之前，命令执行的步数。|  
|Time|Long integer|在将控制权返还给客户端应用程序之前，命令运行的毫秒数。|  
|Optimization|双精度|在将控制权返还给客户端应用程序之前，命令获得的性能提高百分比估计值。|  
|存储|Long integer|在将控制权返还给客户端应用程序之前，命令占用的存储空间的估计字节数。|  
|Aggregations|Long integer|在将控制权返还给客户端应用程序之前，命令定义的聚合数。|  
|LastStep|Boolean|指示行集中的数据是否表示设计过程的最后一步。 如果**具体化**命令的属性已设置为此列的值设置为 true，则为 true。|  
  
 可以使用后每个返回的行集中包含的设计统计信息**DesignAggregations**在迭代中的命令和批处理设计。 在迭代设计过程中，您可以使用设计统计信息来确定并显示进度。 以批处理方式设计聚合时，您可以使用设计统计信息来确定命令所创建的聚合数。  
  
## <a name="see-also"></a>请参阅  
 [在 Analysis Services 中使用 XMLA 开发](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
