---
title: 使用数据迁移助手的到 Azure SQL 数据库迁移的本地 SQL Server 或 Azure Vm 上的 SQL Server |Microsoft Docs
description: 了解如何使用数据迁移助手将本地 SQL Server 迁移到 Azure SQL 数据库
ms.custom: ''
ms.date: 07/15/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: HJToland3
ms.author: rajpo
ms.openlocfilehash: 37e0065ed711c3cf550fec4bafe9aa08be8398e6
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68262312"
---
# <a name="migrate-on-premises-sql-server-or-sql-server-on-azure-vms-to-azure-sql-database-using-the-data-migration-assistant"></a>将本地 SQL Server 或 Azure Vm 上的 SQL Server 迁移到 Azure SQL 数据库使用数据迁移助手

数据迁移助手的 Azure Vm 或 Azure SQL 数据库上提供 SQL Server 内部部署和更高版本的 SQL Server 的升级或迁移到 SQL Server 的无缝的评估。

本文提供分步说明迁移 SQL Server 的本地到 Azure SQL 数据库通过使用数据迁移助手。

## <a name="create-a-new-migration-project"></a>创建新的迁移项目

1. 在左窗格中，选择**新建**（+），然后选择**迁移**项目类型。

2. 将源类型设置为**SQL Server**和目标服务器类型为**Azure SQL 数据库**。

3. 选择“创建”  。

   ![创建迁移项目](../dma/media/NewCreate1.png)

## <a name="specify-the-source-server-and-database"></a>指定源服务器和数据库

1. 源下**连接到源服务器**，在**服务器名称**文字框中，输入源 SQL Server 实例的名称。

2. 选择**身份验证类型**受源 SQL Server 实例。

   > [!NOTE]
   > 建议通过选择加密连接**对连接进行加密**下的复选框**连接 poperties**。

    ![选择源服务器](../dma/media/select-source-server.png)

3. 选择“连接”  。

4. 选择要迁移到 Azure SQL 数据库的单一源数据库。

   > [!NOTE]
   > 如果你想要评估的数据库和视图并应用建议在迁移之前，选择修复**评估数据库迁移之前？** 复选框。

    ![选择源数据库](../dma/media/select-source-database.png)

5. 选择“**下一步**”。

## <a name="specify-the-target-server-and-database"></a>指定目标服务器和数据库

1. 对于目标下,**连接到目标服务器**，在**服务器名称**文字框中，输入 Azure SQL 数据库实例的名称。 

2. 选择**身份验证类型**支持的目标 Azure SQL 数据库实例。

   > [!NOTE]
   > 建议通过选择加密连接**对连接进行加密**下的复选框**连接 poperties**。

     ![选择目标服务器](../dma/media/select-target-server.png)

3. 选择“连接”  。

4. 选择要迁移到单个目标数据库。

   > [!NOTE]
   > 如果你想要将迁移中的 Windows 用户**目标外部用户域名**文字框中，请确保正确指定目标外部用户域名。

    ![选择目标数据库](../dma/media/select-target-database.png)

5. 选择“**下一步**”。

## <a name="select-schema-objects"></a>选择架构对象

1. 从你想要迁移到 Azure SQL 数据库的源数据库中选择架构对象。

    ![选择架构对象](../dma/media/select-schema-objects.png)

       > [!NOTE]
       > Some of the objects that cannot be converted as-is are presented with automatic fix opportunities. Clicking these objects on the left pane displays the suggested fixes on the right pane. Review the fixes and choose to either apply or ignore all changes, object by object. Note that applying or ignoring all changes for one object does not affect changes to other database objects. Statements that cannot be converted or automatically fixed are reproduced to the target database and commented.

    ![建议的修复方法](../dma/media/suggested-fix.png)

2. 选择**常规 SQL 脚本**。

3. 查看生成的脚本。

    ![生成的脚本](../dma/media/generated-script.png)

## <a name="deploy-schema"></a>部署架构

1. 选择**部署架构**。

2. 查看架构部署的结果。

    ![架构部署结果](../dma/media/schema-deployment-results.png)

3. 选择**将数据迁移**来启动数据迁移过程。

4. 选择你想要迁移的数据的表。

    ![选择要迁移的表](../dma/media/select-tables-to-migrate.png) 

5. 选择**开始数据迁移**。

最后一个屏幕显示总体状态。

   ![迁移状态](../dma/media/migration-status.png) 

## <a name="see-also"></a>请参阅

* [数据迁移助手 (DMA)](../dma/dma-overview.md)
* [数据迁移助手：配置设置](../dma/dma-configurationsettings.md)
* [数据迁移助手：最佳做法](../dma/dma-bestpractices.md)
