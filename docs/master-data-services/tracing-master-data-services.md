---
title: 跟踪 (Master Data Services) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 45823fc8-723a-49f2-9a11-94d241245cfd
author: lrtoyou1223
ms.author: lle
manager: erikre
ms.openlocfilehash: d1c438eff7f3543b22fc2c0e4e2a7264cd1a91ee
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68263976"
---
# <a name="tracing-master-data-services"></a>跟踪 (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Web.config 文件包含跟踪部分，如下所示。 此部分是 [!INCLUDE[ssSQL15](../includes/sssql15-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]  
  
```  
<sources>  
      <!-- Adjust the switch value to control the types of messages that should be logged.   
           https://msdn.microsoft.com/library/system.diagnostics.sourcelevels  
           Use the a switchValue of Verbose to generate a full log. Please be aware that   
           the trace file can get quite large very quickly -->  
      <source name="MDS" switchType="System.Diagnostics.SourceSwitch" switchValue="Warning, ActivityTracing">  
        <listeners>  
          <!-- Set a directory path where the service account you chose while setting up Master Data Services has read and write privileges.  
               Default path is Logs in WebApplication folder, for example C:\Program Files\Microsoft SQL Server\130\Master Data Services\WebApplication  
               New log file will be created every day or every 10 mb.  
               When directory size hits the 200mb limitation, the oldest file will be deleted.-->  
          <add name="FileTraceListener"  
               type="Microsoft.MasterDataServices.Core.Logging.FileTraceListener, Microsoft.MasterDataServices.Core"   
               initializeData="DirectoryPath = Logs; FileSizeInMb = 10; MaxDirectorySizeInMb = 200"/>  
          <remove name="Default"/>  
        </listeners>  
      </source>  
    </sources>  
  
```  
  
 以下是默认跟踪行为。  
  
-   为警告和 ActivityTracing 消息启用跟踪。  
  
     有关详细信息，请参阅 [SourceLevels 枚举](https://msdn.microsoft.com/library/system.diagnostics.sourcelevels)。  
  
-   日志保存在 WebApplication 文件夹下的 Logs 文件夹中。 默认位置为 C:\Program Files\Microsoft SQL Server\130\Master Data Services\WebApplication\Logs。  
  
-   每天或每生成 10 MB 数据都会创建一次文件。  
  
-   当大小直接达到 200 MB 时，删除最旧的日志。  
  
-   日志格式为 CSV。 下表介绍了日志格式。  
  
    |元素|描述|  
    |-------------|-----------------|  
    |Time|跟踪条目出现的时间。|  
    |CorrelationID|每个请求都分配有一个相关 ID。 此请求触发的所有跟踪都使用同一个相关 ID。<br /><br /> 当 UI 出错时，错误消息中会显示相关 ID。|  
    |操作|请求操作名称。 如果是 Web UI 请求，则操作名称为 URL。 如果是 API 请求，则操作名称为服务名称。|  
    |级别|此跟踪条目的级别。|  
    |Message|跟踪的消息正文|  
  
## <a name="external-resources"></a>外部资源  
 msdn.com 上的博文 [Troubleshooting Logging Improvement（日志记录故障排除改进）](https://go.microsoft.com/fwlink/p/?LinkId=615377)。  
  
  
