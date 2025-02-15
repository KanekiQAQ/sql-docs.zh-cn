---
title: 针对业务规则验证版本 (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- validating versions [Master Data Services]
- validating versions [Master Data Services], about validating versions
- versions [Master Data Services], validating
- business rules [Master Data Services], applying to all members
ms.assetid: 5aee7901-6d05-41d4-8bbb-c6f26791d1df
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 57a78efbe6de9a4439846df69ac04e32d073fe3e
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68102554"
---
# <a name="validate-a-version-against-business-rules-master-data-services"></a>针对业务规则验证版本 (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，对某一版本进行验证以便将业务规则应用于该模型版本中的所有成员。  
  
 此过程说明如何使用 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序来验证数据。 如果您在 MDS 数据库中具有权限，可以改用存储过程。 有关详细信息，请参阅 [验证存储过程 (Master Data Services)](../master-data-services/validation-stored-procedure-master-data-services.md)。  
  
> [!NOTE]  
>  所有成员必须通过验证后，才能提交版本。  
  
## <a name="prerequisites"></a>系统必备  
 若要执行此过程：  
  
-   您必须有权访问 **“版本管理”** 功能区域。  
  
-   您必须是模型管理员。 有关详细信息，请参阅 [管理员 (Master Data Services)](../master-data-services/administrators-master-data-services.md)。  
  
-   版本的状态必须是 **“打开”** 或 **“已锁定”** 。  
  
-   在 **“验证版本”** 页上，存在的成员必须具有并非 **“验证已成功”** 的其他状态。  
  
### <a name="to-validate-a-version"></a>验证版本  
  
1.  在 [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]中，单击 **“版本管理”** 。  
  
2.  在 **“管理版本”** 页上，从菜单栏中，单击 **“验证版本”** 。  
  
3.  在 **“验证版本”** 页上，选择要验证的模型和版本。  
  
4.  单击 **“验证”** 。  
  
5.  在确认对话框中，单击 **“确定”** 。  
  
    > [!NOTE]  
    >  一旦不再显示进度指示器，则表明该版本已完成验证。  
  
## <a name="next-steps"></a>后续步骤  
  
-   [锁定版本 (Master Data Services)](../master-data-services/lock-a-version-master-data-services.md)  
  
## <a name="see-also"></a>请参阅  
 [验证状态 (Master Data Services)](../master-data-services/validation-statuses-master-data-services.md)   
 [验证存储过程 (Master Data Services)](../master-data-services/validation-stored-procedure-master-data-services.md)   
 [版本 (Master Data Services)](../master-data-services/versions-master-data-services.md)   
 [业务规则 (Master Data Services)](../master-data-services/business-rules-master-data-services.md)   
 [针对业务规则验证特定成员 (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
  
