---
title: 运行 SQL Server 的 Red Hat Enterprise Linux 共享的群集
description: 为 SQL Server 配置 Red Hat Enterprise Linux 共享的磁盘群集实现高可用性。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 075ab7d8-8b68-43f3-9303-bbdf00b54db1
ms.openlocfilehash: e7b81a97ab186ef79f27ee3456a5761157c02f3f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68032241"
---
# <a name="operate-red-hat-enterprise-linux-shared-disk-cluster-for-sql-server"></a>对适用于 SQL Server 的 Red Hat Enterprise Linux 共享磁盘群集进行操作

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

本文档介绍如何在 Red Hat Enterprise Linux 的共享磁盘故障转移群集上为 SQL Server 执行以下任务。

- 手动故障转移群集
- 监视故障转移群集 SQL Server 服务
- 添加群集节点
- 删除群集节点
- 更改 SQL Server 资源监视频率

## <a name="architecture-description"></a>体系结构说明

群集层基于 Red Hat Enterprise Linux (RHEL) 上[HA 加载项](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf)基础上构建[Pacemaker](https://clusterlabs.org/)。 Corosync 和 Pacemaker 协调群集通信和资源管理。 SQL Server 实例在一个节点或另一个节点上处于活动状态。

下图说明了 SQL Server 的 Linux 群集中的组件。 

![Red Hat Enterprise Linux 7 共享磁盘的 SQL 群集](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

有关群集配置、 资源代理选项和管理的详细信息，请访问[RHEL 参考文档](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html)。

## <a name = "failManual"></a>手动故障转移群集

`resource move`命令创建强制资源在目标节点上启动的约束。  执行后`move`命令中，执行资源`clear`将删除该约束，以便可以再次移动资源或具有自动故障转移的资源。 

```bash
sudo pcs resource move <sqlResourceName> <targetNodeName>  
sudo pcs resource clear <sqlResourceName> 
```

以下示例将移动**mssqlha**到一个名为节点的资源**sqlfcivm2**，然后删除该约束，以便可以将资源移到另一个节点更高版本。  

```bash
sudo pcs resource move mssqlha sqlfcivm2 
sudo pcs resource clear mssqlha 
```

## <a name="monitor-a-failover-cluster-sql-server-service"></a>监视故障转移群集 SQL Server 服务

查看当前群集状态：

```bash
sudo pcs status  
```

查看群集和资源的实时状态：

```bash
sudo crm_mon 
```

查看资源代理日志 `/var/log/cluster/corosync.log`

## <a name="add-a-node-to-a-cluster"></a>向群集添加节点

1. 检查每个节点的 IP 地址。 以下脚本显示当前节点的 IP 地址。 

   ```bash
   ip addr show
   ```

3. 为每个节点提供长度不超过 15 个字符的唯一名称。 默认情况下，在 Red Hat Linux 计算机名称是`localhost.localdomain`。 此默认名称可能不是唯一的，并且过长。 在新节点上设置计算机名称。 通过将其添加到设置的计算机名称`/etc/hosts`。 以下脚本可使用 `vi` 编辑 `/etc/hosts`。 

   ```bash
   sudo vi /etc/hosts
   ```

   下面的示例演示`/etc/hosts`与名为三个节点的添加件`sqlfcivm1`， `sqlfcivm2`，和`sqlfcivm3`。

   ```
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1         localhost localhost6 localhost6.localdomain6
   10.128.18.128 fcivm1
   10.128.16.77 fcivm2
   10.128.14.26 fcivm3
    ```
    
   此文件在每个节点上应相同。 

1. 在新节点上停止 SQL Server 服务。

1. 按照说明将数据库文件目录装载到共享位置：

   在 NFS 服务器上，安装 `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils 
   ``` 

   打开客户端和 NFS 服务器上的防火墙 

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

   编辑 /etc/fstab 文件，将装载命令包括在内： 

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr
   ```

   运行`mount -a`的更改才会生效。
   
1. 在新节点上，创建一个文件来存储用于登录 Pacemaker 的 SQL Server 用户名和密码。 以下命令创建并填充此文件：

   ```bash
   sudo touch /var/opt/mssql/passwd
   sudo echo "<loginName>" >> /var/opt/mssql/secrets/passwd
   sudo echo "<loginPassword>" >> /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/passwd
   sudo chmod 600 /var/opt/mssql/passwd
   ```

3. 在新节点上，打开 Pacemaker 防火墙端口。 若要使用 `firewalld` 打开这些端口，请运行以下命令：

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > [!NOTE]
   > 如果正在使用不具有内置的高可用性配置的另一个防火墙，则需要为 Pacemaker 将无法与群集中的其他节点通信打开以下端口
   >
   > * TCP:端口 2224、 3121、 21064
   > * UDP:端口 5405

1. 在新节点上安装 Pacemaker 包。

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
 
2. 为安装 Pacemaker 和 Corosync 包时创建的默认用户设置密码。 使用相同的密码作为现有节点。 

   ```bash
   sudo passwd hacluster
   ```
 
3. 启用并启动 `pcsd` 服务和 Pacemaker。 这样，新节点则可以在重新启动后重新加入群集。 在新节点上运行以下命令。

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. 为 SQL Server 安装 FCI 资源代理。 在新节点上运行以下命令。 

   ```bash
   sudo yum install mssql-server-ha
   ```

1. 在群集中的现有节点上，验证新节点并将其添加到群集中：

    ```bash
    sudo pcs    cluster auth <nodeName3> -u hacluster 
    sudo pcs    cluster node add <nodeName3> 
    ```

    下面的示例添加一个名为节点**vm3**到群集。

    ```bash
    sudo pcs    cluster auth  
    sudo pcs    cluster start 
    ```

## <a name="remove-nodes-from-a-cluster"></a>从群集中删除节点

若要从运行以下命令的群集删除节点：

```bash
sudo pcs    cluster node remove <nodeName>  
```

## <a name="change-the-frequency-of-sqlservr-resource-monitoring-interval"></a>更改 sqlservr 资源监视间隔的频率

```bash
sudo pcs    resource op monitor interval=<interval>s <sqlResourceName> 
```

以下示例将 mssql 资源的监视间隔设为 2 秒：

```bash
sudo pcs    resource op monitor interval=2s mssqlha 
```
## <a name="troubleshoot-red-hat-enterprise-linux-shared-disk-cluster-for-sql-server"></a>对适用于 SQL Server 的 Red Hat Enterprise Linux 共享磁盘群集进行故障排除

在对群集进行故障排除期间，有助于了解这三个守护程序是如何协同工作以管理群集资源的。 

| 后台程序 | 描述 
| ----- | -----
| Corosync | 提供仲裁成员身份并在群集节点之间发送消息。
| Pacemaker | 驻留在 Corosync 顶部，并为资源提供状态机。 
| PCSD | 管理通过 Corosync 和 Pacemaker`pcs`工具

必须运行 PCSD 才能使用`pcs`工具。 

### <a name="current-cluster-status"></a>当前群集状态 

`sudo pcs status` 返回有关每个节点的群集、 仲裁、 节点、 资源和守护程序状态的基本信息。 

一个运行状况良好的 pacemaker 仲裁输出的示例如下：

```
Cluster name: MyAppSQL 
Last updated: Wed Oct 31 12:00:00 2016  Last change: Wed Oct 31 11:00:00 2016 by root via crm_resource on sqlvmnode1 
Stack: corosync 
Current DC: sqlvmnode1  (version 1.1.13-10.el7_2.4-44eb2dd) - partition with quorum 
3 nodes and 1 resource configured 

Online: [ sqlvmnode1 sqlvmnode2 sqlvmnode3] 

Full list of resources: 

mssqlha (ocf::sql:fci): Started sqlvmnode1 

PCSD Status: 
sqlvmnode1: Online 
sqlvmnode2: Online 
sqlvmnode3: Online 

Daemon Status: 
corosync: active/disabled 
pacemaker: active/enabled 
```

在示例中，`partition with quorum`意味着节点的大多数仲裁处于联机状态。 如果群集丢失节点多数仲裁`pcs status`将返回`partition WITHOUT quorum`，将停止所有资源。 

`online: [sqlvmnode1 sqlvmnode2 sqlvmnode3]` 返回当前参与群集的所有节点的名称。 如果没有参与任何节点，`pcs status`返回`OFFLINE: [<nodename>]`。

`PCSD Status` 显示每个节点的群集状态。

### <a name="reasons-why-a-node-may-be-offline"></a>节点可能处于脱机状态的原因

如果节点处于脱机状态，请检查以下各项。

- **防火墙**

    需要在所有节点上打开以下端口，才能使 Pacemaker 进行通信。
    
    - \* * TCP:2224、 3121、 21064

- **Pacemaker 或 Corosync 服务运行**

- **节点通信**

- **节点名映射**

## <a name="additional-resources"></a>其他资源

* [从头开始群集](https://clusterlabs.org/doc/Cluster_from_Scratch.pdf)pacemaker 的指南

## <a name="next-steps"></a>后续步骤

[为 SQL Server 配置 Red Hat Enterprise Linux 共享的磁盘群集](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)

