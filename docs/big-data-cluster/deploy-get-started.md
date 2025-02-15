---
title: 入门
titleSuffix: SQL Server big data clusters
description: 了解用于部署 SQL Server 2019 大数据群集 (预览版) 的步骤和资源。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 07/24/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 41dd39c7aa613083f8ff47824ed6238b6f3c61b9
ms.sourcegitcommit: 1f222ef903e6aa0bd1b14d3df031eb04ce775154
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2019
ms.locfileid: "68419432"
---
# <a name="get-started-with-sql-server-big-data-clusters"></a>SQL Server 大数据群集入门

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

本文提供了有关如何部署[SQL Server 2019 大数据群集 (预览版)](big-data-cluster-overview.md)的概述。 它旨在为您提供概念, 并提供一个框架来了解本节中的其他部署文章。 具体的部署步骤因客户端和服务器的平台选择而异。

## <a id="tools"></a>客户端工具

大数据群集需要一组特定的客户端工具。 将大数据群集部署到 Kubernetes 之前, 应安装以下工具:

| Tool | 描述 |
|---|---|
| **azdata** | 部署和管理大数据群集。 |
| **kubectl** | 创建并管理基础 Kubernetes 群集。 |
| **Azure Data Studio** | 使用大数据群集的图形界面。 |
| **SQL Server 2019 扩展** | 支持大数据群集功能 Azure Data Studio 扩展。 |

不同方案需要其他工具。 每篇文章应说明用于执行特定任务的先决条件工具。 有关工具和安装链接的完整列表, 请参阅[安装 SQL Server 2019 大数据工具](deploy-big-data-tools.md)。

## <a name="kubernetes"></a>Kubernetes

大数据群集部署为在[Kubernetes](https://kubernetes.io/docs/home)中托管的一系列相关容器。 可以通过多种方式托管 Kubernetes。 即使已有现有的 Kubernetes 环境, 也应查看大数据群集的相关要求。

- **Azure Kubernetes 服务 (AKS)** :AKS 允许你在 Azure 中部署托管 Kubernetes 群集。 你只管理和维护代理节点。 使用 AKS, 无需为群集预配硬件。 使用大数据群集[部署脚本](quickstart-big-data-cluster-deploy.md), 还可以轻松地创建 AKS 群集并部署大数据群集。 有关将 AKS 与大数据群集配合使用的详细信息, 请参阅为[SQL Server 2019 大数据群集 (预览版) 部署配置 Azure Kubernetes 服务](deploy-on-aks.md)。

- **多台计算机**:你还可以将 Kubernetes 部署到多台 Linux 计算机, 这些计算机可以是物理服务器或虚拟机。 [Kubeadm](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/)工具可用于创建 Kubernetes 群集。 如果已经有想要用于大数据群集的现有基础结构, 此方法会很好地运行。 有关将**kubeadm**部署用于大数据群集的详细信息, 请参阅[在多台计算机上配置 Kubernetes SQL Server 2019 大数据群集 (预览版) 部署](deploy-with-kubeadm.md)。

- **Minikube**:Minikube 允许你在单个服务器上本地运行 Kubernetes。 如果你要尝试大数据群集, 或者需要在测试或开发方案中使用它, 则这是一个不错的选择。 有关使用 Minikube 的详细信息, 请参阅[Minikube 文档](https://kubernetes.io/docs/setup/minikube/)。 有关将 Minikube 与大数据群集配合使用的具体要求, 请参阅[Configure Minikube for SQL Server 2019 大数据群集部署](deploy-on-minikube.md)。

## <a name="deploy-a-big-data-cluster"></a>部署大数据群集

配置 Kubernetes 后, 可以使用`azdata bdc create`命令部署大数据群集。 部署时, 可以采用多种不同的方法。

- 如果要部署到开发测试环境, 可以选择使用**azdata**提供的[默认配置](deployment-guidance.md#deploy)之一。

- 若要自定义你的部署, 你可以创建并使用你自己的[部署配置文件](deployment-guidance.md#configfile)。

- 对于完全无人参与的安装, 可以在环境变量中传递所有其他设置。 有关详细信息, 请参阅[无人参与的部署](deployment-guidance.md#unattended)。

## <a name="deployment-scripts"></a>部署脚本

部署脚本可以在单个步骤中帮助部署 Kubernetes 和大数据群集。 它们还经常为大数据群集设置提供默认值。 有关 Azure Kubernetes 服务 (AKS) 上大数据群集的部署脚本的示例, 请参阅以下文章:

[使用部署脚本 (AKS) 部署 SQL Server 2019 大数据群集](quickstart-big-data-cluster-deploy.md)。

你可以通过创建自己的版本来自定义任何部署脚本, 以不同的方式配置大数据群集环境变量。

[使用 Azure Data Studio 笔记本部署 SQL Server 2019 大数据群集](deploy-notebooks.md)。

## <a name="next-steps"></a>后续步骤

成功部署大数据群集后,[连接到该群集](connect-to-big-data-cluster.md), 并考虑加载用于多个演练的[示例数据](tutorial-load-sample-data.md)。