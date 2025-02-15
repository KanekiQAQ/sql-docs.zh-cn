---
title: 连接到主服务器和 HDFS
titleSuffix: SQL Server big data clusters
description: 了解如何连接到 SQL Server 主实例和 SQL Server 2019 大数据群集 （预览版） 的 HDFS/Spark 网关。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 05/22/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1f09763b210427c84efe75d693fee302d7048db7
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67958647"
---
# <a name="connect-to-a-sql-server-big-data-cluster-with-azure-data-studio"></a>连接到 SQL Server 大数据群集使用 Azure Data Studio

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

本文介绍如何从 Azure Data Studio 连接到 SQL Server 2019 大数据群集 （预览版）。

## <a name="prerequisites"></a>先决条件

- 已部署[SQL Server 2019 大数据群集](deployment-guidance.md)。
- [SQL Server 2019 大数据工具](deploy-big-data-tools.md):
   - **Azure Data Studio**
   - **SQL Server 2019 扩展**
   - **kubectl**

## <a id="master"></a> 连接到群集

若要连接到具有 Azure Data Studio 的大数据群集，请在群集中与 SQL Server 主实例的新连接。 以下步骤介绍如何连接到使用 Azure Data Studio 的主实例。

1. 从命令行中，查找使用以下命令在主实例的 IP:

   ```
   kubectl get svc master-svc-external -n <your-big-data-cluster-name>
   ```

   > [!TIP]
   > 大数据群集名称默认为**mssql 群集**除非自定义部署配置文件中的名称。 有关详细信息，请参阅[配置的大数据群集部署设置](deployment-custom-configuration.md#clustername)。

1. 在 Azure Data Studio，按**F1** > **新连接**。

1. 在中**连接类型**，选择**Microsoft SQL Server**。

1. 键入 SQL Server 主实例中的 IP 地址**服务器名称**(例如： **\<IP 地址\>、 31433**)。

1. 输入 SQL 登录名**用户名**并**密码**。

   > [!TIP]
   > 默认情况下，用户名称是**SA**和密码更改，除非对应于**MSSQL_SA_PASSWORD**部署过程中使用的环境变量。

1. 将目标更改**数据库名称**到一个关系数据库。

   ![连接到主实例](./media/connect-to-big-data-cluster/connect-to-cluster.png)

1. 按**Connect**，并**Server 仪表板**应显示。

Azure Data Studio 2019 年 2 月版本中，连接到 SQL Server 主实例还可以与 HDFS/Spark 网关进行交互。 这意味着不需要使用单独的连接是 HDFS 和下一节介绍的 Spark。

- 在对象资源管理器现在包含一个新**Data Services**节点，右键单击支持大数据群集任务，例如创建新的笔记本或提交 spark 作业。 
- **Data Services**节点还包含**HDFS**文件夹中的 HDFS 探索并在 Notebook 中执行操作，例如 Create External Table 或分析。
- **Server 仪表板**连接还包含用于选项卡**SQL Server 大数据群集**并**SQL Server 2019 （预览版）** 时安装该扩展。

   ![Azure 数据 Studio 数据服务节点](./media/connect-to-big-data-cluster/connect-data-services-node.png)

## <a name="next-steps"></a>后续步骤

有关 SQL Server 2019 大数据群集的详细信息，请参阅[什么是 SQL Server 2019 大数据群集](big-data-cluster-overview.md)。