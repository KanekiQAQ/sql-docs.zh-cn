---
title: 修改统计信息 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- statistics [SQL Server], modifying
- modifying statistics
ms.assetid: b06299ca-ed52-411a-b245-45eac4628c99
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 593a30631eed27db108c79dd70840d1fee3f6964
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68134010"
---
# <a name="modify-statistics"></a>修改统计信息
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  您可以通过使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 修改 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的现有统计信息。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要修改统计信息，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Security"></a> Security  
  
####  <a name="Permissions"></a> 权限  
 要求：  
  
-   用户对表或视图具有 ALTER 权限。  
  
-   用户是表或索引视图所有者，或者是以下角色之一的成员： **sysadmin** 固定服务器角色、 **db_owner** 固定数据库角色或 **db_ddladmin** 固定数据库角色。  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-modify-statistics"></a>修改统计信息  
  
1.  在 **“对象资源管理器”** 中，单击加号以便展开要修改统计信息的数据库。  
  
2.  单击加号以便展开 **“表”** 文件夹。  
  
3.  单击加号以便展开要修改统计信息的表。  
  
4.  单击加号以便展开 **“统计信息”** 文件夹。  
  
5.  右键单击要修改的统计信息对象，然后选择  “属性”。  
  
6.  在  “统计信息属性 -”*statistics_name* 对话框中的  “常规页”上，单击  “添加”  “删除”  “上移”  “下移”或上述任意组合，以更改统计信息的属性。 请记住，某一列在“统计信息列”网格内的位置可能会显著影响统计信息的有用性  。  
  
7.  单击“确定”  。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **修改统计信息**  
  
 无法使用 Transact-SQL 语句执行此任务。 若要使用 Transact-SQL 修改统计信息，必须首先删除现有的统计信息，然后使用新属性重新创建统计信息。  
  
 详细信息，请参阅 [DROP STATISTICS (Transact SQL)](../../t-sql/statements/drop-statistics-transact-sql.md) 和 [CREATE STATISTICS (Transact SQL)](../../t-sql/statements/create-statistics-transact-sql.md)。  
  
  
