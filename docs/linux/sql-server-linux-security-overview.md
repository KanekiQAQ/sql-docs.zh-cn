---
title: Linux 上 SQL Server 的安全限制
description: 本文介绍了 SQL Server Linux 限制。
author: VanMSFT
ms.author: vanto
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 64da74cc-14bf-4636-a55e-8cc1fce2aaff
ms.openlocfilehash: 9f54197c8613293b36c1eb1ec362a8ed4db835e4
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68065117"
---
# <a name="security-limitations-for-sql-server-on-linux"></a>Linux 上 SQL Server 的安全限制

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

Linux 上的 SQL Server 当前具有以下限制：

* 提供了标准密码策略。 MUST_CHANGE 是可以配置的唯一选项。  
* 不支持可扩展密钥管理。 
* 不支持使用 Azure Key Vault 中存储的密钥。
* SQL Server 生成自己的自签名证书，用于加密连接。 SQL Server 可以配置为使用用户提供的 TLS 的证书。 

有关 SQL Server 中提供的安全功能的详细信息，请参阅[SQL Server 数据库引擎和 Azure SQL 数据库安全中心](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)。

## <a name="next-steps"></a>后续步骤

常见安全任务，请参阅[开始使用 Linux 上的 SQL Server 的安全功能](sql-server-linux-security-get-started.md)。 为脚本以更改 TCP 端口号，SQL Server 目录和配置跟踪标志或排序规则，请参阅[在 Linux 上使用 mssql conf 配置 SQL Server](sql-server-linux-configure-mssql-conf.md)。
