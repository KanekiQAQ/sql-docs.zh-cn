---
title: MSSQL_ENG018456 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG018456 error
ms.assetid: 3daa8144-d81f-445a-b6c3-4bb3e9fd1526
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7a73779de5f03f24454ea7bb45f0ca24bc266f14
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68092154"
---
# <a name="mssqleng018456"></a>MSSQL_ENG018456
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|||  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|18456|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|用户 '%.*ls'.%.\*ls 登录失败|  
  
## <a name="explanation"></a>解释  
 每当尝试登录失败时都会引发 MSSQL_ENG018456 错误。 如果错误消息包括帐户 **distributor_admin** （用户“distributor_admin”登录失败。)，则问题出在复制所用的帐户上。 复制过程将创建远程服务器 **repl_distributor**，该服务器允许在分发服务器和发布服务器之间进行通信。 登录名 **distributor_admin** 与此远程服务器关联，并且必须具有有效的密码。  
  
## <a name="user-action"></a>用户操作  
 确保为此帐户指定了密码。 有关详细信息，请参阅[保护分发服务器的安全](../../relational-databases/replication/security/secure-the-distributor.md)。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
