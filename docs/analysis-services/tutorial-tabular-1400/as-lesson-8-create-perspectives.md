---
title: Analysis Services 教程第课 8： 创建透视 |Microsoft Docs
ms.date: 03/08/2019
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
monikerRange: '>= sql-server-2017 || = sqlallproducts-allversions'
ms.openlocfilehash: c74e457d29f39201601a3a62d651cce97c52fa70
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68207320"
---
# <a name="create-perspectives"></a>创建透视

[!INCLUDE[ssas-appliesto-sql2017-later-aas](../../includes/ssas-appliesto-sql2017-later-aas.md)]

在本课程中，将创建 Internet Sales 透视。 透视可定义模型的可查看子集，借此您可以将注意力集中在特定业务或特定应用程序上。 当用户使用透视连接到模型时，他们只能看到那些模型对象 （表、 列、 度量值、 层次结构和 Kpi） 作为该透视中定义的字段。 若要了解详细信息，请参阅[透视](../tabular-models/perspectives-ssas-tabular.md)。
  
在本课程中创建的 Internet Sales 透视将排除 DimCustomer 表对象。 在创建从视图中排除某些对象的透视时，该对象仍存在模型中。 但是，不在报告客户端字段列表中可见。 无论计算列和度量值是否包含在透视中，都仍可以通过排除的对象数据进行计算。  
  
本课程的目的是介绍如何创建透视以及如何逐步熟悉表格模型创作工具。 如果您以后扩展此模型以包括其他表，可以创建其他透视以定义不同视点的模型，例如，库存和销售额。  
  
估计的时间才能完成本课程中：**五分钟**  
  
## <a name="prerequisites"></a>先决条件  

本文是表格建模教程应按顺序完成的一部分。 执行任务之前在本课程中，您应当已完成上一课：[第 7 课：创建关键绩效指标](../tutorial-tabular-1400/as-lesson-7-create-key-performance-indicators.md)。  
  
## <a name="create-perspectives"></a>创建透视  
  
#### <a name="to-create-an-internet-sales-perspective"></a>创建“Internet Sales”透视  
  
1.  单击**模型**菜单 >**透视** > **创建和管理**。  
  
2.  在“透视”  对话框中，单击“新建透视”  。  
  
3.  双击**新的透视**列标题，然后重命名**Internet Sales**。  
  
4.  选择所有表*除* **DimCustomer**。  
  
    ![as-lesson8-perspectives](../tutorial-tabular-1400/media/as-lesson8-perspectives.png)
  
    在后面的课程，您使用分析在 Excel 功能来测试此透视。 Excel 数据透视表字段列表包括除 DimCustomer 表之外的每个表。  

## <a name="whats-next"></a>下一步是什么？

[第 9 课：创建层次结构](../tutorial-tabular-1400/as-lesson-9-create-hierarchies.md)。
  
  
  
  
