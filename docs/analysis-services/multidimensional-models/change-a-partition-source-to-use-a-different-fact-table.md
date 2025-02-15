---
title: 更改分区源以使用不同的事实数据表 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 53609a1c252455f8372fd43b6a1323e93f0b3024
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62991778"
---
# <a name="change-a-partition-source-to-use-a-different-fact-table"></a>更改分区源以使用不同的事实数据表
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  在创建多维数据集的分区时，可以选择使用不同的事实数据表。 不同的数据表可能来自单个数据源视图，也可能来自不同的数据源视图或不同的数据源。 数据源视图也可能包含来自多个数据源的不同表。  
  
 多维数据集分区的所有事实数据表和维度必须与该多维数据集的事实数据表和维度具有相同的结构。 例如，不同的事实数据表可以具有相同结构，同时却包含不同年份或不同产品系列的数据。  
  
 您可以在分区向导的 **“指定源信息”** 页中指定事实数据表。 在该页的 **“查找范围”** 旁边的“分区源”组中，指定要在其中进行查找的数据源或数据源视图。 然后，单击 **“查找表”** ，再在 **“可用表”** 下选择要使用的表。  
  
 使用不同的事实数据表时，请确保分区间没有重复的数据。 例如，如果一个事实数据表只包含 2012 年的事务，而另一个事实数据表只包含 2013 年的事务，则二者所包含的数据相互独立。 同样，针对非重复的产品系列或非重复的地域的事实数据表也是相互独立的。  
  
 可以使用包含重复数据的不同事实数据表，但建议不要这样做。 因为在此情况下，您必须在分区中使用筛选器，以确保一个分区使用的数据不被任何其他分区使用。 有关详细信息，请参阅[创建和管理本地分区 (Analysis Services)](../../analysis-services/multidimensional-models/create-and-manage-a-local-partition-analysis-services.md)。  
  
## <a name="see-also"></a>请参阅  
 [创建和管理本地分区 (Analysis Services)](../../analysis-services/multidimensional-models/create-and-manage-a-local-partition-analysis-services.md)  
  
  
