---
title: MSSQLSERVER_8645 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 8645 (Database Engine error)
ms.assetid: 63d6d6d7-3850-4061-8e96-b1fa665e3180
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a00288d4aa7d98ac74be6703cc0754ea4bedb798
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68132298"
---
# <a name="mssqlserver8645"></a>MSSQLSERVER_8645
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>详细信息  
  
|||  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|8645|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|MEMTIMEDOUT_ERR|  
|消息正文|等待内存资源来执行该查询时发生超时。 请重新运行查询。|  
  
## <a name="explanation"></a>解释  
等待资源池“default”中的内存资源来执行该查询时发生超时。  
  
## <a name="user-action"></a>用户操作  
如果您未在使用资源调控器，我们建议您对整个服务器状态或负荷进行验证，或者检查资源池或工作负荷组设置。  
  
下面的列表概述了有助于解决内存错误的一般步骤：  
  
1.  验证其他应用程序或服务是否占用此服务器上的内存。 重新配置不太重要的应用程序或服务，使其占用更少的内存。  
  
2.  开始收集以下内容的性能监视器计数器：**SQL Server:Buffer Manager**、**SQL Server:Memory Manager**。  
  
3.  检查以下 SQL Server 内存配置参数：  
  
    -   **max server memory**  
  
    -   **min server memory**  
  
    -   **min memory per query**  
  
    注意不正常的设置。 根据需要更正它们。 满足 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 的更高内存要求。 SQL Server 联机丛书的“设置服务器配置选项”中列出了默认设置。  
  
4.  在您看到这些错误消息时，观察 DBCC MEMORYSTATUS 输出及其变化情况。  
  
5.  检查工作负荷（例如，并发会话数，当前执行的查询）。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

以下操作可能会为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供更多内存：  
  
-   如果除 SQL Server 外的应用程序正在占用资源，请尝试停止运行这些应用程序，或者考虑在单独的服务器上运行它们。 这样做将消除外部内存压力。  
  
-   如果已配置 **max server memory**，请增大其设置。  
  
运行以下 DBCC 命令以释放几个 SQL Server 内存缓存。  
  
-   DBCC FREESYSTEMCACHE  
  
-   DBCC FREESESSIONCACHE  
  
-   DBCC FREEPROCCACHE  
  
如果问题仍存在，则您将需要进一步调查，可能需要减小工作负荷。  
  
