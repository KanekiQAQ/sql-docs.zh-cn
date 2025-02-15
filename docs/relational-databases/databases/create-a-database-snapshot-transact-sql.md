---
title: 创建数据库快照 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- database snapshots [SQL Server], creating
ms.assetid: 187fbba3-c555-4030-9bdf-0f01994c5230
author: stevestein
ms.author: sstein
ms.openlocfilehash: 7376d783bd2aab594d0beeb18dc1cc03af4b9c50
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68025879"
---
# <a name="create-a-database-snapshot-transact-sql"></a>创建数据库快照 (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库快照的唯一方式是使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 不支持创建数据库快照。  
  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Prerequisites"></a> 先决条件  
 可以使用任何恢复模式的源数据库必须满足以下先决条件：  
  
-   服务器实例必须运行支持数据库快照的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。 有关 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中数据库快照支持的详细信息，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
-   源数据库必须处于联机状态，除非该数据库是数据库镜像会话中的镜像数据库。  
  
-   若要在镜像数据库中创建数据库快照，数据库必须处于同步 [镜像状态](../../database-engine/database-mirroring/mirroring-states-sql-server.md)。  
  
-   不能将源数据库配置为可缩放共享数据库。  

- 源数据库不得包含 MEMORY_OPTIMIZED_DATA 文件组。 有关详细信息，请参阅 [内存中 OLTP 不支持的 SQL Server 功能](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)。

> [!IMPORTANT]
> 有关其他重要事项的信息，请参阅 [数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)。  
  
##  <a name="Recommendations"></a> 建议  
 本节讨论以下最佳做法：  
  
