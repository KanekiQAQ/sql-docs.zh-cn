---
title: FILESTREAM 支持 |Microsoft Docs
description: 适用于 SQL Server 的 OLE DB 驱动程序的 FILESTREAM 支持
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- FILESTREAM [SQL Server], OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server [FILESTREAM support]
author: pmasl
ms.author: pelopes
ms.openlocfilehash: c4f8b047a18d2bfc3aea33c72a29efcde1869c3a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67989077"
---
# <a name="filestream-support"></a>FILESTREAM 支持
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

从开始[!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], SQL Server OLE DB 驱动程序支持增强的 FILESTREAM 功能。 有关示例, 请参阅[Filestream 和 OLE DB](../../oledb/ole-db-how-to/filestream/filestream-and-ole-db.md)。  

FILESTREAM 允许通过 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 或通过直接访问 Windows 文件系统来存储和访问大型二进制值。 大型二进制值是大于 2 GB 的值。 有关增强的 FILESTREAM 支持的详细信息,[请&#40;参阅&#41;filestream SQL Server](../../../relational-databases/blob/filestream-sql-server.md)。  
  
默认情况下，在打开数据库连接时，@@TEXTSIZE 将设置为 -1（“无限制”）  。  
  
还可以使用 Windows 文件系统 API 访问和更新 FILESTREAM 列。  
  
有关详细信息，请参阅[使用 OpenSqlFilestream 访问 FILESTREAM 数据](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
  
## <a name="querying-for-filestream-columns"></a>查询 FILESTREAM 列  
OLE DB 中的架构行集不会报告某个列是否为 FILESTREAM 列。 OLE DB 中的 ITableDefinition 不能用于创建 FILESTREAM 列。    
  
若要创建 FILESTREAM 列或检测哪些现有列是 FILESTREAM 列，可以使用 [sys.columns](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) 目录视图的 is_filestream 列  。  
  
以下是一个示例：  
  
```sql  
-- Create a table with a FILESTREAM column.  
CREATE TABLE Bob_01 (GuidCol1 uniqueidentifier ROWGUIDCOL NOT NULL UNIQUE DEFAULT NEWID(), IntCol2 int, varbinaryCol3 varbinary(max) FILESTREAM);  
  
-- Find FILESTREAM columns.  
SELECT name FROM sys.columns WHERE is_filestream=1;  
  
-- Determine whether a column is a FILESTREAM column.  
SELECT is_filestream FROM sys.columns WHERE name = 'varbinaryCol3' AND object_id IN (SELECT object_id FROM sys.tables WHERE name='Bob_01');  
```  
  
## <a name="down-level-compatibility"></a>下级兼容性  
如果客户端是使用 SQL Server 的 OLE DB 驱动程序编译的, 并且应用程序[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]连接[!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]到[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)](到), 则**varbinary (max)** 行为将与引入[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的行为兼容。中的[!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]Native Client。 就是说，返回数据的最大大小将限制为不超过 2 GB。 对于超过 2 GB 的结果值，将发生截断，并将返回“字符串数据，右截断”警告。 
  
如果将数据类型兼容性设置为 80，则客户端行为将与下级客户端行为一致。  
  
如果客户端使用 SQLOLEDB，或使用在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 之前发布的其他访问接口，则 varbinary(max) 将映射到映像  。  
  
## <a name="comments"></a>注释
- 若要发送和接收大于 2 GB 的**varbinary (max)** 值, 应用程序将在参数和结果绑定中使用**DBTYPE_IUNKNOWN** 。 对于参数, 提供程序必须为 ISequentialStream 调用 IUnknown:: QueryInterface, 并为返回 ISequentialStream 的结果调用。  

-  对于 OLE DB, 与 ISequentialStream 值相关的检查将被宽松。 **DBBINDING**结构中的*wType*为**DBTYPE_IUNKNOWN**时, 可以通过省略*dwPart*中的**DBPART_LENGTH**或设置数据的长度 (在数据中的偏移*obLength*处) 来禁用长度检查。buffer) 到 ~ 0。 在此情况下，提供程序将不检查值的长度，并且将请求和返回可通过流提供的所有数据。 这一更改将适用于所有大型对象 (LOB) 类型和 XML，但只在连接到 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]（或更高版本）服务器时才适用。 这将为开发人员提供更高的灵活性，同时为现有应用程序和下级服务器维护一致性和向下兼容性。  此更改会影响传输数据的所有接口, 主要 IRowset::, ICommand:: Execute 和 IRowsetFastLoad:: InsertRow。
 

## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
