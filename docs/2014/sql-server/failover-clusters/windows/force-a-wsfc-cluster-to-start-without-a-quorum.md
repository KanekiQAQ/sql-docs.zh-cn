---
title: 在无仲裁情况下强制启动 WSFC 群集 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
ms.assetid: 4a121375-7424-4444-b876-baefa8fe9015
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 674f6f53610c8bf864aba5a2b5c7310c10f969c2
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "63049480"
---
# <a name="force-a-wsfc-cluster-to-start-without-a-quorum"></a>在无仲裁情况下强制启动 WSFC 群集
  本主题说明如何在无仲裁情况下强制启动 Windows Server 故障转移群集 (WSFC) 群集节点。  在灾难恢复和多子网方案中，可能需要它来为 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例恢复数据和完全重建高可用性。  
  
-   **开始之前：** [建议](#Recommendations)，[安全](#Security)  
  
-   **若要强制群集启动而无需仲裁使用：** [使用故障转移群集管理器](#FailoverClusterManagerProcedure)，[使用 Powershell](#PowerShellProcedure)，[使用 Net.exe](#CommandPromptProcedure)  
  
-   **跟进：** [跟进：在强制仲裁情况下启动群集后](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Recommendations"></a> 建议  
 除了明确指出的情况外，从 WSFC 群集中的任意节点执行时，本主题中的步骤都应适用。  但是，通过从要在无仲裁情况下强制启动的节点执行这些步骤，可能获得更好的效果并避免网络问题。  
  
###  <a name="Security"></a> 安全性  
 用户必须是一个域帐户，该帐户是每个 WSFC 群集节点上本地 Administrators 组的成员。  
  
##  <a name="FailoverClusterManagerProcedure"></a> 使用故障转移群集管理器  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>在无仲裁情况下强制启动群集  
  
1.  打开故障转移群集管理器并连接到所需的群集节点，以强制联机。  
  
2.  在“操作”窗格中，单击“强制启动群集”，然后单击“是 - 强制启动我的群集”    。  
  
3.  在左窗格中，在 **“故障转移群集管理器”** 树中单击该群集名称。  
  
4.  在摘要窗格中，确认当前**仲裁配置**值是：**警告：群集正在 ForceQuorum 状态**。  
  
##  <a name="PowerShellProcedure"></a> 使用 PowerShell  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>在无仲裁情况下强制启动群集  
  
1.  通过 **“以管理员身份运行”** 启动提升的 Windows PowerShell。  
  
2.  导入 `FailoverClusters` 模块以启用群集 commandlet。  
  
3.  使用 `Stop-ClusterNode` 以确保群集服务已停止。  
  
4.  将 `Start-ClusterNode` 和 `-FixQuorum` 结合使用以强制启动群集服务。  
  
5.  将 `Get-ClusterNode` 和 `-Propery NodeWieght = 1` 结合使用以设置确保节点是仲裁的投票成员的值。  
  
6.  以可读格式输出群集节点属性。  
  
### <a name="example-powershell"></a>示例 (PowerShell)  
 下面的示例在无仲裁情况下强制启动 AlwaysOnSrv02 节点群集服务，设置 `NodeWeight = 1`，然后枚举新强制的节点的群集节点状态。  
  
```powershell  
Import-Module FailoverClusters  
  
$node = "AlwaysOnSrv02"  
Stop-ClusterNode -Name $node  
Start-ClusterNode -Name $node -FixQuorum  
  
(Get-ClusterNode $node).NodeWeight = 1  
  
$nodes = Get-ClusterNode -Cluster $node  
$nodes | Format-Table -property NodeName, State, NodeWeight  
  
```  
  
##  <a name="CommandPromptProcedure"></a> 使用 Net.exe  
  
##### <a name="to-force-a-cluster-to-start-without-a-quorum"></a>在无仲裁情况下强制启动群集  
  
1.  使用远程桌面连接到所需的群集节点，以强制联机。  
  
2.  通过 **“以管理员身份运行”** 启动提升的命令提示符。  
  
3.  使用 **net.exe** 以确保本地群集服务已停止。  
  
4.  将 **net.exe** 和 `/forcequorum` 结合使用以强制启动本地群集服务。  
  
### <a name="example-netexe"></a>示例 (Net.exe)  
 下面的示例在无仲裁情况下强制启动一个节点群集服务，设置 `NodeWeight = 1`，然后枚举新强制的节点的群集节点状态。  
  
```ms-dos  
net.exe stop clussvc  
net.exe start clussvc /forcequorum  
```  
  
##  <a name="FollowUp"></a> 跟进：在强制仲裁情况下启动群集后  
  
-   在使其他节点重新联机前，必须重新计算和重新配置 NodeWeight 值以正确构造新的仲裁。 否则，该群集可能再次脱机。  
  
     有关详细信息，请参阅 [WSFC 仲裁模式和投票配置 (SQL Server)](wsfc-quorum-modes-and-voting-configuration-sql-server.md)。  
  
-   如果出现意外的仲裁失败，本主题所述的步骤仅仅是使 WSFC 群集重新联机中的一个步骤。  您可能还要执行其他步骤来阻止其他 WSFC 群集节点干扰新的仲裁配置。  
  
-   其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能（如 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]、数据库镜像和日志传送）可能也需要执行后续操作来恢复数据和完全重建高可用性。  
  
     **详细信息：**  
  
     [执行可用性组的强制手动故障转移 (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
     [在数据库镜像会话中强制服务 (Transact-SQL)](../../../database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)  
  
     [故障转移到日志传送辅助服务器 (SQL Server)](../../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
##  <a name="RelatedContent"></a> 相关内容  
  
-   [查看故障转移群集的事件和日志](https://technet.microsoft.com/en-us/library/cc772342\(WS.10\).aspx)  
  
-   [Get-ClusterLog 故障转移群集 Cmdlet](https://technet.microsoft.com/library/ee461045.aspx)  
  
## <a name="see-also"></a>请参阅  
 [通过强制仲裁进行 WSFC 灾难恢复 (SQL Server)](wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [配置群集仲裁 NodeWeight 设置](configure-cluster-quorum-nodeweight-settings.md)   
 [Windows PowerShell 中按任务焦点列出的故障转移群集 Cmdlet](https://technet.microsoft.com/library/ee619761\(WS.10\).aspx)  
  
  
