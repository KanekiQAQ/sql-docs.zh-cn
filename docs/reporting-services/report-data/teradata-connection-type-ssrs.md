---
title: Teradata 连接类型 (SSRS) | Microsoft Docs
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: b02779c2-a6b9-453c-815f-adad53353952
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4c50b757f6866307b52106055fd5affbf46789c0
ms.sourcegitcommit: 1800fc15075bb17b50d0c18b089d8a64d87ae726
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2019
ms.locfileid: "66500130"
---
# <a name="teradata-connection-type-ssrs"></a>Teradata 连接类型 (SSRS)
  若要在报表中包含来自 Teradata 关系数据库的数据，您必须拥有一个基于 Teradata 类型的报表数据源的数据集。 此内置数据源类型基于 .NET Managed Provider for Teradata 数据处理扩展插件。  
  
 使用本主题中的信息来生成一个数据源。 有关分步说明，请参阅 [添加和验证数据连接（报表生成器和 SSRS）](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)。  
  
##  <a name="Connection"></a> 连接字符串  
 请联系数据库管理员，获取连接信息以及用于连接到数据源的凭据。 下面的连接字符串示例指定使用 IP 地址指定的服务器上的 Teradata 数据库：  
  
```  
data source=<IP Address>  
```  
  
 有关更多连接字符串的示例，请参阅 [报表生成器中的数据连接、数据源和连接字符串](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)。  
  
##  <a name="Credentials"></a> 凭据  
 执行以下操作时需要提供凭据：运行查询、本地预览报表以及从报表服务器预览报表。  
  
 报表发布后，您可能需要更改数据源的凭据，以使报表在报表服务器上运行时，用于检索数据的权限有效。  
  
 有关详细信息，请参阅[数据连接、 数据源和连接字符串&#40;报表生成器和 SSRS&#41; ](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)或[指定凭据和连接信息的报表数据源](specify-credential-and-connection-information-for-report-data-sources.md).  
  
  
##  <a name="Remarks"></a> 注释  
 在可以连接 Teradata 数据源之前，系统管理员必须已安装支持从 Teradata 数据库中检索数据的 .NET Data Provider for Teradata 版本。 此数据访问接口必须与报表生成器安装在同一台计算机上，报表服务器上也是如此。  
  
 不是所有的报表传递模式都受到此数据访问接口的支持。 此数据处理扩展插件不支持通过数据驱动订阅传递报表。 有关详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [ 联机丛书 ](https://go.microsoft.com/fwlink/?linkid=121312) 中 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 文档中的[使用外部数据源提供订阅方数据（数据驱动订阅）](../../reporting-services/subscriptions/use-an-external-data-source-for-subscriber-data-data-driven-subscription.md)。  
  
  
##  <a name="Models"></a> 报表模型  
 若要从基于 Teradata 数据源的报表模型创建数据集，则模型必须在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 中的模型设计器内设计，并在报表服务器上发布。  
  
  
##  <a name="Related"></a> 相关章节  
 文档中的这些章节提供有关报表数据的深入概念性信息，以及有关如何定义、自定义和使用与数据相关的报表部件的步骤信息。  
  
 [报表数据集 (SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md)  
 提供访问报表数据的概述。  
  
 [报表生成器中的数据连接、数据源和连接字符串](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
 提供有关数据连接和数据源的信息。  
  
 [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 提供有关嵌入数据集和共享数据集的信息。  
  
 [数据集字段集合（报表生成器和 SSRS）](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 提供有关数据集查询生成的字段集合的信息。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [联机丛书](https://go.microsoft.com/fwlink/?linkid=121312)中 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 文档中的 [Reporting Services 支持的数据源 (SSRS) ](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)。  
 提供有关每个数据扩展插件的平台和版本支持的详细信息。  
  
 [将 SQL Server 2008 Reporting Services 与 .NET Framework Data Provider for Teradata 一起使用](https://go.microsoft.com/fwlink/?LinkID=130848)  
 提供有关使用此数据扩展插件的详细信息。  
  
  
## <a name="see-also"></a>另请参阅  
 [报表参数（报表生成器和报表设计器）](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [对数据进行筛选、分组和排序（报表生成器和 SSRS）](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
  
