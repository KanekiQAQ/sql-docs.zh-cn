---
title: WideWorldImporters OLAP 示例数据库的安装和配置-SQL |Microsoft Docs
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=sqlallproducts-allversions||=azuresqldb-mi-current'
ms.openlocfilehash: ed3c5f1a4f2168196a651aea64c9c88311a197b9
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68104255"
---
# <a name="wideworldimportersdw-installation-and-configuration"></a>WideWorldImportersDW 安装和配置
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
WideWorldImportersDW 数据库的安装和配置说明。

- [SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) （或更高版本） 或[Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)。 若要使用示例的完整版本，请使用 SQL Server 评估/开发人员/企业版。
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。 为获得最佳结果使用 2016 年 6 月发行版或更高版本。

## <a name="download"></a>下载

该示例的最新版本：

[wide-world-importers-release](https://go.microsoft.com/fwlink/?LinkID=800630)

将示例 WideWorldImportersDW 数据库备份/bacpac 下载对应到你的 SQL Server 或 Azure SQL 数据库版本。

从以下位置提供了源代码以重新创建示例数据库。 请注意，数据填充基于 ETL (WideWorldImporters) 包括 OLTP 数据库中：

[wide-world-importers-source](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/wwi-dw-database-scripts)

## <a name="install"></a>安装


### <a name="sql-server"></a>SQL Server

若要将备份还原到 SQL Server 实例，可以使用 Management Studio。

1. 打开 SQL Server Management Studio 并连接到目标 SQL Server 实例。
2. 右键单击**数据库**节点，然后选择**Restore Database**。
3. 选择**设备**，然后单击按钮 **...**
4. 在对话框**选择备份设备**，单击**添加**，导航到的服务器的文件系统中的数据库备份，并选择的备份。 单击 **“确定”** 。
5. 如果需要更改数据的目标位置，并在日志文件，**文件**窗格。 请注意，它将数据和日志文件的不同驱动器上的最佳做法。
6. 单击 **“确定”** 。 这将启动数据库还原。 完成后，您将拥有数据库安装在您的 SQL Server 实例上的 WideWorldImporters。

### <a name="azure-sql-database"></a>Azure SQL Database

将 bacpac 导入新的 SQL 数据库，可以使用 Management Studio。

1. （可选）如果你还没有 SQL Server 在 Azure 中，导航到[Azure 门户](https://portal.azure.com/)并创建新的 SQL 数据库。 过程中创建数据库，将创建的服务器。 记下的服务器。
   - 请参阅[本教程](https://azure.microsoft.com/documentation/articles/sql-database-get-started/)在几分钟内创建数据库
2. 打开 SQL Server Management Studio 并连接到 Azure 中的服务器。
3. 右键单击**数据库**节点，然后选择**导入数据层应用程序**。
4. 在中**导入设置**选择**从本地磁盘导入**和选择示例数据库的 bacpac 从您的文件系统。
5. 下**数据库设置**更改到的数据库名称*WideWorldImportersDW*和选择要使用的目标版本和服务目标。
6. 单击**下一步**并**完成**开始部署。 它将需要几分钟才能完成。 指定服务的目标低于 S2 时可能需要更长。

## <a name="configuration"></a>配置

[适用于 SQL Server 2016 （及更高版本） 开发人员/评估/Enterprise Edition]

示例数据库可使用 PolyBase 的 Hadoop 或 Azure blob 存储中的查询文件。 但是，与 SQL Server 的默认情况下不安装该功能-需要 SQL Server 安装过程中选择它。 因此，安装后步骤是必需的。

1. 在 SQL Server Management Studio，连接到 WideWorldImportersDW 数据库并打开新查询窗口。
2. 运行以下 T-SQL 命令，以便在数据库中的 PolyBase 使用：

   执行 [应用程序]。[Configuration_ApplyPolyBase]
