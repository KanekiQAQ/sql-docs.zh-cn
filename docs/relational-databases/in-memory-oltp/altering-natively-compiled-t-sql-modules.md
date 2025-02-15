---
title: 更改本机编译的 T-SQL 模块 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 010318a0-6807-47c3-8ecc-bb7cb60513f0
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 03f7e4fd87068e31674e74dc81dd33dadba323cd
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67951266"
---
# <a name="altering-natively-compiled-t-sql-modules"></a>Altering Natively Compiled T-SQL Modules
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，可以使用 `ALTER` 语句在本机编译的存储过程和其他本机编译的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 模块上（如标量 UDF 和触发器）执行 `ALTER` 操作。  
  
在对本机编译的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 模块执行 `ALTER` 时，系统会使用新定义重新编译模块。 在重新编译期间，旧版模块仍可继续执行。 在编译完成后，系统将阻止旧版模块的运行，并安装新版模块。 在更改本机编译的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 模块时，你可以修改以下选项。  
  
-   Parameters  
-   EXECUTE AS  
-   TRANSACTION ISOLATION LEVEL  
-   LANGUAGE  
-   DATEFIRST  
-   DATEFORMAT  
-   DELAYED_DURABILITY  
  
> [!NOTE]  
> 本机编译的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 模块无法转换成非本机编译的模块。 非本机编译的 T-SQL 模块无法转换成本机编译的模块。  
  
有关 `ALTER PROCEDURE` 功能和语法的详细信息，请参阅 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)。  
  
可以对本机编译的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 模块执行 [sp_recompile](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md)，使其在下一次执行时进行重新编译。  
  
## <a name="example"></a>示例  
下面的示例创建了内存优化表 (T1)，以及选择所有 T1 列的本机编译的存储过程 (usp_1)。 然后，更改 usp_1 以删除 `EXECUTE AS` 子句，更改 `LANGUAGE`，并且仅从 T1 选择一列 (C1)。  
  
```sql  
CREATE TABLE [dbo].[T1] (  
  [c1] [int] NOT NULL,  
  [c2] [float] NOT NULL,  
  CONSTRAINT [PK_T1] PRIMARY KEY NONCLUSTERED ([c1])  
  ) WITH ( MEMORY_OPTIMIZED = ON , DURABILITY = SCHEMA_AND_DATA )  
GO  
  
CREATE PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
)  
   SELECT c1, c2 FROM dbo.T1  
END  
GO  
  
ALTER PROCEDURE [dbo].[usp_1]  
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'Dutch'  
)  
   SELECT c1 FROM dbo.T1  
END  
GO    
```   
  
## <a name="see-also"></a>另请参阅  
 [本机编译的存储过程](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)    
