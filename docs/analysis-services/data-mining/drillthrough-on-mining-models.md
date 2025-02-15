---
title: 对挖掘模型钻取功能 |Microsoft Docs
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ccb3e36739043684f2a86a082dd04ab749a0b031
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62467423"
---
# <a name="drillthrough-on-mining-models"></a>对挖掘模型的钻取功能
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  “钻取”  意味着能够查询挖掘模型或挖掘结构并且获取在模型中未公开的详细数据。  
  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 提供了两种不同的选项来钻取到事例数据中。 您可以钻取到用来生成数据的事例，也可以钻取到挖掘结构中的事例。  
  
## <a name="drillthrough-to-model-cases-vs-drillthrough-to-structure"></a>钻取到模型事例与钻取到结构  
 钻取到“模型事例”  用于查找与模型中的规则、模式或群集有关的其他详细信息。 例如，您不会使用客户联系信息进行聚类分析模型中的分析，即使数据可用，使用钻取功能，您可以获得从模型的访问权限。  
  
 相反，“钻取到结构”  数据旨在提供对在模型中未提供的信息的访问。 例如，一些结构列可能已从模型中排除，因为数据类型不兼容或者数据对分析无用。  
  
## <a name="enabling-drillthrough-on-a-model"></a>对模型启用钻取  
 若要对挖掘模型使用钻取，必须满足以下条件：  
  
-   可以只对模型事例配置钻取，而不对挖掘结构配置钻取，反之则不然。  换言之，必须对挖掘模型启用钻取才能允许钻取到挖掘结构。  
  
-   默认情况下，会对模型和结构禁用钻取。 如果使用数据挖掘向导，则用以对模型事例启用钻取的选项位于向导的最后一页。  
  
-   您可以向现有的挖掘模型中添加钻取功能，但是，如果这样做，必须重新处理该模型，才能钻取到所需的数据。  
  
-   除非已保留在定型过程中创建的缓存，否则钻取将不会工作。 有关控制缓存的属性的详细信息，请参阅 [对挖掘结构的钻取功能](../../analysis-services/data-mining/drillthrough-on-mining-structures.md)。  
  
## <a name="models-that-support-drillthrough"></a>支持钻取的模型  
 如果挖掘模型已经配置为允许进行钻取，而且如果您具有适当的权限，那么，当您浏览模型时，可以在相应的查看器中单击某个节点，并检索有关该特定节点中各个事例的详细信息。  
  
 不是所有的模型都支持钻取；这取决于创建模型所使用的算法。 下表列出了不支持钻取或对钻取的支持有限的模型的类型。 如果此处未列出某个模型类型，则表示该模型类型支持钻取。  
  
|**算法名称**|**对钻取的支持**|  
|------------------------|----------------------------------|  
|Microsoft Naïve Bayes 算法|不提供支持。<br /><br /> 这些算法不为内容中的特定节点分配事例。|  
|Microsoft 神经网络算法|不提供支持。<br /><br /> 这些算法不为内容中的特定节点分配事例。|  
|Microsoft 逻辑回归算法|不提供支持。<br /><br /> 这些算法不为内容中的特定节点分配事例。|  
|Microsoft 线性回归算法|支持。<br /><br /> 但是，由于该模型创建一个节点 ( **All**)，因此钻取时会返回该模型的所有定型事例。 如果定型集非常大，则加载结果可能会需要很长时间。|  
|Microsoft 时序算法|支持。<br /><br /> 但是，不能通过使用数据挖掘设计器的 **“挖掘模型查看器”** 来钻取到结构或事例数据， 而必须创建一个 DMX 查询。<br /><br /> 同样，不能钻取到特定节点，也不能编写一个 DMX 查询来检索时序模型内特定节点中的事例。 可以通过使用其他条件（如日期或属性值）来从模型或结构中检索事例数据。<br /><br /> 如果希望查看由 Microsoft 时序算法创建的 ARTXP 和 ARIMA 节点的详细信息，使用 [Microsoft 一般内容树查看器（数据挖掘）](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c)可能简单一些。|  
  
## <a name="related-tasks"></a>Related Tasks  
 有关如何将钻取功能用于挖掘模型的详细信息，请参阅下列主题：  
  
|“任务”|链接|  
|-----------|-----------|  
|在挖掘模型查看器中使用钻取|[从模型查看器使用钻取](../../analysis-services/data-mining/use-drillthrough-from-the-model-viewers.md)|  
|使用钻取检索模型的事例数据|[从挖掘模型钻取到事例数据](../../analysis-services/data-mining/drill-through-to-case-data-from-a-mining-model.md)|  
|对现有挖掘模型启用钻取|[对挖掘模型启用钻取](../../analysis-services/data-mining/enable-drillthrough-for-a-mining-model.md)|  
|有关特定的模型类型，请参阅钻取查询的示例。|[数据挖掘查询](../../analysis-services/data-mining/data-mining-queries.md)|  
|在数据挖掘向导中启用钻取|[完成向导（数据挖掘向导）](http://msdn.microsoft.com/library/6aef1548-35eb-42fd-ae87-63650a79eda1)。|  
  
## <a name="see-also"></a>请参阅  
 [对挖掘结构的钻取功能](../../analysis-services/data-mining/drillthrough-on-mining-structures.md)  
  
  
