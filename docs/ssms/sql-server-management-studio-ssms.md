---
title: SQL Server Management Studio (SSMS)
description: 介绍什么是 SQL Server Management Studio (SSMS) 及其功能。
ms.prod: sql
ms.technology: ssms
ms.topic: overview
author: markingmyname
ms.author: maghan
ms.reviewer: ''
f1_keywords:
- sql13.ssms.viewhelp.f1
helpviewer_keywords:
- SQL Server Management Studio
- SQL Server Management Studio for Integration Services
- SQL Server Management Studio for Reporting Services
- SQL Server Management Studio for Analysis Services
ms.custom: ''
ms.date: 05/29/2019
ms.openlocfilehash: aba53145a679503fde2a965b818949885edaa92a
ms.sourcegitcommit: 5d839dc63a5abb65508dc498d0a95027d530afb6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2019
ms.locfileid: "67680746"
---
# <a name="what-is-sql-server-management-studio-ssms"></a>什么是 SQL Server Management Studio (SSMS)？ 

[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md.md](../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) 是用于管理任何 SQL 基础结构的集成环境。 使用 SSMS，可以访问、配置、管理和开发 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]、Azure SQL 数据库和 SQL 数据仓库的所有组件。 SSMS 在一个综合实用工具中汇集了大量图形工具和丰富的脚本编辑器，为各种技能水平的开发者和数据库管理员提供对 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的访问权限。

- [**下载 SQL Server Management Studio (SSMS)** ](download-sql-server-management-studio-ssms.md)
- [**下载 SQL Server Developer**](https://my.visualstudio.com/Downloads?q=SQL%20Server%20Developer)
- [**下载 Visual Studio**](https://www.visualstudio.com/downloads/)

## <a name="sql-server-management-studio-components"></a>SQL Server Management Studio 组件  
  
|描述|组件|  
|---------------|---------|  
|使用“对象资源管理器”  查看和管理 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的一个或多个实例中的所有对象。|[对象资源管理器](../ssms/object/object-explorer.md)|  
|如何使用“模板资源管理器”生成和管理用于加快查询和脚本开发速度的样板文本文件  。|[模板资源管理器](../ssms/template/template-explorer.md)|  
|如何使用不推荐使用的“解决方案资源管理器”  生成用于管理诸如脚本和查询等管理项的项目。|[解决方案资源管理器](../ssms/solution/solution-explorer.md)|  
|如何使用 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]中包含的可视化设计工具。|[Visual Database Tools](../ssms/visual-db-tools/visual-database-tools.md)|  
|如何使用 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 语言编辑器交互式生成和调试查询和脚本。|[查询和文本编辑器 (SQL Server Management Studio)](../relational-databases/scripting/query-and-text-editors-sql-server-management-studio.md)|

## <a name="sql-server-management-studio-for-business-intelligence"></a>适用于商业智能的 SQL Server Management Studio

若要访问、配置和管理 [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)]、[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 和 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]，请使用 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]。 虽然这三种商业智能技术均依赖于 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]，但与每种技术关联的管理任务却略有不同。
  
> [!NOTE]
> 若要创建和修改 [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)]、 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]和 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 解决方案，请使用 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)]，而不是 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]。 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)] 是一个基于 [!INCLUDE[msCoName](../includes/msconame_md.md)][!INCLUDE[vsprvs](../includes/vsprvs-md.md)]的开发环境。
  
### <a name="managing-analysis-services-solutions-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 管理 Analysis Services 解决方案

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 可以管理 [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] 对象，如执行备份和处理对象。
  
[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 提供一个 [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] 脚本项目，可在其中开发并保存使用多维表达式 (MDX)、数据挖掘扩展插件 (DMX) 和 XML for Analysis (XMLA) 编写的脚本。 可以使用 [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] 脚本项目在 [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] 实例中执行管理任务或重新创建对象（如数据库和多维数据集）。 例如，可以在 [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] 脚本项目中开发一个 XMLA 脚本，以直接在现有 [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] 实例中创建新的对象。 可以将 [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] 脚本项目保存为解决方案的一部分，以及与源代码管理集成在一起。
  
有关如何使用 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]的详细信息，请参阅 [使用 SQL Server Management Studio 进行开发和实现](../analysis-services/instances/analysis-services-scripts-project-in-sql-server-management-studio.md)
  
