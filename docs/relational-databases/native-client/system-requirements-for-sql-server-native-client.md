---
title: SQL Server Native Client 的系统要求 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- system requirements [SQL Server Native Client]
- data access [SQL Server Native Client], system requirements
- SQL Server Native Client, system requirements
- SQLNCLI, system requirements
ms.assetid: 1c8e2f8a-a440-44da-8e3a-af632d34c52c
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8af6065a08cc85c7bdc2c36c6032506ba6d56c57
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68031760"
---
# <a name="system-requirements-for-sql-server-native-client"></a>SQL Server Native Client 的系统要求
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的数据访问功能（如 MARS），必须安装以下软件：  
  
-   客户端上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client。  
  
-   服务器上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 要求使用 Windows Installer 3.0。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 操作系统上已经安装了 Windows Installer 3.0。 您需要将其显式安装到所有其他平台上。 有关详细信息，请参阅[Windows Installer 3.0 Redistributable](https://go.microsoft.com/fwlink/?LinkId=46459)。  
  
> [!NOTE]  
>  确保以管理员权限进行登录，然后再安装此软件。  
  
## <a name="operating-system-requirements"></a>操作系统要求  
 有关支持的操作系统的列表[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client，请参阅[SQL Server Native client 支持策略](../../relational-databases/native-client/applications/support-policies-for-sql-server-native-client.md)。  
  
## <a name="sql-server-requirements"></a>SQL Server 要求  
 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的数据，必须安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 支持与 MDAC 的所有版本、Windows 数据访问组件以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的所有版本的连接。 如果客户端的较旧版本连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则该客户端未知的服务器数据类型将映射到与客户端版本相兼容的类型。 有关详细信息，请参阅本主题后面的“客户端版本的数据类型兼容性”。  
  
## <a name="cross-language-requirements"></a>跨语言要求  
 支持的操作系统的所有本地化版本均支持英文版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client。 与本地化版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 使用同一语言的本地化操作系统支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的本地化版本。 只要安装了匹配的语言设置，则支持的操作系统的英文版还支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的本地化版本。  
  
 对于升级：  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的英文版可以升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的任一本地化版本。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的本地化版本可以升级到同一语言的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 本地化版本。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的本地化版本可以升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的英文版。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的本地化版本无法升级到不同本地化语言的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 本地化版本。  
  
## <a name="data-type-compatibility-for-client-versions"></a>客户端版本的数据类型兼容性  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 将新数据类型映射到与下级客户端相兼容的较旧的数据类型，如下表所示。  
  
 OLE DB 和 ADO 应用程序可以使用**DataTypeCompatibility**连接字符串关键字用于[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]本机客户端的较旧的数据类型。 如果 DataTypeCompatibility=80，OLE DB 客户端将使用 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 表格格式数据流 (TDS) 版本而不是 TDS 版本进行连接  。 也就是说，对于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和较新的数据类型，将由服务器而不是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 执行下级转换。 此外，这还意味着，连接中的可用功能将被限制为 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 功能集。 尽可能早地对 API 调用中使用新数据类型或功能的尝试进行检测，将错误返回给调用应用程序，而不是尝试将无效的请求传递给服务器。  
  
 没有任何**DataTypeCompatibility**用于 ODBC 的控件。  
  
 IDBInfo::GetKeywords 将始终返回的关键字列表，在连接上的服务器版本相对应，不受**DataTypeCompatibility**。  
  
|数据类型|SQL Server Native Client<br /><br /> SQL Server 2005|SQL Server Native Client 11.0<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|Windows 数据访问组件、MDAC 及<br /><br /> SQL Server Native Client OLE DB 应用程序（其中 DataTypeCompatibility=80）|  
|---------------|--------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|  
|CLR UDT (\<= 8Kb)|udt|Udt|Varbinary|  
|varbinary(max)|varbinary|varbinary|图像|  
|varchar(max)|varchar|varchar|Text|  
|nvarchar(max)|NVARCHAR|NVARCHAR|Ntext|  
|xml|xml|xml|Ntext|  
|CLR UDT (> 8 Kb)|udt|varbinary|图像|  
|date|date|varchar|Varchar|  
|datetime2|datetime2|varchar|Varchar|  
|datetimeoffset|datetimeoffset|varchar|Varchar|  
|time|time|varchar|Varchar|  
  
## <a name="see-also"></a>请参阅  
 [SQL Server Native Client 编程](../../relational-databases/native-client/sql-server-native-client-programming.md)   
 [安装 SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
  
  
