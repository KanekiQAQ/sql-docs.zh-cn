---
title: ISO 选项的作用 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ISO options (ODBC)
- ODBC applications, ISO options
- ODBC applications, statements
- SQL Server Native Client ODBC driver, ISO options
- statements [ODBC], ISO options
ms.assetid: 813f1397-fa0b-45ec-a718-e13fe2fb88ac
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 442900f22aa408d91175e0a58b9f5b7ca7aceaef
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67937264"
---
# <a name="effects-of-iso-options"></a>ISO 选项的作用
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  ODBC 标准与 ISO 标准十分相似，而且 ODBC 应用程序期望 ODBC 驱动程序能提供标准行为。 若要使其行为更紧密地符合的定义在 ODBC 标准中， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序始终使用它所连接的 SQL Server 版本中可用的任何 ISO 选项。  
  
 当[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ODBC 驱动程序连接到的实例[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，服务器检测到客户端使用[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ODBC 驱动程序，并打开若干选项。  
  
 驱动程序将自己发出这些语句；ODBC 应用程序不会做任何工作以请求它们。 设置这些选项可提高使用驱动程序的 ODBC 应用程序的可移植性，因为服务器的行为将与 ISO 标准保持一致。  
  
 基于 DB-Library 的应用程序通常不会打开这些选项。 观察不同的站点之间 ODBC 或 Db-library 客户端，当运行同一 SQL 语句不应假定此行为指向问题[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ODBC 驱动程序。 它们应首先重新运行该语句包含相同的 SET 选项的 DB 库环境中将由[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ODBC 驱动程序。  
  
 由于用户和应用程序可能会随时打开和关闭 SET 选项，存储过程和触发器的开发人员还应分别在上面列出的 SET 选项处于打开和关闭状态的情况下对其存储过程和触发器进行认真测试。 这可以确保存储过程和触发器正常工作，而无论特定连接在调用过程和触发器时打开哪些选项。 如果存储过程或触发器需要对这些选项之一进行特殊设置，则应该在触发器或存储过程的开始处发出 SET 语句。 此 SET 语句仅在触发器或存储过程执行期间保持有效；过程或触发器结束后，即会还原原始设置。  
  
 在连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例时，还会打开第四个 SET 选项 (CONCAT_NULL_YIELDS_NULL)。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序将启用这些选项不设置 AnsiNPW = NO 在数据源中或在指定[SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)或[SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)。  
  
 如前文所述的 ISO 选项[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ODBC 驱动程序不会启用 QUOTED_IDENTIFIER 选项启用了 QuotedID = NO 在数据源中或在指定**SQLDriverConnect**或**SQLBrowseConnect**。  
  
 若要使驱动程序能够了解 SET 选项的当前状态，ODBC 应用程序不应使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] SET 语句设置这些选项。 它们只应使用数据源或连接选项设置这些选项。 如果应用程序发出 SET 语句，则驱动程序可能会生成错误的 SQL 语句。  
  
## <a name="see-also"></a>请参阅  
 [执行语句&#40;ODBC&#41;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)   
 [SQLDriverConnect](../../../relational-databases/native-client-odbc-api/sqldriverconnect.md)   
 [SQLBrowseConnect](../../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md)  
  
  
