---
title: 使用 SQL Server Profiler 监视 Analysis Services |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 190ae82b43f7a4f311d358ea9597a10cd5cc39dc
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68181227"
---
# <a name="use-sql-server-profiler-to-monitor-analysis-services"></a>Use SQL Server Profiler to Monitor Analysis Services
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 跟踪引擎进程事件（例如批处理或事务的启动），并捕获有关这些事件的数据，从而使你可以监视服务器和数据库活动（例如，用户查询或登录活动）。 可以将 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据捕获到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表或文件中，供后续分析使用，还可以重播在相同或其他 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例中捕获的事件，以确切了解所发生的情况。 可以实时或分步重播事件。 在同一计算机中运行跟踪事件和性能计数器是非常有用的。 事件探查器可基于时间将跟踪事件与性能计数器相关联，并在一条时间线上同时显示这两者。 跟踪事件提供详细信息，而性能计数器提供聚合视图。 有关如何创建和运行跟踪的信息，请参阅 [为重播创建事件探查器跟踪 (Analysis Services)](../../analysis-services/instances/create-profiler-traces-for-replay-analysis-services.md)。  
  
 下表对本部分的主题进行了说明：  
  
## <a name="in-this-section"></a>本节内容  
  
|主题|描述|  
|-----------|-----------------|  
|[通过 SQL Server Profiler 监视 Analysis Services 简介](../../analysis-services/instances/introduction-to-monitoring-analysis-services-with-sql-server-profiler.md)|说明如何使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 监视 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例。|  
|[为重播创建事件探查器跟踪 (Analysis Services)](../../analysis-services/instances/create-profiler-traces-for-replay-analysis-services.md)|说明使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]创建用于重播的跟踪的要求。|  
|[Analysis Services 跟踪事件](https://docs.microsoft.com/bi-reference/trace-events/analysis-services-trace-events)|说明 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 事件类。 这些事件类映射到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 所生成的操作并用于跟踪重播。|  
  

  
  
