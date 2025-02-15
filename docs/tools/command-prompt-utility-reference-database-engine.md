---
title: SQL 命令提示实用工具 (数据库引擎) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server]
- command prompt utilities [SQL Server], about command prompt utilities
- command prompt [SQL Server]
- utilities [SQL Server], command prompt
- command prompt [SQL Server], utilities
ms.assetid: 48364bd9-6ea7-45e9-a332-acf3d81bbfae
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017'
ms.openlocfilehash: 8fdcb3b307f525ff9eda14006e6fd4f6378f418c
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68010418"
---
# <a name="sql-command-prompt-utilities-database-engine"></a>SQL 命令提示实用工具 (数据库引擎)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  使用命令提示实用工具，可以编写 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 操作的脚本。 下表包含了随 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供的多个命令提示实用工具列表。  

有关*主要*的 SQL gui 和命令行工具的信息, 请参阅[sql 工具概述](overview-sql-tools.md)。

  
|**实用工具**|**Description**|**安装位置**|  
|-----------------|---------------------|----------------------|  
|[bcp 实用工具](../tools/bcp-utility.md)|用于在 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例和用户指定格式的数据文件之间复制数据。|\<驱动器:>\Program Files\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn |  
|[dta 实用工具](../tools/dta/dta-utility.md)|用于分析工作负荷并建议物理设计结构，以优化该工作负荷下的服务器性能。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[dtexec 实用工具](../integration-services/packages/dtexec-utility.md)|用于配置和执行 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包。 该命令提示实用工具的用户界面版本称为 **DTExecUI**，它可提供执行包实用工具。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[dtutil 实用工具](../integration-services/dtutil-utility.md)|用于管理 SSIS 包。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[使用部署实用工具部署模型解决方案](../analysis-services/multidimensional-models/deploy-model-solutions-with-the-deployment-utility.md)|用于将 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] 项目部署到 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]实例。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VShell\Common7\IDE|   
|[osql 实用工具](../tools/osql-utility.md)|您可以在命令提示符下输入 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句、系统过程和脚本文件。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Profiler 实用工具](../tools/profiler-utility.md)|用于在命令提示符下启动 [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] 。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[RS.exe 实用工具 (SSRS)](../reporting-services/tools/rs-exe-utility-ssrs.md)|用于运行专门管理 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 报表服务器的脚本。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[rsconfig 实用工具 (SSRS)](../reporting-services/tools/rsconfig-utility-ssrs.md)|用于配置报表服务器连接。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[rskeymgmt 实用工具 (SSRS)](../reporting-services/tools/rskeymgmt-utility-ssrs.md)|用于管理报表服务器上的加密密钥。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlagent90 应用程序](../tools/sqlagent90-application.md)|用于在命令提示符下启动 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理。|\<驱动器>:\Program Files\Microsoft SQL Server\\<*instance_name*>\MSSQL\Binn|  
|[sqlcmd Utility](../tools/sqlcmd-utility.md)|您可以在命令提示符下输入 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句、系统过程和脚本文件。|\<驱动器:>\Program Files\\[!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]\Client SDK\ODBC\110\Tools\Binn |  
|[SQLdiag 实用工具](../tools/sqldiag-utility.md)|用于为 [!INCLUDE[msCoName](../includes/msconame-md.md)] 客户服务和支持部门收集诊断信息。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqllogship 应用程序](../tools/sqllogship-application.md)|应用程序可用其执行日志传送配置中的备份、复制和还原操作以及相关的清除任务，而无需运行备份、复制和还原作业。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[SqlLocalDB 实用工具](../tools/sqllocaldb-utility.md)|针对程序开发人员的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的执行模式。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\|  
|[sqlmaint 实用工具](../tools/sqlmaint-utility.md)|用于执行在早期版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]中创建的数据库维护计划。|\<驱动器>:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[sqlps 实用工具](../tools/sqlps-utility.md)|用于运行 PowerShell 命令和脚本。 加载和注册 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell 提供程序和 cmdlet。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlservr Application](../tools/sqlservr-application.md)|用于在命令提示符下启动和停止 [!INCLUDE[ssDE](../includes/ssde-md.md)] 实例以进行故障排除。|\<驱动器>:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn|  
|[Ssms 实用工具](../tools/sql-server-management-studio/ssms-utility.md)|用于在命令提示符下启动 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]Tools\Binn\VSShell\Common7\IDE|  
|[tablediff 实用工具](../tools/tablediff-utility.md)|用于比较两个表中的数据以查看数据是否无法收敛，这对于排除复制拓扑故障很有用。|[!INCLUDE[ssInstallPathVar](../includes/ssinstallpathvar-md.md)]COM|  

## <a name="command-prompt-utilities-syntax-conventions"></a>命令提示实用工具语法约定  
  
|**约定**|**用于**|  
|--------------------|------------------|  
|大写|在操作系统层使用的语句和术语。|  
|`monospace`|示例命令和程序代码。|  
|*斜体*|用户提供的参数。|  
|**粗体**|命令、参数和其他必须严格按所给形式键入的语法。|  
  
## <a name="see-also"></a>另请参阅  
 [复制分发代理](../relational-databases/replication/agents/replication-distribution-agent.md)   
 [复制日志读取器代理](../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [复制合并代理](../relational-databases/replication/agents/replication-merge-agent.md)   
 [复制队列读取器代理](../relational-databases/replication/agents/replication-queue-reader-agent.md)   
 [复制快照代理](../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
  
