---
title: JDBC 驱动程序的系统要求 | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 447792bb-f39b-49b4-9fd0-1ef4154c74ab
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 9acd6eeec40421a778c1ab829b9633e1e93a80b7
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68004264"
---
# <a name="system-requirements-for-the-jdbc-driver"></a>JDBC 驱动程序的系统要求
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 或 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 中的数据，计算机上必须安装有下列组件：

- [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]（[下载](download-microsoft-jdbc-driver-for-sql-server.md)）
- Java Runtime Environment

## <a name="java-runtime-environment-requirements"></a>Java 运行时环境要求  

 自 Microsoft JDBC Driver 7.2 for SQL Server 起，支持 Java 开发工具包 (JDK) 11.0 和 Java 运行时环境 (JRE) 11.0。
 
 自 Microsoft JDBC Driver 7.0 for SQL Server 起，支持 Java 开发工具包 (JDK) 10.0 和 Java 运行时环境 (JRE) 10.0。

 自 Microsoft JDBC Driver 6.4 for SQL Server 起，支持 Java 开发工具包 (JDK) 9.0 和 Java 运行时环境 (JRE) 9.0。

 自 Microsoft JDBC Driver 4.2 for SQL Server 起，支持 Java 开发工具包 (JDK) 8.0 和 Java 运行时环境 (JRE) 8.0。 对 Java Database Connectivity (JDBC) 规范 API 的支持已扩展为包含 JDBC 4.1 和 4.2 API。
  
 自 Microsoft JDBC Driver 4.1 for SQL Server 起，支持 Java 开发工具包 (JDK) 7.0 和 Java 运行时环境 (JRE) 7.0。
  
 从 [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)] 开始，Java 数据库连接 (JDBC) 规范 API 的JDBC 驱动程序支持扩展为包括 JDBC 4.0 API。 已在 Java 开发工具包 (JDK) 6.0 和 Java 运行时环境 (JRE) 6.0 中引入了 JDBC 4.0 API。 JDBC 4.0 是 JDBC 3.0 API 的超集。
  
 当你在 Windows 和 UNIX 操作系统上部署 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 时，你必须分别使用安装包 sqljdbc_\<version>_enu.exe 和 sqljdbc_\<version>_enu.tar.gz   。 有关如何部署 JDBC 驱动程序的详细信息，请参阅[部署 JDBC 驱动程序](../../connect/jdbc/deploying-the-jdbc-driver.md)主题。  

**Microsoft JDBC Driver 7.2 for SQL Server：**  

  JDBC Driver 7.2 在每个安装包内有两个 JAR 类库：mssql-jdbc-7.2.2.jre8.jar  和 mssql-jdbc-7.2.2.jre11.jar  。

  JDBC Driver 7.2 适用于各种主要 Java 虚拟机，且受到这些虚拟机的支持，但仅在 OpenJDK 8.0、OpenJDK 11.0、Azul Zulu JRE 8.0 和 Azul Zulu JRE 11.0 上经过测试。
  
  下面汇总了 Microsoft JDBC Drivers 7.2 for SQL Server 随附的两个 JAR 文件所提供的支持：  
  
  |JAR|JDBC 版本法规遵从性|推荐的 Java 版本|描述|  
|---------|-----------------------------|----------------------|-----------------|   
|mssql-jdbc-7.2.2.jre8.jar|4.2|8|需要 Java Runtime Environment (JRE) 8.0。 使用 JRE 7.0 或更低版本会引发异常。<br /><br /> 7\.2 中的新增功能包括：JDK 11 支持、Active Directory 托管服务标识 (MSI) 身份验证、OSGi 支持、SQLServerError API。 |    
|mssql-jdbc-7.2.2.jre11.jar|4.3|10|需要 Java 运行时环境 (JRE) 11.0. 使用 JRE 10.0 或更低版本会引发异常。<br /><br /> 7\.2 中的新增功能包括：JDK 11 支持、Active Directory 托管服务标识 (MSI) 身份验证、OSGi 支持、SQLServerError API。 |    


  JDBC Driver 7.2 还适用于 Maven Central Repository，并且可以通过在 POM.XML 中添加以下代码来添加到 Maven 项目：  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.2.2.jre11</version>
