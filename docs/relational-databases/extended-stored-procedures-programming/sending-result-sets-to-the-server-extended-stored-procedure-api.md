---
title: 发送到服务器 （扩展存储的过程 API） 的结果集 |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- extended stored procedures [SQL Server], sending result sets
- result sets [SQL Server], extended stored procedures
ms.assetid: 9d54673d-ea9d-4ac6-825a-f216ad8b0e34
author: rothja
ms.author: jroth
ms.openlocfilehash: 7121626f3de850c670f160a945ba3de8533cd7ee
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68064293"
---
# <a name="sending-result-sets-to-the-server-extended-stored-procedure-api"></a>将结果集发送到服务器（扩展存储过程 API）
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 请改用 CLR 集成。  
  
 发送结果集时[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，扩展存储的过程应调用合适的 API，如下所示：  
  
-   **Srv_sendmsg**之前或之后发送所有行 （如果有），可能会按任意顺序调用函数**srv_sendrow**。 与发送完成状态之前的所有消息必须都发送到客户端**srv_senddone**。  
  
-   对于发送到客户端的每行调用一次 srv_sendrow 函数  。 必须将所有行都发送到客户端之前任何消息、 状态值或完成状态会自动都发送带有**srv_sendmsg**，则**srv_status**自变量**srv_pfield**，或**srv_senddone**。  
  
-   发送的某行未与定义其所有列**srv_describe**导致应用程序引发信息性错误消息并向客户端返回 FAIL。 在此情况下，将不发送该行。  
  
## <a name="see-also"></a>请参阅  
 [创建扩展存储过程](../../relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures.md)  
  
  
