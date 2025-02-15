---
title: 使属性组对用户可见 (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: b2f6cc27-dbc9-4f3f-961e-e81e76375248
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ce8d0490625780459ecafd8e9d346af0fc9bd1f2
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68054527"
---
# <a name="make-an-attribute-group-visible-to-users-master-data-services"></a>使属性组对用户可见 (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，当您想要用户在 **“资源管理器”** 功能区域中将选项卡置于网格上方时，使属性组对于用户或组是可见的。  
  
 创建属性组时，对除创建者之外的所有用户自动隐藏它。  
  
## <a name="prerequisites"></a>系统必备  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 (Master Data Services)](../master-data-services/administrators-master-data-services.md)。  
  
-   必须至少存在一个属性组。 有关详细信息，请参阅 [创建属性组 (Master Data Services)](../master-data-services/create-an-attribute-group-master-data-services.md)。  
  
### <a name="to-make-an-attribute-group-visible-to-users"></a>使属性组对用户可见  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”** 。  
  
2.  在“管理模型”  页上，从网格中选择一个模型，然后单击“实体”  。  
  
3.  在“管理实体”  页上，从网格中选择要为其编辑属性组的实体所在的行。  
  
4.  单击“属性组”  。  
  
5.  在“管理属性组”页上，从“成员类型”下拉列表框中选择成员类型，展开“叶”、“合并”或“集合”，具体视你要显示的组类型而定。       
  
6.  从网格中选择要编辑的属性组，然后单击“编辑”  。  
  
7.  单击“可用”  框中的用户或组，然后单击“添加”  箭头。 若要全部添加，请单击 **“全部添加”** 箭头。  
  
8.  单击“保存”  。  
  
## <a name="see-also"></a>请参阅  
 [属性组 (Master Data Services)](../master-data-services/attribute-groups-master-data-services.md)   
 [创建属性组 (Master Data Services)](../master-data-services/create-an-attribute-group-master-data-services.md)  
  
  
