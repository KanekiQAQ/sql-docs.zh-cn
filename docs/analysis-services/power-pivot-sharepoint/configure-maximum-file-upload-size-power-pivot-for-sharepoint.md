---
title: 配置最大文件上载大小 (Powerpivot for SharePoint) |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 15ce366acc4244db16f0b58bbdc8226e9327416c
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68164276"
---
# <a name="configure-maximum-file-upload-size-power-pivot-for-sharepoint"></a>配置最大文件上载大小 (Power Pivot for SharePoint)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 工作簿常常包含大量的数据，导致文件超出为 SharePoint 上载所允许的最大文件大小。 尝试上载超过该上限的文件时，SharePoint 会显示以下错误：  
  
-   “指定的文件超过受支持的最大文件大小。”  
  
 若要增加文件大小，可从将针对 Excel Services 的“工作簿最大大小”调整到 50 MB 开始。 这会将针对 Excel 的最大文件大小限制调整为针对 SharePoint Web 应用程序的最大文件大小限制（SharePoint 2010 中的默认设置为 50 MB，SharePoint 2013 中为 200 MB）。 如果您的文件超过 50 MB 的大小限制，则应同时增加针对 Excel Services 和您的 Web 应用程序的文件大小限制。  
  
 您必须是 SharePoint 管理员才能更改最大文件上载大小。  
  
### <a name="configure-maximum-file-size-for-excel-services"></a>配置针对 Excel Services 的最大文件大小  
  
1.  在“管理中心”的“应用程序管理”中，单击 **“管理服务应用程序”** 。  
  
2.  单击 Excel Services 应用程序的名称。  
  
3.  单击 **“受信任文件位置”** 。  
  
4.  单击该位置以编辑属性。 默认情况下，Excel Services 将默认 Web 应用程序视作可信站点。 如果你在使用默认 Web 应用程序，则单击 **http://** 以便打开针对此位置的配置页。  
  
5.  滚动到 **“工作簿属性”** 。  
  
6.  在“工作簿最大大小”中，将文件大小从 10（默认值）增加到 50 或者适合您在使用的文件的更大大小。  
  
     默认情况下，50 MB 是针对 SharePoint Web 应用程序的最大文件上载大小。 如果您将“工作簿最大大小”设置为大于 50 MB 的数值，则应执行下面过程中的步骤以便将针对 SharePoint Web 应用程序的“最大上载大小”增加到相同值。  
  
     您可以指定的最大值是 2 GB（或“管理中心”中指定的 2047 MB）。  
  
7.  单击 **“确定”** 。  
  
### <a name="configure-maximum-file-size-for-a-sharepoint-web-application"></a>为 SharePoint Web 应用程序配置最大文件大小  
  
1.  在“管理中心”的“应用程序管理”中，单击 **“管理 Web 应用程序”** 。  
  
    > [!NOTE]  
    >  仅当在 Excel Services 中增加过“工作簿最大大小”时，才执行以下步骤。  
  
2.  选择应用程序（例如 **SharePoint - 80**）。  
  
3.  在“Web 应用程序”功能区上，单击“常规设置”按钮上的向下箭头。  
  
4.  单击 **“常规设置”** 。  
  
5.  滚动到 **“最大上载大小”** 。  
  
6.  将该属性设置为与 Excel Services 中的“工作簿最大大小”相同或更大的数值。  
  
7.  单击 **“确定”** 。  
  
  