### <a name="managing-integration-services-solutions-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 管理 Integration Services 解决方案

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 可以使用 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 服务管理包并监视正在运行的包。 还可以使用 [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] 将包组织到文件夹中、运行包、导入和导出包、迁移 Data Transformation Services (DTS) 包以及升级 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 包。

### <a name="managing-reporting-services-projects-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 管理 Reporting Services 项目

可以使用 SQL Server Management Studio 启用 Reporting Services 功能、管理服务器和数据库以及管理角色和作业。

可通过使用“共享计划”文件夹来管理共享计划以及管理报表服务器数据库（ReportServer、ReportServerTempdb）。 在将报表服务器数据库移动到新的或不同的 SQL Server 数据库引擎（[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../includes/ssde_md.md)]）时，还可以在 Master 系统数据库中创建 RSExecRole。 有关这些任务的详细信息，请参阅以下主题：  

- [Management Studio 操作指南主题](https://msdn.microsoft.com/60685458-9108-47bf-820a-5e7db454d408)
- [管理报表服务器数据库](../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md)
- [如何：创建 RSExecRole](../reporting-services/security/create-the-rsexecrole.md)
  
也可以通过以下方法来管理服务器：启用并配置各种功能、设置服务器默认设置以及管理角色和作业。 有关这些任务的详细信息，请参阅以下主题：

- [如何：设置报表服务器属性 (Management Studio)](https://msdn.microsoft.com/1ed0f84b-b12a-4e49-b65c-a11a99f9093f)
- [如何：创建、删除或修改角色 (Management Studio)](https://msdn.microsoft.com/3d1d56e6-a283-44a7-8417-36cb4d2c74d1)
- [启用和禁用 Reporting Services 的客户端打印](https://msdn.microsoft.com/0e709c96-7517-4547-8ef6-5632f8118524)

## <a name="non-english-language-versions-of-sql-server-management-studio-ssms"></a>非英语语言版本的 SQL Server Management Studio (SSMS)

已解除阻止混合语言设置。 可以在法语版 Windows 上安装 SSMS 德语版。 如果 OS 语言与 SSMS 语言不匹配，则用户需要在“工具”>“选项”>“国际设置”下更改语言。 否则，SSMS 显示英语版 UI。

有关早期版本的不同区域设置的详细信息，请参阅[安装非英语语言版本的 SSMS](install-other-languages.md)。

## <a name="support-policy-for-ssms"></a>SSMS 的支持策略

- 从 SSMS 17.0 开始，SQL 工具团队采用 [Microsoft 新式生命周期策略](https://support.microsoft.com/help/30881/modern-lifecycle-policy)。
- 阅读原版[新式生命周期策略公告](https://support.microsoft.com/help/447912/announcing-microsoft-modern-lifecycle-policy)。
- 有关详细信息，请参阅[新式策略常见问题解答](https://support.microsoft.com/help/30882/modern-lifecycle-policy-faq)。

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

## <a name="next-steps"></a>后续步骤

- [安装非英语语言版本的 SQL Server Management Studio (SSMS)](install-other-languages.md)
- [教程：SQL Server Management Studio](tutorials/tutorial-sql-server-management-studio.md)
- [教程：编写 Transact-SQL 语句](https://msdn.microsoft.com/2addc9be-67d0-423d-a457-192fe9d7d058)
- [使用 SQL Server Data Tools 进行开发和实现](../analysis-services/multidimensional-models/creating-multidimensional-models-using-sql-server-data-tools-ssdt.md)
- [SQL Server Data Tools 中的 Reporting Services](../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md)
- [教程：SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/tutorials/tutorial-sql-server-management-studio)

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]