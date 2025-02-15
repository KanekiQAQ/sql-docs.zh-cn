---
title: 在分析平台系统中配置 PolyBase Hadoop 安全性 |Microsoft Docs
description: 介绍如何在并行数据仓库，若要将连接到外部 Hadoop 配置 PolyBase。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 1aebac3f63a105f0276e676ccc807aaebdbf097d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67960297"
---
# <a name="polybase-configuration-and-security-for-hadoop"></a>Hadoop 的 PolyBase 配置和安全

本文提供有关影响 APS PolyBase 连接到 Hadoop 的各种配置设置的引用。 什么是 PolyBase 的演练，请参阅[什么是 PolyBase](configure-polybase-connectivity-to-external-data.md)。

> [!NOTE]
> AP 上所有计算节点和控制节点上需在 XML 文件的更改。
> 
> 修改 AP 中的 XML 文件时要特别小心。 任何缺少的标记或不需要的字符可能会使妨碍功能的 usablilty 的 xml 文件无效。
> Hadoop 配置文件位于以下路径：  
> ```  
> C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\Hadoop\conf 
> ``` 
> 对 xml 文件的任何更改需要重启服务才能生效。

## <a id="rpcprotection"></a> Hadoop.RPC.Protection 设置

在 Hadoop 群集中保护通信的常用方法是将 hadoop.rpc.protection 配置更改为“隐私”或“完整性”。 默认情况下，PolyBase 假定配置设置为“身份验证”。 要替代此默认设置，请将以下属性添加到 core-site.xml 文件。 通过更改此配置，可实现 hadoop 节点之间以及 SSL 到 SQL Server 的连接之间安全的数据传输。

```xml
<!-- RPC Encryption information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG -->
   <property>
     <name>hadoop.rpc.protection</name>
     <value></value>
   </property> 
```

## <a id="kerberossettings"></a> Kerberos 配置  

请注意，当 PolyBase 向 Kerberos 保护的群集证明身份时，默认情况下需要将 hadoop.rpc.protection 设置设为“身份验证”。 这会使 Hadoop 节点间的数据通信保持非加密状态。 要为 hadoop.rpc.protection 使用“隐私”或“完整性”设置，请在 PolyBase 服务器上更新 core-site.xml 文件。 有关详细信息，请参阅上一节的[使用 Hadoop.rpc.protection 连接到 Hadoop 群集](#rpcprotection)。

若要连接到 Kerberos 保护的 Hadoop 群集上所有 AP 使用 MIT KDC 需要进行以下更改计算节点和控制节点：

1. AP 的安装路径中查找 Hadoop 配置目录。 通常情况下，该路径为：  

   ```  
   C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\Hadoop\conf  
   ```  

2. 查找表中列出的配置密钥 Hadoop 端配置值。 （对于 Hadoop 计算机，在 Hadoop 配置目录中查找文件。）  
   
3. 将配置值复制到 SQL Server 计算机上对应文件的值属性中。  
   
   |**#**|**配置文件**|**配置密钥**|**操作**|  
   |------------|----------------|---------------------|----------|   
   |1|core-site.xml|polybase.kerberos.kdchost|指定 KDC 主机名。 例如：kerberos.your-realm.com。|  
   |2|core-site.xml|polybase.kerberos.realm|指定 Kerberos 领域。 例如：YOUR-REALM.COM|  
   |3|core-site.xml|hadoop.security.authentication|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如：KERBEROS<br></br>**安全说明：** KERBEROS 必须采用大写形式。 如果采用小写，则可能不会打开。|   
   |4|hdfs-site.xml|dfs.namenode.kerberos.principal|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如： hdfs/_HOST@YOUR-REALM.COM|  
   |5|mapred-site.xml|mapreduce.jobhistory.principal|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如： mapred/_HOST@YOUR-REALM.COM|  
   |6|mapred-site.xml|mapreduce.jobhistory.address|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如：10.193.26.174:10020|  
   |7|yarn-site.xml yarn。|yarn.resourcemanager.principal|查找 Hadoop 端配置并复制到 SQL Server 计算机。 例如： yarn/_HOST@YOUR-REALM.COM|  

**core-site.xml**
```xml
<property>
  <name>polybase.kerberos.realm</name>
  <value></value>
</property>
<property>
  <name>polybase.kerberos.kdchost</name>
  <value></value>
</property>
<property>
    <name>hadoop.security.authentication</name>
    <value>KERBEROS</value>
</property>
```

**hdfs-site.xml**
```xml
<property>
  <name>dfs.namenode.kerberos.principal</name>
  <value></value> 
</property>
```

**mapred-site.xml**
```xml
<property>
  <name>mapreduce.jobhistory.principal</name>
  <value></value>
</property>
<property>
  <name>mapreduce.jobhistory.address</name>
  <value></value>
</property>
```

**yarn-site.xml**
```xml
<property>
  <name>yarn.resourcemanager.principal</name>
  <value></value>
</property>
```

4. 创建数据库范围内的凭据对象，以指定每个 Hadoop 用户的身份验证信息。 请参阅 [PolyBase T-SQL 对象](../relational-databases/polybase/polybase-t-sql-objects.md)。

## <a id="encryptionzone"></a> Hadoop 加密区域设置
如果您使用的 Hadoop 加密区域修改的 core-site.xml 和 hdfs-site.xml 如下所示。 提供与对应的端口号运行 KMS 服务的 ip 地址。 在 CDH KMS 的默认端口为 16000。

**core-site.xml**
```xml
<property>
  <name>hadoop.security.key.provider.path</name>
  <value>kms://http@<ip address>:16000/kms</value> 
</property>
```

**hdfs-site.xml**
```xml
<property>
  <name>dfs.encryption.key.provider.uri</name>
  <value>kms://http@<ip address>:16000/kms</value>
</property>
<property>
  <name>hadoop.security.key.provider.path</name>
  <value>kms://http@<ip address>:16000/kms</value>
  </property>
```