---
title: 验证 Power Pivot for SharePoint 安装 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f047593657806b872aafdda802c9c85ac4526b56
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68209467"
---
# <a name="verify-a-power-pivot-for-sharepoint-installation"></a>验证 Power Pivot for SharePoint 安装
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  在 SharePoint 场中安装的 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] for SharePoint 实例可通过 SharePoint 管理中心进行管理。 至少，你可以检查管理中心和 SharePoint 网站上的页面以便确认 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 服务器组件和功能可用。 但是，若要完全验证某一安装，你必须具有可发布到 SharePoint 并从库中访问的 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 工作簿。 出于测试目的，你可以发布已包含 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 数据的示例工作簿并使用它来确认 SharePoint 集成已正确配置。  

  
##  <a name="verifyinstall"></a> 验证管理中心集成  
 若要验证 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] integration 与管理中心的集成，请执行以下操作：  
  
1.  在“开始”菜单上，单击“所有程序”  ，打开“Microsoft SharePoint 2016 产品”或“Microsoft SharePoint 2013 产品”，单击“SharePoint 2016 Central Administration”或“SharePoint 2013 Central Administration”  。  
  
2.  输入您的用户名和密码，然后单击 **“确定”** 。  
  
     或者，您可以修改浏览器设置，以便避免每次打开管理中心时都不得不输入用户名和密码。 若要将管理中心作为可信站点添加，请执行以下操作。  
  
    1.  在 Internet Explorer 中的“工具”菜单上，单击 **“Internet 选项”** 。  
  
    2.  在“安全”选项卡中，在 **“选择要查看或更改安全设置的区域”** 部分中，单击“受信任的站点”，然后单击“站点”。  
  
    3.  取消选中“对该区域中的所有站点要求服务器验证(https:)”  复选框。  
  
    4.  在 **“将该网站添加到区域中”** 中，键入您的网站的 URL，然后单击 **“添加”** 。  
  
    5.  单击 **“关闭”** ，然后单击 **“确定”** 。  
  
        > [!NOTE]  
        >  SharePoint 安装文档包含有关解决代理服务器错误和禁用 Internet Explorer 安全增强配置的附加说明，以便您可以下载和安装更新。 有关详细信息，请参阅 Microsoft 网站上 **Deploy a single server with SQL Server** （部署单台带有 SQL Server 的服务器）中的 [Perform additional tasks](http://go.microsoft.com/fwlink/?LinkId=177754) “执行附加任务”部分。  
  
3.  在管理中心的“系统设置”中，单击 **“管理场功能”** 。  
  
4.  验证 **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 集成功能**状态为“活动”  。  
  
5.  在管理中心的“系统设置”中，单击 **“管理服务器上的服务”** 。  
  
6.  确认“SQL Server [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 系统服务”  已启动。  
  
     在多服务器 SharePoint 场中，你可能需要更改正在查看的服务器以验证部署到 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 的所有服务器都在运行。  
  
7.  在“管理中心”的“应用程序管理”中，单击 **“管理服务应用程序”** 。  
  
8.  单击**默认[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]服务应用程序**以打开[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]为此应用程序管理仪表板。 第一次使用时，面板要花几分钟的加载时间。  
  
     或者，单击空白区域旁边**默认[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]服务应用程序**以选择该行，然后单击**属性**若要查看此服务应用程序的配置设置。 您可以修改配置设置和应用程序属性以更改服务器配置。 有关详细信息，请参阅 [在管理中心中创建和配置 PowerPivot 服务应用程序](../../../analysis-services/power-pivot-sharepoint/create-and-configure-power-pivot-service-application-in-ca.md)。  
  
## <a name="verify-integration-at-the-site-level"></a>验证网站级别的集成  
 若要验证 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 与 SharePoint 网站的集成，请执行以下操作：  
  
1.  在浏览器中，打开您创建的 Web 应用程序。 如果你使用默认值，则可以指定 http://\<您的计算机名称 > 中的 URL 地址。  
  
2.  验证 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 数据访问和处理功能在应用程序中是否可用。 你可以通过验证 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 提供的库模板是否存在来验证此可用性：  
  
    1.  选择“网站内容”  。  
  
    2.  在应用列表中，你应当可以看到“数据馈送库”  和“[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 库”  。 这些库模板由 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 功能提供，并且在正确集成了该功能的情况下在“库”列表中将可见。  
  
## <a name="verify-data-access-on-the-server"></a>验证服务器上的数据访问。  
 若要验证服务器上的 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 数据访问，请执行以下操作：  
  
1.  [下载](http://go.microsoft.com/fwlink/?LinkID=219108) Reporting Services 教程附带的“野餐”数据示例。 你将使用此下载中的示例工作簿来验证 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 数据访问。 提取文件。  
  
2.  将 Excel 工作簿 (.xlsx) 上载到“共享文档”。 该工作簿包含嵌入的 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 数据。  
  
3.  单击该文档以便从库中打开它。  
  
4.  在该工作簿顶部单击某个切片器或筛选器。 月份、颜色和类型是此工作簿中的切片器。 单击切片器可以启动 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 查询并证明你的服务器可以正常运行。 该服务器将在后台加载 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 数据并返回结果。  
  
5.  返回到库。 选择该工作簿右侧的向下箭头，然后单击 **“启动 Power View”** 。 此步骤确认 Reporting Services 中的 [!INCLUDE[ssCrescent](../../../includes/sscrescent-md.md)] 功能可以正常工作。 如果您未安装 Reporting Services，请跳过此步骤。  
  
     在下一步骤中，您将在 Management Studio 中连接到该服务器以确认数据已加载并且缓存。  
  
6.  从“开始”菜单中的 [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)] 程序组启动 SQL Server Management Studio。 如果未在您的服务器上安装此工具，则可以跳到最后一步以便确认缓存文件是否存在。  
  
7.  在“服务器类型”中，选择 **“Analysis Services”** 。  
  
8.  在服务器名称，输入 **\<服务器名称 > \powerpivot**，其中 **\<服务器名称 >** 的计算机的名称[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)]for SharePoint安装。  
  
9. 单击 **“连接”** 。 这将验证 Analysis Services 服务器是否可用。  
  
10. 在对象资源管理器中，可以单击“数据库”  查看加载的 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 数据文件的列表。  
  
11. 在计算机文件系统上，检查以下文件夹以便确定文件是否已缓存到磁盘。 存在缓存文件将进一步证实您的部署正常工作。 若要查看文件缓存，请转到 [!INCLUDE[ssInstallPathVar](../../../includes/ssinstallpathvar-md.md)]MSAS13.POWERPIVOT\OLAP\Backup\Sandboxes\Default [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 服务应用程序文件夹。 每个缓存数据库均存储在自己的文件夹中，可使用基于 GUID 的命名约定来确保唯一名称。  
  
  
