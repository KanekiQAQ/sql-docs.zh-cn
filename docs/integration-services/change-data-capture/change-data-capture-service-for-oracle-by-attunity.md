---
title: Change Data Capture Service for Oracle by Attunity | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 22ec8a5c-9550-4d38-8a4a-485ec3e53ea8
author: janinezhang
ms.author: janinez
ms.openlocfilehash: cac42c7e892f81e7182d075cb5ac251eb42fc921
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68099855"
---
# <a name="change-data-capture-service-for-oracle-by-attunity"></a>Change Data Capture Service for Oracle by Attunity

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  Oracle CDC 服务是一种 Windows 服务，该服务将扫描 Oracle 事务日志并将对有关 Oracle 表的更改捕获到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更改表中。 存储从 Oracle 捕获的更改的 SQL 更改表具有与本机 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 变更数据捕获功能使用的更改表相同的类型。 这使得使用这些更改就像使用对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库进行的更改一样简单。  
  
## <a name="installation"></a>安装  

Microsoft Change Data Capture Designer and Service for Oracle by Attunity for Microsoft SQL Server 2016 属于 SQL Server 2016 功能包的一部分。 从 [SQL Server 2016 功能包网页](https://go.microsoft.com/fwlink/?LinkId=746297)下载功能包的组件。
  
 Oracle CDC 服务可以安装在对要捕获的源 Oracle 数据库以及目标 CDC 数据库所在的目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例具有访问权限的任何支持的 Windows 计算机上。 CDC 服务不需要 Oracle 数据库或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的本地安装，只需要其支持的客户端。 有关安装所需数据库组件的位置的信息，请参阅本主题中的 **数据库必备组件** 。  
  
 用于 Oracle 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CDC 服务的安装会将服务配置 UI 和服务程序放置于所选位置中。 使用 Oracle CDC 服务配置控制台单独配置 Oracle CDC 服务。 有关配置 Oracle CDC 服务的详细信息，请参阅 [Change Data Capture Service for Oracle by Attunity F1 Help](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-f1-help.md)。  
  
 Oracle CDC 服务可以安装在安装了 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] Native Client 的任何支持的 Windows 计算机上；它无需安装在安装有目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的那一台计算机上。  
  
## <a name="supported-windows-environments"></a>支持的 Windows 环境  
 Change Data Capture Service for Oracle by Attunity 可在以下 Windows 环境中运行：  
  
-   Windows 8 和 8.1  
-   Windows 10  
-   Windows Server 2012 和 2012 R2
-   Windows Server 2016
  
## <a name="database-prerequisites"></a>数据库必备组件  
 若要使用 Oracle CDC 服务，您必须安装 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] Native Client Oracle 软件。 这是应从 Oracle 获取并在安装 Oracle CDC 服务之前安装的必备组件。 此外，您需要使用 SQL Server 安装程序安装 SQL Server ODBC 客户端。  
  
 Oracle CDC 服务支持以下版本：  
  
### <a name="source-oracle-database"></a>源 Oracle 数据库  
  
-   Oracle 数据库 10g 发行版 2
-   Oracle 数据库 11g 发行版 1 和发行版 2
-   经典安装的 Oracle Database 12c。 （不支持多租户安装。）  
  
### <a name="target-sql-server-database"></a>目标 SQL Server 数据库  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
## <a name="running-the-installation-program"></a>运行安装程序  
 若要安装 Oracle CDC 服务，请打开针对您正使用的 Windows 平台（32/64 位）的安装向导，并且按照屏幕上的指示执行。  
  
## <a name="uninstalling-change-data-capture-service-for-oracle-by-attunity"></a>卸载 Change Data Capture Service for Oracle by Attunity  
 可使用“控制面板”的“程序和功能”卸载 CDC CDC 服务。  
  
 卸载该 CDC 服务不会删除已创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。 若要完全删除该工具，您必须删除已在您使用的目标 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中创建的 MSXDBCDC 数据库和特定的 CDC 数据库。  
  
 如果您从一台计算机卸载该 CDC 服务软件并且将其安装在另一台计算机上，则仅需提供以下定义：  
  
-   服务帐户  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接字符串和凭据  
  
-   主密码  
  
 所有其他定义均存储于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中并且可从其他计算机上之前的安装获取。  
  
## <a name="in-this-documentation"></a>本文档内容  
  
-   [Change Data Capture Service for Oracle by Attunity 系统体系结构](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-system-architecture.md)  
  
-   [Oracle CDC 服务](../../integration-services/change-data-capture/the-oracle-cdc-service.md)  
  
-   [Change Data Capture Service for Oracle by Attunity F1 帮助](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-f1-help.md)  
  
-   [Change Data Capture Service for Oracle by Attunity 操作指南](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-how-to-guide.md)  
  
## <a name="see-also"></a>另请参阅  
 [使用 Oracle CDC 服务](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md)  
  
  
