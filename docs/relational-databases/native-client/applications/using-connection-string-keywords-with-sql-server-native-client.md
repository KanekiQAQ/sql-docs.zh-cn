---
title: 将连接字符串关键字用于 SQL Server 本机客户端 |Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
f1_keywords:
- sql13.swb.connecttoserver.options.registeredservers.f1
helpviewer_keywords:
- data access [SQL Server Native Client], connection string keywords
- SQLNCLI, connection string keywords
- connection strings [SQL Server Native Client]
- SQL Server Native Client, connection string keywords
ms.assetid: 16008eec-eddf-4d10-ae99-29db26ed6372
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d7d5bd6584316ceb1da4462ab2459781b1cf1e32
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68069187"
---
# <a name="using-connection-string-keywords-with-sql-server-native-client"></a>将连接字符串关键字用于 SQL Server Native Client
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  某些 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client API 使用连接字符串来指定连接属性。 连接字符串是关键字和关联的值的列表；每个关键字标识特定的连接属性。  

> [!IMPORTANT]
> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB (SQLNCLI) 保持不推荐使用，不建议用于新的开发工作。 相反，使用新[Microsoft OLE DB 驱动程序适用于 SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL) 这将使用最新的服务器功能更新。    
> 有关信息，请参阅[OLE DB 驱动程序适用于 SQL Server 连接字符串关键字](../../../connect/oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)。

