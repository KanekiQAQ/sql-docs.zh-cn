---
title: 使用不带验证的加密 |Microsoft Docs
ms.custom: ''
ms.date: 12/21/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data access [SQL Server Native Client], encryption
- cryptography [SQL Server Native Client]
- SQLNCLI, encryption
- encryption [SQL Server Native Client]
- SQL Server Native Client, encryption
ms.assetid: f4c63206-80bb-4d31-84ae-ccfcd563effa
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba8a09a5d67529e341462da082e17e9b0e1a5eab
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68112189"
---
# <a name="using-encryption-without-validation"></a>使用不带验证的加密
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 始终对与登录相关的网络数据包进行加密。 如果在服务器启动时未为其提供任何证书，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将生成可用于对登录数据包进行加密的自签名证书。  

自签名的证书并不保证安全。 加密的握手基于 NT LAN Manager (NTLM)。 强烈建议预配 SQL Server 上的安全连接性可验证的证书。 传输安全层 (TLS) 可以进行安全仅使用证书验证。

应用程序还可以通过使用连接字符串关键字或连接属性请求对所有网络流量加密。 使用的提供程序字符串时，关键字是"Encrypt"用于 ODBC 和 OLE DB **idbinitialize:: Initialize**，或"Use Encryption for Data"的 ADO 和 OLE DB 使用将初始化字符串用于时**IDataInitialize**. 这也可以通过配置[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]使用 Configuration Manager**强制协议加密**选项，并通过配置客户端为请求加密的连接。 默认情况下，对某一连接的所有网络流量加密要求在服务器上设置证书。 通过设置你的客户端信任的服务器上的证书，你可能会变得很容易受到人为干预攻击。 如果部署服务器上的可验证证书，请确保将信任该证书有关的客户端设置更改为 FALSE。

有关连接字符串关键字的信息，请参阅[将连接字符串关键字用于 SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
 若要在服务器上未设置证书时启用加密，可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器设置“强制协议加密”和“信任服务器证书”选项   。 在这种情况下，如果服务器上未设置可验证的证书，加密将使用不带验证的自签名服务器证书。  
  
 应用程序还可以使用 "TrustServerCertificate" 关键字或与其关联的连接属性确保执行加密。 应用程序设置从不会降低 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 客户端配置管理器设置的安全级别，相反还可能提高安全级别。 例如，如果没有为客户端设置“强制协议加密”，应用程序可能自行请求加密  。 甚至是在未设置服务器证书的情况下，为保证加密，应用程序也可能请求加密和 "TrustServerCertificate"。 不过，如果在客户端配置中未启用 "TrustServerCertificate"，则仍需要已设置的服务器证书。 下表介绍了各种情况：  
  
|“强制协议加密”客户端设置|“信任服务器证书”客户端设置|连接字符串/连接属性加密/对数据使用加密|连接字符串/连接属性信任服务器证书|结果|  
|----------------------------------------------|---------------------------------------------|------------------------------------------------------------------------------|----------------------------------------------------------------------|------------|  
|否|不可用|否（默认值）|忽略|无加密。|  
|否|不可用|是|否（默认值）|仅当存在可验证的服务器证书时才加密，否则连接尝试将失败。|  
|否|不可用|是|是|始终加密，但可能使用自签名的服务器证书。|  
|是|否|忽略|忽略|仅当存在可验证的服务器证书时才加密，否则连接尝试将失败。|  
|是|是|否（默认值）|忽略|始终加密，但可能使用自签名的服务器证书。|  
|是|是|是|否（默认值）|仅当存在可验证的服务器证书时才加密，否则连接尝试将失败。|  
|是|是|是|是|始终加密，但可能使用自签名的服务器证书。|  
||||||

> [!CAUTION]
> 上表仅在不同配置下的系统行为提供的指南。 安全连接，请确保客户端和服务器都需要加密。 此外请确保服务器具有可验证的证书，并且**TrustServerCertificate**上客户端设置设置为 FALSE。

## <a name="sql-server-native-client-ole-db-provider"></a>SQL Server Native Client OLE DB 访问接口  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口支持加密，而无需通过添加 SSPROP_INIT_TRUST_SERVER_CERTIFICATE 数据源初始化属性，它将实现在 DBPROPSET_SQLSERVERDBINIT 属性验证设置。 此外，还添加了新的连接字符串关键字“TrustServerCertificate”。 它接受值 yes 或 no；默认值为 no。 使用服务组件时，它接受值 true 或 false；默认值为 false。  
  
 有关 DBPROPSET_SQLSERVERDBINIT 属性集对所做的增强功能的详细信息，请参阅[初始化和授权属性](../../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md)。  
  
## <a name="sql-server-native-client-odbc-driver"></a>SQL Server Native Client ODBC 驱动程序  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序支持而无需通过添加验证的加密[SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)并[SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md)函数。 添加了 SQL_COPT_SS_TRUST_SERVER_CERTIFICATE 以接受 SQL_TRUST_SERVER_CERTIFICATE_YES 或 SQL_TRUST_SERVER_CERTIFICATE_NO，默认值为 SQL_TRUST_SERVER_CERTIFICATE_NO。 此外，还添加了新的连接字符串关键字 "TrustServerCertificate"。 它接受值 yes 或 no；默认值为 no。  
  
## <a name="see-also"></a>请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)  
  
  
