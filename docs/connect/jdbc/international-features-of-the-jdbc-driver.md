---
title: JDBC 驱动程序的国际功能 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: bbb74a1d-9278-401f-9530-7b5f45aa79de
author: MightyPen
ms.author: genemi
ms.openlocfilehash: f5e9d6902733ea8a9cca91b4bd33adcb66708672
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67956478"
---
# <a name="international-features-of-the-jdbc-driver"></a>JDBC Driver 的国际功能
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 的国际化功能包括以下各项：  
  
-   对使用与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 相同的语言来实现完全本地化体验的支持  
  
-   对受区域设置影响的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据进行 Java 语言转换的支持  
  
-   对于国际语言的支持，而不考虑操作系统  
  
-   对国际域名的支持（从 Microsoft JDBC Driver 6.0 for SQL Server 开始）  
  
## <a name="handling-of-character-data"></a>处理字符数据  
 默认情况下，Java 中的字符数据是作为 Unicode 来处理的；Java 字符串对象表示 Unicode 字符数据  。 在 JDBC Driver 中，唯一可以不遵守此规则的是 ASCII 流的 getter 和 setter 方法，这属于比较特殊的情况，因为这些方法使用的字节流带有单个已知代码页 (ASCII) 的隐式假定。  
  
 此外, JDBC driver 还提供了**sendStringParametersAsUnicode**连接字符串属性。 此属性可用于指定字符数据的预定义参数作为 ASCII 或多字节字符集 (MBCS) 而不是 Unicode 来发送。 有关**sendStringParametersAsUnicode**连接字符串属性的详细信息, 请参阅[设置连接属性](../../connect/jdbc/setting-the-connection-properties.md)。  
  
### <a name="driver-incoming-conversions"></a>驱动程序传入转换  
 来自服务器的 Unicode 文本数据不是必须要转换的数据。 它将作为 Unicode 直接进行传递。 来自服务器的非 Unicode 数据从数据库或列级别的数据代码页直接转换为 Unicode。 JDBC 驱动程序将使用 Java 虚拟机 (JVM) 转换例程执行这些转换。 这些转换将通过所有类型化 String 和 Character 流 getter 方法得到执行。  
  
 如果 JVM 无法对来自数据库的数据提供相应的代码页支持，则 JDBC Driver 将引发“Java 环境不支持 XXX 代码页”异常。 要解决此问题，您应安装该 JVM 所需的完整国际字符支持。 有关示例，请参阅 Sun Microsystems 网站上的 Supported Encodings（受支持的编码）文档。  
  
### <a name="driver-outgoing-conversions"></a>驱动程序传出转换  
 从驱动程序发送至服务器的字符数据可以为 ASCII 或 Unicode。 例如，新的 JDBC 4.0 国家字符方法（如 [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) 和 [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) 类的 setNString、setNCharacterStream 和 setNClob 方法）始终将它们的参数值以 Unicode 格式发送到服务器。  
  
 另一方面，非国家字符 API 方法（例如 [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) 和 [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md) 类的 setString、setCharacterStream 和 setClob 方法）仅在 sendStringParametersAsUnicode 属性设置为默认值“true”时才将它们的值以 Unicode 格式发送到服务器  。  
  
## <a name="non-unicode-parameters"></a>非 Unicode 参数  
 为实现使用非 Unicode 参数的**CHAR**、 **VARCHAR**或**LONGVARCHAR**类型的最佳性能, 请将**sendStringParametersAsUnicode**连接字符串属性设置为 "false", 并使用非区域字符方法。  
  
## <a name="formatting-issues"></a>格式问题  
 对于日期、时间和货币，将使用 Locale 对象在 Java 语言级别对本地化数据执行所有格式设置；并针对 Date、Calendar 和 Number 数据类型使用各种不同的格式设置方法    。 只有在极少数的情况下，JDBC Driver 才需要以本地化格式传递受区域设置影响的数据，此时需要根据默认的 JVM 区域设置来使用相应的格式化程序。  
  
## <a name="collation-support"></a>排序规则支持  
 JDBC Driver 3.0 支持 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 和 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 所支持的所有排序规则，并且还支持 [!INCLUDE[ssKatmai](../../includes/sskatmai_md.md)] 中引入的新排序规则或新版 Windows 排序规则名称。  
  
 有关排序规则的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的[排序规则和 Unicode 支持](https://go.microsoft.com/fwlink/?LinkId=131366)和 [Windows 排序规则名称 (Transact-SQL)](https://go.microsoft.com/fwlink/?LinkId=131367)。  
  
## <a name="using-international-domain-names-idn"></a>使用国际域名 (IDN)  
 JDBC Driver 6.0 for SQL Server 支持使用国际化域名 (IDN)，并且可以在连接期间按照要求将 Unicode serverName 转换为 ASCII 兼容编码 (Punycode)。  如果 IDN 以 Punycode 格式（由 RFC 3490 指定）作为 ASCII 字符串存储在域名系统 (DNS) 中，通过将 serverNameAsACE 属性设置为 true 来启用 Unicode 服务器名称的转换。  否则，如果 DNS 服务配置为允许使用 Unicode 字符，请将 serverNameAsACE 属性设置为 false（默认值）。  对于旧版 JDBC 驱动程序，还可以先使用 [Java 的 IDN.toASCII](https://docs.oracle.com/javase/8/docs/api/java/net/IDN.html) 方法将 serverName 转换为 Punycode，再为连接设置此属性。  
  
> [!NOTE]  
>  为非 Windows 平台编写的大多数解析程序软件基于 Internet DSN 标准，因此很可能为 IDN 使用 Punycode 格式，而专用网络上的基于 Windows 的 DNS 服务器可以配置为允许针对每个服务器使用 UTF-8 字符。  有关详细信息，请参阅 [Unicode 字符支持](https://technet.microsoft.com/library/cc738403(v=ws.10).aspx)。  
  
## <a name="see-also"></a>另请参阅  
 [JDBC 驱动程序的概述](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
