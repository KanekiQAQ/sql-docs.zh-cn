---
title: Always Encrypted（客户端开发）| Microsoft Docs
ms.custom: ''
ms.date: 08/21/2018
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
dev_langs:
- CSharp
ms.assetid: 9595eb66-284c-4474-828f-8961a05ce989
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 475a030972819515a2f8f346b5644139dd7fdf90
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68086905"
---
# <a name="always-encrypted-client-development"></a>始终加密（客户端开发）
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

[始终加密](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)是可确保敏感数据（和相关的加密密钥）永远不会泄露到 SQL Server 或 Azure SQL 数据库的客户端加密技术。 通过“始终加密”，客户端驱动程序可在将敏感数据传至数据库引擎之前对其以透明方式加密，并对从加密数据库列中检索的数据以透明方式解密。

有关开发使用受“始终加密”技术保护的数据库的应用程序以及哪些客户端驱动程序和哪些驱动程序版本支持“始终加密”的详细信息，请参阅：

- [对用于 SQL Server 的 .NET Framework 数据提供程序使用 Always Encrypted](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)
- [对 JDBC 驱动程序使用始终加密](../../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)
- [对 ODBC 驱动程序使用 Always Encrypted](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
- [对 PHP 驱动程序使用 Always Encrypted](../../../connect/php/using-always-encrypted-php-drivers.md)

> [!NOTE]
> [.NET CORE](https://docs.microsoft.com/dotnet/core/) 目前不支持 Always Encrypted。

## <a name="see-also"></a>另请参阅

[始终加密（数据库引擎）](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)