</dependency>
```
 
**Microsoft JDBC Driver 7.0 for SQL Server：**  

  JDBC Driver 7.0 在每个安装包中包含两个 JAR 类库：mssql-jdbc-7.0.0.jre8.jar  和 mssql-jdbc-7.0.0.jre10.jar  。

  JDBC Driver 7.0 专门适用于各种主要 Java 虚拟机且受到这些虚拟机的支持，但仅在 OpenJDK 8.0 和 10.0 上经过测试。
  
  下面概述了 Microsoft JDBC Driver 7.0 for SQL Server 附带的两个 JAR 文件提供的支持：  
  
  |JAR|JDBC 版本法规遵从性|推荐的 Java 版本|描述|  
|---------|-----------------------------|----------------------|-----------------|   
|mssql-jdbc-7.0.0.jre8.jar|4.2|8|需要 Java Runtime Environment (JRE) 8.0。 使用 JRE 7.0 或更低版本会引发异常。<br /><br /> 7\.0 中的新增功能包括：JDK 10 支持、已将默认符合性级别更新为 JDBC 4.2 规范、空间数据类型支持、cancelQueryTimeout 连接属性、请求边界方法、useBulkCopyForBatchInsert 连接属性、数据发现和分类信息、UTF-8 功能扩展和 CityHash 支持。 |    
|mssql-jdbc-7.0.0.jre10.jar|4.3|10|需要 Java Runtime Environment (JRE) 10.0。 使用 JRE 9.0 或更低版本会引发异常。<br /><br /> 7\.0 中的新增功能包括：JDK 10 支持、已将默认符合性级别更新为 JDBC 4.2 规范、空间数据类型支持、cancelQueryTimeout 连接属性、请求边界方法、useBulkCopyForBatchInsert 连接属性、数据发现和分类信息、UTF-8 功能扩展和 CityHash 支持。 |    


  JDBC Driver 7.0 还适用于 Maven Central Repository，并且可以通过在 POM.XML 中添加以下代码来添加到 Maven 项目：  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.0.0.jre10</version>
</dependency>
```
  
**Microsoft JDBC Driver 6.4 for SQL Server：**  

  JDBC Driver 6.4 在每个安装包中包含三个 JAR 类库：mssql-jdbc-6.4.0.jre7.jar  、mssql-jdbc-6.4.0.jre8.jar  和 mssql-jdbc-6.4.0.jre9.jar  。

  JDBC Driver 6.4 专门适用于各种主要 Java 虚拟机且受到这些虚拟机的支持，但仅在 OpenJDK 7.0、8.0 和 9.0 上经过测试。
  
  下面概述了由 Microsoft JDBC Driver 6.4 for SQL Server 附带的三个 JAR 文件提供的支持：  
  
  |JAR|JDBC 版本法规遵从性|推荐的 Java 版本|描述|  
|---------|-----------------------------|----------------------|-----------------|   
|mssql-jdbc-6.4.0.jre7.jar|4.1|7|需要 Java Runtime Environment (JRE) 7.0。 使用 JRE 6.0 或更低版本会引发异常。<br /><br /> 6\.4 中的新增功能包括：适用于 Linux 的 Azure AD 身份验证、Kerberos 的主体/密码方法、在跨域身份验证的 SPN 中自动检测领域、Kerberos 约束委派、查询超时、套接字超时和已准备好的语句句柄重复使用。 |  
|mssql-jdbc-6.4.0.jre8.jar|4.2|8|需要 Java Runtime Environment (JRE) 8.0。 使用 JRE 7.0 或更低版本会引发异常。<br /><br /> 6\.4 中的新增功能包括：适用于 Linux 的 Azure AD 身份验证、Kerberos 的主体/密码方法、在跨域身份验证的 SPN 中自动检测领域、Kerberos 约束委派、查询超时、套接字超时和已准备好的语句句柄重复使用。 |    
|mssql-jdbc-6.4.0.jre9.jar|4.3|9|需要 Java Runtime Environment (JRE) 9.0。 使用 JRE 8.0 或更低版本会引发异常。<br /><br /> 6\.4 中的新增功能包括：适用于 Linux 的 Azure AD 身份验证、Kerberos 的主体/密码方法、在跨域身份验证的 SPN 中自动检测领域、Kerberos 约束委派、查询超时、套接字超时和已准备好的语句句柄重复使用。 |

JDBC Driver 6.4 还适用于 Maven Central Repository，并且可以通过在 POM.XML 中添加以下代码来添加到 Maven 项目 

 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.4.0.jre9</version>
