---
title: 运行故障转移群集实例 - Linux 上的 SQL Server
description: 此文章介绍了如何运行在 Linux 上的 SQL Server 故障转移群集实例 (FCI)。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: a29d1d61b628126d03458fced964bde7c92b6d68
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68032295"
---
# <a name="operate-failover-cluster-instance---sql-server-on-linux"></a>运行故障转移群集实例 - Linux 上的 SQL Server

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

此文章介绍了如何运行在 Linux 上的 SQL Server 故障转移群集实例 (FCI)。 如果你尚未在 Linux 上创建 SQL Server FCI，请参阅[配置故障转移群集实例-Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure.md)。 

## <a name="failover"></a>故障转移

针对 Fci 的故障转移是类似于 Windows Server 故障转移群集 (WSFC)。 如果承载 FCI 的群集节点遇到某种形式的故障，FCI 应自动故障转移到另一个节点。 与不同的 WSFC，没有任何方法可设置首选的所有者，使 Pacemaker 选取将为 FCI 的新主机的节点。

有些的时候你可能想要手动故障到另一个节点 FCI。 进程不与 Fci 在 WSFC 上相同。 在 WSFC 故障转移在角色级别的资源。 Pacemaker 中, 选择要移动的资源，假定所有约束正确都无误，其他所有内容会将也。 

故障转移到的方式取决于 Linux 分发版。 按照 linux 分发版的说明。

- [RHEL 或 Ubuntu](#-manual-failover-rhel-or-ubuntu)
- [SLES](#-manual-failover-sles)

## <a name = "#-manual-failover-rhel-or-ubuntu"></a> 手动故障转移 （RHEL 或 Ubuntu）

若要执行的手动故障转移，在 Red Hat Enterprise Linux (RHEL) 或 Ubuntu 服务器，请执行以下步骤。
1.  发出以下命令： 

   ```bash
   sudo pcs resource move <FCIResourceName> <NewHostNode> 
   ```

   \<FCIResourceName > 是 SQL Server FCI 的 Pacemaker 资源名称。

   \<NewHostNode > 是你想要承载 FCI 的群集节点的名称。 

   不会获得任何确认。

2.  在手动故障转移时，Pacemaker 创建选择的目的是手动移动资源的位置约束。 若要查看此约束，请运行`sudo pcs constraint`。

3.  在故障转移完成后，删除该约束通过发出`sudo pcs resource clear <FCIResourceName>`。 

\<FCIResourceName > 是在 FCI 的 Pacemaker 资源名称。 

## <a name = "#-manual-failover-sles"></a> 手动故障转移 (SLES)


在 Suse Linux Enterprise Server (SLES)，使用`migrate`命令手动故障转移到 SQL Server FCI。 例如：

```bash
crm resource migrate <FCIResourceName> <NewHostNode>
```

\<FCIResourceName > 是故障转移群集实例的资源名称。 

\<NewHostNode > 是新的目标主机的名称。 


<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

--->

## <a name="next-steps"></a>后续步骤

- [配置故障转移群集实例-Linux 上的 SQL Server](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
