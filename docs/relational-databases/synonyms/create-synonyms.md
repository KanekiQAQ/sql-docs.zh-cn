---
title: 创建同义词 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- sql13.swb.synonym.general.f1
helpviewer_keywords:
- creating synonyms
- synonyms [SQL Server], creating
ms.assetid: fedfa7a5-d0b6-4e2b-90f4-a08122958e33
author: CarlRabeler
ms.author: carlrab
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3500552ae0dde03b7cd4b354560bc3d0857eebec
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68050899"
---
# <a name="create-synonyms"></a>创建同义词
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中创建同义词。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要创建同义词，可使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Security"></a> Security  
 若要在给定架构中创建同义词，则用户必须具有 CREATE SYNONYM 权限，并拥有架构或具有 ALTER SCHEMA 权限。 CREATE SYNONYM 权限是可授予的权限。  
  
####  <a name="Permissions"></a> 权限  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-synonym"></a>创建同义词  
  
1.  在 **“对象资源管理器”** 中，展开要创建新视图的数据库。  
  
2.  右键单击“同义词”文件夹，然后单击“新建同义词...”   。  
  
3.  在 **“添加同义词”** 对话框中，输入以下信息。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

     **Synonym name**  
     Type the new name you will use for this object.  
  
     **Synonym schema**  
     Type the schema of the new name you will use for this object.  
  
     **Server name**  
     Type the server instance to connect to.  
  
     **Database name**  
     Type or select the database containing the object.  
  
     **Schema**  
     Type or select the schema that owns the object.  
  
     **Object type**  
     Select the type of object.  
  
     **Object name**  
     Type the name of the object to which the synonym refers.  
  
##  <a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-synonym"></a>创建同义词  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击 **“执行”** 。  
  
###  <a name="TsqlExample"></a> 示例 (Transact-SQL)  
 下面的示例为 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的现有表创建一个同义词。 后续示例中将使用该同义词。  
  
```  
USE tempdb;  
GO  
CREATE SYNONYM MyAddressType  
FOR AdventureWorks2012.Person.AddressType;  
GO  
```  
  
 以下示例将行插入到由 `MyAddressType` 同义词引用的基表。  
  
```  
USE tempdb;  
GO  
INSERT INTO MyAddressType (Name)  
VALUES ('Test');  
GO  
```  
  
 以下示例说明了如何在动态 SQL 中引用同义词。  
  
```  
USE tempdb;  
GO  
EXECUTE ('SELECT Name FROM MyAddressType');  
GO  
```  
  
  
