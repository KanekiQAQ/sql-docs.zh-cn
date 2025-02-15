---
title: Analysis Services 表格模型中的列属性 |Microsoft Docs
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 95a8f86b2cf2bc3cf28a128349540183c88d7fd0
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68163107"
---
# <a name="column-properties"></a>列属性 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  本文介绍表格模型列属性。  
  
> [!NOTE]
>  所有兼容级别不支持某些属性。    
  
##  <a name="bkmk_properties"></a> 列属性  
**高级**  
  
|属性|默认设置|描述|  
|--------------|---------------------|-----------------|  
|**显示文件夹**||单个或嵌套的组织的客户端应用程序字段列表中的列的文件夹。|  

**基本**  
  
|属性|默认设置|描述|  
|--------------|---------------------|-----------------|  
|**列名**||存储在模型中的列的名称和显示在报告客户端字段列表中的列的名称。|  
|**数据格式**|在导入过程中自动确定。|指定要用于此列中的数据的显示格式。 此属性具有以下选项：<br /><br /> **常规**<br /><br /> **小数**<br /><br /> **整数**<br /><br /> **货币**<br /><br /> **百分比**<br /><br /> **科学记数**<br /><br /> 设置数据格式后，可以设置特定于每种格式的属性。 例如，如果选择 **“货币”** 格式，则可以设置可见小数位数，然后依次选择千位分隔符和货币符号。<br /><br /> <br /><br /> 如果列值包含图像，请查看 **“代表图像”** 。|  
|**数据类型**|在导入过程中自动确定。|指定列中所有值的数据类型。|  
|**说明**||列的文本说明。<br /><br /> 在某些报表客户端中，如果最终用户将游标置于字段列表的此列上方，则说明将以工具提示的形式出现。|  
|**Hidden**|False|指定是否在报告客户端字段列表中隐藏列。<br /><br /> 将此属性设置为 **“True”** 可在显示中隐藏该列。 例如，包含标识符或键的列通常对最终用户没有任何用处。<br /><br /> 如果在报告客户端上隐藏列，则该字段会显示在模型数据中。 如果您创建针对模型的查询，则该字段仍将可见。 隐藏的列仍可用于分组或排序。<br /><br /> **“隐藏”** 属性不提供任何形式的数据安全性。 若要确保数据安全，请在角色中使用行筛选器。 有关详细信息，请参阅[角色](../../analysis-services/tabular-models/roles-ssas-tabular.md)。|  
|**按列排序**||指定另一个列以对此列中的值进行排序。 两个列之间必须存在关系。<br /><br /> 该值必须为现有列的名称。 不能指定公式或度量值。|  
|**唯一**||可以设置来强制实现唯一性的列中的值。 始终为 true 的计算列，即使唯一性为 false。|  

 **Misc.**  
  
|属性|默认设置|描述|  
|--------------|---------------------|-----------------|  
|**编码提示**|默认|指定要优化处理编码。 值编码可提高查询性能通常在聚合中使用的数值列的。 哈希编码是 group by 列 （通常为维度表值） 和外键。 字符串列始终采用哈希编码。|  

 **报表属性**  
  
|属性|默认设置|描述|  
|--------------|---------------------|-----------------|  
|默认图像|False|指定哪一行提供用于表示行数据的图像（例如，员工记录中的照片 ID）。|  
|默认标签|False|指定哪一列提供显示名称来表示行数据（例如，员工记录中的员工姓名）。|  
|图像 URL/数据类别 (SP1)|False|指定此列中的值作为指向服务器上图像的超链接。 例如： `http://localhost/images/image1.jpg`。|  
|保留唯一行|False|指定哪些列提供应视为唯一的值，即使这些值重复也不例外（例如，员工的姓氏和名字，而两个或更多员工同名）。|  
|行标识符|False|指定只包含唯一值的一列，同时允许将该列用作内部分组键。|  
|汇总方式|默认|指定在将此列添加到字段列表时，报表客户端工具将对列计算应用聚合函数 SUM。 若要更改默认计算，请从下拉列表中选择它。 此属性仅应用于属于可聚合类型的列。|  
|表详细信息位置|没有默认字段集|指定将此列或度量值添加到单个表中的一组字段中，以便在报表客户端中增强表的可视化体验。|  
  
##  <a name="bkmk_config_prop"></a> 配置列属性设置  
  
1.  在模型设计器的表中，选择一个列。  
  
2.  在 **“属性”** 窗口中，单击某个属性，然后键入值或单击向下箭头选择设置选项。  
  
## <a name="see-also"></a>请参阅  
 [Power View 报表属性](../../analysis-services/tabular-models/power-view-reporting-properties-ssas-tabular.md)   
 [隐藏或冻结列](../../analysis-services/tabular-models/hide-or-freeze-columns-ssas-tabular.md)   
 [向表中添加列](../../analysis-services/tabular-models/add-columns-to-a-table-ssas-tabular.md)  
  
  
