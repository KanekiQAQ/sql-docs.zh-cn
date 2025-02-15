---
title: 编辑和删除实体同步关系 (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 9a5e37f3-352e-45a6-b4a0-6f98f83b4bd8
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 52ffe540d1afe9334533c2a5a8a631f342aed983
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68086464"
---
# <a name="edit-and-delete-an-entity-sync-relationship-master-data-services"></a>编辑和删除实体同步关系 (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  实体同步是实体版本间的单向可重复同步。 它提供了一种在不同模型间共享实体数据的方法。 可以编辑和删除自己创建的同步关系。  
  
## <a name="prerequisites"></a>先决条件  
 编辑实体同步关系的先决条件。  
  
-   你必须有权访问“系统管理”功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
-   你必须是目标模型的模型管理员。 有关详细信息，请参阅 [管理员 (Master Data Services)](../master-data-services/administrators-master-data-services.md)。  
  
-   至少需要拥有对源实体及其所有属性和成员的读取访问权限。  
  
 删除实体同步关系的先决条件。  
  
-   你必须有权访问“系统管理”功能区域。 有关详细信息，请参阅[功能区域权限 (Master Data Services)](../master-data-services/functional-area-permissions-master-data-services.md)。  
  
-   你必须是目标模型的模型管理员。 有关详细信息，请参阅 [管理员 (Master Data Services)](../master-data-services/administrators-master-data-services.md)。  
  
 在编辑实体同步关系时，请注意以下事项。  
  
-   源实体和目标实体必须处于不同的模型中。  
  
-   不得确认目标实体版本状态。  
  
-   建立同步关系后，目标会立即与源进行同步。  
  
-   目标实体版本不能是另一同步关系中的源实体版本。  
  
 在执行实体同步关系时，请注意以下事项  
  
-   仅复制叶成员。  
  
-   不会复制基于域的属性。  
  
-   不会复制软删除的成员。  
  
-   同步不会生成目标实体事务/历史记录。  
  
 **编辑实体同步关系**  
  
1.  在主数据管理器中，单击“系统管理”  。  
  
2.  在“模型视图”  页上，从菜单栏中指向“管理”  ，然后单击“实体同步”  。  
  
3.  在“实体同步维护”  页上，选择网格中的同步关系。  
  
4.  单击 **“编辑”** 。 右侧将显示一个面板。  
  
5.  请更改  “频率”。 选择“按需同步”  ，或选择“自动同步”  并设置频率。  
  
6.  单击“保存”  。  
  
 **删除实体同步关系**  
  
1.  在主数据管理器中，单击“系统管理”  。  
  
2.  在“模型视图”  页上，从菜单栏中指向“管理”  ，然后单击“实体同步”  。  
  
3.  在“实体同步维护”  页上，选择网格中的同步关系。  
  
4.  单击 **“删除”** 。  
  
5.  在确认对话框中，单击“确定”  。  
  
## <a name="see-also"></a>请参阅  
 [创建和执行实体同步关系 (Master Data Services)](../master-data-services/create-and-execute-an-entity-sync-relationship-master-data-services.md)   
 [实体同步关系 (Master Data Services)](../master-data-services/entity-sync-relationship-master-data-services.md)  
  
  
