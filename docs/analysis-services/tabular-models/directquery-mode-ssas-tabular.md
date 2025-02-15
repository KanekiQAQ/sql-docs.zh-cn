---
title: 在 Analysis Services 中的 DirectQuery 模式下 |Microsoft Docs
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a51b38dacf5a1ebaf67a19bf8b3761800425a347
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68207679"
---
# <a name="directquery-mode"></a>DirectQuery 模式
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  本文介绍*DirectQuery 模式下*的 Analysis Services 表格模型 1200年及更高版本的兼容性级别。 你可以为正在 SSDT 中设计的模型打开 DirectQuery 模式；对于已部署的表格模型，你可以在 SSMS 中更改为 DirectQuery 模式。 在选择 DirectQuery 模式之前，务必先了解其优势和限制。
  
##  <a name="bkmk_Benefits"></a> 优势
 默认情况下，表格模型使用内存中缓存来存储和查询数据。 当表格模型查询内存中驻留的数据时，即使是复杂查询也可以非常快地执行。 但是，使用缓存数据存在许多限制。 也就是说，如果无法按计划定期进行处理，大型数据集可能会超出可用内存，并且可能难以达到数据刷新要求。  
  
 DirectQuery 克服了这些限制，同时还利用 RDBMS 功能提高了查询执行效率。 使用 DirectQuery：  
  
-   数据是最新的，并且没有不得不维护数据的单独副本（在内存中缓存中）的额外管理开销。 对于基础数据源的更改可立即反映在针对数据模型的查询中。  
  
-   数据集可以大于 Analysis Services 服务器的内存容量。  
  
-   DirectQuery 可以利用提供程序端查询加速效果，例如提供的内存优化列索引。  
  
-   后端数据库可以使用数据库中的行级别安全性功能（也可以通过 DAX 使用模型中的行级别安全性）强制实施安全性。  
  
-   如果模型包含可能要求多个查询的复杂公式，则 Analysis Services 可以执行优化以便确保对后端数据库执行的查询的查询计划将尽可能高效。  
  
##  <a name="bkmk_prereq"></a>限制
DirectQuery 模式下的表格模型存在许多限制。 在切换模式前，务必确定在后端服务器上执行查询所带来的优势是否大于功能的减少。  
  
 如果你在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中更改现有模型的模式，模型设计器将通知你模型中与 DirectQuery 模式不兼容的任何功能。  
  
 下表总结了需要牢记的主要功能限制：  
  
