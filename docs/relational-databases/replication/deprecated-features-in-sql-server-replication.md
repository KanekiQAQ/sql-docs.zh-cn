---
title: SQL Server 复制中不推荐使用的功能 | Microsoft Docs
ms.custom: ''
ms.date: 01/22/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- deprecated features [SQL Server replication]
ms.assetid: 46bd3edd-d6de-40a6-a015-21cce8321feb
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0cce122c394453d842f7edcedc75cc5bea4dde9a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68128478"
---
# <a name="deprecated-features-in-sql-server-replication"></a>SQL Server 复制中不推荐使用的功能
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  本主题介绍 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中仍然可用但不推荐使用的复制功能。 按照计划， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]未来版本将不再具有这些功能。 在新的应用程序中不应使用这些不推荐使用的功能。  
  
## <a name="items-deprecated-in-includesssql15includessssql15-mdmd"></a> [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]  
  
|功能|描述|  
|-------------|-----------------|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|如果每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 端点都处于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的当前版本的两个主版本内，则支持复制。 因此， [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 不支持面向或源自 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]的复制。|  
|[!INCLUDE[ssEW](../../includes/ssew-md.md)]|如果每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 端点都处于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的当前版本的两个主版本内，则支持复制。 因此， [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 不支持面向或源自 [!INCLUDE[ssEW](../../includes/ssew-md.md)]的复制。|  
  
## <a name="see-also"></a>另请参阅  
 [复制的向后兼容性](../../relational-databases/replication/replication-backward-compatibility.md)  
  
  
