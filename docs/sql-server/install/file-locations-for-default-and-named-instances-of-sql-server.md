---
title: SQL Server 的默认实例和命名实例的文件位置 | Microsoft Docs
ms.custom: ''
ms.date: 08/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 463c570e-9f75-4653-b3b8-4d61753b0013
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f6e34fbab18c987a635250bba5557a4d3a214b44
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68126214"
---
# <a name="file-locations-for-default-and-named-instances-of-sql-server"></a>SQL Server 的默认实例和命名实例的文件位置
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将安装一个或多个单独的实例。 无论是默认实例还是命名实例都有自己的一组程序文件和数据文件，同时还有在计算机上的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例之间共享的一组公共文件。  
  
 对于包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssDE](../../includes/ssde-md.md)]和 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]实例，每个组件都有一套完整的数据文件和可执行文件，以及由所有组件共享的公共文件。  
  
 为了隔离每个组件的安装位置，将为给定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中的每个组件都生成一个唯一的实例 ID。  
  
> [!IMPORTANT]  
>  程序文件和数据文件不能安装在以下位置：可移动磁盘驱动器、使用压缩的文件系统、系统文件所在的目录以及故障转移群集实例上的共享驱动器。  
>  
>  可能需要安装扫描软件（如防病毒应用程序和反间谍应用程序）以排除 SQL Server 文件夹和文件类型。 有关详细信息，请参阅以下支持文章：[运行 SQL Server 的计算机上的防病毒软件](https://support.microsoft.com/kb/309422)。
> 
>  在安装系统数据库（master、model、MSDB 和 tempdb）和 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 用户数据库时可以选择 Server Message Block (SMB) 文件服务器作为存储。 这同时适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 独立安装和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集安装 (FCI)。 有关详细信息，请参阅 [安装 SQL Server，并使用 SMB 文件共享作为存储选项](../../database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option.md)。  
>   
>  请勿删除以下任何目录或其内容：Binn、Data、Ftdata、HTML 或 1033。 如有必要，可以删除其他目录；但是，如果不卸载并重新安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则可能无法检索失去的功能或数据。 不要删除或修改 HTML 目录中的任何 .htm 文件。 它们对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具的正常运行是必需的。  
  
## <a name="shared-files-for-all-instances-of-includessnoversionincludesssnoversion-mdmd"></a>所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 单台计算机上所有实例使用的公共文件安装在以下文件夹中：[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]。 其中 \<drive> 是安装组件的驱动器号。 默认值通常为驱动器 C。\<nnn> 标识版本。 下表标识路径的版本。 

|\<nnn>|版本
|-----|-----
|140|[!INCLUDE[ssqlv14](../../includes/sssqlv14-md.md)]
|130|[!INCLUDE[ssqlv13](../../includes/sssql15-md.md)]
|120|SQL Server 2014
|110|[!INCLUDE[sssql11](../../includes/sssql11-md.md)] 
  
## <a name="file-locations-and-registry-mapping"></a>文件位置和注册表映射  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中，为每个服务器组件生成一个实例 ID。 此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中的服务器组件分别是 [!INCLUDE[ssDE](../../includes/ssde-md.md)]、 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]。  
  
 使用以下格式构造默认实例 ID：  
  
-   对于 [!INCLUDE[ssDE](../../includes/ssde-md.md)]采用的是 MSSQL，后面依次跟有主版本号、下划线和次版本号（如果适用）、一个句点以及实例名。  
  
-   对于 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]采用的是 MSAS，后面依次跟有主版本号、下划线和次版本号（如果适用）、一个句点以及实例名。  
  
-   对于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]采用的是 MSRS，后面依次跟有主版本号、下划线和次版本号（如果适用）、一个句点以及实例名。  
  
 此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本中的默认实例 ID 的示例如下：  
  
-   对于 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 默认实例，为 MSSQL14.MSSQLSERVER。  
  
-   对于 [!INCLUDE[ssASCurrent](../../includes/ssascurrent-md.md)] 默认实例，为 MSAS14.MSSQLSERVER。  
  
-   对于名为“MyInstance”的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 命名实例，为 MSSQL14.MyInstance。  
  
    >[!NOTE]
    >实例 ID 路径中的两位数字标识版本号。 在上述示例中，版本号 14 为 [!INCLUDE[ssqlv14](../../includes/sssqlv14-md.md)]。 

 包括 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 和 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]命名实例（名为“MyInstance”并且按照默认目录安装）的目录结构如下所示：  
  
-   C:\Program Files\Microsoft SQL Server\MSSQL14.MyInstance\  
  
