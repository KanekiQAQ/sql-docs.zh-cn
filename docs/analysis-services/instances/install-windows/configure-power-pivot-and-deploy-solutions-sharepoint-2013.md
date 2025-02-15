---
title: 配置 Power Pivot 和部署解决方案 (SharePoint 2013) |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 079988eb037ebeffbbbe6cae053e241518e41c81
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "63054554"
---
# <a name="configure-power-pivot-and-deploy-solutions-sharepoint-2013"></a>配置 Power Pivot 和部署解决方案 (SharePoint 2013)
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  本主题介绍如何部署和配置 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 中 [!INCLUDE[SPS2013](../../../includes/sps2013-md.md)] 功能的中间层增强功能，包括 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 库、计划数据刷新、管理面板和数据提供程序。 运行 **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] for SharePoint 2013 配置** 工具以便完成以下任务：  
  
-   部署 SharePoint 解决方案文件。  
  
-   创建 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 服务应用程序。  
  
-   将 Excel Services 应用程序配置为在 SharePoint 模式下使用 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 服务器。 有关后端服务和在 SharePoint 模式下安装 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 服务器的信息，请参阅[在 Power Pivot 模式下安装 Analysis Services](../../../analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode.md)。  
  
 有关安装 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] for SharePoint 2013 配置工具的信息，请参阅[安装或卸载 Power Pivot for SharePoint 外接程序 (SharePoint 2013)](../../../analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013.md)  
  
##  <a name="bkmk_run_configuration_tool"></a> 运行 Power Pivot for SharePoint 2013 配置  
 **注意：** [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]安装向导将安装两种不同的配置工具来[!INCLUDE[ssGeminiLong](../../../includes/ssgeminilong-md.md)]。 这两个工具均支持不同版本的 SharePoint。  
  
|“属性”|Description|  
|----------|-----------------|  
|[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] for SharePoint 2013 配置|SharePoint 2013|  
|[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 配置工具|具有 SharePoint 2010 Service Pack 1 (SP1) 的 SharePoint 2010|  
  
 **注意：** 若要完成以下步骤，必须是场管理员。 如果您看到与以下内容类似的错误消息：  
  
-   "用户不是场管理员。 请解决验证问题并重试。”  
  
 以安装了 SharePoint 的帐户登录或将安装帐户配置为 SharePoint 管理中心网站的主管理员。  
  
1.  在“开始”  菜单中，单击“所有程序”  ，然后依次单击 [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)]、“配置工具”  和  “[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] For SharePoint 2013 配置”。 只有在本地服务器上安装了 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] for SharePoint 后，才会列出工具。  
  
2.  单击“配置或修复 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] for SharePoint”  ，然后单击“确定”  。  
  
3.  该工具将运行验证以验证 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 的当前状态和完成配置所需的步骤。 将窗口放大为实际大小。 您应该在该窗口的底部看到一个菜单栏，其中包含 **“验证”** 、 **“运行”** 和 **“退出”** 命令。  
  
4.  在 **“参数”** 选项卡上：  
  
    1.  **默认帐户用户名**:输入默认帐户的域用户帐户。 此帐户将用于设置服务，包括 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 服务应用程序池。 不要指定 Network Service 或 Local System 之类的内置帐户。 该工具将阻止指定内置帐户的配置。  
  
    2.  **数据库服务器**:可以使用 SQL Server 数据库引擎支持的 SharePoint 场。  
  
    3.  **通行短语**:输入的密码。 如果您在创建新的 SharePoint 场，则在您向该 SharePoint 场中添加新的服务器或应用程序时，将使用该通行短语。 如果该场已存在，则输入允许您向该场添加服务器应用程序的通行短语。  
  
    4.  **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] Server for Excel Services**:键入的名称[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]SharePoint 模式服务器。 在单服务器部署中，它与数据库服务器相同。 `[ServerName]\powerpivot`  
  
    5.  在左窗口中单击 **“创建网站集”** 。 请注意 **“网址 URL”** ，以便您可以在后面的步骤中引用它。 如果 SharePoint 服务器尚未配置，则配置向导默认使用 Web 应用程序，并且将网站集 URL 默认为 `http://[ServerName]`的根。 若要修改这些默认设置，请查看在左窗口中的以下页面：**创建默认的 Web 应用程序**和**部署 Web 应用程序解决方案**  
  
