---
title: 使用服务器游标 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC applications, cursors
- cursors [ODBC], server cursors
- ODBC cursors, server cursors
- server cursors [SQL Server]
ms.assetid: 8a6d99b7-10b8-4474-8639-4914b25ba170
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 74107cf78427efca5042edbf10e3e0991ec47b8c
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68134016"
---
# <a name="using-server-cursors"></a>使用服务器游标
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  如果 ODBC 应用程序将任何 ODBC 游标属性设置为默认值以外的任何[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client ODBC 驱动程序将请求服务器实现相同类型的 API 服务器游标。 如果使用 API 服务器游标，将在客户端上释放内存，并且可以大幅减少客户端与服务器之间的网络通信量。  
  
 API 服务器游标的潜在缺点是它们当前不支持所有 SQL 语句。 API 服务器游标无法用于执行：  
  
-   返回多个结果集的批处理或存储过程。  
  
-   包含 COMPUTE、COMPUTE BY、FOR BROWSE 或 INTO 子句的 SELECT 语句。  
  
-   引用远程存储过程的 EXECUTE 语句。  
  
 连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例时，如果使用服务器游标执行具有这些特征的语句，将导致游标转换到默认结果集。 连接到较早版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时，它将导致错误。  
  
## <a name="see-also"></a>请参阅  
 [如何实现游标](../../../relational-databases/native-client-odbc-cursors/implementation/how-cursors-are-implemented.md)  
  
  
