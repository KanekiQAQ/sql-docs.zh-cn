---
title: 查看在临时过程中出现的错误 (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- staging process [Master Data Services], viewing errors
ms.assetid: 6d2bff84-624b-47fc-a4a5-d9ea01d13412
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 54f38d156a93576d508aa24d7864dbb2d6de7392
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68017314"
---
# <a name="view-errors-that-occur-during-staging-master-data-services"></a>查看在临时过程中出现的错误 (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，您可以查看在临时过程中出现的错误。 在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中，有两个视图显示错误：  
  
-   stg.viw_name_MemberErrorDetails 用于叶成员或合并成员更新。  
  
-   stg.viw_name_RelationshipErrorDetails 用于层次结构关系更新。  
  
## <a name="prerequisites"></a>先决条件  
 若要执行此过程：  
  
-   在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库中，必须具有对 stg.viw_name_MemberErrorDetails 或 stg.viw_name_RelationshipErrorDetails 视图的 SELECT 权限。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 (Master Data Services)](../master-data-services/administrators-master-data-services.md)。  
  
### <a name="to-view-staging-errors"></a>查看临时错误  
  
1.  打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 并连接到 [!INCLUDE[ssDE](../includes/ssde-md.md)] 数据库的 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 实例。  
  
2.  打开新查询。  
  
3.  键入以下文本，用您的临时表名称（例如 viw_Product_MemberErrorDetails）替换该名称。  
  
     `SELECT * FROM stg.viw_name_MemberErrorDetails`  
  
4.  执行该查询。 错误详细信息显示在 **ErrorDescription** 字段中。  
  
## <a name="next-steps"></a>后续步骤  
 有关错误消息的详细信息，请参阅[临时过程错误 (Master Data Services)](../master-data-services/staging-process-errors-master-data-services.md)。  
  
## <a name="see-also"></a>请参阅  
 [概述：导入表中数据 (Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md)   
 [临时过程故障排除 (Master Data Services)](https://social.technet.microsoft.com/wiki/contents/articles/troubleshooting-the-staging-process-master-data-services.aspx)  
  
  