-   [最佳做法：命名数据库快照](#Naming)  
  
-   [最佳做法：限制数据库快照的数量](#Limiting_Number)  
  
-   [最佳做法：将客户端连接到数据库快照](#Client_Connections)  
  
####  <a name="Naming"></a> 最佳做法：命名数据库快照  
 创建数据库快照之前，考虑如何命名它们是非常重要的。 每个数据库快照都需要一个唯一的数据库名称。 为了便于管理，数据库快照的名称可以包含标识数据库的信息，例如：  
  
-   源数据库的名称。  
  
-   表明新名称用于快照的指示信息。  
  
-   快照的创建日期和时间、序列号或一些其他的信息（例如一天中的某个时间）以区分给定的数据库上的连续快照。  
  
 例如，假设有一系列的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库快照。 在上午 6 时和下午 6 时（基于 24 小时制）之间， 以 6 个小时为间隔创建三个每日快照。 每个每日快照保持 24 小时才被删除并被同一名称的新快照替换。 请注意，每个快照名称指明了小时，而非天：  
  
```  
AdventureWorks_snapshot_0600  
AdventureWorks_snapshot_1200  
AdventureWorks_snapshot_1800  
```  
  
 另外，如果这些每日快照创建的时间每天都变化，则推荐使用不太精确的命名约定，例如：  
  
```  
AdventureWorks_snapshot_morning  
AdventureWorks_snapshot_noon  
AdventureWorks_snapshot_evening  
```  
  
#### <a name="Limiting_Number"></a> 最佳做法：限制数据库快照的数量  
 随着时间的变化创建一系列快照可捕获源数据库的连续快照。 每个数据库快照会一直保存在系统中，直到被显式删除。 因为每个快照会随着原始页的更新而不断增长，所以您可能想在创建新快照后通过删除旧的快照来节省空间。  
  

**注意！** 若要还原到某个数据库快照，则需要从该数据库中删除所有其他快照。  
  
####  <a name="Client_Connections"></a> 最佳做法：将客户端连接到数据库快照  
 若要使用数据库快照，客户端需要知道它的位置。 正在创建或删除另一个数据库快照时，用户可以从一个数据库快照读取。 但是，如果用新快照替代现有快照，您需要将客户端重新定向到新快照。 用户可以通过 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]手动连接到数据库快照。 但是，若要支持生产环境，您应该创建一个编程解决方案，该方案透明地将报表编写客户端定向到数据库的最新数据库快照。  
  

  
####  <a name="Permissions"></a> 权限  
 可创建数据库的任何用户都可以创建数据库快照；但是，若要创建镜像数据库的快照，你必须是 **sysadmin** 固定服务器角色的成员。  
  
##  <a name="TsqlProcedure"></a> 如何创建数据库快照（使用 Transact-SQL）  
 **创建数据库快照**  
  
>  有关此过程的示例，请参阅本节后面的 [示例 (Transact-SQL)](#TsqlExample)。  
  
1.  根据源数据库的当前大小，确保有足够的磁盘空间存放数据库快照。 数据库快照的最大大小为创建快照时源数据库的大小。 有关详细信息，请参阅[查看数据库快照的稀疏文件大小 (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md)。  
  
2.  使用 AS SNAPSHOT OF 子句对文件执行 CREATE DATABASE 语句。 创建快照需要指定源数据库的每个数据库文件的逻辑名称。 语法如下：  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

     CREATE DATABASE *database_snapshot_name*  
  
     ON  
  
     (  
  
     NAME =*logical_file_name*,  
  
     FILENAME ='*os_file_name*'  
  
     ) [ ,...*n* ]  
  
     AS SNAPSHOT OF *source_database_name*  
  
     [;]  
  
     Where *source_**database_name* is the source database, *logical_file_name i*s the logical name used in SQL Server when referencing the file, *os_file_name* is the path and file name used by the operating system when you create the file, and *database_snapshot_name* is the name of the snapshot to which you want to revert the database. For a full description of this syntax, see [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md).  
  
    > [!NOTE]  
    >  When you create a database snapshot, log files, offline files, restoring files, and defunct files are not allowed in the CREATE DATABASE statement.  
  
###  <a name="TsqlExample"></a> 示例 (Transact-SQL)  
  
> [!NOTE]  
>  示例中使用的扩展名 `.ss` 是随意选择的。  
  
 本部分包含以下示例：  
  
-   A. [对 AdventureWorks 数据库创建快照](#Creating_on_AW)  
  
-   B. [对 Sales 数据库创建快照](#Creating_on_Sales)  
  
####  <a name="Creating_on_AW"></a> A. 对 AdventureWorks 数据库创建快照  
 此示例对 `AdventureWorks` 数据库创建数据库快照。 快照名称 `AdventureWorks_dbss_1800`及其稀疏文件的名称 `AdventureWorks_data_1800.ss`指明了创建时间 6 P.M.（1800 小时）。  
  
```  
CREATE DATABASE AdventureWorks_dbss1800 ON  
( NAME = AdventureWorks_Data, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks_data_1800.ss' )  
AS SNAPSHOT OF AdventureWorks;  
GO  
```  
  
####  <a name="Creating_on_Sales"></a> B. 对 Sales 数据库创建快照  
 此示例对 `sales_snapshot1200`数据库创建数据库快照 `Sales` 。 此数据库是在 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-sql-server-transact-sql.md)中的“创建具有文件组的数据库”示例中创建的。  
  
```  
--Creating sales_snapshot1200 as snapshot of the  
--Sales database:  
CREATE DATABASE sales_snapshot1200 ON  
( NAME = SPri1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SPri1dat_1200.ss'),  
( NAME = SPri2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SPri2dt_1200.ss'),  
( NAME = SGrp1Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\mssql\data\SG1Fi1dt_1200.ss'),  
( NAME = SGrp1Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG1Fi2dt_1200.ss'),  
( NAME = SGrp2Fi1_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG2Fi1dt_1200.ss'),  
( NAME = SGrp2Fi2_dat, FILENAME =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\data\SG2Fi2dt_1200.ss')  
AS SNAPSHOT OF Sales;  
GO  
```  
  
##  <a name="RelatedTasks"></a> 相关任务  
  
-   [查看数据库快照 (SQL Server)](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)  
  
-   [将数据库恢复到数据库快照](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md)  
  
-   [删除数据库快照 (Transact-SQL)](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
## <a name="see-also"></a>另请参阅  
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)  
  
  

