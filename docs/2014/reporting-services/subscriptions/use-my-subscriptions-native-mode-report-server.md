---
title: 使用我的订阅 |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [Reporting Services], My Subscriptions page
- My Subscriptions page [Reporting Services]
ms.assetid: e96623ba-677e-4748-8787-f32bed3b5c12
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: ddd8eba477d0426fc0bf8eaeac9dcd2c1ec60b78
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66100621"
---
# <a name="use-my-subscriptions"></a>使用我的订阅
  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 报表管理器包含**我的订阅**组织您的所有订阅在一个位置的页。 您可以使用“我的订阅”来查看、修改和删除现有订阅。 不过，您不能使用该页来创建订阅。  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 本机模式|  
  
 在“我的订阅”中，您可以按文件夹、报表、说明、触发器、上次运行时间或状态来对订阅进行排序。 除了“上次运行时间”是按时间顺序排序之外，其他所有值都是按字母顺序排序的。  
  
 “我的订阅”仅显示您创建的订阅。 它不会列出其他用户拥有的订阅（即使您已添加为这些订阅的订阅者），也不会显示数据驱动订阅。  
  
 您既不能根据名称来搜索订阅，也不能根据触发器信息、状态信息等来搜索订阅。 有关详细信息，请参阅[创建、 修改和删除标准订阅&#40;本机模式下的 Reporting Services&#41;](create-and-manage-subscriptions-for-native-mode-report-servers.md)。  
  
## <a name="how-to-use-my-subscriptions"></a>如何使用“我的订阅”  
 可以通过报表管理器使用“我的订阅”。 若要访问“我的订阅”，请在报表管理器全局工具栏上单击 **“我的订阅”** 。  
  
## <a name="use-windows-powershell-to-list-mysubscriptions"></a>使用 Windows PowerShell 列出 MySubscription  
 ![与 PowerShell 相关的内容](../media/rs-powershellicon.jpg "PowerShell related content")  
  
 以下 PowerShell 脚本将返回当前用户的订阅和订阅属性的列表。 有关详细信息，请参阅 [ReportingService2010.ListMySubscriptions 方法](https://technet.microsoft.com/library/reportservice2010.reportingservice2010.listmysubscriptions.aspx)。  
  
```  
#server -  all subscriptions of the current user at the given server or site  
$server="[server name]/reportserver"  
$rs2010 = New-WebServiceProxy -Uri "http://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
  
$subscriptions=ListMySubscriptions(ItemPathOrSiteURL)  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted,Status  
#uncomment the following to list all the subscription properties  
#$subscriptions  
  
```  
  
## <a name="see-also"></a>请参阅  
 [数据驱动订阅](data-driven-subscriptions.md)   
 [订阅和传递 (Reporting Services)](subscriptions-and-delivery-reporting-services.md)   
 [创建和管理本机模式报表服务器的订阅](../create-manage-subscriptions-native-mode-report-servers.md)  
  
  
