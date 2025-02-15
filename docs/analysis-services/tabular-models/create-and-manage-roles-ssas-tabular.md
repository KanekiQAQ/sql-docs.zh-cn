---
title: 创建和管理 Analysis Services 表格模型中的角色 |Microsoft Docs
ms.date: 09/17/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 3723d83fb28f6d2824be921906326d719e705478
ms.sourcegitcommit: a6949111461eda0cc9a71689f86b517de3c5d4c1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2019
ms.locfileid: "67263409"
---
# <a name="create-and-manage-roles"></a>创建和管理角色 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  在表格模型中，角色定义模型的成员权限。 可以使用 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中的“角色管理器”对话框为模型项目定义角色。 

> [!IMPORTANT]
> 如果你正在部署到 Azure Analysis Services 项目，使用**集成工作区**作为工作区数据库。 若要了解详细信息，请参阅[工作区数据库](workspace-database-ssas-tabular.md)。
  
 在本文中的任务说明如何创建和使用中的角色管理器对话框在模型创作期间管理角色[!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]。 在已部署的模型数据库中管理角色的信息，请参阅[表格模型角色](../../analysis-services/tabular-models/tabular-model-roles-ssas-tabular.md)。  
  
## <a name="tasks"></a>“任务”  
 若要创建、编辑、复制和删除角色，可使用 **“角色管理器”** 对话框。 若要查看 **“角色管理器”** 对话框，请在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，单击 **“模型”** 菜单，然后单击 **“角色管理器”** 。  
  
###  <a name="bkmk_new_role"></a> 创建新角色  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，单击 **“模型”** 菜单，然后单击 **“角色管理器”** 。  
  
2.  在 **“角色管理器”** 对话框中，单击 **“新建”** 。  
  
     一个新的突出显示的角色会添加到“角色”列表中。  
  
3.  在 **“角色”** 列表的 **“名称”** 字段中，键入角色的名称。  
  
     默认情况下，对于每个新建角色，默认角色的名称将为递增式编号。 建议您键入明确标识成员类型的名称，例如财务经理或人力资源专员。  
  
4.  在 **“权限”** 字段中，单击向下箭头，然后选择以下权限类型之一：  
  
    |权限|Description|  
    |----------------|-----------------|  
    |**无**|成员无法对模型架构进行任何修改，也无法查询数据。|  
    |**读取**|允许成员查询数据（基于行筛选器），但不能对模型架构进行任何更改。|  
    |**读取和处理**|允许成员查询数据（基于行级别筛选器）并运行“处理”和“全部处理”操作，但无法对模型架构进行任何更改。|  
    |**处理**|成员可以运行“处理”和“全部处理”操作。 不能修改模型架构且不能查询数据。|  
    |**管理员**|成员可以对模型架构进行修改并可以查询所有数据。|  
  
5.  若要输入角色的说明，请单击 **“说明”** 字段，然后键入说明。  
  
6.  如果您创建的角色已具有“读取”或者“读取和处理”权限，则您可以使用 DAX 公式添加行筛选器。 若要添加行筛选器，请单击 **“行筛选器”** 选项卡，选择某个表，然后单击 **“DAX 筛选器”** 字段，再键入 DAX 公式。  
  
7.  若要向角色添加成员，请单击 **“成员”** 选项卡，然后单击 **“添加”** 。  
  
    > [!NOTE]  
    >  也可以通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]将角色成员添加到已部署的模型中。 有关详细信息，请参阅[通过使用 SSMS 管理角色](../../analysis-services/tabular-models/manage-roles-by-using-ssms-ssas-tabular.md)。  
  
8.  在 **“选择用户或组”** 对话框中，将 Windows 用户或 Windows 组对象作为成员输入。  
  
9. 单击 **“确定”** 。  
  
## <a name="see-also"></a>另请参阅  
 [角色](../../analysis-services/tabular-models/roles-ssas-tabular.md)   
 [透视](../../analysis-services/tabular-models/perspectives-ssas-tabular.md)   
 [“在 Excel 中分析”](../../analysis-services/tabular-models/analyze-in-excel-ssas-tabular.md)   
 [USERNAME 函数 (DAX)](/dax/username-function-dax)   
 [CUSTOMDATA 函数 (DAX)](/dax/customdata-function-dax)  
  
  
