---
title: 将 MSOLAP.5 添加为 Excel Services 中的受信任的数据提供程序 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d7576aadda3739709acdffcb1b2419c20d39ed4e
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68164458"
---
# <a name="add-msolap5-as-a-trusted-data-provider-in-excel-services"></a>将 MSOLAP.5 添加为 Excel Services 中的受信任数据访问接口
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  MSOLAP.5 指代用于 SQL Server 2012 的 Analysis Services OLE DB 访问接口。 Excel Services 必须信任此访问接口，然后才能发出连接请求，请求在服务器上提供 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据。  
  
 如果使用 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 配置工具配置了 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint，MSOLAP.5 可能已是受信任的访问接口，因为该工具包含一项可满足此要求的操作。 但是，如果使用的是 PowerShell、管理中心，或在配置工具中排除了受信任的访问接口操作，该访问接口可能缺失，在这种情况下，你应该在配置用于 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据访问的场的过程中立即添加该访问接口。  
  
 对于每个 Excel Services 服务应用程序只需执行此步骤一次。  
  
 处理 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 数据请求的每个物理服务器（如 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 服务器或 Excel Services 服务器）必须在计算机上安装了 OLE DB 访问接口。 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 安装始终包含 OLE DB 访问接口，但是如果 Excel Services 运行在未安装 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 的计算机上，必须手动安装该访问接口。 有关详细信息，请参阅 [在 SharePoint 服务器上安装 Analysis Services OLE DB 提供程序](http://msdn.microsoft.com/2c62daf9-1f2d-4508-a497-af62360ee859)。  
  
## <a name="add-a-trusted-provider-to-excel-services"></a>将受信任的访问接口添加到 Excel Services  
  
1.  在“管理中心”中，单击 **“管理服务应用程序”** ，然后单击 Excel Services 服务应用程序。  
  
2.  单击 **“受信任的数据访问接口”** 。  
  
3.  验证 MSOLAP.5 显示在列表中。 根据你配置 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 的不同方式，MSOLAP.5 可能已受信任。  
  
4.  如果它未列出，请单击 **“添加受信任的数据访问接口”** 。  
  
5.  在“访问接口 ID”中，键入 **MSOLAP.5**。  
  
6.  对于“访问接口类型”，请确保选择 OLE DB。  
  
7.  在“访问接口说明”中，键入 **Microsoft OLE DB Provider for OLAP Services 11.0**。  
  
  
