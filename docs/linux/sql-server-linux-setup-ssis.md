---
title: 在 Linux 上安装 SQL Server Integration Services
description: 本文介绍如何在 Linux 上安装 SQL Server Integration Services (SSIS)。
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 01/09/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 68f3497e9f3f47d7e43c2bda0083bc25632d8221
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68032439"
---
# <a name="install-sql-server-integration-services-ssis-on-linux"></a>在 Linux 上安装 SQL Server Integration Services (SSIS)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

按照这篇文章来安装 SQL Server Integration Services 中的步骤 (`mssql-server-is`) 在 Linux 上。 有关此版本的适用于 Linux Integration Services 中支持的功能的信息，请参阅[发行说明](sql-server-linux-release-notes.md)。

为你的平台安装 SQL Server 的集成服务器：

- [Ubuntu 16.04](#ubuntu)
- [Red Hat Enterprise Linux](#RHEL)

## <a name="ubuntu"></a> 在 Ubuntu 上安装 SSIS
若要安装`mssql-server-is`包在 Ubuntu 上，请执行以下步骤：

1. 导入公共存储库 GPG 密钥。

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. 注册 Microsoft SQL Server Ubuntu 存储库。

   ```bash
   sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```

3. 运行以下命令以安装 SQL Server Integration Services。

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server-is
   ```

4. 安装集成服务之后，运行`ssis-conf`。 有关详细信息，请参阅[在 Linux 上使用 ssis conf 配置 SSIS](sql-server-linux-configure-ssis.md)。

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

5. 在完成配置后，设置的路径。

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

### <a name="update-ssis"></a>更新 SSIS
如果已有`mssql-server-is`安装，您可以更新最新版本使用以下命令：

```bash
sudo apt-get install mssql-server-is
```

### <a name="remove-ssis"></a>删除 SSIS
若要删除`mssql-server-is`，可以运行以下命令：
```bash
sudo apt-get remove mssql-server-is
```

## <a name="RHEL"></a> 在 RHEL 上安装 SSIS
若要安装`mssql-server-is`RHEL 上包，请执行以下步骤：

1. 下载 Microsoft SQL Server Red Hat 存储库配置文件。

   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```

1. 运行以下命令以安装 SQL Server Integration Services。

   ```bash
   sudo yum install -y mssql-server-is
   ```


1. 安装完成后，运行`ssis-conf`。 有关详细信息，请参阅[在 Linux 上使用 ssis conf 配置 SSIS](sql-server-linux-configure-ssis.md)。

   ```bash
   sudo /opt/ssis/bin/ssis-conf setup
   ```

1. 完成配置后，将路径设置。

   ```bash
   export PATH=/opt/ssis/bin:$PATH
   ```

### <a name="update-ssis"></a>更新 SSIS
如果已有`mssql-server-is`安装，您可以更新最新版本使用以下命令：

```bash
sudo yum update mssql-server-is
```

### <a name="remove-ssis"></a>删除 SSIS
若要删除`mssql-server-is`，可以运行以下命令：
```bash
sudo yum remove mssql-server-is
```

## <a name="unattended-installation"></a>无人参与的安装
若要在运行时运行无人参与的安装`ssis-conf setup`，执行以下操作：
1.  指定`-n`（无提示） 选项。
2.  通过设置环境变量来提供所需的值。

下面的示例将执行以下操作：
-   安装 SSIS。
-   通过提供的值指定开发人员版`SSIS_PID`环境变量。
-   通过提供的值接受 EULA`ACCEPT_EULA`环境变量。
-   通过指定运行无人参与的安装`-n`（无提示） 选项。

```
sudo SSIS_PID=Developer ACCEPT_EULA=Y /opt/ssis/bin/ssis-conf -n setup 
```

### <a name="environment-variables-for-unattended-installation"></a>无人参与安装的环境变量

| 环境变量 | 描述 |
|---|---|
| **ACCEPT_EULA** | 接受 SQL Server 许可协议设置为任何值时 (例如， `Y`)。|
| **SSIS_PID** | 设置 SQL Server 版本或产品密钥。 下面是可能的值：<br/>Evaluation<br/>开发人员<br/>Express <br/>Web <br/>标准<br/>Enterprise <br/>产品密钥<br/><br/>如果指定产品密钥，产品密钥必须在窗体`#####-#####-#####-#####-#####`，其中`#`是以字母或数字。  |
| | |

## <a name="next-steps"></a>后续步骤

若要在 Linux 上运行 SSIS 包，请参阅[提取、 转换和加载数据使用 SSIS 在 Linux 上的 SQL Server](sql-server-linux-migrate-ssis.md)。

若要配置 Linux 上的 SSIS 的其他设置，请参阅[配置使用 ssis conf Linux 上 SQL Server Integration Services](sql-server-linux-configure-ssis.md)。

## <a name="related-content-about-ssis-on-linux"></a>有关在 Linux 上的 SSIS 的相关的内容
-   [提取、 转换和加载使用 SSIS 的 Linux 上的数据](sql-server-linux-migrate-ssis.md)
-   [使用 ssis conf 配置 Linux 上的 SQL Server Integration Services](sql-server-linux-configure-ssis.md)
-   [限制和 Linux 上的 SSIS 的已知的问题](sql-server-linux-ssis-known-issues.md)
-   [计划 SQL Server Integration Services 包在 Linux 上的使用 cron 执行](sql-server-linux-schedule-ssis-packages.md)
