---
title: MSSQL_ENG014157 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG014157 error
ms.assetid: 1a0890cf-d977-43e0-a2ba-9c5ff1a8f856
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5ac7abab429d56aa835c46479140f0a29013db6a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68100046"
---
# <a name="mssqleng014157"></a>MSSQL_ENG014157
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|||  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|14157|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|由发布 '%s' 的订阅服务器 '%s' 创建的订阅已过期，且已停止。|  
  
## <a name="explanation"></a>解释  
 订阅服务器在指定的发布保持期内必须与发布服务器同步。 如果订阅服务器在此期间内没有同步，则该订阅将过期。 有关详细信息，请参阅 [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
## <a name="user-action"></a>用户操作  
 必须先重新创建和初始化订阅，订阅服务器才可以再次接收数据更改：  
  
-   有关创建订阅的信息，请参阅[订阅发布](../../relational-databases/replication/subscribe-to-publications.md)。  
  
-   有关初始化订阅的信息，请参阅[初始化订阅](../../relational-databases/replication/initialize-a-subscription.md)。  
  
 如果订阅数据库包含尚未与发布服务器同步的更改，则可以使用 [tablediff Utility](../../tools/tablediff-utility.md) 来确定发布和订阅数据库中的哪些行不同。  
  
 可以延长发布保持期来避免订阅过期。 请谨慎设置较高的值，因为这可能导致存储更多的数据和元数据，从而影响性能。 有关详细信息，请参阅 [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
