---
title: SERVERPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SERVERPROPERTY_TSQL
- SERVERPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- SERVERPROPERTY function
- server instance property information [SQL Server]
- IsHadrEnabled server property
- instances of SQL Server, property information
- server properties [SQL Server]
ms.assetid: 11e166fa-3dd2-42d8-ac4b-04f18c612c4a
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7f01150c469af8d12535e6858bb68ec9b758aab8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68111309"
---
# <a name="serverproperty-transact-sql"></a>SERVERPROPERTY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  返回有关服务器实例的属性信息。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
SERVERPROPERTY ( 'propertyname' )  
```  
  
## <a name="arguments"></a>参数  
 propertyname   
 一个表达式，包含要为服务器返回的属性信息。 propertyname 可以是下列值之一  。  
  
|属性|返回的值|  
|--------------|---------------------|  
|BuildClrVersion|在生成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 公共语言运行时 (CLR) 的版本。<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：nvarchar(128) |  
|排序规则|服务器的默认排序规则名称。<br /><br /> NULL = 输入无效或错误。<br /><br /> 基本数据类型：nvarchar(128) |  
|CollationID|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则的 ID。<br /><br /> 基本数据类型：int |  
|ComparisonStyle|排序规则的 Windows 比较样式。<br /><br /> 基本数据类型：int |  
|ComputerNamePhysicalNetBIOS|当前运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的本地计算机的 NetBIOS 名称。<br /><br /> 对于故障转移群集上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 群集实例，当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例故障转移到故障转移群集中的其他节点时，该值将更改。<br /><br /> 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的独立实例上，该值保持不变，并返回与 MachineName 属性相同的值。<br /><br /> **注意：** 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例位于故障转移群集中，并且您要获取故障转移群集实例的名称，请使用 MachineName 属性。<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：nvarchar(128) |  
|版本|所安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的产品版本。 使用该属性的值确定功能和限制，例如[按 SQL Server 版本划分的计算能力限制](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)。 64 位版本的[!INCLUDE[ssDE](../../includes/ssde-md.md)]向此版本追加（64 位）。<br /><br /> 返回：<br /><br /> “Enterprise Edition”<br /><br /> “Enterprise Edition:基于内核的许可”<br /><br /> “Enterprise Evaluation Edition”<br /><br /> “Business Intelligence Edition”<br /><br /> “Developer Edition”<br /><br /> “Express Edition”<br /><br /> “Express Edition with Advanced Services”<br /><br /> “Standard Edition”<br /><br /> “Web Edition”<br /><br /> “SQL Azure”表示 [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 或 [!INCLUDE[ssDW](../../includes/ssdw-md.md)]<br /><br /> 基本数据类型：nvarchar(128) |  
|EditionID|EditionID 表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的已安装产品版本。 使用该属性的值确定功能和限制，例如[按 SQL Server 版本划分的计算能力限制](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)。<br /><br /> 1804890536 = Enterprise<br /><br /> 1872460670 = Enterprise Edition：基于内核的许可<br /><br /> 610778273= Enterprise Evaluation<br /><br /> 284895786 = Business Intelligence<br /><br /> -2117995310 = Developer<br /><br /> -1592396055 = Express<br /><br /> -133711905= Express with Advanced Services<br /><br /> -1534726760 = Standard<br /><br /> 1293598313 = Web<br /><br /> 1674378470 = SQL 数据库或 SQL 数据仓库<br /><br /> 基本数据类型：bigint |  
|EngineEdition|服务器上安装的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]版本。<br /><br /> 1 = Personal 或 Desktop Engine（不适用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更高版本。）<br /><br /> 2 = Standard（对 Standard、Web 和 Business Intelligence 返回该值。）<br /><br /> 3 = Enterprise（对 Enterprise、Developer 以及两个 Enterprise 版本返回该值。）<br /><br /> 4 = Express（对 Express、Express with Tools 和 Express with Advanced Services 返回该值）<br /><br /> 5 = [!INCLUDE[ssSDS](../../includes/sssds-md.md)]<br /><br /> 6 - [!INCLUDE[ssDW](../../includes/ssdw-md.md)]<br /><br /> 8 = 托管实例<br /><br /> 基本数据类型：int |  
|HadrManagerStatus|**适用范围**： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 指示 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]管理器是否已启动。<br /><br /> 0 = 未启动，通信挂起。<br /><br /> 1 = 已启动，正在运行。<br /><br /> 2 = 未启动，已失败。<br /><br /> NULL = 输入无效，出现错误或不适用。|  
|InstanceDefaultDataPath|适用范围：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到当前版本 2015 年底开始的更新  。<br /><br /> 实例数据文件的默认路径的名称。|  
|InstanceDefaultLogPath|适用范围：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到当前版本 2015 年底开始的更新  。<br /><br /> 实例日志文件的默认路径的名称。|  
|InstanceName|用户连接到的实例的名称。<br /><br /> 如果实例名称是默认实例，或者输入无效或发生错误，则返回 NULL。<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：nvarchar(128) |  
|IsAdvancedAnalyticsInstalled|如果安装期间安装了高级分析功能，则返回 1；如果未安装，则返回 0。|  
|IsClustered|服务器实例已配置在故障转移群集中。<br /><br /> 1 = 群集。<br /><br /> 0 = 非群集。<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：int |  
|IsFullTextInstalled|全文和语义索引组件安装在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的当前实例上。<br /><br /> 1 = 已安装全文和语义索引组件。<br /><br /> 0 = 未安装全文和语义索引组件。<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：int |  
|IsHadrEnabled|**适用范围**： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 在此服务器实例上启用 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]。<br /><br /> 0 = [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 功能已禁用。<br /><br /> 1 = [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 功能已启用。<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：int <br /><br /> 要创建可用性副本并使该副本在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上运行，必须在该服务器实例上启用 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]。 有关详细信息，请参阅[启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)。<br /><br /> **注意：** IsHadrEnabled 属性仅与 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 相关。 其他高可用性或灾难恢复功能（如数据库镜像或日志传送）都不受此服务器属性影响。|  
|IsIntegratedSecurityOnly|服务器处于集成安全性模式下。<br /><br /> 1 = 集成安全性（Windows 身份验证）<br /><br /> 0 = 非集成安全性。 （Windows 身份验证和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。）<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：int |  
|IsLocalDB|**适用范围**： [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 服务器是 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] LocalDB 的实例。<br /><br /> NULL = 输入无效，出现错误或不适用。|  
|IsPolyBaseInstalled|**适用于**： [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 返回服务器实例是否安装了 PolyBase 功能的结果。<br /><br /> 0 = 未安装 PolyBase。<br /><br /> 1 = 安装了 PolyBase。<br /><br /> 基本数据类型：int |  
|IsSingleUser|服务器处于单用户模式下。<br /><br /> 1 = 单个用户。<br /><br /> 0 = 非单个用户<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：int |  
|IsXTPSupported|**适用对象**：SQL Server（[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]）、[!INCLUDE[ssSDS](../../includes/sssds-md.md)]。<br /><br /> 服务器支持内存中 OLTP。<br /><br /> 1= 服务器支持内存中 OLTP。<br /><br /> 0= 服务器不支持内存中 OLTP。<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：int |  
|LCID|排序规则的 Windows 区域设置标识符 (LCID)。<br /><br /> 基本数据类型：int |  
|LicenseType|未使用。 许可证信息不由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品保留或维护。 始终返回 DISABLED。<br /><br /> 基本数据类型：nvarchar(128) |  
|MachineName|运行服务器实例的 Windows 计算机名称。<br /><br /> 对于群集实例，即在 Microsoft 群集服务的虚拟服务器上运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，返回虚拟服务器的名称。<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：nvarchar(128) |  
|NumLicenses|未使用。 许可证信息不由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品保留或维护。 始终返回 NULL。<br /><br /> 基本数据类型：int |  
|ProcessID|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的进程 ID。 ProcessID 对于标识属于该实例的 Sqlservr.exe 很有用。<br /><br /> NULL = 输入无效，出现错误或不适用。<br /><br /> 基本数据类型：int |  
|ProductBuild|适用范围：2015 年 10 月开始的 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  。<br /><br /> 生成号。|  
|ProductBuildType|适用范围：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到当前版本 2015 年底开始的更新  。<br /><br /> 当前版本的版本类型。<br /><br /> 返回下列项之一：<br /><br /> OD = 特定客户按需版本。<br /><br /> GDR = 通过 Windows 更新发布的常规分发版本。<br /><br /> NULL<br />= 不适用。|  
|ProductLevel|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的版本级别。<br /><br /> 返回下列项之一：<br /><br /> “RTM”= 原始发布版本<br /><br /> “SPn”= 服务包版本 <br /><br /> “CTPn”= 社区技术预览版版本 <br /><br /> 基本数据类型：nvarchar(128) |  
|ProductMajorVersion|适用范围：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到当前版本 2015 年底开始的更新  。<br /><br /> 主版本。|  
|ProductMinorVersion|适用范围：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到当前版本 2015 年底开始的更新  。<br /><br /> 次版本。|  
|ProductUpdateLevel|适用范围：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到当前版本 2015 年底开始的更新  。<br /><br /> 当前版本的更新级别。 CU 表示累积更新。<br /><br /> 返回下列项之一：<br /><br /> CUn = 累积更新 <br /><br /> NULL<br />= 不适用。|  
|ProductUpdateReference|适用范围：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到当前版本 2015 年底开始的更新  。<br /><br /> 该版本的知识库文章。|  
|ProductVersion|实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本，格式为“major.minor.build.revision”  。<br /><br /> 基本数据类型：nvarchar(128) |  
|ResourceLastUpdateDateTime|返回资源数据库上次更新的日期和时间。<br /><br /> 基本数据类型：datetime |  
|ResourceVersion|返回版本资源数据库。<br /><br /> 基本数据类型：nvarchar(128) |  
|ServerName|Windows 服务器和与指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例关联的实例信息。<br /><br /> NULL = 输入无效或错误。<br /><br /> 基本数据类型：nvarchar(128) |  
|SqlCharSet|排序规则 ID 中的 SQL 字符集 ID。<br /><br /> 基本数据类型：tinyint |  
|SqlCharSetName|来自排序规则的 SQL 字符集名称。<br /><br /> 基本数据类型：nvarchar(128) |  
|SqlSortOrder|排序规则中的 SQL 排序顺序 ID<br /><br /> 基本数据类型：tinyint |  
|SqlSortOrderName|来自排序规则的 SQL 排序顺序名称。<br /><br /> 基本数据类型：nvarchar(128) |  
|FilestreamShareName|FILESTREAM 使用的共享的名称。<br /><br /> NULL = 输入无效，出现错误或不适用。|  
|FilestreamConfiguredLevel|FILESTREAM 访问的配置级别。 有关详细信息，请参阅[文件流访问级别](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md)。|  
|FilestreamEffectiveLevel|FILESTREAM 访问的有效级别。 如果级别已更改，并且实例重新启动或计算机重新启动处于挂起状态，则该值可以不同于 FilestreamConfiguredLevel。 有关详细信息，请参阅[文件流访问级别](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md)。|  
  
## <a name="return-types"></a>返回类型  
 **sql_variant**  
  
## <a name="remarks"></a>Remarks  
  
### <a name="servername-property"></a>ServerName 属性  
 `SERVERPROPERTY` 函数的 `ServerName` 属性和 [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) 返回相似的信息。 `ServerName` 属性提供 Windows 服务器和实例名称，两者共同构成唯一的服务器实例。 [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) 提供当前配置的本地服务器名称。  
  
 如果安装时未更改默认服务器名称，则 `ServerName` 属性和 [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) 返回相同的信息。 可以通过执行以下过程配置本地服务器的名称：  
  
```  
EXEC sp_dropserver 'current_server_name';  
GO  
EXEC sp_addserver 'new_server_name', 'local';  
GO  
```  
  
 如果在安装时已将本地服务器名称从默认服务器名称更改为其他名称，则 [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) 返回更改后的新名称。  
  
### <a name="version-properties"></a>版本属性  
 `SERVERPROPERTY` 函数返回与版本信息有关的各个属性，而 [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md) 函数将输出合并为一个字符串。 如果应用程序需要各个属性字符串，则可以使用 `SERVERPROPERTY` 函数返回它们，而不是分析 [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md) 结果。  

> [!NOTE]  
> 我们已注意到一个问题，即 SERVERPROPERTY 报告的版本属性对于 Azure SQL 数据库来说不正确。 Azure SQL 数据库运行的 SQL Server 数据库引擎的版本始终领先于 SQL Server 的本地版本，且包含最新安全修补程序。 这意味着，此修补程序级别始终等同于或领先于 SQL Server 的本地版本，且 SQL Server 中提供的最新功能在 Azure SQL 数据库中可用。
>
> 若要以编程方式确定引擎版本，请使用 SELECT SERVERPROPERTY('EngineEdition')。 此查询将在 Azure SQL 数据库中针对单一数据库/弹性池返回“5”，针对托管实例返回“8”。 
>
> 解决此问题后，我们将更新文档。

## <a name="permissions"></a>权限

所有用户都可以查询服务器属性。 
  
## <a name="examples"></a>示例  
 下面的示例在 `SELECT` 语句中使用 `SERVERPROPERTY` 函数返回有关当前 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 实例的信息。   
  
```  
SELECT  
  SERVERPROPERTY('MachineName') AS ComputerName,
  SERVERPROPERTY('ServerName') AS InstanceName,  
  SERVERPROPERTY('Edition') AS Edition,
  SERVERPROPERTY('ProductVersion') AS ProductVersion,  
  SERVERPROPERTY('ProductLevel') AS ProductLevel;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 2016 的版本和组件](../../sql-server/editions-and-components-of-sql-server-2016.md)  
  
  
