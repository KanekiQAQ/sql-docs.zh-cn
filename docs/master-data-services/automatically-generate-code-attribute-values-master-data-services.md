---
title: 自动生成代码属性值 (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 19b354ee-2906-4cc7-ba2f-32b4543bddcf
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: d837d188dc28e0c06d3e66162d1ab12755ac809d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68047502"
---
# <a name="automatically-generate-code-attribute-values-master-data-services"></a>自动生成 Code 属性值 (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 中，当希望在每次创建新成员时自动为 Code 值分配一个整数时，自动为实体的 Code 属性设置值。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 (Master Data Services)](../master-data-services/administrators-master-data-services.md)。  
  
-   实体必须存在。 有关详细信息，请参阅[创建实体 (Master Data Services)](../master-data-services/create-an-entity-master-data-services.md)。  
  
### <a name="to-automatically-generate-code-values"></a>自动生成 Code 值  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”** 。  
  
2.  在“管理模型”  页上，选择包含要编辑的实体的模型行，然后单击“实体”  。  
  
3.  在“管理实体”  页上，选择要为其生成代码的实体行，然后单击“编辑”  。  
  
4.  选中 **“自动创建代码值”** 复选框。  
  
5.  在 **“起始”** 框中，键入开始递增的数字。 如果成员已存在，则将基于最大的现有值设置 Code。 例如，如果最大的现有 Code 值为 299，则下一个成员的 Code 值将设置为 300。  
  
6.  单击“保存”  。  
  
## <a name="see-also"></a>请参阅  
 [自动创建代码 (Master Data Services)](../master-data-services/automatic-code-creation-master-data-services.md)   
 [自动生成 Code 之外的属性值 (Master Data Services)](../master-data-services/automatically-generate-attribute-values-other-than-code-master-data-services.md)  
  
  
