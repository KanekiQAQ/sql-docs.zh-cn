---
title: 使用 SQL Server 默认结果集 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLSetStmtAttr function
- ODBC cursors, default result sets
- cursors [ODBC], default result sets
- default result sets
- result sets [ODBC], default
- ODBC applications, cursors
ms.assetid: ee1db3e5-60eb-4425-8a6b-977eeced3f98
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23d2767b8ec4d82fe283666338501efdc67c708b
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68134040"
---
# <a name="using-sql-server-default-result-sets"></a>使用 SQL Server 默认结果集
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  默认的 ODBC 游标属性包括：  
  
```  
SQLSetStmtAttr(hstmt, SQL_ATTR_CURSOR_TYPE, SQL_CURSOR_FORWARD_ONLY, SQL_IS_INTEGER);  
SQLSetStmtAttr(hstmt, SQL_ATTR_CONCURRENCY, SQL_CONCUR_READ_ONLY, SQL_IS_INTEGER);  
SQLSetStmtAttr(hstmt, SQL_ATTR_ROW_ARRAY_SIZE, 1, SQL_IS_INTEGER);  
```  
  
 这些属性设置为其默认值，只要[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ODBC 驱动程序将使用[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]默认结果集。 默认结果集可用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持的任意 SQL 语句，并且是将整个结果集传输到客户端的最有效的方法。  
  
 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 引入了对多个活动结果集 (MARS); 支持应用程序现在可以有多个活动的默认结果集的每个连接。 默认情况下未启用 MARS。  
  
 在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 之前，默认结果集不支持同一连接上具有多个活动语句。 对连接执行 SQL 语句后，服务器不接受来自该连接上的客户端的命令（取消结果集剩余内容的请求除外），直到结果集中的所有行均处理完毕。 若要取消部分处理的结果集的其余部分，请调用[SQLCloseCursor](../../../relational-databases/native-client-odbc-api/sqlclosecursor.md)或[SQLFreeStmt](../../../relational-databases/native-client-odbc-api/sqlfreestmt.md)与*fOption*参数设置为 SQL_CLOSE。 若要完成部分处理的结果集和测试的另一个结果集是否存在，请调用[SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md)。 如果默认结果集处理完毕之前，ODBC 应用程序将尝试连接句柄上的命令，该调用将生成 SQL_ERROR 并且调用**SQLGetDiagRec**返回：  
  
```  
szSqlState: "HY000", pfNativeError: 0  
szErrorMsg: "[Microsoft][SQL Server Native Client]  
                Connection is busy with results for another hstmt."  
```  
  
## <a name="see-also"></a>请参阅  
 [如何实现游标](../../../relational-databases/native-client-odbc-cursors/implementation/how-cursors-are-implemented.md)  
  
  
