---
title: 设置事务发布的分发保持期 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transaction retention periods [SQL Server replication]
- retention periods [SQL Server replication]
ms.assetid: 9a98c53a-fea5-4235-b23d-6c69587c5676
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8e39be7010f24f33ae7f31217c7eba8c5b467654
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68051708"
---
# <a name="set-distribution-retention-period-for-transactional-publications"></a>设置事务发布的分发保持期
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  在“分发数据库属性 - \<分发数据库>”  对话框中指定最小分发保持期和最大分发保持期。 可以从“分发服务器属性 - \<分发服务器>”  对话框的“常规”  页中访问该对话框。 有关访问此对话框的详细信息，请参阅[查看和修改分发服务器和发布服务器属性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)。  
  
### <a name="to-specify-the-distribution-retention-period"></a>指定分发保持期  
  
1.  在“分发服务器属性 - \<分发服务器>”对话框的“常规”页上，单击分发数据库的属性按钮 (…)    。  
  
2.  若要指定最小分发保持期，请在 **“至少”** 框中输入一个值；若要指定最大分发保持期，请在 **“但不超过”** 框中输入一个值。  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

## <a name="see-also"></a>另请参阅  
 [“配置分发”](../../relational-databases/replication/configure-distribution.md)   
 [订阅过期和停用](../../relational-databases/replication/subscription-expiration-and-deactivation.md)  
  
  