</dependency>
```

**Microsoft JDBC Driver 6.2 for SQL Server：**  
  
  JDBC Driver 6.2 在每个安装包中包含两个 JAR 类库：mssql-jdbc-6.2.2.jre7.jar  和 mssql-jdbc-6.2.2.jre8.jar  。 
  
 JDBC Driver 6.2 专门适用于各种主要 Java 虚拟机且受到这些虚拟机的支持，但仅在 Sun JRE 5.0、6.0、7.0 和 8.0 上经过测试。
  
 下面概述了由 Microsoft JDBC Driver for SQL Server 6.0 和 4.2 附带的两个 JAR 文件提供的支持：  
  
|JAR|JDBC 版本法规遵从性|推荐的 Java 版本|描述|  
|---------|-----------------------------|----------------------|-----------------|
|mssql-jdbc-6.2.2.jre7.jar|4.1|7|需要 Java Runtime Environment (JRE) 7.0。 使用 JRE 6.0 或更低版本会引发异常。<br /><br /> 6\.2 中的新增功能包括：适用于 Linux 的 Azure AD 身份验证、Kerberos 的主体/密码方法、在跨域身份验证的 SPN 中自动检测领域、Kerberos 约束委派、查询超时、套接字超时和已准备好的语句句柄重复使用。 |  
|mssql-jdbc-6.2.3.jre8.jar|4.2|8|需要 Java Runtime Environment (JRE) 8.0。 使用 JRE 7.0 或更低版本会引发异常。<br /><br /> 6\.2 中的新增功能包括：适用于 Linux 的 Azure AD 身份验证、Kerberos 的主体/密码方法、在跨域身份验证的 SPN 中自动检测领域、Kerberos 约束委派、查询超时、套接字超时和已准备好的语句句柄重复使用|    

  JDBC Driver 6.2 还适用于 Maven Central Repository，并且可以通过在 POM.XML 中添加以下代码来添加到 Maven 项目 
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.2.jre8</version>
</dependency>
```    

 **Microsoft JDBC Driver 6.0 and 4.2 for SQL Server：**  
  
  JDBC Drivers 6.0 和 4.2 的每个安装包中都有两个 JAR 类库：sqljdbc41.jar  和 sqljdbc42.jar  。 
  
 DBC Drivers 6.0 和 4.2 专门适用于各种主要 Java 虚拟机且受到这些虚拟机的支持，但仅在 Sun JRE 5.0、6.0、7.0 和 8.0 上经过测试。
  
 下面概述了由 Microsoft JDBC Driver for SQL Server 6.0 和 4.2 附带的两个 JAR 文件提供的支持：  
  
|JAR|JDBC 版本法规遵从性|推荐的 Java 版本|描述|  
|---------|-----------------------------|----------------------|-----------------|   
|sqljdbc41.jar|4.1|7|需要 Java Runtime Environment (JRE) 7.0。 使用 JRE 6.0 或更低版本会引发异常。<br /><br /> 6\.0 和 4.2 包中的新增功能包括：JDBC 4.1 符合性和大容量复制<br /><br /> 此外，6.0 包中独有的新增功能包括：Always Encrypted、表值参数、Azure Active Directory 身份验证、与 AlwaysOn 可用性组的透明连接、已准备好的查询和国际化域名 (IDN) 的参数元数据检索的改进|  
|sqljdbc42.jar|4.2|8|需要 Java Runtime Environment (JRE) 8.0。 使用 JRE 7.0 或更低版本会引发异常。<br /><br /> 6\.0 和 4.2 包中的新增功能包括：JDBC 4.1 符合性、JDBC 4.2 符合性和大容量复制<br /><br /> 此外，6.0 包中独有的新增功能包括：Always Encrypted、表值参数、Azure Active Directory 身份验证、与 AlwaysOn 可用性组的透明连接、已准备好的查询和国际化域名 (IDN) 的参数元数据检索的改进|  
  
 **Microsoft JDBC Driver 4.1 for SQL Server：**  
  
 JDBC Driver 4.1 的每个安装包中都有一个 JAR 类库：sqljdbc41.jar  。  
    
|JAR|描述|  
|---------|-----------------|  
|sqljdbc41.jar|sqljdbc41.jar 类库提供对 JDBC 4.0 API 的支持  。 它包含 JDBC 4.0 驱动程序的所有功能以及 JDBC 4.0 API 方法。 JDBC 4.1 不受支持（引发异常“SQLFeatureNotSupportedException”）。<br /><br /> sqljdbc41.jar 类库要求使用 Java Runtime Environment (JRE) 7.0。  使用 JRE 6.0 和 5.0 上的 sqljdbc41.jar  引发异常。<br /><br /> 
  
 JDBC 驱动程序专门适用于各种主要 Java 虚拟机且受到这些虚拟机的支持，但是在 Sun JRE 5.0、6.0 和 7.0 上测试的。
  
 下面概述了由 Microsoft JDBC Driver 4.1 for SQL Server 附带的 JAR 文件提供的支持。  
  
|JAR|JDBC 版本|JRE（可以运行）|JDK（可以编译）|  
|---------|------------------|---------------------|-------------------------|   
|sqljdbc41.jar|4|7|7 6 5|  
  
## <a name="sql-server-requirements"></a>SQL Server 要求  
 JDBC 驱动程序支持连接到 Azure SQL Database 和 SQL Server。 对于适用于 SQL Server 的 Microsoft JDBC Driver 4.2 和 4.1，该支持从 SQL Server 2008 开始。
  
## <a name="operating-system-requirements"></a>操作系统要求  
 JDBC 驱动程序可在任何支持使用 Java 虚拟机 (JVM) 的操作系统上工作。 但是，只有 Sun Solaris、SUSE Linux 以及 Windows 操作系统经过了官方测试。  
  
## <a name="supported-languages"></a>支持的语言  
 JDBC 驱动程序支持所有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 列排序规则。 有关 JDBC 驱动程序支持的排序规则的详细信息，请参阅 [JDBC 驱动程序的国际功能](../../connect/jdbc/international-features-of-the-jdbc-driver.md)。  
  
 有关排序规则的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的“使用排序规则”。  
  
## <a name="see-also"></a>另请参阅  
 [JDBC 驱动程序的概述](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
  
