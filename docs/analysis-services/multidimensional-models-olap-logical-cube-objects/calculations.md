---
title: 计算 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7fefceede2cc3b76a36615a050037eb9e1fba9dc
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68181046"
---
# <a name="calculations"></a>“新建命名集”
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  计算是多维表达式 (MDX) 表达式或脚本，用于定义中的多维数据集中的计算的成员、 命名的集或指定了作用域的分配[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]。 使用计算，您可以添加不是由多维数据集的数据而是由特定表达式（这些表达式可以引用多维数据集的其他部分、其他多维数据集，或者甚至引用 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库以外的信息）所定义的对象。 使用计算，您还可以扩展多维数据集的功能，提高商业智能应用程序的灵活性和能力。 有关编写计算脚本的详细信息，请参阅[Microsoft SQL Server 2005 中的 MDX 脚本简介](http://go.microsoft.com/fwlink/?LinkId=81892)。 
  
## <a name="calculated-members"></a>计算的成员  
 计算成员是在运行时使用对其进行定义时所指定的多维表达式 (MDX) 表达式来计算其值的成员。 计算成员就像其他任何成员一样，可用于商业智能应用程序。 计算成员不会增加多维数据集的大小，因为多维数据集中只存储定义；值的计算则在需要回答查询时才在内存中执行。  
  
 可以为任何维度（包括度量值维度）定义计算成员。 在度量值维度中创建的计算成员称为计算度量值。  
  
 尽管计算成员通常基于多维数据集中已存在的数据，但是可以通过将数据与算术运算符、数字和函数进行组合而创建复杂的表达式。 还可以使用 MDX 函数（例如，LookupCube）来访问 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库内其他多维数据集中的数据。 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 包括标准化 Visual Studio 函数库，可以使用存储过程从除当前 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库以外的源中检索数据。 有关存储过程的详细信息，请参阅[定义存储过程](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/defining-stored-procedures.md)。  
  
 例如，假设货运公司的行政人员希望基于每体积单位的利润确定运输哪种类型的货物利润更高。 他们使用包含“货物”、“船队”和“时间”维度以及 Price_to_Ship、Cost_to_Ship 和 Volume_in_Cubic_Meters 度量值的“发运”多维数据集；但是，该多维数据集中未包含盈利率的度量值。 通过在下列表达式中合并现有度量值，可以在多维数据集中创建一个计算成员来作为度量值，并将它命名为 Profit_per_Cubic_Meter：  
  
```  
([Measures].[Price_to_Ship] - [Measures].[Cost_to_Ship]) /  
[Measures].[Volume_in_Cubic_Meters]  
```  
  
 创建计算成员之后，下一次浏览“发运”多维数据集时，Profit_per_Cubic_Meter 将与其他度量值一起出现。  
  
 若要创建计算的成员，请使用**计算**s 选项卡，多维数据集设计器中的。 有关详细信息，请参阅[创建计算成员](../../analysis-services/multidimensional-models/create-calculated-members.md)  
  
## <a name="named-sets"></a>命名集  
 命名集是返回集的 CREATE SET MDX 语句表达式。 MDX 表达式保存为在多维数据集定义的一部分[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]。 创建命名集的目的是为了重用多维表达式 (MDX) 查询。 使用命名集，业务用户可以简化查询，并针对复杂、常用的集表达式使用集名称而不是集表达式。 **相关的主题：** [创建命名集](../../analysis-services/multidimensional-models/create-named-sets.md)  
  
## <a name="script-commands"></a>脚本命令  
 脚本命令是一个 MDX 脚本，是多维数据集定义的一部分。 使用脚本命令，您可以执行几乎所有的受多维数据集的 MDX 支持的操作，例如，确定将计算仅应用于多维数据集的一部分的作用域。 在中[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]，MDX 脚本可以应用到整个多维数据集或多维数据集的脚本执行中的特定点上的特定部分。 默认脚本命令（CALCULATE 语句）会基于默认作用域使用聚合数据来填充多维数据集中的单元。  
  
 默认作用域为整个多维数据集，但是您可以定义更具限制的作用域（称为子多维数据集），然后将 MDX 脚本仅应用于该特定的多维数据集空间。 SCOPE 语句定义计算脚本中所有后续 MDX 表达式和语句的作用域，直到作用域终止或重新定义为止。 然后，使用 THIS 语句将 MDX 表达式应用于当前作用域。 可以使用 BACK_COLOR 语句为当前作用域中的单元指定背景单元颜色，以在调试期间提供帮助。  
  
 例如，您可以使用脚本命令并基于以前时间段销售额的加权值，跨时间和销售区域为雇员分配销售配额。  
  
## <a name="see-also"></a>请参阅  
 [多维模型中的计算](../../analysis-services/multidimensional-models/calculations-in-multidimensional-models.md)  
  
  