5.  或者，查看用于完成各操作的剩余输入值。 单击左窗口中的每个操作以查看操作的详细信息。 有关每个详细信息，请参阅部分"输入值用于配置中的服务器[配置或修复 Power Pivot for SharePoint 2010 （Power Pivot 配置工具）](http://msdn.microsoft.com/d61f49c5-efaa-4455-98f2-8c293fa50046)本主题中。  
  
6.  您还可以删除不想在此时处理的任何操作。 例如，如果您想要在以后配置 Secure Store Service，则单击 **“配置 Secure Store Service”** ，然后清除 **“在任务列表中包括此操作”** 复选框。  
  
7.  单击 **“验证”** 以便检查该工具是否有足够的信息来处理列表中的操作。 如果看到验证错误，请单击左窗格中的警告查看验证错误的详细信息。 更正任何验证错误，然后再次单击 **“验证”** 。  
  
8.  单击 **“运行”** 来处理该任务列表中的所有操作。 请注意， **“运行”** 将在您验证操作之后才可用。 如果 **“运行”** 未启用，请首先单击 **“验证”** 。  
  
 有关详细信息，请参阅 [配置或修复 Power Pivot for SharePoint 2010（Power Pivot 配置工具）](http://msdn.microsoft.com/d61f49c5-efaa-4455-98f2-8c293fa50046)  
  
##  <a name="bkmk_verify_powerpivot"></a> 验证 Power Pivot 配置  
 **服务：**  
  
1.  在管理中心的“系统设置”中，单击 **“管理服务器上的服务”** 。  
  
2.  验证  “SQL Server Analysis Services”和  “SQL Server [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 系统服务”是否已启动。  
  
 **场功能：**  
  
1.  在管理中心的“系统设置”中，单击 **“管理场功能”** 。  
  
2.  验证 **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 集成功能**状态为“活动”  。  
  
 **网站集功能：**  
  
1.  浏览到您通过使用配置工具创建的网站 URL。  
  
     单击**设置**![SharePoint 设置](../../../analysis-services/media/as-sharepoint2013-settings-gear.gif "SharePoint 设置")，然后单击**站点设置**。  
  
     单击 **“网站集功能”** 。  
  
2.  确认 **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 针对网站集的功能集成**状态为“活动”  。  
  
 **[!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 服务应用程序：**  
  
1.  在“管理中心”的 **“应用程序管理”** 中，单击 **“管理服务应用程序”** 。  
  
2.  确认服务应用程序状态为 **“已启动”** 。 默认名称为**默认的 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 服务应用程序”** 。  
  
     单击服务应用程序的名称，以便打开 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 管理面板以使服务应用程序打开。 第一次使用时，面板要花几分钟的加载时间。  
  
 有关详细信息，请参阅 [Verify a Power Pivot for SharePoint Installation](../../../analysis-services/instances/install-windows/verify-a-power-pivot-for-sharepoint-installation.md)。  
  
##  <a name="bkmk_troubleshoot_issues"></a> 解决问题  
 为了协助解决问题，最好验证诊断日志记录已启用。  
  
1.  在 SharePoint 管理中心中，单击 **“监视”** ，然后单击 **“配置使用情况和运行状况数据收集”** 。  
  
2.  确认选择了 **“启用使用情况数据收集”** 。  
  
3.  验证是否选择了以下事件：  
  
    -   针对教育遥测的使用情况字段的定义  
  
    -   [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 连接  
  
    -   [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 加载数据使用情况  
  
    -   [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 查询使用情况  
  
    -   [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] 卸载数据使用情况  
  
4.  确认选择了 **“启用运行状况数据收集”** 。  
  
5.  单击“确定”  。  
  
 有关故障排除数据刷新的详细信息，请参阅[故障排除 Power Pivot 数据刷新](http://social.technet.microsoft.com/wiki/contents/articles/3870.troubleshooting-powerpivot-data-refresh.aspx)(http://social.technet.microsoft.com/wiki/contents/articles/3870.troubleshooting-powerpivot-data-refresh.aspx) 。  
  
 有关配置工具的详细信息，请参阅 [Power Pivot Configuration Tools](../../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools.md)。  
  
  
