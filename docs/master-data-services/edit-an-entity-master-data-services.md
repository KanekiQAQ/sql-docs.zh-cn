---
title: 编辑实体 (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- entities [Master Data Services], changing name
ms.assetid: 6a5b9f14-6dfc-49d7-a771-e96461d4feae
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 88a4850c80404ebeb002c5d17af69b0f10dc5bf6
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68086482"
---
# <a name="edit-an-entity-master-data-services"></a>编辑实体 (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，可以编辑实体。  
  
## <a name="prerequisites"></a>系统必备  
 若要执行此过程：  
  
-   您必须有权访问 **“系统管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 (Master Data Services)](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-edit-an-entity"></a>编辑实体  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“系统管理”** 。  
  
2.  在“管理模型”  页上，从网格中选择一个模型，然后单击“实体”  。  
  
3.  在“管理实体”  页上，从“网格”中选择要更改的实体所在的行，然后单击“编辑”  。  
  
4.  在“名称”  框中，键入属性的新名称。  
  
5.  在“说明”  字段中，键入实体的新说明。  
  
6.  在“临时表的名称”  字段中，键入临时表的新名称。  
  
7.  对于“事务日志类型”  字段，请在下拉列表中选择更新的事务日志类型。  
  
     有关详细信息，请参阅[更改实体事务日志类型 (Master Data Services)](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md)  
  
8.  选中或取消选中“自动创建代码值”  复选框。  
  
     有关详细信息，请参阅[自动创建代码 (Master Data Services)](../master-data-services/automatic-code-creation-master-data-services.md)  
  
9. 选中或取消选中“启用数据压缩”  复选框。 默认情况下，系统将打开行压缩。  
  
     有关详细信息，请参阅 [Data Compression](../relational-databases/data-compression/data-compression.md)  
  
## <a name="status"></a>“登录属性”  
 在网格的状态列中显示实体上所执行操作的状态。 单击“保存实体”  时，将显示下图，指示实体正在更新。  
  
 ![更新状态图标](../master-data-services/media/mds-statusicon-updating.png "Icon for updating status")  
  
 如果在创建或编辑实体时出错，将显示下面的图像。  
  
 ![错误状态图标](../master-data-services/media/mds-statusicon-error.png "Icon for error status")  
  
 如果状态为“正常”，则将显示下面的图像。  
  
 ![正常状态图标](../master-data-services/media/mds-statusicon-ok.png "Icon for OK status")  
  
## <a name="see-also"></a>请参阅  
 [显式层次结构 (Master Data Services)](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [删除实体 (Master Data Services)](../master-data-services/delete-an-entity-master-data-services.md)   
 [实体 (Master Data Services)](../master-data-services/entities-master-data-services.md)  
  
  
