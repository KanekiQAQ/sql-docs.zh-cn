---
title: MSSQL_ENG014117 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG014117 error
ms.assetid: e5906a76-9511-4c47-8826-8c765b58a39d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0cb4c55cd131256410b799dd33fc370497964877
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67990521"
---
# <a name="mssqleng014117"></a>MSSQL_ENG014117
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|||  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|14117|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|未将 '%s' 配置为分发数据库。|  
  
## <a name="explanation"></a>解释  
 如果下列之一或两者均为 True，则会出现此错误：  
  
-   msdb..MSdistributiondbs 中缺少指定分发数据库的条目。  
  
-   在 master  数据库中没有本地服务器入口，或者存在的入口不正确。  
  
     复制要求一个拓扑中的所有服务器都使用具有可选实例名称的计算机名称（如果为群集实例，则为具有可选实例名称的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 虚拟服务器名称）来注册。 `SELECT @@SERVERNAME` 为拓扑中每个服务器返回的值必须与具有可选实例名称的计算机名称或虚拟服务器名称相匹配，复制才能正常运行。  
  
     如果已按 IP 地址或完全限定域名 (FQDN) 注册了任意 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，将不支持复制。 配置复制时，如果已在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中通过 IP 地址或 FQDN 注册了任何 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 实例，则可能会引发此错误。  
  
## <a name="user-action"></a>用户操作  
 验证分发服务器实例正确注册。 如果计算机的网络名称和 SQL Server 实例的名称不一致，则：  
  
-   添加 SQL Server 实例名称作为有效的网络名称。 一种设置备用网络名称的方法是将其添加到本地主机文件中。 默认情况下，本地主机文件位于 WINDOWS\system32\drivers\etc 或 WINNT\system32\drivers\etc 中。有关详细信息，请参阅 Windows 文档。  
  
     例如，如果计算机名称为 comp1，IP 地址为 10.193.17.129，实例名称为 inst1/instname，则将以下条目添加到主机文件中：  
  
     10.193.17.129 inst1  
  
-   禁用分发，注册实例，然后重新建立分发。 如果 @@SERVERNAME 的值对于某个非群集实例是不正确的，请执行下列步骤：  
  
    ```  
    sp_dropserver '<old_name>', 'droplogins'  
    go  
    sp_addserver '<new_name>', 'local'  
    go  
    ```  
  
     执行 [sp_addserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md) 存储过程后，必须重启 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务，对 @@SERVERNAME 的更改才能生效。  
  
     如果 @@SERVERNAME 的值对于某个群集实例是不正确的，则必须使用群集管理器更改该名称。 有关详细信息，请参阅 [AlwaysOn 故障转移群集实例 (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)。  
  
 验证已正确注册分发服务器实例后，请验证分发数据库是否已在 msdb..MSdistributiondbs 中列出。 如果未列出：  
  
1.  请编写分发配置的脚本。 有关详细信息，请参阅 [Scripting Replication](../../relational-databases/replication/scripting-replication.md)。  
  
2.  禁用分发，然后重新启用它。 有关详细信息，请参阅 [Configure Distribution](../../relational-databases/replication/configure-distribution.md)。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
