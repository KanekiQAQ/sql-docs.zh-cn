---
title: 重命名数据库 | Microsoft Docs
ms.custom: ''
ms.date: 10/02/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], renaming
- renaming databases
ms.assetid: 44c69d35-abcb-4da3-9370-5e0bc9a28496
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1ade7deb2fd86f5dfd0f89aa1f13d5352e6e5fc9
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68127293"
---
# <a name="rename-a-database"></a>重命名数据库

[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  本主题说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 Azure SQL 数据库中重命名用户定义的数据库。 数据库名称可以包含任何符合标识符规则的字符。  
  
## <a name="in-this-topic"></a>本主题内容
  
- 开始之前：  
  
     [限制和局限](#limitations-and-restrictions)  
  
     [安全性](#security)  
  
- 重命名数据库，使用：  
  
     [SQL Server Management Studio](#rename-a-database-using-sql-server-management-studio)  
  
     [Transact-SQL](#rename-a-database-using-transact-sql)  
  
- **跟进：**[在重命名数据库之后](#backup-after-renaming-a-database)  

> [!NOTE]
> 若要重命名 Azure SQL 数据仓库或并行数据仓库中的数据库，可使用 [RENAME (Transact-SQL)](../../t-sql/statements/rename-transact-sql.md) 语句。
  
## <a name="before-you-begin"></a>开始之前
  
### <a name="limitations-and-restrictions"></a>限制和局限  
  
- 无法重命名系统数据库。
- 在其他用户正在访问数据库时，无法更改数据库名称。 
  - 在 SQL Server 中，可将数据库设置为单用户模式，关闭任何打开的连接。 有关详细信息，请参阅[将数据库设置为单用户模式](../../relational-databases/databases/set-a-database-to-single-user-mode.md)。
  - 在 Azure SQL 数据库中，必须确保其他用户与要重命名的数据库之间没有打开的连接。
  
### <a name="security"></a>Security  
  
#### <a name="permissions"></a>权限

需要对数据库拥有 ALTER 权限。  
  
## <a name="rename-a-database-using-sql-server-management-studio"></a>使用 SQL Server Management Studio 重命名数据库

使用以下步骤通过 SQL Server Management Studio 重命名 SQL Server 或 Azure SQL 数据库。
  
1. 在“对象资源管理器”中，连接到 SQL 实例。  
  
2. 请确保该数据库没有打开的连接。 如果使用 SQL Server，则可以[将数据库设置为单用户模式](../../relational-databases/databases/set-a-database-to-single-user-mode.md)，关闭任何打开的连接并防止其他用户在你更改数据库名称时进行连接。  
  
3. 在“对象资源管理器”中，展开“数据库”，右键单击要重命名的数据库，然后单击“重命名”。  
  
4. 输入新的数据库名称，然后单击 **“确定”**。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

## <a name="rename-a-database-using-transact-sql"></a>使用 Transact-SQL 重命名数据库  
  
### <a name="to-rename-a-sql-server-database-by-placing-it-in-single-user-mode"></a>通过将 SQL Server 数据库置于单用户模式，对其重命名

使用以下步骤通过 SQL Server Management Studio 中的 T-SQL 重命名 SQL Server 数据库，具体步骤包括：将数据库置于单用户模式，重命名，然后将数据库恢复多用户模式。
  
1. 为实例连接到 `master` 数据库。  
2. 打开一个查询窗口。  
3. 将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例将 `MyTestDatabase` 数据库的名称更改为 `MyTestDatabaseCopy`。
  
   ```sql
   USE master;  
   GO  
   ALTER DATABASE MyTestDatabase SET SINGLE_USER WITH ROLLBACK IMMEDIATE
   GO
   ALTER DATABASE MyTestDatabase MODIFY NAME = MyTestDatabaseCopy ;
   GO  
   ALTER DATABASE MyTestDatabaseCopy SET MULTI_USER
   GO
   ```  

### <a name="to-rename-an-azure-sql-database-database"></a>重命名 Azure SQL 数据库

使用以下步骤通过 SQL Server Management Studio 中的 T-SQL 重命名 Azure SQL 数据库。
  
1. 为实例连接到 `master` 数据库。  
2. 打开一个查询窗口。
3. 请确保当前无人使用该数据库。
4. 将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。 此示例将 `MyTestDatabase` 数据库的名称更改为 `MyTestDatabaseCopy`。
  
   ```sql
   ALTER DATABASE MyTestDatabase MODIFY NAME = MyTestDatabaseCopy ;
   ```  

## <a name="backup-after-renaming-a-database"></a>在重命名数据库之后备份  

重命名 SQL Server 中的数据库后，请备份 `master` 数据库。 在 Azure SQL 数据库中无需进行该操作，因为会自动执行备份。  
  
## <a name="see-also"></a>另请参阅

- [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)
- [数据库标识符](../../relational-databases/databases/database-identifiers.md)  