> [!NOTE]
> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 允许连接字符串中存在多义性，以保持向后兼容（例如，可以多次指定某些关键字，并且允许基于位置或优先级对发生冲突的关键字进行解析）。 在修改要使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 的应用程序时，最好消除对连接字符串多义性的任何依赖。  
  
 下面几节描述了可以在使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 作为数据提供程序时用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口、[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序和 ActiveX 数据对象 (ADO) 的关键字。  
  
## <a name="odbc-driver-connection-string-keywords"></a>ODBC 驱动程序连接字符串关键字  
 ODBC 应用程序使用连接字符串作为参数[SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)并[SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)函数。  
  
 ODBC 使用的连接字符串有以下语法：  
  
 `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 `empty-string ::=`  
  
 `attribute ::= attribute-keyword=[{]attribute-value[}]`  
  
 `attribute-value ::= character-string`  
  
 `attribute-keyword ::= identifier`  
  
 属性值可以选择放在大括号中，并且最好这样做。 这样可以避免当属性值包含非字母数字字符时出现问题。 值中的第一个右大括号用于终止值，因此值不能包含右大括号。  
  
 下表对可能与 ODBC 连接字符串一起使用的关键字进行了说明。  
  
|关键字|描述|  
|-------------|-----------------|  
|**Addr**|“Address”的同义词。|  
|**Address**|运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的服务器的网络地址。 “Address”通常是服务器的网络名称，也可以是诸如管道、IP 地址或 TCP/IP 端口和套接字地址之类的其他名称  。<br /><br /> 如果指定了 IP 地址，请确保在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器中启用了 TCP/IP 或 named pipes 协议。<br /><br /> 值**地址**传递给的值的优先级高于**服务器**时使用的 ODBC 连接字符串中[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]本机客户端。 另外请注意，`Address=;` 将连接到在 Server 关键字中指定的服务器，而 `Address= ;, Address=.;`、 `Address=localhost;` 和 `Address=(local);` 都会产生本地服务器的连接  。<br /><br /> Address 关键字的完整语法如下所示  ：<br /><br /> [_protocol_ **:** ]*Address*[ **,** _port &#124;\pipe\pipename_]<br /><br /> *protocol* 可以是 **tcp** (TCP/IP)、 **lpc** （共享内存）或 **np** （命名管道）。 有关协议的详细信息，请参阅[配置客户端协议](../../../database-engine/configure-windows/configure-client-protocols.md)。<br /><br /> 如果既没有*协议*也不**网络**指定关键字，则[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]本机客户端将使用中指定的协议顺序[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]配置管理器。<br /><br /> “port”是指定服务器上所要连接到的端口  。 默认情况下，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用端口 1433。|  
|**AnsiNPW**|如果是“yes”，则驱动程序使用 ANSI 定义的行为来处理 NULL 比较、字符数据填充、警告和 NULL 串联。 如果是“no”，则不公开 ANSI 定义的行为。 有关 ANSI NPW 行为的详细信息，请参阅[效果的 ISO 选项](../../../relational-databases/native-client-odbc-queries/executing-statements/effects-of-iso-options.md)。|  
|**APP**|应用程序调用的名称[SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md) （可选）。 如果指定，此值存储在**master.dbo.sysprocesses**列**program_name**并返回由[sp_who](../../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)和[a p p _](../../../t-sql/functions/app-name-transact-sql.md)函数。|  
|**ApplicationIntent**|连接到服务器时声明应用程序工作负荷类型。 可能的值为 ReadOnly 和 ReadWrite   。 默认值是**ReadWrite**。  例如：<br /><br /> `ApplicationIntent=ReadOnly`<br /><br /> 有关详细信息[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]对本机客户端的支持[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]，请参阅[SQL Server 本机客户端支持对高可用性和灾难恢复](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。|  
|**AttachDBFileName**|可附加数据库的主文件的名称。 如果使用 C 字符串变量，请包括完整路径，并将所有 \ 字符转义：<br /><br /> `AttachDBFileName=c:\\MyFolder\\MyDB.mdf`<br /><br /> 附加此数据库并使其成为连接的默认数据库。 若要使用**AttachDBFileName**还必须指定数据库名称中任意一种[SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)数据库参数或 SQL_COPT_CURRENT_CATALOG 连接属性。 如果该数据库以前已经附加，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不重新附加它；而是使用已附加的数据库作为连接的默认数据库。|  
|**AutoTranslate**|如果是“yes”，则客户端和服务器之间发送的 ANSI 字符串将通过 Unicode 进行转换，以使在客户端与服务器的代码页之间匹配扩展字符的问题最小化。<br /><br /> 客户端 SQL_C_CHAR 数据发送到[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **char**， **varchar**，或**文本**变量、 参数或列从字符转换为 Unicode 使用客户端ANSI 代码页 (ACP)，然后从 Unicode 字符使用服务器的 ACP 转换。<br /><br /> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **char**， **varchar**，或**文本**发送到客户端 SQL_C_CHAR 变量的数据是从字符转换为 Unicode 使用服务器 ACP，则从 Unicode 字符使用客户端ACP。<br /><br /> 这些转换由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序在客户端上执行。 这要求在服务器上使用的相同 ANSI 代码页 (ACP) 在客户端上可用。<br /><br /> 这些设置对于为下面这些传输而发生的转换无效：<br /><br /> \* Unicode SQL_C_WCHAR 客户端数据发送到**char**， **varchar**，或**文本**在服务器上。<br /><br /> &#42;**char**， **varchar**，或**文本**发送到客户端上的 Unicode SQL_C_WCHAR 变量的服务器数据。<br /><br /> \* ANSI SQL_C_CHAR 客户端数据发送到 Unicode **nchar**， **nvarchar**，或**ntext**在服务器上。<br /><br /> \* Unicode **nchar**， **nvarchar**，或**ntext**服务器数据发送到客户端上的 ANSI SQL_C_CHAR 变量。<br /><br /> 如果是“no”，则不执行字符转换。<br /><br /> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序不会转换发送到客户端 ANSI 字符 SQL_C_CHAR 数据**char**， **varchar**，或**文本**变量、 参数，或服务器上的列。 在执行任何转换**char**， **varchar**，或**文本**从服务器发送到客户端上的 SQL_C_CHAR 变量的数据。<br /><br /> 如果客户端和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 正在使用不同的 ACP，则扩展字符可能被错误解释。|  
|**“数据库”**|连接的默认 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库的名称。 如果**数据库**未指定，则使用为登录定义的默认数据库。 来自 ODBC 数据源的默认数据库将覆盖为登录定义的默认数据库。 该数据库必须是现有数据库，除非**AttachDBFileName**还指定了。 如果**AttachDBFileName**还指定了，它指向的主文件已连接，并由指定的数据库名称**数据库**。|  
|**驱动程序**|通过返回的驱动程序的名称[SQLDrivers](../../../relational-databases/native-client-odbc-api/sqldrivers.md)。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序的关键字值是“{SQL Server Native Client 11.0}”。 **服务器**关键字是必需的如果**驱动程序**指定并**DriverCompletion**设置为 SQL_DRIVER_NOPROMPT。<br /><br /> 有关驱动程序名称的详细信息，请参阅[使用 SQL Server Native Client 头文件和库文件](../../../relational-databases/native-client/applications/using-the-sql-server-native-client-header-and-library-files.md)。|  
|**DSN**|现有 ODBC 用户或系统数据源的名称。 此关键字将覆盖可能中指定任何值**服务器**，**网络**，并**地址**关键字。|  
|**Encrypt**|指定在通过网络发送数据之前是否应当将其加密。 可能的值为“yes”和“no”。 默认值为“no”。|  
|**Fallback**|不推荐使用该关键字，并且 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序将忽略其设置。|  
|**Failover_Partner**|不能与主服务器建立连接时要使用的故障转移伙伴服务器的名称。|  
|**FailoverPartnerSPN**|故障转移伙伴的 SPN。 默认值为空字符串。 空字符串导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用驱动程序生成的默认 SPN。|  
|**FileDSN**|现有 ODBC 文件数据源的名称。|  
|**语言**|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 语言名称（可选）。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 可以存储在多个语言的消息**sysmessages**。 如果连接到[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]使用多个语言时，**语言**指定哪组消息使用的连接。|  
|**MARS_Connection**|启用或禁用连接上的多个活动结果集 (MARS)。 可识别的值为“yes”和“no”。 默认值为"否"。|  
|**MultiSubnetFailover**|始终指定**multiSubnetFailover = Yes**连接到的可用性组侦听器时[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]可用性组或[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]故障转移群集实例。 **multiSubnetFailover = Yes**配置[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client 以便更快地检测和连接到 （当前） 活动服务器。 可能的值为“是”  和“否”  。 默认值为 No  。 例如：<br /><br /> `MultiSubnetFailover=Yes`<br /><br /> 有关详细信息[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]对本机客户端的支持[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]，请参阅[SQL Server 本机客户端支持对高可用性和灾难恢复](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。|  
|**Net**|“Network”的同义词。|  
|**Network**|有效的值为**dbnmpntw** （命名管道） 和**dbmssocn** (TCP/IP)。<br /><br /> 它是错误指定了一个值，**网络**关键字和协议上前缀**Server**关键字。|  
|**PWD**|在 UID 参数中指定的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录帐户的密码。 **PWD**如果登录有 NULL 密码或使用 Windows 身份验证时无需指定 (`Trusted_Connection = yes`)。|  
|**QueryLog_On**|如果是“yes”，则在连接上启用对长时间运行的查询数据的日志记录。 如果是“no”，则不记录长时间运行的查询数据。|  
|**QueryLogFile**|文件的完整路径和文件名，该文件用于记录长时间运行的查询上的数据。|  
|**QueryLogTime**|数字字符串，指定记录长时间运行的查询的阈值（毫秒）。 未在指定时间内获得响应的任何查询将写入长时间运行查询日志文件。|  
|**QuotedId**|如果是“yes”，则 QUOTED_IDENTIFIERS 对连接设置为 ON，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用关于 SQL 语句中引号方法的 ISO 规则。 如果是“no”，则 QUOTED_IDENTIFIERS 对连接设置为 OFF。 然后，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 遵循关于 SQL 语句中引号用法的早期 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 规则。 有关详细信息，请参阅[效果的 ISO 选项](../../../relational-databases/native-client-odbc-queries/executing-statements/effects-of-iso-options.md)。|  
|**Regional**|如果是“yes”，则在将货币、日期和时间数据转换为字符数据时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序将使用客户端设置。 该转换是单向的；驱动程序不会识别其中非 ODBC 标准格式的日期字符串或货币值；例如，在 INSERT 或 UPDATE 语句中使用的参数。 如果是“no”，则驱动程序使用 ODBC 标准字符串来表示转换为字符数据的货币、日期和时间数据。|  
|**SaveFile**|如果连接成功则在其中保存当前连接的属性的 ODBC 数据源文件的名称。|  
|**Server**|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。 该值必须是服务器的网络名称、IP 地址或者 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器别名。<br /><br /> **地址**关键字将覆盖**Server**关键字。<br /><br /> 通过指定以下条件之一，可连接到本地服务器的默认实例：<br /><br /> **Server=;**<br /><br /> **Server=.;**<br /><br /> **Server=(local);**<br /><br /> **Server=(local);**<br /><br /> **Server=(localhost);**<br /><br /> **Server=(localdb)\\** _instancename_ **;**<br /><br /> 有关 LocalDB 的支持的详细信息，请参阅[SQL Server 本机客户端支持对 LocalDB](../../../relational-databases/native-client/features/sql-server-native-client-support-for-localdb.md)。<br /><br /> 若要指定的命名的实例[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，将追加 **\\** _InstanceName_。<br /><br /> 如果不指定服务器，则与本地计算机上的默认实例建立连接。<br /><br /> 如果指定了 IP 地址，请确保在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器中启用了 TCP/IP 或 named pipes 协议。<br /><br /> Server  关键字的完整语法如下：<br /><br /> **Server=** [_protocol_ **:** ]*Server*[ **,** _port_]<br /><br /> *protocol* 可以是 **tcp** (TCP/IP)、 **lpc** （共享内存）或 **np** （命名管道）。<br /><br /> 以下示例将指定一个命名管道：<br /><br /> `np:\\.\pipe\MSSQL$MYINST01\sql\query`<br /><br /> 此行指定 named pipes 协议、本地计算机上的一个命名管道 (`\\.\pipe`)、[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称 (`MSSQL$MYINST01`)，以及命名管道的默认名称 (`sql/query`)。<br /><br /> 如果既没有*协议*也不**网络**指定关键字，则[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]本机客户端将使用中指定的协议顺序[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]配置管理器。<br /><br /> “port”是指定服务器上所要连接到的端口  。 默认情况下，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用端口 1433。<br /><br /> 将忽略传递给的值开头的空格**服务器**时使用的 ODBC 连接字符串中[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]本机客户端。|  
|**ServerSPN**|服务器的 SPN。 默认值为空字符串。 空字符串导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用驱动程序生成的默认 SPN。|  
|**StatsLog_On**|如果是“yes”，则启用对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序性能数据的捕获。 如果是“no”，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序性能数据在连接上不可用。|  
|**StatsLogFile**|用于记录 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序性能统计信息的文件的完整路径和文件名。|  
|**Trusted_Connection**|如果是“yes”，则指示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序使用 Windows 身份验证模式进行登录验证。 否则指示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 用户名和密码进行登录验证，并且必须指定 UID 和 PWD 关键字。|  
|**TrustServerCertificate**|与一起使用时**Encrypt**，使用自签名的服务器证书启用加密。|  
|**UID**|有效的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录帐户。 使用 Windows 身份验证时，不需要指定 UID。|  
|**UseProcForPrepare**|此关键字已弃用，并且忽略其设置[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ODBC 驱动程序。|  
|**WSID**|工作站 ID。 通常，这是应用程序驻留于其上的计算机的网络名称（可选）。 如果指定，此值存储在**master.dbo.sysprocesses**列**主机名**并返回由[sp_who](../../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)和[HOST_NAME](../../../t-sql/functions/host-name-transact-sql.md)函数。|  
  
> **注意**：区域转换设置应用于货币、数字、日期和时间数据类型。 转换设置仅应用于输出转换，并仅在货币、数字、日期或时间值转换为字符串时可见。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序使用当前用户的区域设置注册表设置。 该驱动程序不会遵循当前线程的区域设置，如果应用程序连接之后设置它，例如，调用**SetThreadLocale**。  
  
 改变数据源的区域行为可能导致应用程序失败。 对于那些分析日期字符串并期望日期字符串按 ODBC 定义的方式显示的应用程序来说，改变该值可能对它们有负面影响。  
  
## <a name="ole-db-provider-connection-string-keywords"></a>OLE DB 访问接口连接字符串关键字  
 OLE DB 应用程序可以用两种方式初始化数据源对象：  
  
-   **IDBInitialize::Initialize**  
  
-   **IDataInitialize::GetDataSource**  
  
 在第一种情况下，通过在 DBPROPSET_DBINIT 属性集中设置属性 DBPROP_INIT_PROVIDERSTRING，可以用访问接口字符串初始化连接属性。 在第二种情况下，可以将初始化字符串传递到 IDataInitialize::GetDataSource 方法以初始化连接属性  。 两种方法都初始化相同的 OLE DB 连接属性，但使用不同的关键字集合。 IDataInitialize::GetDataSource 所使用的关键字集合至少是初始化属性组内的属性的说明  。  
  
 将对应的 OLE DB 属性设置为某个默认值或显式设置为某个值的任何提供程序字符串设置，OLE DB 属性值将覆盖提供程序字符串中的设置。  
  
 通过 DBPROP_INIT_PROVIDERSTRING 值在访问接口字符串中所设置的布尔属性要使用值“yes”和“no”进行设置。 通过使用 IDataInitialize::GetDataSource 在初始化字符串中设置的布尔属性是使用值“true”和“false”进行设置的  。  
  
 使用 IDataInitialize::GetDataSource 的应用程序也可以使用由 IDBInitialize::Initialize 使用的关键字，但仅用于没有默认值的属性   。 如果应用程序在初始化字符串中同时使用 IDataInitialize::GetDataSource 关键字和 IDBInitialize::Initialize 关键字，则使用 IDataInitialize::GetDataSource 关键字设置    。 强烈建议应用程序不要在 IDataInitialize:GetDataSource 连接字符串中使用 IDBInitialize::Initialize 关键字，因为在未来的版本中可能不保留该行为   。  
  
> [!NOTE]  
>  通过 IDataInitialize::GetDataSource 传递的连接字符串将转换为属性并且通过 IDBProperties::SetProperties 应用   。 如果组件服务在 IDBProperties::GetPropertyInfo 中找到了属性说明，则此属性将作为独立属性应用  。 否则，它将通过 DBPROP_PROVIDERSTRING 属性应用。 例如，如果您指定连接字符串**数据源 = server1;Server = server2**，**数据源**将设置为一个属性，但**Server**将进入提供程序字符串。  
  
 如果指定访问接口特定的属性相同的多个实例，则使用第一个属性的第一个值。  
  
 将 IDBInitialize::Initialize 和 DBPROP_INIT_PROVIDERSTRING 配合使用的 OLE DB 应用程序所使用的连接字符串具有以下语法  ：  
  
 `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 `empty-string ::=`  
  
 `attribute ::= attribute-keyword=[{]attribute-value[}]`  
  
 `attribute-value ::= character-string`  
  
 `attribute-keyword ::= identifier`  
  
 属性值可以选择放在大括号中，并且最好这样做。 这样可以避免当属性值包含非字母数字字符时出现问题。 值中的第一个右大括号用于终止值，因此值不能包含右大括号。  
  
 连接字符串关键字的等号 (=) 之后的空白字符将被解释为文本，即使该值在引号内也是如此。  
  
 下表对可能与 DBPROP_INIT_PROVIDERSTRING 一起使用的关键字进行了说明。  
  
|关键字|初始化属性|描述|  
|-------------|-----------------------------|-----------------|  
|**Addr**|SSPROP_INIT_NETWORKADDRESS|“Address”的同义词。|  
|**Address**|SSPROP_INIT_NETWORKADDRESS|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的网络地址。<br /><br /> 有关有效地址语法的详细信息，请参阅的说明**地址**ODBC 关键字，本主题中的更高版本。|  
|**APP**|SSPROP_INIT_APPNAME|用于标识应用程序的字符串。|  
|**ApplicationIntent**|SSPROP_INIT_APPLICATIONINTENT|连接到服务器时声明应用程序工作负荷类型。 可能的值为 ReadOnly 和 ReadWrite   。<br /><br /> 默认值是**ReadWrite**。 有关详细信息[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]对本机客户端的支持[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]，请参阅[SQL Server 本机客户端支持对高可用性和灾难恢复](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。|  
|**AttachDBFileName**|SSPROP_INIT_FILENAME|可附加数据库的主文件的名称（包括完整路径名）。 若要使用 AttachDBFileName，还必须使用访问接口字符串 Database 关键字来指定数据库名称  。 如果该数据库以前已经附加，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不会重新附加它（而是使用已附加的数据库作为连接的默认数据库）。|  
|**自动翻译**|SSPROP_INIT_AUTOTRANSLATE|“AutoTranslate”的同义词。|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|配置 OEM/ANSI 字符转换。 可识别的值为“yes”和“no”。|  
|**“数据库”**|DBPROP_INIT_CATALOG|数据库名称。|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|指定要使用的数据类型的处理模式。 对于访问接口数据类型，可识别的值为“0”，对于 SQL Server 2000 数据类型则为“80”。|  
|**Encrypt**|SSPROP_INIT_ENCRYPT|指定在通过网络发送数据之前是否应当将其加密。 可能的值为“yes”和“no”。 默认值为“no”。|  
|**FailoverPartner**|SSPROP_INIT_FAILOVERPARTNER|用于数据库镜像的故障转移服务器的名称。|  
|**FailoverPartnerSPN**|SSPROP_INIT_FAILOVERPARTNERSPN|故障转移伙伴的 SPN。 默认值为空字符串。 空字符串导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用访问接口生成的默认 SPN。|  
|**语言**|SSPROPT_INIT_CURRENTLANGUAGE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 语言。|  
|**MarsConn**|SSPROP_INIT_MARSCONNECTION|如果服务器是 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本，则启用或禁用连接上的多个活动结果集 (MARS)。 可能的值为“yes”和“no”。 默认值为“no”。|  
|**Net**|SSPROP_INIT_NETWORKLIBRARY|“Network”的同义词。|  
|**Network**|SSPROP_INIT_NETWORKLIBRARY|用于与组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例建立连接的网络库。|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|“Network”的同义词。|  
|**PacketSize**|SSPROP_INIT_PACKETSIZE|网络数据包大小。 默认值为 4096。|  
|**PersistSensitive**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|接受字符串“yes”和“no”作为值。 如果是“no”，则不允许数据源对象保留敏感的身份验证信息。|  
|**PWD**|DBPROP_AUTH_PASSWORD|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录密码。|  
|**Server**|DBPROP_INIT_DATASOURCE|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。<br /><br /> 如果不指定，则与本地计算机上的默认实例建立连接。<br /><br /> 有关有效地址语法的详细信息，请参阅的说明**Server** ODBC 关键字，本主题中的。|  
|**ServerSPN**|SSPROP_INIT_SERVERSPN|服务器的 SPN。 默认值为空字符串。 空字符串导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用访问接口生成的默认 SPN。|  
|**超时**|DBPROP_INIT_TIMEOUT|等待数据源初始化完成的时间（秒）。|  
|**Trusted_Connection**|DBPROP_AUTH_INTEGRATED|如果是“yes”，则指示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口使用 Windows 身份验证模式进行登录验证。 否则指示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 用户名和密码进行登录验证，并且必须指定 UID 和 PWD 关键字。|  
|**TrustServerCertificate**|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|接受字符串“yes”和“no”作为值。 默认值为“no”，它意味着将验证服务器证书。|  
|**UID**|DBPROP_AUTH_USERID|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名。|  
|**UseProcForPrepare**|SSPROP_INIT_USEPROCFORPREP|不推荐使用该关键字，并且 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口将忽略其设置。|  
|**WSID**|SSPROP_INIT_WSID|工作站标识符。|  
  
 使用 IDataInitialize::GetDataSource 的 OLE DB 应用程序所使用的连接字符串有以下语法  ：  
  
 `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 `empty-string ::=`  
  
 `attribute ::= attribute-keyword=[quote]attribute-value[quote]`  
  
 `attribute-value ::= character-string`  
  
 `attribute-keyword ::= identifier`  
  
 `quote ::= " | '`  
  
 属性使用必须符合其作用域中允许的语法。  例如， **WSID**使用大括号 ( **{}** ) 引号字符并**应用程序名称**使用单引号 (  ) 或双精度 ( **"** ) 引号字符。 只有字符串属性可以加引号。 尝试将整数或枚举属性用引号引起来将导致“连接字符串不符合 OLE DB 规范”错误。  
  
 属性值可以选择放在单引号或双引号内，并且最好这样做。 这样可以避免当值包含非字母数字字符时出现问题。 也可以在值内使用引号字符，只要使用的是双引号。  
  
 连接字符串关键字的等号 (=) 之后的空白字符将被解释为文本，即使该值在引号内也是如此。  
  
 如果某一连接字符串具有下表所列的多个属性，将使用最后一个属性的值。  
  
 下表对可能与 IDataInitialize::GetDataSource 配合使用的关键字进行了说明  ：  
  
|关键字|初始化属性|描述|  
|-------------|-----------------------------|-----------------|  
|**应用程序名称**|SSPROP_INIT_APPNAME|用于标识应用程序的字符串。|  
|**Application Intent**|SSPROP_INIT_APPLICATIONINTENT|连接到服务器时声明应用程序工作负荷类型。 可能的值为 ReadOnly 和 ReadWrite   。<br /><br /> 默认值是**ReadWrite**。 有关详细信息[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]对本机客户端的支持[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]，请参阅[SQL Server 本机客户端支持对高可用性和灾难恢复](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。|  
|**自动翻译**|SSPROP_INIT_AUTOTRANSLATE|“AutoTranslate”的同义词。|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|配置 OEM/ANSI 字符转换。 可识别的值为“true”和“false”。|  
|**Connect Timeout**|DBPROP_INIT_TIMEOUT|等待数据源初始化完成的时间（秒）。|  
|**Current Language**|SSPROPT_INIT_CURRENTLANGUAGE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 语言名称。|  
|**数据源**|DBPROP_INIT_DATASOURCE|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。<br /><br /> 如果不指定，则与本地计算机上的默认实例建立连接。<br /><br /> 有关有效地址语法的详细信息，请参阅的说明**Server** ODBC 关键字，本主题中的更高版本。|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|指定要使用的数据类型的处理模式。 可识别的值对访问接口数据类型为“0”，对 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 数据类型为“80”。|  
|**Failover Partner**|SSPROP_INIT_FAILOVERPARTNER|用于数据库镜像的故障转移服务器的名称。|  
|**Failover Partner SPN**|SSPROP_INIT_FAILOVERPARTNERSPN|故障转移伙伴的 SPN。 默认值为空字符串。 空字符串导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用访问接口生成的默认 SPN。|  
|**初始目录**|DBPROP_INIT_CATALOG|数据库名称。|  
|**初始文件名**|SSPROP_INIT_FILENAME|可附加数据库的主文件的名称（包括完整路径名）。 若要使用 AttachDBFileName，还必须使用访问接口字符串 DATABASE 关键字来指定数据库名称  。 如果该数据库以前已经附加，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不会重新附加它（而是使用已附加的数据库作为连接的默认数据库）。|  
|**Integrated Security**|DBPROP_AUTH_INTEGRATED|接受 Windows 身份验证的值“SSPI”。|  
|**MARS Connection**|SSPROP_INIT_MARSCONNECTION|启用或禁用连接上的多个活动结果集 (MARS)。 可识别的值为“true”和“false”。 默认值为“false”。|  
|**Network Address**|SSPROP_INIT_NETWORKADDRESS|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的网络地址。<br /><br /> 有关有效地址语法的详细信息，请参阅的说明**地址**ODBC 关键字，本主题中的更高版本。|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|用于与组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例建立连接的网络库。|  
|**Packet Size**|SSPROP_INIT_PACKETSIZE|网络数据包大小。 默认值为 4096。|  
|**密码**|DBPROP_AUTH_PASSWORD|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录密码。|  
|**Persist Security Info**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|接受字符串“true”和“false”作为值。 如果是“false”，则不允许数据源对象保留敏感的身份验证信息|  
|**提供程序**||对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client，这应当是“SQLNCLI11”。|  
|**Server SPN**|SSPROP_INIT_SERVERSPN|服务器的 SPN。 默认值为空字符串。 空字符串导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用访问接口生成的默认 SPN。|  
|**信任服务器证书**|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|接受字符串“true”和“false”作为值。 默认值为“false”，它意味着将验证服务器证书。|  
|**Use Encryption for Data**|SSPROP_INIT_ENCRYPT|指定在通过网络发送数据之前是否应当将其加密。 可能的值为“true”和“false”。 默认值为“false”。|  
|**用户 ID**|DBPROP_AUTH_USERID|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名。|  
|**Workstation ID**|SSPROP_INIT_WSID|工作站标识符。|  
  
 **注意**：在连接字符串中，“旧密码”属性会设置 SSPROP_AUTH_OLD_PASSWORD，它是当前密码（可能已过期），通过访问接口字符串属性无法获取该密码。  
  
## <a name="activex-data-objects-ado-connection-string-keywords"></a>ActiveX 数据对象 (ADO) 连接字符串关键字  
 ADO 应用程序设置 ADODBConnection 对象的 ConnectionString 属性，或提供连接字符串作为 ADODBConnection 对象的 Open 方法的参数     。  
  
 ADO 应用程序还可以使用由 OLE DB IDBInitialize::Initialize 方法使用的关键字，但仅针对没有默认值的属性  。 如果应用程序在初始化字符串中同时使用这些 ADO 关键字和 IDBInitialize::Initialize 关键字，则使用 ADO 关键字设置  。 强烈建议应用程序仅使用 ADO 连接字符串关键字。  
  
 ADO 使用的连接字符串有以下语法：  
  
 `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 `empty-string ::=`  
  
 `attribute ::= attribute-keyword=["]attribute-value["]`  
  
 `attribute-value ::= character-string`  
  
 `attribute-keyword ::= identifier`  
  
 属性值可以选择放在双引号中，并且最好这样做。 这样可以避免当值包含非字母数字字符时出现问题。 属性值不能包含双引号。  
  
 下表对可能与 ADO 连接字符串一起使用的关键字进行了说明：  
  
|关键字|初始化属性|描述|  
|-------------|-----------------------------|-----------------|  
|**Application Intent**|SSPROP_INIT_APPLICATIONINTENT|连接到服务器时声明应用程序工作负荷类型。 可能的值为 ReadOnly 和 ReadWrite   。<br /><br /> 默认值是**ReadWrite**。 有关详细信息[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]对本机客户端的支持[!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]，请参阅[SQL Server 本机客户端支持对高可用性和灾难恢复](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)。|  
|**应用程序名称**|SSPROP_INIT_APPNAME|用于标识应用程序的字符串。|  
|**自动翻译**|SSPROP_INIT_AUTOTRANSLATE|“AutoTranslate”的同义词。|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|配置 OEM/ANSI 字符转换。 可识别的值为“true”和“false”。|  
|**Connect Timeout**|DBPROP_INIT_TIMEOUT|等待数据源初始化完成的时间（秒）。|  
|**Current Language**|SSPROPT_INIT_CURRENTLANGUAGE|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 语言名称。|  
|**数据源**|DBPROP_INIT_DATASOURCE|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。<br /><br /> 如果不指定，则与本地计算机上的默认实例建立连接。<br /><br /> 有关有效地址语法的详细信息，请参阅的说明**Server** ODBC 关键字，本主题中的。|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|指定将使用的数据类型的处理模式。 对于访问接口数据类型，可识别的值为“0”，对于 SQL Server 2000 数据类型则为“80”。|  
|**Failover Partner**|SSPROP_INIT_FAILOVERPARTNER|用于数据库镜像的故障转移服务器的名称。|  
|**Failover Partner SPN**|SSPROP_INIT_FAILOVERPARTNERSPN|故障转移伙伴的 SPN。 默认值为空字符串。 空字符串导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用访问接口生成的默认 SPN。|  
|**初始目录**|DBPROP_INIT_CATALOG|数据库名称。|  
|**初始文件名**|SSPROP_INIT_FILENAME|可附加数据库的主文件的名称（包括完整路径名）。 若要使用 AttachDBFileName，还必须使用访问接口字符串 DATABASE 关键字来指定数据库名称  。 如果该数据库以前已经附加，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 不会重新附加它（而是使用已附加的数据库作为连接的默认数据库）。|  
|**Integrated Security**|DBPROP_AUTH_INTEGRATED|接受 Windows 身份验证的值“SSPI”。|  
|**MARS Connection**|SSPROP_INIT_MARSCONNECTION|如果服务器是 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本，则启用或禁用连接上的多个活动结果集 (MARS)。 可识别的值为“true”和“false”。默认值为“false”。|  
|**Network Address**|SSPROP_INIT_NETWORKADDRESS|组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的网络地址。<br /><br /> 有关有效地址语法的详细信息，请参阅的说明**地址**ODBC 关键字，本主题中的。|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|用于与组织中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例建立连接的网络库。|  
|**Packet Size**|SSPROP_INIT_PACKETSIZE|网络数据包大小。 默认值为 4096。|  
|**密码**|DBPROP_AUTH_PASSWORD|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录密码。|  
|**Persist Security Info**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|接受字符串“true”和“false”作为值。 如果是“false”，则不允许数据源对象保留敏感的身份验证信息。|  
|**提供程序**||对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client，这应当是“SQLNCLI11”。|  
|**Server SPN**|SSPROP_INIT_SERVERSPN|服务器的 SPN。 默认值为空字符串。 空字符串导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 使用访问接口生成的默认 SPN。|  
|**信任服务器证书**|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|接受字符串“true”和“false”作为值。 默认值为“false”，它意味着将验证服务器证书。|  
|**Use Encryption for Data**|SSPROP_INIT_ENCRYPT|指定在通过网络发送数据之前是否应当将其加密。 可能的值为“true”和“false”。 默认值为“false”。|  
|**用户 ID**|DBPROP_AUTH_USERID|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名。|  
|**Workstation ID**|SSPROP_INIT_WSID|工作站标识符。|  
  
 **注意**：在连接字符串中，“旧密码”属性会设置 SSPROP_AUTH_OLD_PASSWORD，它是当前密码（可能已过期），通过访问接口字符串属性无法获取该密码。  
  
## <a name="see-also"></a>请参阅  
 [使用 SQL Server Native Client 生成应用程序](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
