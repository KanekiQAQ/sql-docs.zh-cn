---
title: Analysis Services 中的服务器属性 |Microsoft Docs
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ce74bb210e3d5d3cd01120b0bd406672db6dd5ed
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68207957"
---
# <a name="server-properties-in-analysis-services"></a>Analysis Services 中的服务器属性
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 管理员可以修改 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例的默认服务器配置属性。 每个实例都有自己的配置属性，可以独立于同一服务器上的其他实例进行设置。  
  
 若要将服务器配置，使用 SQL Server Management Studio 或编辑特定的 SQL Server Analysis Services 实例的 msmdsrv.ini 文件。  
 
SQL Server Management Studio 中的“属性”页显示最可能需要修改的属性子集。 属性的完整列表位于 msmdsrv.ini 文件中。   
  
> [!NOTE]  
>  在默认 SQL Server Analysis Services 安装中，可以在 \Program Files\Microsoft SQL Server\MSAS13 找到 msmdsrv.ini。MSSQLSERVER\OLAP\Config 文件夹。
> 
> 其他影响服务器配置的属性包括 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中的部署配置属性。 有关这些属性的详细信息，请参阅 [为解决方案部署指定配置设置](../../analysis-services/multidimensional-models/deployment-script-files-solution-deployment-config-settings.md)。
 
## <a name="configure-properties-in-management-studio"></a>在 Management Studio 中配置属性 
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例。  
  
2. 在对象资源管理器中，右键单击 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例，再单击  “属性”。 随即出现“常规”页，显示更为常用的属性。  

3.  若要查看更多属性，请选中该页底部的  “显示高级(全部)属性”复选框。  
  
     只有表格模式服务器和多维模式服务器才支持修改服务器属性。 如果安装了 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]，除非 Microsoft 支持另有说明，否则请始终使用默认值。  
  
  
## <a name="configure-properties-in-msmdsrvini"></a>在 msmdsrv.ini 中配置属性
  
某些属性只能在 msmdrsrv.ini 文件中进行设置。 Azure Analysis Services 不适用于这些属性。
如果在显示高级属性后仍然看不见要设置的属性，则可能需要直接编辑 msmdsrv.ini 文件。 
  
1.  请在 Management Studio 的“常规”属性页中检查 **DataDir** 属性以确认 Analysis Services 程序文件（包括 msmdsrv.ini 文件）的位置。

     在具有多实例的服务器上，检查程序文件位置可确保修改的是正确的文件。  
  
2.  导航到程序文件文件夹位置下的 **config** 文件夹。

3. 创建该文件的备份，以备将来恢复原始文件时使用。  
  
4.  使用文本编辑器查看或编辑 msmdsrv.ini 文件。  
  
5.  保存文件并重启服务。  
  
##  <a name="server-property-reference"></a>服务器属性参考  
  
 以下主题介绍了各种 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 配置属性：  
  
|主题|描述|  
|-----------|-----------------|  
|[常规属性](../../analysis-services/server-properties/general-properties.md)|常规属性既是基本属性，又是高级属性，包括定义数据目录、备份目录和其他服务器行为的属性。|  
|[数据挖掘属性](../../analysis-services/server-properties/data-mining-properties.md)|数据挖掘属性控制着启用和禁用哪些数据挖掘算法。 默认情况下，启用所有算法。| 
|[DAX 属性](../../analysis-services/server-properties/dax-properties.md)|定义与 DAX 查询相关的属性。|
|DSO|不再支持 DSO。 忽略 DSO 属性。|  
|[功能属性](../../analysis-services/server-properties/feature-properties.md)|功能属性与产品功能有关，大多数是高级属性，包括控制服务器实例之间的链接的属性。|  
|[Filestore 属性](../../analysis-services/server-properties/filestore-properties.md)|文件存储属性仅用于高级用途。 其中包括高级内存管理设置。|  
|[锁管理器属性](../../analysis-services/server-properties/lock-manager-properties.md)|锁管理器属性定义与锁定和超时有关的服务器行为。 这些属性多数仅适用于高级用途。|  
|[日志属性](../../analysis-services/server-properties/log-properties.md)|日志属性控制是否在服务器上记录事件以及记录事件的位置和方式。 其中包括错误日志记录、异常日志记录、网络流量记录器、查询日志记录和跟踪。|  
|[内存属性](../../analysis-services/server-properties/memory-properties.md)|内存属性控制服务器如何使用内存。 这些属性主要用于高级用途。|  
|[网络属性](../../analysis-services/server-properties/network-properties.md)|网络属性控制与网络有关的服务器行为，包括控制压缩和二进制 XML 的属性。 这些属性多数仅适用于高级用途。|  
|[OLAP 属性](../../analysis-services/server-properties/olap-properties.md)|OLAP 属性控制多维数据集和维度处理、迟缓处理、数据缓存和查询行为。 其中既包括基本属性，也包括高级属性。|  
|[安全属性](../../analysis-services/server-properties/security-properties.md)|安全部分包含定义访问权限的基本属性和高级属性。 其中包括与管理员和用户有关的设置。|  
|[线程池属性](../../analysis-services/server-properties/thread-pool-properties.md)|线程池属性控制服务器创建多少线程。 这些属性主要是高级属性。|  
  
## <a name="see-also"></a>请参阅  
 [Analysis Services 实例管理](../../analysis-services/instances/analysis-services-instance-management.md)   
 [为解决方案部署指定配置设置](../../analysis-services/multidimensional-models/deployment-script-files-solution-deployment-config-settings.md)  
  
  
