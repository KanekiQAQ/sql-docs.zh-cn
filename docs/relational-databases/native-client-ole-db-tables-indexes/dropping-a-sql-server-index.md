---
title: 删除 SQL Server 索引 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- removing indexes
- deleting indexes
- DropIndex function
- dropping indexes
- SQL Server Native Client OLE DB provider, indexes
- indexes [OLE DB]
ms.assetid: add3ba14-10b1-4723-b7c0-3e83689e9fdd
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 4178e4b44bc226e31b6f6287b366d154cc306a9c
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68069551"
---
# <a name="dropping-a-sql-server-index"></a>删除 SQL Server 索引
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口公开**iindexdefinition:: Dropindex**函数。 这允许使用者从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中删除某一索引。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序提供了一些[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]PRIMARY KEY 和 UNIQUE 约束作为索引。 表所有者、数据库所有者或某些管理角色成员可以修改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表，以及删除约束。 默认情况下，只有表所有者才能删除现有索引。 因此，DropIndex 成功或失败不仅取决于应用程序用户的访问权限，而且取决于指示的索引的类型  。  
  
 在 pTableID 参数的 uName 联合的 pwszName 成员中，使用者将表名指定为 Unicode 字符串    。 pTableID 的 eKind 成员必须是 DBKIND_NAME   。  
  
 在 pIndexID 参数的 uName 联合的 pwszName成员中，使用者将索引名指定为 Unicode 字符串    。 pIndexID 的 eKind 成员必须是 DBKIND_NAME   。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序不支持删除表中的所有索引的 OLE DB 功能时*pIndexID*为 null。 如果 pIndexID 为 Null，则返回 E_INVALIDARG  。  
  
## <a name="see-also"></a>请参阅  
 [表和索引](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)  
  
  
