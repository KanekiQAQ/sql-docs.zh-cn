---
title: 快照代理安全性 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.security.SSA.f1
helpviewer_keywords:
- Snapshot Agent Security dialog box
ms.assetid: 64e84c67-acc6-4906-98d4-3451767363fe
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6f0db203880a8219547296695ff3de4863b98e12
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68051675"
---
# <a name="snapshot-agent-security"></a>快照代理安全性
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  可以使用 **“快照代理安全性”** 对话框指定以下内容：  
  
-   用于在分发服务器上运行快照代理的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帐户。 Windows 帐户也称为“进程帐户 ”，因为代理进程是在此帐户下运行。  
  
-   快照代理与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器建立连接时所处的上下文。 通过模拟 Windows 帐户，或在指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户的上下文中可以进行连接。  
  
    > [!NOTE]  
    >  即使发布服务器与分发服务器在同一台计算机上，快照代理也会与发布服务器建立连接。 快照代理还可以与分发服务器建立连接；这些连接始终是通过模拟运行代理的 Windows 帐户来建立的。  
  
     对于 Oracle 发布服务器，请在 **“发布服务器属性”** 对话框（可以通过 **“分发服务器属性”** 对话框访问）中指定快照代理连接发布服务器时所处的上下文。 有关详细信息，请参阅 [View and Modify Replication Security Settings](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)。  
  
 所有帐户必须是有效的，并且为每个帐户指定了正确的密码。 在运行代理之前不会对帐户和密码进行验证。  
  
## <a name="options"></a>选项  
 **进程帐户**  
 输入用于在分发服务器上运行快照代理的 Windows 帐户。 所指定的 Windows 帐户必须满足以下条件：  
  
-   至少是分发数据库中的 **db_owner** 固定数据库角色的成员。  
  
-   对快照共享具有写权限。  
  
  和“确认密码”   
 输入 Windows 帐户的密码。  
  
 **连接到发布服务器**  
 选择快照代理应该通过模拟 **“进程帐户”** 文本框中指定的帐户，还是通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户来建立与发布服务器的连接。 如果选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户，请输入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和密码。  
  
> [!NOTE]  
>  建议您选择模拟 Windows 帐户，而不要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户。  
  
 用于连接的 Windows 帐户或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帐户至少必须是发布数据库中的 **db_owner** 固定数据库角色的成员。  
  
## <a name="see-also"></a>另请参阅  
 [复制的标识和访问控制](../../relational-databases/replication/security/identity-and-access-control-replication.md)   
 [复制代理安全模式](../../relational-databases/replication/security/replication-agent-security-model.md)   
 [复制代理概述](../../relational-databases/replication/agents/replication-agents-overview.md)   
 [复制安全最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
