---
title: 访问 Reporting Services WMI 提供程序 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: reporting-services-native
ms.topic: conceptual
api_name:
- Reporting Services WMI Provider
api_location:
- reportingservices.mof
topic_type:
- apiref
helpviewer_keywords:
- WMI provider [Reporting Services]
- programming [Reporting Services]
ms.assetid: 22cfbeb8-4ea3-4182-8f54-3341c771e87b
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: deeba8dd32d50b2bb31da49e798cd867d5913fa6
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66100493"
---
# <a name="access-the-reporting-services-wmi-provider"></a>访问 Reporting Services WMI 提供程序
  Reporting Services WMI 提供程序公开了两个 WMI 类，用于通过脚本管理本机模式报表服务器实例：  
  
> [!IMPORTANT]  
>  从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 版本开始，只有本机模式报表服务器才支持 WMI 提供程序。 可以使用 SharePoint 管理中心页和 PowerShell 脚本管理 SharePoint 模式报表服务器。  
  
|类|命名空间|Description|  
|-----------|---------------|-----------------|  
|MSReportServer_Instance|root\Microsoft\SqlServer\ReportServer\RS_*\<EncodedInstanceName>* \v11|为客户端提供连接到已安装的报表服务器所需的基本信息。|  
|MSReportServer_ConfigurationSetting|root\Microsoft\SqlServer\ReportServer\RS_*\<EncodedInstanceName>* \v11\Admin|表示报表服务器实例的安装和运行时参数。 这些参数存储在报表服务器的配置文件中。<br /><br /> **\*\* 重要提示 \*\*** 只有拥有管理权限才能访问此类。|  
  
 以上每个类实例是为每个报表服务器实例创建的。 您可以使用任何 Microsoft 或第三方工具来访问报表服务器公开的 WMI 对象，包括 .NET Framework 本身公开的 WMI 编程接口。 本主题介绍如何使用 PowerShell 命令 [Get-WmiObject](https://technet.microsoft.com/library/dd315295.aspx)访问和使用 WMI 类实例。  
  
## <a name="determine-the-instance-name-in-the-namespace-string"></a>确定命名空间字符串中的实例名称  
 命名空间路径中针对 Reporting Services WMI 类的实例名称是您在安装命名 Reporting Services 实例时指定的实例名称的编码形式。 也就是说，对实例名称中的特殊字符进行编码。 例如，下划线 (_) 编码为“_5f”，这样，实例名称“My_Instance”在 WMI 命名空间路径中就编码为“My_5fInstance”。  
  
 若要列出 WMI 命名空间路径中报表服务器实例的编码实例名称，请使用以下 PowerShell 命令：  
  
```  
PS C:\windows\system32> Get-WmiObject -namespace root\Microsoft\SqlServer\ReportServer  -class __Namespace -ComputerName hostname | select Name  
```  
  
## <a name="access-the-wmi-classes-using-powershell"></a>使用 PowerShell 访问 WMI 类  
 若要访问 WMI 类，请运行以下命令：  
  
```  
PS C:\windows\system32> Get-WmiObject -namespace <namespacename> -class <classname> -ComputerName <hostname>  
```  
  
 例如，若要访问主机 myrshost 的默认报表服务器实例上的 MSReportServer_ConfigurationSetting 类，请运行以下命令： 默认报表服务器实例必须安装在 myrshost 上，这样此命令才会成功。  
  
```  
PS C:\windows\system32> Get-WmiObject -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERER\v11\Admin" -class MSReportServer_ConfigurationSetting -ComputerName myrshost  
```  
  
 此命令语法会输出所有的类属性名称和值。 请注意，会返回 MSReportServer_ConfigurationSetting 类的所有实例，即使您在默认报表服务器实例 (RS_MSSQLSERVER) 的命名空间中访问此类也是如此。 例如，如果 myrshost 上安装了默认报表服务器实例和名为 SHAREPOINT 的命名报表服务器实例，此命令将返回两个 WMI 对象并输出这两个报表服务器实例的属性名称和值。  
  
 若要在返回多个实例时返回特定类实例，请使用 -Filter 参数基于值唯一的属性（如 InstanceName）对结果进行筛选。 例如，若要仅返回默认报表服务器实例的 WMI 对象，请使用以下命令：  
  
```  
PS C:\windows\system32> Get-WmiObject -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLServer\v11\Admin" -class MSReportServer_ConfigurationSetting -ComputerName myrshost -filter "InstanceName='MSSQLSERVER'"  
```  
  
## <a name="query-the-available-methods-and-properties"></a>查询可用的方法和属性  
 若要查看某个 Reporting Services WMI 类中有哪些可用的方法和属性，请将结果从 Get-WmiObject 管道传送到 Get-Member 命令。 例如：  
  
```  
PS C:\windows\system32> Get-WmiObject -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLServer\v11\Admin" -class MSReportServer_ConfigurationSetting -ComputerName myrshost | Get-Member  
```  
  
 有关的属性和方法的 Reporting Services WMI 类的文档，请参阅...  
  
## <a name="use-a-wmi-method-or-property"></a>使用 WMI 方法或属性  
 一旦您将 WMI 对象传送到 Reporting Services 类并且知道可用的方法和属性，就可以使用这些方法和属性。 例如，如果您在 SharePoint 集成模式中有一个名为 SHAREPOINT 的命名报表服务器实例，可以使用以下命令序列检索 SharePoint 管理中心站点的 URL：  
  
```  
PS C:\windows\system32> $rsconfig = Get-WmiObject -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLServer\v11\Admin" -class MSReportServer_ConfigurationSetting -ComputerName myrshost -filter "InstanceName='SHAREPOINT'"  
PS C:\windows\system32> $rsconfig.GetAdminSiteUrl()  
  
```  
  
## <a name="see-also"></a>请参阅  
 [Reporting Services WMI 提供程序库引用 (SSRS)](../wmi-provider-library-reference/reporting-services-wmi-provider-library-reference-ssrs.md)   
 [RSReportServer 配置文件](../report-server/rsreportserver-config-configuration-file.md)  
  
  
