---
title: Reporting Services 支持的数据源 (SSRS) | Microsoft Docs
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
helpviewer_keywords:
- SQL Server data processing extension [Reporting Services]
- XML data processing extension [Reporting Services]
- reports [Reporting Services], data
- data processing extensions [Reporting Services], data sources
- Oracle [Reporting Services]
- OLE DB data processing extension
- data sources [Reporting Services], about data sources
- ODBC data processing extension
- Reporting Services, data sources
ms.assetid: 9d11d055-a3be-45aa-99a7-46447a94ed42
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c00b9536ad5a97e5e0d3d302e2960bc1b1a9f077
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66500463"
---
# <a name="data-sources-supported-by-reporting-services-ssrs"></a>Reporting Services 支持的数据源 (SSRS)
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 通过一个使用数据处理扩展插件的可扩展模块化数据层从数据源中检索报表数据。 若要从数据源检索报表数据，必须选择一个数据处理扩展插件，该扩展插件必须支持数据源类型、数据源上运行的软件版本，以及数据源平台（32 位或 64 位 [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]）。  
  
 部署 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]时，会自动在报表创作客户端和报表服务器上安装并注册一组数据处理扩展插件，以提供对各种数据源类型的访问。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装以下数据源类型：  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 用于 MDX、DMX、 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]和表格模型  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]  
  
-   Oracle  
  
-   SAP BW 
  
-   [!INCLUDE[extEssbase](../../includes/extessbase-md.md)]  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] SharePoint 列表  
  
-   Teradata  
  
-   OLE DB  
  
-   ODBC  
  
-   XML  
  
 此外，系统管理员还可以安装和注册自定义数据处理扩展插件和标准 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口。 若要处理和查看报表，必须在报表服务器上安装并注册数据处理扩展插件和数据访问接口；若要预览报表，必须在报表创作客户端上安装并注册数据处理扩展插件和数据访问接口。 注册数据处理扩展插件和数据访问接口必须针对其安装平台进行本机编译。 如果您通过使用 SOAP Web 服务以编程方式部署数据源，则必须定义数据源扩展插件。 使用来自 **RSReportDesigner.config** 文件的数据扩展插件值。 默认情况下，该文件位于以下文件夹中：  
  
