---
title: 从 Analysis Services 中的 Power Pivot 导入 |Microsoft Docs
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 21abcfbec94808df6560af887ff0598d97fcb068
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68207674"
---
# <a name="import-from-power-pivot"></a>从 Power Pivot 导入 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  本文介绍如何通过导入的元数据和从数据创建新的表格模型项目[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]通过使用从导入工作簿[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]中的项目模板[!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]。  
  
## <a name="create-a-new-tabular-model-from-a-power-pivot-for-excel-file"></a>从 Power Pivot for Excel 文件创建新的表格模型  
 在通过从 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿中导入来创建新的表格模型项目时，将使用定义工作簿结构的元数据来在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中创建和定义表格模型项目的结构。 表、列、度量值和关系之类的对象将与它们处于 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿中一样保留并出现在表格模型项目中。 不对 .xlsx 工作簿文件进行任何更改。  
  
> [!NOTE]  
>  表格模型不支持链接表。 在从包含链接表的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿中导入时，链接表数据将作为复制\粘贴的数据处理并且存储于 Model.bim 文件中。 在查看复制\粘贴的表的属性时，“源数据”  属性将被禁用，并且“表”  菜单上的“表属性”  对话框将被禁用。  
>   
>  对于可添加到在模型中嵌入的数据中的行数，有最多 10,000 行的限制。 如果从模型中导入[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]和看到错误，"数据已被截断。 粘贴的表不能包含超过 10000 行"您应该修改[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]通过将嵌入的数据移到另一个数据源，例如 SQL Server 中的表的模型，然后重新导入。  
  
 有一些特别注意事项，这取决于工作区数据库是在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 所处的计算机（本地）上的 Analysis Services 实例中还是在远程 Analysis Services 实例中。  
  
 如果工作区数据库在本地 Analysis Services 实例中，则可以从 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿导入元数据和数据。 从工作簿复制元数据并使用该元数据创建表格模型项目。 然后从工作簿复制和 （复制/粘贴的数据除外，它存储在 Model.bim 文件） 的项目的工作区数据库中存储数据。  
  
 如果工作区数据库在远程 Analysis Services 实例中，则无法从 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for Excel 工作簿导入数据。 您仍可以导入工作薄元数据；不过，这将导致脚本在远程 Analysis Services 实例中运行。 你应只从受信任的 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作薄导入元数据。 必须从数据源连接所定义的源中导入数据。 必须将 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿中的复制/粘贴的数据和链接表数据复制并粘贴到表格模型项目中。  
  
#### <a name="to-create-a-new-tabular-model-project-from-a-power-pivot-for-excel-file"></a>从 Power Pivot for Excel 文件创建新的表格模型项目  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，在 **“文件”** 菜单上，单击 **“新建”** ，然后单击 **“项目”** 。  
  
2.  在“新建项目”对话框中，在“已安装的模板”下，单击“商业智能”    ，然后单击“从 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 导入”  。  
  
3.  在   “名称”中，键入项目的名称，然后指定位置和解决方案名称，再单击  “确定”。  
  
4.  在 **“打开”** 对话框中，选择包含您要导入的模型元数据和数据的 [!INCLUDE[ssGeminiClient](../../includes/ssgeminiclient-md.md)] 文件，然后单击 **“打开”** 。  
  
## <a name="see-also"></a>请参阅  
 [工作区数据库](../../analysis-services/tabular-models/workspace-database-ssas-tabular.md)   
 [复制并粘贴数据](../../analysis-services/tabular-models/ssas-import-data-copy-and-paste-data.md)  
  
  