-   C:\Program Files\Microsoft SQL Server\MSAS14.MyInstance\  
  
 可为实例 ID 指定任何值，但应避免使用特殊字符和保留关键字。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装期间可指定一个非默认实例 ID。 如果用户选择更改默认安装目录，则不使用 \<Program Files>\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，而使用 \<自定义路径\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 请注意，不支持以下划线 (_) 开头或者包含数字符号 (#) 或美元符号 ($) 的实例 ID。  
  
> [!NOTE]  
>  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和客户端组件是不识别实例的，因此不为它们指定实例 ID。 默认情况下，将不识别实例的组件安装在单个目录 [!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]中。 更改一个共享组件的安装路径会同时更改其他共享组件的安装路径。 后续安装会将非实例识别组件安装到与原始安装相同的目录。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 是唯一的在安装后支持实例重命名的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件。 如果重命名 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例，实例 ID 不会发生变化。 重命名实例后，目录和注册表项将继续使用在安装期间创建的实例 ID。  
  
 将在 HKLM\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<*Instance_ID*> 下为识别实例的组件创建注册表配置单元。 例如，  
  
-   HKLM\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.MyInstance  
  
-   HKLM\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSAS14.MyInstance  
  
-   HKLM\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSRS14.MyInstance  
  
 注册表还维护实例 ID 到实例名的映射。 实例 ID 到实例名的映射按如下方式维护：  
  
-   [HKEY_LOCAL_MACHINE\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\Instance Names\SQL] "InstanceName"="MSSQL14"  
  
-   [HKEY_LOCAL_MACHINE\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\Instance Names\OLAP] "InstanceName"="MSAS14"  
  
-   [HKEY_LOCAL_MACHINE\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\Instance Names\RS] "InstanceName"="MSRS14"  
  
## <a name="specifying-file-paths"></a>指定文件路径  
 安装过程中，可以更改下列功能的安装路径：  
  
 安装程序中仅显示具有用户可配置目标文件夹的功能的安装路径：  
  
|组件|默认路径|可配置或固定路径|  
|---------------|------------------|--------------------------------|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] 服务器组件|\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.\<InstanceID>\ |可配置|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] 数据文件|\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.\<InstanceID>\ |可配置|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务器|\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSAS14.\<InstanceID>\ |可配置|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据文件|\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSAS14.\<InstanceID>\ |可配置|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表服务器|\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSRS14.\<InstanceID>\Reporting Services\ReportServer\Bin\ |可配置|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表管理器|\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSRS14.\<InstanceID>\Reporting Services\ReportManager\ |固定路径|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|\<Install Directory>\140\DTS\\ <sup>1</sup> |可配置 |  
|客户端组件（bcp.exe 和 sqlcmd.exe 除外）|\<Install Directory>\140\Tools\\ <sup>1</sup> |可配置 |  
|客户端组件（bcp.exe 和 sqlcmd.exe）|\<安装目录>\Client SDK\ODBC\110\Tools\Binn|固定路径|  
|复制和服务器端 COM 对象|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]COM\\ <sup>2</sup> |固定路径|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 用于数据转换运行时引擎、数据转换管道引擎和 **dtexec** 命令提示实用工具的组件 DLL|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|固定路径|  
|为 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Connections|固定路径|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 支持的每种类型枚举器的 DLL|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\ForEachEnumerators|固定路径|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器服务、WMI 提供程序|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Shared\ |固定路径|  
|所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Shared\ |固定路径|  
  
> [!WARNING]
> 确保通过限制权限对 \Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\ 文件夹进行保护。  
  
注意，文件位置的默认驱动器为 *systemdrive*，通常为驱动器 C。子功能的安装路径由父功能的安装路径确定。  
  
<sup>1</sup> [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和客户端组件共享单个安装路径。 更改一个组件的安装路径会同时更改其他组件的安装路径。 后续安装将组件安装到与原始安装相同的位置。  
  
<sup>2</sup> 此目录由计算机上的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例使用。 如果对计算机上的任一实例应用更新，则对此文件夹中的文件所做的任何更改都将影响计算机上的所有实例。 向现有安装添加功能时，不能更改以前安装的功能的位置，也不能为新功能指定该位置。 必须将其他功能安装到安装程序已建立的目录，或者卸载并重新安装产品。  
  
> [!NOTE]  
>  对于群集配置，必须选择在该群集的每个节点上都可用的本地驱动器。  
  
 当在安装过程中为服务器组件或数据文件指定安装路径时，安装程序除了为程序和数据文件使用指定的位置外，还使用实例 ID。 安装程序不会将实例 ID 用于工具和其他共享文件。 安装程序也不会将任何实例 ID 用于 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 程序和数据文件，尽管它会将实例 ID 用于 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 存储库。  
  
 如果为 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 功能设置了安装路径，则对于此次安装， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序会将该路径用作所有特定于实例的文件夹（包括 SQL 数据文件）的根目录。 在这种情况下，如果将根目录设置为“C:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.\<InstanceName>\MSSQL\\”，则特定于实例的目录将添加到此路径的末尾。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导（安装程序用户界面模式）中选择使用 USESYSDB 升级功能的客户会很容易将产品安装到递归文件夹结构中。 例如，\<SQLProgramFiles>\MSSQL14\MSSQL\MSSQL10_50\MSSQL\Data\\。 这时，为了使用 USESYSDB 功能，请为 SQL 数据文件功能而非 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 功能设置安装路径。  
  
> [!NOTE]
>  数据文件始终应位于名为 Data 的子目录中。 例如，当数据文件位于 C:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.\<InstanceName>\MSSQL\Data 下时，应指定 C:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.\<InstanceName>\ 作为升级期间系统数据库的数据目录的根路径。  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎配置 - 数据目录](https://msdn.microsoft.com/library/9b1fa0fc-623b-479a-afc3-4f13bd850487)   
 [Analysis Services 配置 - 数据目录](https://msdn.microsoft.com/library/ef732855-b7af-4f40-a619-5573c1c354bb)  
  
  
