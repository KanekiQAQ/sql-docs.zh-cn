---
title: 执行大容量复制操作 (ODBC) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [ODBC]
- ODBC, bulk copy operations
- minimally logged operations [SQL Server Native Client]
- bulk copy [ODBC], about bulk copy
ms.assetid: 5c793405-487c-4f52-88b8-0091d529afb3
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 387d33930e59a9b5b934993b280060bc6efa5d40
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68134266"
---
# <a name="performing-bulk-copy-operations-odbc"></a>执行大容量复制操作 (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  ODBC 标准不直接支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 大容量复制操作。 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本 7.0 或更高版本的实例时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序支持执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 大容量复制操作的 DB-Library 函数。 这个特定于驱动程序的扩展为使用大容量复制函数的现有 DB-Library 应用程序提供了容易实现的升级路径。 专用的大容量复制支持包含在以下文件中：  
  
-   sqlncli.h  
  
     包括用于大容量复制函数的函数原型和常量定义。 必须在执行大容量复制操作的 ODBC 应用程序中包括 sqlncli.h，并且在编译应用程序时必须将其放在应用程序的 include 路径中。  
  
-   sqlncli11.lib  
  
     必须放在链接器的库路径中，并将其指定为要链接的文件。 sqlncli11.lib 随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序一起分发。  
  
-   sqlncli11.dll  
  
     在执行时必须存在。 sqlncli11.dll 随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序一起分发。  
  
> [!NOTE]  
>  ODBC **SQLBulkOperations**函数与没有任何关系[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]大容量复制函数。 应用程序必须使用特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的大容量复制函数，才能执行大容量复制操作。  
  
## <a name="minimally-logging-bulk-copies"></a>按最小方式记录大容量复制  
 使用完整恢复模式，将在事务日志中完整记录大容量加载所执行的所有行插入操作。 对于大型数据加载，这会导致事务日志被迅速填充。 在某些情况下，按最小方式记录是可能的。 按最小方式记录将减少大容量加载操作填充日志空间的可能性，并且比完整记录更有效。  
  
 有关使用最小日志记录的信息，请参阅[Prerequisites for Minimal Logging 中大容量导入](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md)。  
  
## <a name="remarks"></a>备注  
 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本中使用 bcp.exe 时，可能在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 之前未出现错误的情形下出现错误。 这是因为在更高版本中 bcp.exe 不再执行隐式的数据类型转换。 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 之前，如果目标表有 money 数据类型，则 bcp.exe 会将数字数据转换为 money 数据类型。 但是，在这种情况下，bcp.exe 只是截断额外的字段。 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始，如果文件和目标表的数据类型不匹配，那么，只要有任何数据必须在截断后才能适合放到目标表中，则 bcp.exe 将引发错误。 若要解决该错误，请修复数据，使其与目标数据类型匹配。 另外，也可以选择使用在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 以前的版本中的 bcp.exe。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [使用数据文件和格式化文件](../../relational-databases/native-client-odbc-bulk-copy-operations/using-data-files-and-format-files.md)  
  
-   [从程序变量执行大容量复制](../../relational-databases/native-client-odbc-bulk-copy-operations/bulk-copying-from-program-variables.md)  
  
-   [管理大容量复制的批处理大小](../../relational-databases/native-client-odbc-bulk-copy-operations/managing-bulk-copy-batch-sizes.md)  
  
-   [大容量复制文本和图像数据](../../relational-databases/native-client-odbc-bulk-copy-operations/bulk-copying-text-and-image-data.md)  
  
-   [从 DB-Library 转换到 ODBC 大容量复制](../../relational-databases/native-client-odbc-bulk-copy-operations/converting-from-db-library-to-odbc-bulk-copy.md)  
  
## <a name="see-also"></a>请参阅  
 [SQL Server 本机客户端&#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [批量导入和导出数据 (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)  
  
  
