---
title: MSSQL_ENG021075 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG021075 error
ms.assetid: c8c29543-d1f6-49d5-b6c8-e8c3aa373090
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5a366cceef67bc92465bc9a8645b4c862ac7d1a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68091967"
---
# <a name="mssqleng021075"></a>MSSQL_ENG021075
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|||  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|21075|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|发布 '%s' 的初始快照尚不可用。|  
  
## <a name="explanation"></a>解释  
 如果在快照代理完成生成快照的过程之前启动分发代理或合并代理，便会引发错误 MSSQL_ENG021075。  
  
## <a name="user-action"></a>用户操作  
 如果自创建订阅后或者上次选择重新初始化订阅后一直未启动发布的快照代理，则请启动该快照代理并使其在启动分发代理或合并代理前完成。 有关详细信息，请参阅[创建并应用快照](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
 如果快照代理没有完成，请检查快照代理历史记录以查找错误并将其消除。 有关在复制监视器中查看代理状态和错误详细资料的信息，请参阅[使用复制监视器查看信息和执行任务](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)。  
  
 如果错误继续出现，请增加代理的日志记录并指定日志的输出文件。 此操作可能会提供找到该错误和/或其他错误消息的步骤，具体取决于错误的上下文。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
