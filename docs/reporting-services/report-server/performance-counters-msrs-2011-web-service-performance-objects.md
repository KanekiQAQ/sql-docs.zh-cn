---
title: MSRS 2011 Web Service 性能对象的性能计数器 | Microsoft Docs
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- Report Server Web service, performance counters
- Reporting Services performance object
- RS Web Service performance object
- counters [Reporting Services]
- performance [Reporting Services]
ms.assetid: c642fc4f-8734-4626-a194-42ac9cd8e2ef
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: d5d9c379aaa2b8b0b92e064ed6dc21a4eb968b95
ms.sourcegitcommit: 0a4879dad09c6c42ad1ff717e4512cfea46820e9
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67412597"
---
# <a name="performance-counters-msrs-2011-web-service-performance-objects"></a>MSRS 2011 Web Service 性能对象的性能计数器
  本主题介绍 **MSRS 2011 Web Service** 和 **MSRS 2011 Windows Service** 性能对象的性能计数器。 这些对象是 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 本机模式部署的组成部分。  
  
> [!NOTE]  
>  这些性能对象监视本地报表服务器上的事件。 如果是在扩展部署中运行报表服务器，则只对当前服务器（而不是扩展部署）进行计数。  
  
 Windows 性能监视器 (**Perfmon.exe**) 中提供了性能对象。 有关详细信息，请参阅 Windows 文档，[运行时分析](https://msdn.microsoft.com/library/w4bz2147.aspx) (https://msdn.microsoft.com/library/w4bz2147.aspx) 。  
  
 有关 SharePoint 模式性能计数器的信息，请参阅 [MSRS 2011 Web Service SharePoint Mode 性能对象和 MSRS 2011 Windows Service SharePoint Mode 性能对象的性能计数器（SharePoint 模式）](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)。  
  
 本主题内容：  
  
-   [MSRS 2011 Web Service 性能计数器](#bkmk_webservice)  
  
-   [MSRS 2011 Windows Service 性能计数器](#bkmk_windowsservice)  
  
-   [使用 PowerShell Cmdlet 返回列表](#bkmk_powershell)  
  
##  <a name="bkmk_webservice"></a> MSRS 2011 Web Service 性能计数器  
 **MSRS 2011 Web Service** 性能对象监视报表服务器性能。 此性能对象包括一系列用于跟踪报表服务器处理的计数器，这些处理通常通过交互式报表查看操作启动。 在设置计数器时，可将此计数器应用于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的全部实例，也可以选择特定实例。 只要 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止报表服务器 Web 服务，这些计数器就会重置。  
  
 下表列出了 **MSRS 2011 Web Service** 性能对象中包含的计数器。  
  
|计数器|描述|  
|-------------|-----------------|  
|**活动会话**|活动会话的数目。 此计数器提供报表执行生成的所有浏览器会话的累积数，而不管这些会话是否仍处于活动状态。<br /><br /> 删除会话记录后，此计数器的计数即会相应减少。 默认情况下，如果会话在 10 分钟之内无任何活动，就会被删除。|  
|**每秒缓存命中数**|每秒请求缓存报表的次数。 这些请求是对重新呈现的报表的请求，而不是对直接从缓存处理的报表的请求。 （请参阅本主题后面的 **总缓存命中数** 。）|  
|**每秒缓存命中数（语义模型）**|每秒请求缓存模型的次数。 这些请求是对重新呈现的报表的请求，而不是对直接从缓存处理的报表的请求。|  
|**每秒缓存未命中数**|每秒未能从缓存返回报表的请求次数。 使用此计数器可以查明是否有足够的资源（磁盘或内存）用于缓存。|  
|**每秒缓存未命中数（语义模型）**|每秒未能从缓存返回模型的请求次数。 使用此计数器可以查明是否有足够的资源（磁盘或内存）用于缓存。|  
|**第一个每秒会话请求数**|每秒从报表服务器缓存启动的新用户会话的数目。|  
|**每秒内存缓存命中数**|每秒从内存中缓存检索到报表的次数。 *内存中缓存* 是缓存的一部分，可将报表存储在 CPU 内存中。 使用内存中缓存时，报表服务器不会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中查询缓存的内容。|  
|**每秒内存缓存未命中数**|每秒从内存中缓存未检索到报表的次数。|  
|**下一个每秒会话请求数**|对当前会话中打开的报表（例如通过会话快照呈现的报表）的每秒请求数。|  
|**报表请求**|当前处于活动状态且正由报表服务器管理的报表的数目。|  
|**每秒执行的报表**|每秒成功执行报表的次数。 此计数器提供有关报表量的统计信息。 将此计数器与 **每秒请求数** 一起使用，可以对报表执行与可以从缓存返回的报表请求进行比较。|  
|**每秒请求数**|每秒向报表服务器发出的请求的次数。 此计数器可跟踪报表服务器管理的所有类型请求。|  
|**总缓存命中数**|服务启动后请求缓存报表的总次数。 只要 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止报表服务器 Web 服务，此计数器就会重置。|  
|**总缓存命中数（语义模型）**|服务启动后请求缓存中的模型的总次数。 只要 ASP.NET 停止报表服务器 Web 服务，此计数器就会重置。|  
|**总缓存未命中数**|服务启动后未能从缓存返回报表的总次数。 只要 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止报表服务器 Web 服务，此计数器就会重置。 使用此计数器可确定是否有足够的磁盘空间和内存。|  
|**总缓存未命中数（语义模型）**|服务启动后未能从缓存返回模型的总次数。 只要 ASP.NET 停止报表服务器 Web 服务，此计数器就会重置。 使用此计数器可确定是否有足够的磁盘空间和内存。|  
|**总内存缓存命中数**|服务启动后从内存中缓存返回的缓存报表的总数。 只要 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止报表服务器 Web 服务，此计数器就会重置。 *内存中缓存* 是缓存的一部分，可将报表存储在 CPU 内存中。 使用内存中缓存时，报表服务器不会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中查询缓存的内容。|  
|**总内存缓存未命中数**|服务启动后在内存中缓存的缓存失误总数。 只要 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止报表服务器 Web 服务，此计数器就会重置。|  
|**总处理失败数**|针对报表服务器 Web 服务的请求处理错误数。|  
|**总拒绝线程数**|系统拒绝异步处理的总线程数，这些线程随后将以同一线程中同步进程的形式处理。 每个数据源在一个线程中进行处理。 如果线程量超出了系统容量，系统将拒绝对线程进行异步处理，随后会按串行方式处理。|  
|**总执行的报表数**|服务启动后成功运行的报表的总数。 只要 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止报表服务器 Web 服务，此计数器就会重置。|  
|**总请求数**|服务启动后对报表服务器发出的全部请求的总数。 只要 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止报表服务器 Web 服务，此计数器就会重置。|  
  
##  <a name="bkmk_windowsservice"></a> MSRS 2011 Windows Service 性能计数器  
 **MSRS 2011 Windows Service** 性能对象监视报表服务器 Windows 服务。 此性能对象包括一系列用于跟踪报表处理的计数器，这些处理通过计划操作启动。 计划操作可以包括订阅和传递、报表执行快照和报表历史记录。 在设置计数器时，可将此计数器应用于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的全部实例，也可以选择特定实例。  
  
 下表列出了 **MSRS 2011 Windows Service** 性能对象中包含的计数器。  
  
|计数器|描述|  
|-------------|-----------------|  
|**活动会话**|存储在报表服务器数据库中的活动会话数。 此计数器提供报表订阅生成的所有可用浏览器会话的累积数，而不管这些会话是否仍处于活动状态。|  
|**每秒缓存刷新数**|每秒刷新的缓存数。|  
|**每秒缓存命中数**|每秒请求缓存报表的次数。 这些请求是对重新呈现的报表的请求，而不是对直接从缓存处理的报表的请求。 （请参阅本主题后面的 **总缓存命中数** 。）|  
|**每秒缓存命中数（语义模型）**|每秒请求缓存模型的次数。|  
|**每秒缓存未命中数**|每秒未能从缓存返回报表的请求次数。 使用此计数器可以查明是否有足够的资源（磁盘或内存）用于缓存。|  
|**每秒缓存未命中数（语义模型）**|每秒未能从缓存返回模型的请求次数。 使用此计数器可以查明是否有足够的资源（磁盘或内存）用于缓存。|  
|**每秒交付数**|所有传递扩展插件每秒进行报表传递的次数。|  
|**每秒事件数**|每秒处理的事件数。 监视的事件包括 **SnapshotUpdated** 和 **TimedSubscription**。|  
|**第一个每秒会话请求数**|每秒创建的新报表执行会话的数目。|  
|**每秒内存缓存命中数**|每秒从内存中缓存检索到报表的次数。 *内存中缓存* 是缓存的一部分，可将报表存储在 CPU 内存中。 使用内存中缓存时，报表服务器不会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中查询缓存的内容。|  
|**每秒内存缓存未命中数**|每秒从内存中缓存未检索到报表的次数。|  
|**下一个每秒会话请求数**|对当前会话中打开的报表（例如通过会话快照呈现的报表）的每秒请求数。|  
|**报表请求**|当前处于活动状态且正由报表服务器管理的报表的数目。 使用此计数器可以评估缓存策略。 请求数可能会比报表生成的请求数更多。|  
|**每秒执行的报表**|每秒成功生成的报表数。|  
|**每秒请求数**|报表服务器服务每秒处理的成功请求的总数。|  
|**每秒快照更新数**|每秒报表执行快照更新的总数。|  
|**总应用域回收数**|报表服务器 Windows 服务启动后应用程序域循环的总次数。|  
|**总缓存刷新数**|服务启动后报表服务器缓存更新的总次数。 当应用程序域回收时，将重置此计数器。 请参阅 **每秒缓存刷新数**。|  
|**总缓存命中数**|报表服务器 Windows 服务启动后请求直接从缓存处理的报表的总次数。 当应用程序域回收时，将重置此计数器。 请参阅 **每秒缓存命中数**。|  
|**总缓存命中数（语义模型）**|报表服务器 Windows 服务启动后请求直接从缓存处理模型的总次数。 当应用程序域回收时，将重置此计数器。 请参阅 **每秒缓存命中数**。|  
|**总缓存未命中数**|报表服务器 Windows 服务启动后报表未能从缓存返回的总次数。 当应用程序域回收时，将重置此计数器。 请参阅 **每秒缓存未命中数**。|  
|**总缓存未命中数（语义模型）**|报表服务器 Windows 服务启动后模型未能从缓存返回的总次数。 当应用程序域回收时，将重置此计数器。|  
|**总传递数**|对于所有传递扩展插件，通过计划和传递处理器所传递的报表总数。 当应用程序域回收时，将重置此计数器。|  
|**Total Events**|报表服务器 Windows 服务启动后发生的事件总数。 当应用程序域回收时，将重置此计数器。|  
|**总内存缓存命中数**|报表服务器 Windows 服务启动后从内存中缓存返回的缓存报表总数。 当应用程序域回收时，将重置此计数器。|  
|**总内存缓存未命中数**|服务启动后在内存中缓存的缓存失误总数。 当应用程序域回收时，将重置此计数器。|  
|**总处理失败数**|报表服务器 Windows 服务中的请求处理错误数。|  
|**总拒绝线程数**|系统拒绝异步处理的总线程数，这些线程随后将以同一线程中同步进程的形式处理。 在中等或高负载下，该计数器将稳定增长。|  
|**总执行的报表数**|运行报表的总数。|  
|**总请求数**|服务启动后成功运行的报表的总数。 当应用程序域回收时，将重置此计数器。|  
|**总快照更新数**|报表执行快照更新的总数|  
  
##  <a name="bkmk_powershell"></a> 使用 PowerShell Cmdlet 返回列表  
 ![与 PowerShell 相关的内容](../../analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell related content")下面的 Windows PowerShell 脚本将返回 CounterSetName 以“msr”开头的计数器集：  
  
```  
get-counter -listset msr*  
```  
  
 下面的 Windows PowerShell 脚本将返回 CounterSetName 的性能计数器列表。  
  
```  
(get-counter -listset "MSRS 2011 Windows Service").paths  
```  
  
## <a name="see-also"></a>另请参阅  
 [监视报表服务器性能](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [MSRS 2011 Web Service SharePoint Mode 性能对象和 MSRS 2011 Windows Service SharePoint Mode 性能对象的性能计数器（SharePoint 模式）](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)   
 [ReportServer:Service 和 ReportServerSharePoint:Service 性能对象的性能计数器](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
  
  
