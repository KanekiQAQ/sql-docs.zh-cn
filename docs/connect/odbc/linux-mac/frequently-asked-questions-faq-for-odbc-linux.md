---
title: ODBC Linux 和 macOS 的常见问题解答 (FAQ) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 65bfd6d2-c83d-4528-a5e1-a85b125a4f4a
author: MightyPen
ms.author: genemi
ms.openlocfilehash: d3de76486a44d8c107d0ee35f6069f6854758477
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68008809"
---
# <a name="frequently-asked-questions-faq-for-odbc-linux-and-macos"></a>ODBC Linux 和 macOS 的常见问题解答 (FAQ)
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

以下是有关 Linux 和 macOS 上的 ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的问题解答。
  
## <a name="frequently-asked-questions"></a>常见问题

**Linux 或 macOS 上的现有 ODBC 应用程序如何使用该驱动程序？**  
你应该能够编译并运行已使用其他驱动程序在 Linux 或 macOS 上编译并运行的 ODBC 应用程序。 
  
**此版本的驱动程序支持 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 的哪些功能？**

Linux 和 macOS 上的 ODBC 驱动程序支持 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 中除 LocalDB 以外的所有服务器功能。 有关[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]支持的功能的详细信息, 请参阅[编程指导原则](../../../connect/odbc/linux-mac/programming-guidelines.md)。  
  
**该驱动程序是否支持 Kerberos 身份验证？**  
是。 如果已安装现有的 Kerberos 环境, 则应该可以使用`Trusted_Connection=Yes` DSN 或连接字符串选项连接到服务器。 有关详细信息，请参阅[使用集成身份验证](../../../connect/odbc/linux-mac/using-integrated-authentication.md)。  
  
**应用程序应使用哪种 Unicode 编码？**  
为 SQL_CHAR 数据使用 UTF-8，为 SQL_WCHAR 数据使用 UTF-16。  

**是否提供可下载并通过该驱动程序运行以对其进行体验和评估的 ODBC 示例？**

请参阅 [使用 Linux 上的 ODBC 驱动程序的现有 MSDN C++ ODBC 示例](https://blogs.msdn.com/b/sqlblog/archive/2012/01/26/use-existing-msdn-c-odbc-samples-for-microsoft-linux-odbc-driver.aspx) 以获取示例。 这也适用于 macOS ODBC 驱动程序。 

**是 Linux 还是 macOS 开源上的 ODBC 驱动程序？**

不能, Linux 和 macOS 上的 ODBC 驱动程序不是开源产品。  

## <a name="see-also"></a>另请参阅
[在 Linux 和 macOS 上安装 Microsoft ODBC Driver for SQL Server](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)