|||  
|-|-|  
|**功能区**|**限制**|  
|**数据源**|DirectQuery 模型只能使用以下类型的单个关系数据库中的数据：SQL Server、 Azure SQL 数据库、 Oracle 和 Teradata。  有关版本和提供程序信息，请参阅本文后面的“DirectQuery 支持的数据源”。| 
|**SQL 存储过程**|对于 DirectQuery 模型，在使用数据导入向导时，不能在 SQL 语句中指定存储过程来定义表。 |   
|**计算表**|DirectQuery 模型不支持计算表，但支持计算列。 如果你尝试转换一个包含计算表的表格模型，则将发生错误，指出该模型不能包含粘贴数据。|  
|**查询限制**|默认行数限制为 100 万行，可以通过指定 msmdsrv.ini 文件中的 **MaxIntermediateRowSize** 增加此值。 有关详细信息，请参阅 [DAX 属性](../../analysis-services/server-properties/dax-properties.md) 。
|**DAX 公式**|在 DirectQuery 模式下查询表格模型时， [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 会将 DAX 公式和度量值定义转换为 SQL 语句。 所含元素不能转换为 SQL 语法的 DAX 公式将在模型上返回验证错误。<br /><br /> 此限制主要限于特定的 DAX 函数。 对于度量值，DAX 公式将针对关系数据存储转换为基于集的操作。 这意味着将支持所有隐式创建的度量值。 <br /><br /> 发生验证错误时，必须重写公式，替换为其他函数，或通过使用数据源中的派生列来解决该问题。  如果表格模型所含的公式包含不兼容的函数，则在设计器中切换到 DirectQuery 模式时将报告这一问题。 <br /><br />**注意：** 在你将模型切换到 DirectQuery 模式时，模型中的某些公式可能会进行验证，但在对缓存和关系数据存储执行验证时将返回不同的结果。 这是因为对缓存的计算使用内存中分析引擎（包含用于模拟 Excel 行为的功能）的语义，而针对关系数据源中存储的数据的查询使用 SQL Server 的语义。<br /><br />若要了解详细信息，请参阅[在 DirectQuery 模式下的 DAX 公式兼容性](../../analysis-services/tabular-models/dax-formula-compatibility-in-directquery-mode-ssas-2016.md)。|  
|**公式一致性**|在某些情况下，与仅使用关系数据存储的 DirectQuery 模型相比，相同的公式可能在缓存模型中返回不同的结果。 这些差异是由内存中分析引擎和 SQL Server 之间的语义差异造成的。<br /><br /> 有关兼容性问题，包括实时可能返回不同的结果时部署模型时，函数的完整列表，请参阅[在 DirectQuery 模式 (SQL Server Analysis Services) 中的 DAX 公式兼容性](http://msdn.microsoft.com/981b6a68-434d-4db6-964e-d92f8eb3ee3e)。|  
|**MDX 限制**|没有相对对象名称。 所有对象名称都必须是完全限定的。<br /><br /> 没有会话范围 MDX 语句（命名集、计算成员、计算单元格、直观合计、默认成员等），但你可以使用查询范围构造，例如“WITH”子句。<br /><br /> MDX 再选择子句中没有包含不同级别成员的元组。<br /><br /> 不要使用用户定义的层次结构。<br /><br /> 没有本机 SQL 查询（通常，Analysis Services 支持 T-SQL 子集，但不适用于 DirectQuery 模型）。|  

## <a name="data-sources-supported-for-directquery"></a>DirectQuery 支持的数据源
在兼容级别 1200年及更高版本的 DirectQuery 表格模型是与以下数据源和提供程序兼容：

数据源   |版本  |访问接口
---------|---------|---------
Microsoft SQL Server    |  2008 及更高版本      |       SQL Server 的 OLE DB 提供程序、SQL Server Native Client OLE DB 提供程序、SQL 客户端的 .NET Framework 数据提供程序  
Microsoft Azure SQL 数据库    |   All      |  SQL Server 的 OLE DB 提供程序、SQL Server Native Client OLE DB 提供程序、SQL 客户端的 .NET Framework 数据提供程序            
Microsoft Azure SQL 数据仓库     |   All     |  用于 SQL 客户端的 .NET Framework 数据访问接口       
Microsoft SQL 分析平台系统 (APS)     |   All      |  SQL Server 的 OLE DB 提供程序、SQL Server Native Client OLE DB 提供程序、SQL 客户端的 .NET Framework 数据提供程序       
Oracle 关系数据库     |  Oracle 9i 及更高版本       |  Oracle OLE DB 访问接口       
Teradata 关系数据库    |  Teradata V2R6 及更高版本     | Teradata 的 .NET 数据访问接口        

## <a name="connecting-to-a-data-source"></a>连接到数据源
在 SSDT 中设计 DirectQuery 模型时，连接到数据源并选择要包含在模型中的表和字段的过程与内存中模型差不多。 

如果已经打开 DirectQuery 但尚未连接到数据源，则可以使用表导入向导连接到数据源、选择表和字段、指定 SQL 查询等等。 其不同之处在于，在完成操作后，不会向内存中缓存实际导入任何数据。 

![DirectQuery 导入成功](../../analysis-services/tabular-models/media/directquery-import-success.png)

如果已使用表导入向导导入数据但尚未打开 DirectQuery 模式，那么在打开该模式时，将清除内存中缓存。

  
## <a name="additional-articles-in-this-section"></a>在本部分中的其他文章
[在 SSDT 中启用 DirectQuery 模式](../../analysis-services/tabular-models/enable-directquery-mode-in-ssdt.md)

[在 SSMS 中启用 DirectQuery 模式](../../analysis-services/tabular-models/enable-directquery-mode-in-ssms.md)

[在设计模式下将示例数据添加到 DirectQuery 模型中](../../analysis-services/tabular-models/add-sample-data-to-a-directquery-model-in-design-mode.md)

[在 DirectQuery 模型中定义分区](../../analysis-services/tabular-models/define-partitions-in-directquery-models-ssas-tabular.md)
  
[在 DirectQuery 模式下测试模型](../../analysis-services/tabular-models/test-a-model-in-directquery-mode.md)

[在 DirectQuery 模式下的 DAX 公式兼容性](../../analysis-services/tabular-models/dax-formula-compatibility-in-directquery-mode-ssas-2016.md)
  
