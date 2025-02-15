---
title: 在 SharePoint 2010 场中使用 SQL Server BI 功能的指南 |Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: 5f9a94c4-854b-4577-a8b1-7142f19904e3
author: markingmyname
ms.author: maghan
manager: craigg
ms.openlocfilehash: ae552c12c3d4773d6a05a6d61c7644eb245b68ed
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66094995"
---
# <a name="guidance-for-using-sql-server-bi-features-in-a-sharepoint-2010-farm"></a>在 SharePoint 2010 场中使用 SQL Server BI 功能的指南
  本主题总结了基于您所使用的软件版本的功能可用性。 它还介绍了使用特定 SQL Server 功能时对 SharePoint 2010 的安装要求。 有关 SharePoint 2013 的信息，请参阅[在 SharePoint 中的 SQL Server BI 功能的部署拓扑](deployment-topologies-for-sql-server-bi-features-in-sharepoint.md)。  
  
 本主题内容：  
  
-   [常规 SharePoint 2010 要求](#bkmk_generalsharepoint)  
  
-   [SharePoint 2010 Service Pack 1 (SP1)](#bkmk_sp1)  
  
-   [SharePoint 版本和 BI 功能的支持](#bkmk_vers)  
  
-   [SharePoint 2010 产品准备工具](#bkmk_prereq)  
  
-   [要求和建议用于运行 SharePoint 安装](#bkmk_install)  
  
##  <a name="bkmk_generalsharepoint"></a> 常规 SharePoint 2010 要求  
  
-   SharePoint 2010 产品仅限 64 位。 如果您以前在 SharePoint 集成模式下安装的 SharePoint 和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 属于 32 位版本，则无法升级到 SharePoint 2010。 有关详细信息，请参阅 SharePoint 文档。  
  
-   Reporting Services 包括用于 SharePoint 产品的外接程序。 这里对该外接程序和报表服务器受支持的配置的说明还不够详细。 有关详细信息，请参阅[支持组合的 SharePoint 和 Reporting Services 服务器与外接程序&#40;SQL Server 2014&#41;](../../reporting-services/install-windows/supported-combinations-of-sharepoint-and-reporting-services-server.md)。  
  
-   SharePoint 开发人员工具仅支持 SharePoint 独立配置。  有关详细信息，请参阅 SharePoint 文档：[开发 SharePoint 解决方案的需求](https://msdn.microsoft.com/library/ee231582.aspx)。  
  
##  <a name="bkmk_vers"></a> SharePoint 版本和 BI 功能的支持  
 仅在特定的 SharePoint 产品版本上支持某些 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 商业智能功能。  
  
|支持的功能|SharePoint 产品|  
|------------------------|------------------------|  
|[!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]一项功能[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]外接程序[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[SPS2010](../../includes/sps2010-md.md)] Enterprise Edition。<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据警报。<br /><br /> [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 的用户。|[!INCLUDE[SPS2010](../../includes/sps2010-md.md)] Enterprise Edition。|  
|与 SharePoint 集成的一般 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表查看和功能。|[!INCLUDE[SPS2010](../../includes/sps2010-md.md)] Standard Edition 和 Enterprise Edition。<br /><br /> [!INCLUDE[SPF2010](../../includes/spf2010-md.md)] 的用户。|  
  
 有关详细信息，请参阅[SQL Server 2012 各个版本支持的功能](https://go.microsoft.com/fwlink/?linkid=232473)。  
  
##  <a name="bkmk_sp1"></a> SharePoint 2010 Service Pack 1 (SP1)  
 建议您将 SharePoint 2010 安装更新为 SharePoint 2010 Service Pack 1 (SP1)。 在以下情况下要求 SharePoint SP1：  
  
-   您要使用 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 版的数据库引擎来用于 SharePoint 内容数据库或 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 目录数据库。  
  
-   您要使用 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 和 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具。  
  
 SP1 的主要原因之一是使用运行 SharePoint 安装需要[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]是数据库引擎功能**sp_dboption**，在以前的版本中已弃用，请在中将不再[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]版本。 有关详细信息，请参阅[废止的数据库引擎功能在 SQL Server 2014](../../database-engine/discontinued-database-engine-functionality-in-sql-server-2016.md)  
  
### <a name="sharepoint-2010-sp1-installation-guidance"></a>SharePoint 2010 SP1 安装指南  
 [下载 SharePoint Server 2010 SP1](https://go.microsoft.com/fwlink/?LinkID=219697)并将它应用于场中的所有服务器。  
  
> [!NOTE]  
>  在现有场上，您将需要使用以下值之一**其他**步骤以完成 SharePoint SP1 升级。 有关详细信息，请参阅[时安装 Office 2010 SP1 和 SharePoint 2010 SP1 的已知问题](https://support.microsoft.com/kb/2532126)并[SharePoint Server 2010 SP1 说明](https://support.microsoft.com/kb/2460045):  
  
-   **SharePoint 产品配置向导：** 运行向导以完成 SP1 升级和配置。  
  
-   **完成使用 psconfig 升级：** 运行命令`psconfig -upgrade`完成 SP1 升级  
  
 有关详细信息，请参阅的"升级"部分[(SharePoint Server 2010)](https://technet.microsoft.com/library/cc263093.aspx)和[资源中心：用于 SharePoint 2010 产品的更新](https://technet.microsoft.com/sharepoint/ff800847.aspx)  
  
## <a name="sharepoint-installation-with-sql-server-bi-features"></a>SharePoint 安装与 SQL Server BI 功能  
  
###  <a name="bkmk_prereq"></a> SharePoint 2010 产品准备工具  
 SharePoint 产品准备工具会启用操作系统中的服务器角色并安装 SharePoint 安装所要求的其他必备软件。 下表确定了为 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 配置服务器的附加步骤。  
  
|组件|操作|  
|---------------|------------|  
|Reporting Services 外接程序|SharePoint 2010 产品准备工具安装 Reporting Services 外接程序的 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 版本。 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 包括 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 功能所需的外接程序的更高版本。 可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导安装该外接程序，也可以从 MSDN 下载它。 获取当前版本的外接程序的位置以及如何安装它的详细信息，请参阅[在哪里寻找用于 SharePoint 产品的 Reporting Services 外接](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)和[安装或卸载 Reporting Services用于 SharePoint 外接程序&#40;SharePoint 2010 和 SharePoint 2013&#41;](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)。|  
|Analysis Services OLE DB 访问接口 (MSOLAP)|SharePoint 2010 会在 Excel Services 部署过程中安装 SQL Server 2008 版本的 OLE DB 访问接口。 此版本不支持 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据访问。 应该在支持 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据连接的 SharePoint 服务器上安装该访问接口的较新版本。 有关详细信息，请参阅[SharePoint 服务器上安装 Analysis Services OLE DB 访问接口](../../../2014/sql-server/install/install-the-analysis-services-ole-db-provider-on-sharepoint-servers.md)|  
|ADO.NET Services|SharePoint 2010 在必备组件列表中列出了 ADO.NET Services，但必备组件安装程序不会安装它。 若要添加 ADO.NET Services，必须手动安装它。 如果想以数据馈送的形式将 SharePoint 列表导入到 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿或 Reporting Services 报表中，则必须安装 ADO.NET Services。 有关说明，请参阅[安装 ADO.NET Data Services 以支持数据馈送导出 SharePoint 列表的](../../../2014/sql-server/install/install-ado-net-data-services-to-support-data-feed-exports-of-sharepoint-lists.md)。|  
  
###  <a name="bkmk_install"></a> 要求和建议用于运行 SharePoint 安装  
 您安装哪些 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 功能以及这些功能的安装顺序将决定可能与 SharePoint 集成到哪些级别。 例如，当某个级别的功能集成在使用内置数据库的 SharePoint 服务器上可通过 Reporting Services 使用时，大多数功能集成方案都需要 SharePoint 场安装，因为只有场安装会提供某些 BI 功能所需要的基础结构。  
  
 SharePoint 场可能是单个服务器，也可能是多个服务器。 正因为存在 Claims to Windows Token Service 这类附加基础结构，才使场与独立安装分离。  
  
 当你选择时，将启用场**服务器场**SharePoint 安装程序中的选项。 如果想使用 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 或 [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]，则必须具有场安装。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 开发环境都支持独立 SharePoint 安装。 不过，对于生产环境，推荐采用 SharePoint 场安装。  
  
 ![GMNI_SetupUI_SharePoint2010InstallType](../../../2014/sql-server/install/media/gmni-setupui-sharepoint2010installtype.gif "GMNI_SetupUI_SharePoint2010InstallType")  
  
 对于 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 或 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 共享服务，则还必须采用完整的服务器安装。  
  
 ![GMNI_SetupUI_SharePoint2010ServerType](../../../2014/sql-server/install/media/gmni-setupui-sharepoint2010servertype.gif "GMNI_SetupUI_SharePoint2010ServerType")  
  
 安装向导的最后一页提示您配置场，另外也可选择配置默认 Web 应用程序和根网站集。 大多数安装用户都会按照此安装路径进行安装。 如果您由于某种原因而未配置场，则可使用 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具中的场配置选项来配置场。  
  
 在早期版本中，不建议配置场来安装 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]，因为它在使用 SQL Server 安装选项以随时可用状态安装 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 时会缩减一些需要的步骤。 在此版本中，这已不再重要。 您可以使用 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具优化现有的场，以便使用为 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 安装推荐的设置。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 可安装到现有的 SharePoint 场中。 您可以按任何顺序安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 或 SharePoint，但 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的配置将因该顺序而异。 有关详细信息，请参阅[向场中添加另一个报表服务器&#40;SSRS 横向扩展&#41;](../../reporting-services/install-windows/add-an-additional-report-server-to-a-farm-ssrs-scale-out.md)并[安装 Reporting Services SharePoint 模式适用于 SharePoint 2010](../../../2014/sql-server/install/install-reporting-services-sharepoint-mode-for-sharepoint-2010.md)  
  
 ![GMNI_SetupUI_DoNotConfigureMOSS](../../../2014/sql-server/install/media/gmni-setupui-donotconfiguremoss.gif "GMNI_SetupUI_DoNotConfigureMOSS")  
  
## <a name="see-also"></a>请参阅  
 [安装和部署 SharePoint Server 2010](https://technet.microsoft.com/sharepoint/ee518643.aspx)   
 [三层场 (SharePoint Server 2010) 的多个服务器](https://go.microsoft.com/fwlink/?linkID=219834)  
  
  