```  
<drive letter>\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\PrivateAssemblies  
```  
  
 例如， [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据扩展插件是 OLEDB-MD。  
  
 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Microsoft 下载中心 [和第三方站点提供了许多可供下载的第三方标准](https://go.microsoft.com/fwlink/?linkid=51456) 数据访问接口。 还可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 公共论坛中搜索有关第三方数据访问接口的信息。  
  
> [!NOTE]  
>  标准 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口并非一定要支持 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件提供的所有功能。 此外，可以使用某些 OLE DB 数据访问接口和 ODBC 驱动程序来创作和预览报表，但它们并未设计为支持报表服务器上发布的报表。 例如，报表服务器不支持 [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for Jet。 有关详细信息，请参阅[数据处理扩展插件和 .NET Framework 数据提供程序 (SSRS)](../../reporting-services/report-data/data-processing-extensions-and-net-framework-data-providers-ssrs.md)。  
  
 有关自定义数据处理扩展插件的详细信息，请参阅 [Implementing a Data Processing Extension](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)。 有关标准 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口的详细信息，请参阅 <xref:System.Data> 命名空间。   
  
## <a name="platform-support-for-report-data-sources"></a>报表数据源的平台支持  
 可在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 部署中使用的数据源因 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 版本和平台的不同而不同。 有关功能的详细信息，请参阅 [SQL Server 各个版本支持的 Reporting Services 功能](../../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)。 本主题后面的表按版本和平台列出了所支持数据源的相关信息。  
  
 报表创作客户端和报表服务器对于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据源有不同的平台要求。  
  
### <a name="on-the-report-authoring-client"></a>在报表创作客户端上  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] 是 32 位应用程序。 [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] 在基于 Itanium 的平台上不受支持。 在 x64 平台上，若要在报表设计器中编辑和预览报表，必须在 (x86) 平台目录中安装了 32 位数据访问接口。  
  
### <a name="on-the-report-server"></a>在报表服务器上  
 将报表部署到 64 位报表服务器时，报表服务器必须安装有在本机编译的 64 位数据访问接口。 不支持将 32 位数据访问接口包装在 64 位接口中。 有关详细信息，请参阅数据访问接口文档。  
  
## <a name="supported-data-sources"></a>支持的数据源  
 下表列出了可用来为报表数据集和报表模型检索数据的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 数据处理扩展插件和数据访问接口。 有关扩展插件或数据访问接口的详细信息，请单击第二列中的链接。 表中各列的说明如下：  
  
-   报表数据源：要访问的数据类型，例如，关系数据库、多维数据库、平面文件或 XML。 此列解答问题：“ [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 可为报表使用哪种类型的数据？”  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据源类型：在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]中定义数据源时下拉列表中显示的数据源类型之一。 此列表是根据所安装和注册的 DPE 与数据访问接口填充的。 此列解答问题：“在创建报表数据源时，应从下拉列表中选择哪种数据源类型？”  
  
-   数据处理扩展插件/数据访问接口的名称：对应于所选 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据源类型的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件或其他数据访问接口。 此列解答问题：“我在选择了一种数据源类型时，将使用哪种对应的数据处理扩展插件或数据访问接口？”  
  
-   基础数据访问接口版本（可选）：某些数据源类型支持多种数据访问接口。 它们可能是同一访问接口的不同版本，或由第三方提供的某一数据访问接口类型的不同实现。 在您配置了数据源后，访问接口名称会频繁出现在连接字符串中。 此列解答问题：“选择了数据源类型后，我要在 **“连接属性”** 对话框中选择哪种数据访问接口？”  
  
-   数据源 \<platform>：得到数据处理扩展插件或数据访问接口支持的目标数据源的数据源平台  。 此列解答问题：“此数据处理扩展插件或数据访问接口能否从这类平台上的数据源中检索数据？”  
  
-   数据源的版本：DPE 或数据访问接口支持的目标数据源的版本。 此列解答问题：“此数据处理扩展插件或数据访问接口能否从此版本的数据源检索数据？”  
  
-   RS \<platform>：可在其中安装自定义 DPE 或数据提供程序的报表服务器平台和报表创作客户端平台  。 任何 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装都会包含内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]数据处理扩展插件。 自定义数据处理扩展插件或 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口必须针对特定平台进行本机编译。 此列解答问题：“此数据处理扩展插件或数据访问接口能否安装在这类平台上？”  
  
###  <a name="DataSourcesTable"></a> 数据源的类型  
  
|报表<br /><br /> 数据源|Reporting Services 数据源类型|数据处理扩展插件/数据访问接口的名称|基础数据访问接口版本<br /><br /> （可选）|data<br /><br /> 数据源<br /><br /> 平台 x86|data<br /><br /> 数据源<br /><br /> 平台 x64|数据源的版本|RS &lt;platform<br /><br /> 平台 x86|RS<br /><br /> 平台 x64|  
|-------------------------------|-----------------------------------------|------------------------------------------------------|-------------------------------------------------------|--------------------------------------|--------------------------------------|----------------------------|-------------------------|-------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关系数据库|[Microsoft SQL Server](#MicrosoftSQLServer)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|扩展 System.Data.SqlClient|是|是|SQL Server 2008 和更高版本。|是|是|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关系数据库|OLEDB|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|扩展 System.Data.OledbClient|是|是|SQL Server 2008 和更高版本。|是|是|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关系数据库|[ODBC](#ODBC)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|扩展 System.Data.OdbcClient|是|是|SQL Server 2008 和更高版本。|是|是|  
|[!INCLUDE[ssSDS](../../includes/sssds-md.md)]|[Microsoft Azure SQL 数据库](#Azure)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|扩展 System.Data.SqlClient|N/A|N/A|[!INCLUDE[ssSDS](../../includes/sssds-md.md)]|是|是|
|SQL 数据仓库|[Microsoft Azure SQL 数据库](#Azure)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|扩展 System.Data.SqlClient|N/A|N/A|SQL 数据仓库|是|是| 
|[!INCLUDE[ssDW](../../includes/ssdw-md.md)] 工具|[Microsoft 并行数据仓库](#PWD)|弃用的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|N/A|N/A|N/A|[!INCLUDE[ssDWfull](../../includes/ssdwfull-md.md)]|否|否|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 多维数据库|[Microsoft SQL Server Analysis Services](#AnalysisServices)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|使用 ADOMD.NET|是|是|SQL Server 2008 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 和更高版本|是|是|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 多维数据库|OLEDB|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|扩展 System.Data.OledbClient<br /><br /> 10\.0 版|是|是|SQL Server 2008 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|是|是|   
|SharePoint 列表|[Microsoft SharePoint 列表](#SharePointList)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|从 Lists.asmx 或 SharePoint 对象模型 API 接口获取数据。<br /><br /> 请参阅 [注意](#SharePointList)。|否|是|SharePoint 2013 产品及更高版本|是|是|   
|XML|[XML](#XML)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|XML 数据源与平台无关。|N/A|N/A|[!INCLUDE[vstecwebservices](../../includes/vstecwebservices-md.md)] 或文档|是|是|  
|报表服务器模型|报表模型|用于已发布 SMDL 文件的弃用的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|模型的数据源使用内置数据处理扩展插件。<br /><br /> 基于 Oracle 的模型要求使用 Oracle 客户端组件。<br /><br /> 基于 Teradata 的模型要求使用来自 Teradata 的 .NET Data Provider for Teradata。<br /><br /> 有关平台支持，请参阅 Teradata 文档。|N/A|N/A|可以从[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本创建模型。<br /><br /> [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]<br /><br /> Oracle 9.2.0.3 或更高版本<br /><br /> Teradata V14、v13、v12 和 v6.2|否|否|  
|SAP 多维数据库|SAP BW|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|有关平台支持，请参阅 SAP 文档。|N/A|N/A|SAP BW 7.0-7.5|是|N/A|  
|[!INCLUDE[extEssbase](../../includes/extessbase-md.md)]|[Hyperion Essbase](#Hyperion)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|有关平台支持，请参阅 Hyperion 文档。|是|N/A|[!INCLUDE[extEssbase](../../includes/extessbase-md.md)] 9.3.1|是|N/A|  
|Oracle 关系数据库|[Oracle](#OracleClient)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|要求使用 Oracle 客户端组件 12c 或更高版本。|是|N/A|Oracle 11g, 11g R2, 12c|是|是|  
|Teradata 关系数据库|[Teradata](#Teradata)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|扩展来自 Teradata 的 .NET Data Provider for Teradata。<br /><br /> 要求使用来自 Teradata 的 .NET Data Provider for Teradata。<br /><br /> 有关平台支持，请参阅 Teradata 文档。|是|N/A|Teradata v15<br /><br />Teradata v14<br /><br /> Teradata v13|是|否|  
|DB2 关系数据库|自定义的已注册数据扩展插件名||2004 Host Integration (HI) Server<br /><br /> 请参阅 [HI Server 文档](https://msdn.microsoft.com/library/gg241192\(v=bts.10\).aspx)。|是|N/A|N/A|是|否|  
|一般 OLE DB 数据源|OLEDB|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|支持 OLE DB 的任何数据源。<br /><br /> 有关平台支持，请参阅数据源文档。|是|N/A|支持 OLE DB 的任何数据源。 请参阅 [注意](#OLEDBStandard)。|是|N/A|  
|一般 ODBC 数据源|[ODBC](#ODBCGeneric)|内置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件|支持 ODBC 的任何数据源。<br /><br /> 有关平台支持，请参阅数据源文档。|是|N/A|支持 ODBC 的任何数据源。 请参阅 [注意](#ODBCGeneric)。|是|是|  
  
 有关使用外部数据源的信息，请参阅[从外部数据源中添加数据 (SSRS)](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md)。  
  
 第三方提供了许多标准 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口。 有关详细信息，请搜索第三方网站或论坛。  
  
 若要安装和注册自定义数据处理扩展插件或标准 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口，则需参考数据访问接口的参考文档。 有关详细信息，请参阅[注册标准 .NET Framework 数据访问接口 (SSRS)](../../reporting-services/report-data/register-a-standard-net-framework-data-provider-ssrs.md)。  
  
 [返回数据源表](#DataSourcesTable)  
  
## <a name="reporting-services-data-processing-extensions"></a>Reporting Services 数据处理扩展插件  
 以下数据处理扩展插件随 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 和 [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)]自动安装。 有关详细信息以及安装验证，请参阅 [RSReportDesigner 配置文件](../../reporting-services/report-server/rsreportdesigner-configuration-file.md) 和 [RsReportServer.config 配置文件](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)。  
  
> [!NOTE]
>  目前不支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据处理扩展插件。  
  
 有关报表生成器支持的数据处理扩展插件的详细信息，请参阅 msdn.microsoft.com 上 [报表生成器文档](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) 中的 [报表生成器中的数据连接、数据源和连接字符串](https://go.microsoft.com/fwlink/?LinkId=154494) 。  
  
###  <a name="MicrosoftSQLServer"></a> Microsoft SQL Server 数据处理扩展插件  
 数据源类型 **Microsoft SQL Server** 包装并扩展了 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Data Provider for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 此数据处理扩展插件针对基于 x86 和 [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)]的平台进行了本机编译并在这些平台上运行。  
  
 在 [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] 中，与此数据扩展插件关联的查询设计器是 Visual Database Tool 设计器。 如果您在图形模式下使用该查询设计器，则会分析查询并可能将其重写。 如果希望控制用于查询的精确 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法，请使用基于文本的查询设计器。 有关详细信息，请参阅 [Graphical Query Designer User Interface](../../reporting-services/report-data/graphical-query-designer-user-interface.md)。  
  
 有关详细信息，请参阅 [SQL Server 连接类型 (SSRS)](../../reporting-services/report-data/sql-server-connection-type-ssrs.md)。  
  
 在报表生成器中，与此数据扩展插件关联的查询设计器是关系查询设计器。 
  
 [返回数据源表](#DataSourcesTable)  
  
###  <a name="Azure"></a> Microsoft Azure SQL 数据库处理扩展插件  
 数据源类型 **Microsoft Azure[!INCLUDE[ssSDS](../../includes/sssds-md.md)]** 包装并扩展了适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据提供程序。  
  
 在 [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] 中，与此数据扩展插件关联的图形查询设计器是关系查询设计器，而不是用于“Microsoft SQL Server”数据源类型的 Visual Database Tool 设计器  。  
  
 [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] 自动区分 Microsoft SQL Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 与 Microsoft SQL Server 数据源类型，并打开与该数据源类型关联的图形查询设计器   。  
  
 如果您在图形模式下使用该查询设计器，则会分析查询并可能将其重写。 基于文本的查询设计器也可用于编写查询。 如果希望控制用于查询的精确 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法，请使用基于文本的查询设计器。   
  
 从 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]、SQL 数据仓库和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 检索数据的方式类似，但存在一些仅适用于 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 的要求。 有关详细信息，请参阅 [SQL Azure 连接类型 (SSRS)](../../reporting-services/report-data/sql-azure-connection-type-ssrs.md)。  
  
 [返回数据源表](#DataSourcesTable)  
  
###  <a name="PWD"></a> Microsoft SQL Server 并行数据仓库处理扩展插件  
已弃用此数据源。 使用 SQL Server 数据源类型连接到 Microsoft 分析平台 (APS)。
  
 [返回数据源表](#DataSourcesTable)  
  
###  <a name="AnalysisServices"></a> Microsoft SQL Server Analysis Services 数据处理扩展插件  
 如果选择数据源类型 **Microsoft SQL Server Analysis Services**，则要选择一个扩展 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Data Provider for OLE DB 的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Data Provider for [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]。 此数据处理扩展插件针对基于 x86 和 x64 的平台进行了本机编译并在这些平台上运行。  
  
 此数据访问接口使用 ADOMD.NET 对象模型创建使用 XML for Analysis (XMLA) 版本 1.1 的查询。 结果将以平展行集的形式返回。 有关详细信息，请参阅[针对 MDX 的 Analysis Services 连接类型 (SSRS)](../../reporting-services/report-data/analysis-services-connection-type-for-mdx-ssrs.md)、[针对 DMX 的 Analysis Services 连接类型 (SSRS)](../../reporting-services/report-data/analysis-services-connection-type-for-dmx-ssrs.md)、[Analysis Services MDX 查询设计器用户界面](../../reporting-services/report-data/analysis-services-mdx-query-designer-user-interface.md)和 [Analysis Services DMX 查询设计器用户界面](../../reporting-services/report-data/analysis-services-dmx-query-designer-user-interface.md)。  
  
 连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据源时， [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据处理扩展插件支持多值参数并将单元和成员属性映射到 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]支持的扩展属性。 有关详细信息，请参阅 [Analysis Services 数据库的扩展字段属性 (SSRS)](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md)。  
  
 您还可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据源创建模型。  
  
###  <a name="OLEDBAll"></a> OLE DB Data Processing Extension  
 OLE DB 数据处理扩展插件要求根据要在报表中使用的数据源的版本来选择一个附加数据访问接口层。 如果您未选择特定数据访问接口，系统将提供一个默认接口。 请通过“连接属性”对话框（可通过“数据源”或“共享数据源”对话框中的“编辑”按钮访问）选择特定数据访问接口   。  
  
 有关 OLE DB 关联查询设计器详细信息，请参阅 [图形查询设计器用户界面](../../reporting-services/report-data/graphical-query-designer-user-interface.md)。 有关 OLE DB 访问接口特定支持的详细信息，请参阅 [知识库中的](https://support.microsoft.com/default.aspx/kb/811241) Visual Studio .NET 设计器工具支持 OLE DB 访问接口特定信息 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 。  
  
 [返回数据源表](#DataSourcesTable)  
  
####  <a name="OLEDBSQL"></a> OLE DB for SQL Server  
 如果选择数据源类型 **OLE DB**，则要选择一个扩展 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Data Provider for OLE DB 的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据处理扩展插件。 此数据处理扩展插件针对 x86 和 x64 平台进行了本机编译并在这些平台上运行。  
  
 有关详细信息，请参阅 [OLE DB 连接类型 (SSRS)](../../reporting-services/report-data/ole-db-connection-type-ssrs.md)。  
  
 [返回数据源表](#DataSourcesTable)  
  
#### <a name="ole-db-for-olap-70"></a>OLE DB for OLAP 7.0  
 不支持 OLE DB Provider for OLAP Services 7.0。  
  
 [返回数据源表](#DataSourcesTable)  
  
####  <a name="OracleOLEDB"></a> OLE DB for Oracle  
 数据处理扩展插件 OLE DB for Oracle 不支持以下 Oracle 数据类型：BLOB、CLOB、NCLOB、BFILE 和 UROWID。  
  
 此扩展插件支持与位置相关的未命名参数， 但不支持命名参数。 若要使用命名参数，请使用 [Oracle](#OracleClient) 数据处理扩展插件。  
  
 有关将 Oracle 配置为数据源的详细信息，请参阅 [如何使用 Reporting Services 配置和访问 Oracle 数据源](https://support.microsoft.com/kb/834305)。 有关附加权限配置的详细信息，请参阅 [知识库中的](https://support.microsoft.com/kb/870668) 如何为 NETWORK SERVICE 安全主体添加权限 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 。  
  
 [返回数据源表](#DataSourcesTable)  
  
####  <a name="OLEDBStandard"></a> OLE DB Standard .NET Framework 数据访问接口  
 若要从支持 OLE DB [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口的数据源检索数据，请使用 **OLE DB** 数据源类型并选择默认数据访问接口，或者在 **“连接字符串”** 对话框中从已安装的数据访问接口中进行选择。  
  
> [!NOTE]  
>  虽然某个数据访问接口可能支持在报表创作客户端上预览报表，但并非所有 OLE DB 数据访问接口都设计为支持在报表服务器上发布的报表。  
  
 [返回数据源表](#DataSourcesTable)  
  
###  <a name="ODBC"></a> ODBC Data Processing Extension  
 如果选择数据源类型 **ODBC**，则要选择一个扩展 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Data Provider for ODBC 的 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据处理扩展插件。 此数据处理扩展插件针对 x86 和 [!INCLUDE[vcprx64](../../includes/vcprx64-md.md)] 平台进行了本机编译并在这些平台上运行。 使用此扩展插件可连接到具有 ODBC 访问接口的任何数据源并从中检索数据。  
  
> [!NOTE]  
>  虽然某个数据访问接口可能支持在报表创作客户端上预览报表，但并非所有 ODBC 数据访问接口都设计为支持在报表服务器上发布的报表。  
  
 [返回数据源表](#DataSourcesTable)  
  
####  <a name="ODBCGeneric"></a> ODBC Standard .NET Framework 数据访问接口  
 若要从支持标准 ODBC [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 数据访问接口的数据源检索数据，请使用 **ODBC** 数据源类型并选择默认数据访问接口，或者在 **“连接字符串”** 对话框中从已安装的数据访问接口中进行选择。  
  
> [!NOTE]  
>  虽然某个数据访问接口可能支持在报表创作客户端上预览报表，但并非所有 ODBC 数据访问接口都设计为支持在报表服务器上发布的报表。  
  
 [返回数据源表](#DataSourcesTable)  
  
###  <a name="OracleClient"></a> Oracle 数据处理扩展插件  
 选择数据源类型 Oracle 时，即选择 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件，该扩展直接使用 Oracle 数据提供程序，而不再经过 System.Data.OracleClient 这一步  。 若要从 Oracle 数据库中检索报表数据，您的管理员必须安装 Oracle 客户端工具。 该客户端应用程序版本必须为 11g 或更高版本。 这些工具必须安装在报表创作客户端上以便预览报表，同时还应安装在报表服务器上以便查看发布的报表。  
 
若要安装 Oracle 客户端工具，可执行以下操作。
 
1.  转到 [Oracle 的下载站点](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)
2.  下载适用于 Windows（64 位服务器，32 位工具）的 ODAC 12c 第 4 版 (12.1.0.2.4)
3.  安装 Data Provider for .NET 4
  
 此扩展插件支持命名参数。 对于 Oracle 版本 11g 或更高版本而言，支持多值参数。 对于位置相关的未命名参数，请使用 OLE DB 数据处理扩展插件和数据访问接口 [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for Oracle。 有关将 Oracle 配置为数据源的详细信息，请参阅 [如何使用 Reporting Services 配置和访问 Oracle 数据源](https://support.microsoft.com/kb/834305)。 有关附加权限配置的详细信息，请参阅 [知识库中的](https://support.microsoft.com/kb/870668) 如何为 NETWORK SERVICE 安全主体添加权限 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 。  
  
 您可以使用多个输入参数从存储过程中检索数据，但存储过程必须只返回一个输出游标。 有关详细信息，请参阅 [使用 DataReader 检索数据](https://go.microsoft.com/fwlink/?LinkId=81758)中的 Oracle 部分。  
  
 有关详细信息，请参阅 [Oracle 连接类型 (SSRS)](../../reporting-services/report-data/oracle-connection-type-ssrs.md)。 有关关联查询设计器详细信息，请参阅 [图形查询设计器用户界面](../../reporting-services/report-data/graphical-query-designer-user-interface.md)。  
  
 您还可以创建基于 Oracle 数据库的模型。  
  
 [返回数据源表](#DataSourcesTable)  
  
###  <a name="Teradata"></a> Teradata 数据处理扩展插件  
 如果选择数据源类型 **Teradata**，则选择的是扩展 .NET Framework Data Provider for Teradata 的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据处理扩展插件。 若要从 Teradata 数据库检索报表数据，系统管理员必须在报表创作客户端上安装 .NET Framework Data Provider for Teradata 才能在客户端上编辑和预览报表，并且必须在报表服务器上安装它才能查看已发布的报表。  
  
 对于报表服务器项目，没有可用于此扩展插件的图形查询设计器。 必须使用基于文本的查询设计器来创建查询。  
  
 下表显示了在 [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)]中支持使用哪些版本的 .NET Data Provider for Teradata 在报表定义中定义数据源：  
  
|[!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)] 版本|Teradata 数据库版本|.NET Framework Data Provider for Teradata 版本|  
|-----------------------------------|-------------------------------|-------------------------------------------------------|    
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|12.00|12.00.01|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|6.20|12.00.01|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|13.00|13.0.0.1|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|12.00|12.00.01|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|6.20|12.00.01|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|13.00|13.0.0.1|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|6.20|12.00.01|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|12.00|12.00.01|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|13.00|13.0.0.1|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|14.00|14.00.01| 
|SQL Server 2016|13.00|13.0.0.1|  
|SQL Server 2016|14.00|14.00.01|
|SQL Server 2016|15.00|15.00.01| 
  
 此扩展插件支持多值参数。 可以通过在查询模式 TEXT 下使用 EXECUTE 命令在查询中指定宏。  
  
 有关详细信息，请参阅 [Teradata 连接类型 (SSRS)](../../reporting-services/report-data/teradata-connection-type-ssrs.md)。  
  
 您还可以创建基于 Teradata 数据库的模型。 有关更多信息，请参阅 Teradata 网站上的以下白皮书： [Microsoft SQL Server 2012 Reporting Services 和 Teradata Corporation](https://www.teradata.com/white-papers/Microsoft-SQL-Server-2012-Reporting-Services-and-Teradata-Corporation/?type=WP)。  
  
 [返回数据源表](#DataSourcesTable)  
  
###  <a name="SharePointList"></a> SharePoint 列表数据扩展插件  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 包括 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 列表数据扩展插件，以便您可以使用 SharePoint 列表作为报表中数据的源。 您可以从以下数据源中检索列表数据：  
  
-   SharePoint Server 2016  

-   [!INCLUDE[SPS2013](../../includes/sps2013-md.md)]  
  
 SharePoint 列表数据访问接口有三种实现方式。  
  
1.  从 [!INCLUDE[ss_dtbi](../../includes/ss-dtbi-md.md)]上报表生成器或报表设计器之类的报表创作环境中，或者对于在本机模式下配置的报表服务器，列表数据来自用于 SharePoint 站点的 Lists.asmx Web 服务。  
  
2.  在 SharePoint 集成模式下配置的报表服务器上，列表数据或者来自相应的 Lists.asmx Web 服务，或者来自以编程方式对 SharePoint API 的调用。 在此模式下，您可以从 SharePoint 场检索列表数据。  
  
3.  对于 [!INCLUDE[SPS2013](../../includes/sps2013-md.md)] 和 SharePoint Server 2016，通过用于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] SharePoint 技术的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序，你可以从 SharePoint 站点的 Lists.asmx Web 服务检索列表数据，或者从作为 SharePoint 场的一部分的 SharePoint 站点检索数据。 此应用场景也称作“本地模式”  ，因为不需要报表服务器。  
  
 您可以指定的凭据取决于客户端应用程序所使用的实现。 有关详细信息，请参阅 [SharePoint 列表连接类型 (SSRS)](../../reporting-services/report-data/sharepoint-list-connection-type-ssrs.md)。  
  
###  <a name="XML"></a> XML 数据处理扩展插件  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 包含一个 XML 数据处理扩展插件，以便您可以在报表中使用 XML 数据。 使用此插件，可以从 XML 文档、Web 服务或者从可通过 URL 访问的基于 Web 的应用程序检索数据。 有关详细信息，请参阅 [XML 连接类型 (SSRS)](../../reporting-services/report-data/xml-connection-type-ssrs.md)。 有关关联查询设计器的详细信息，请参阅 [图形查询设计器用户界面](../../reporting-services/report-data/graphical-query-designer-user-interface.md)中的“基于文本的查询设计器”部分。 有关示例，请参阅 [Reporting Services: Using XML and Web Service Data Sources（Reporting Services：使用 XML 和 Web 服务数据源）](https://go.microsoft.com/fwlink/?LinkId=81654)。  
  
 [返回数据源表](#DataSourcesTable)  
  
###  <a name="SAPBW"></a> SAP BW 数据处理扩展插件  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 包含一个数据处理扩展插件，通过此扩展插件，可以在报表中使用来自 SAP BW 数据源的数据。
  
 [返回数据源表](#DataSourcesTable)  
  
###  <a name="Hyperion"></a> Hyperion Essbase 商业智能数据处理扩展插件  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 包含一个数据处理扩展插件，通过此扩展插件，您可以在报表中使用来自 [!INCLUDE[extEssbase](../../includes/extessbase-md.md)] 数据源的数据。  
  
 有关详细信息，请参阅 [Hyperion Essbase 连接类型 (SSRS)](../../reporting-services/report-data/hyperion-essbase-connection-type-ssrs.md)。 有关关联的查询设计器的详细信息，请参阅 [Hyperion Essbase Query Designer User Interface](../../reporting-services/report-data/hyperion-essbase-query-designer-user-interface.md)。  
  
 有关 [!INCLUDE[extEssbase](../../includes/extessbase-md.md)]的详细信息，请参阅 [将 SQL Server 2005 Reporting Services 与 Hyperion Essbase 一起使用](https://go.microsoft.com/fwlink/?LinkId=81970)。  
  
 [返回数据源表](#DataSourcesTable)  
  
## <a name="see-also"></a>另请参阅  
 [数据连接、数据源和连接字符串（报表生成器和 SSRS）](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [报表数据集 (SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md)  
 更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
  
  
