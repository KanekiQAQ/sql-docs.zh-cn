---
title: 在运行管理中心的 Web 前端服务器上安装 ADOMD.NET |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: c2372180-e847-4cdb-b267-4befac3faf7e
author: markingmyname
ms.author: maghan
manager: craigg
ms.openlocfilehash: 2d8a3c6a47a9f353f44a9ec9c4a0cbb2c3cd3505
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66094699"
---
# <a name="install-adomdnet-on-web-front-end-servers-running-central-administration"></a>在运行管理中心的 Web 前端服务器上安装 ADOMD.NET
  如果您将 PowerPivot for SharePoint 安装到具有管理中心拓扑，但没有 Excel Services 或 PowerPivot for SharePoint 的场中，并且希望对 PowerPivot 管理面板中的内置报表具有完全访问权限，则需下载和安装 Microsoft ADOMD.NET 客户端库。 该面板中的某些报表将使用 ADOMD.NET 来访问内部数据，内部数据可提供有关 PowerPivot 查询处理和场中服务器运行状况的报告数据。  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2010  
  
 对于 SharePoint 2013，访问接口包含在 SQL Server 功能包中。 有关如何下载 spPowerPivot.msi 的信息，请参阅 [Microsoft SQL Server 2014 功能包](https://www.microsoft.com/download/details.aspx?id=35577)  
  
### <a name="download-and-install-the-client-library"></a>下载并安装客户端库  
  
1.  在 [“SQL Server 2014 功能包”页](https://go.microsoft.com/fwlink/?LinkID=296473)上查找 Microsoft ADOMD.NET。  
  
2.  下载 `SQL_AS_ADOMD.msi` 安装程序的 x64 包。  
  
3.  运行 .msi 安装库。  
  
4.  安装完成后，重置 IIS。 打开管理命令提示符，然后键入 **IISRESET**。  
  
### <a name="verify-installation"></a>验证安装  
  
1.  转到 Windows\Assembly。  
  
2.  右键单击 Microsoft.AnalysisServices.AdomdClient 并选择 **“属性”** 。  
  
3.  单击 **“版本”** 。  
  
4.  验证版本包含 12.00。\<内部版本号 > 以及说明是否为 Microsoft.AnalysisService.AdomdClient。  
  
## <a name="see-also"></a>请参阅  
 [PowerPivot 管理仪表板和使用情况数据](../../analysis-services/power-pivot-sharepoint/power-pivot-management-dashboard-and-usage-data.md)  
  
  
