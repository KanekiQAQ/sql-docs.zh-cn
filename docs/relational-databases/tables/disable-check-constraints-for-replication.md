---
title: 对复制禁用 CHECK 约束 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- CHECK constraints, disabling
- constraints [SQL Server], disabling
- disabling constraints
- constraints [SQL Server], check
ms.assetid: af98fc70-24dd-4bd3-a0a3-f701dfa67b2c
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 07a6c2d115bb0100aa95fda9607c439af49031e5
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68016433"
---
# <a name="disable-check-constraints-for-replication"></a>对复制禁用 CHECK 约束
[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

  您可以使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 禁用 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的 CHECK 约束。 也可以对复制显式禁用检查约束，这在从早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中发布数据时会非常有用。  
  
> [!NOTE]  
>  如果表是使用复制发布的，则对于复制代理执行的操作将自动禁用检查约束。 当复制代理在订阅服务器上执行插入、更新或删除操作时，将不检查约束；如果用户执行插入、更新或删除操作，则检查约束。 由于最初插入、更新或删除数据时已经在发布服务器上检查过约束，所以对于复制代理将禁用该约束。 有关详细信息，请参阅 [指定架构选项](../../relational-databases/replication/publish/specify-schema-options.md)。  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Security"></a> Security  
  
####  <a name="Permissions"></a> 权限  
 需要对表的 ALTER 权限。  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-disable-a-check-constraint-for-replication"></a>对复制禁用 CHECK 约束  
  
1.  在 **“对象资源管理器”** 中，展开具有要修改的 CHECK 约束的表，再展开 **“约束”** 文件夹。  
  
2.  右键单击要修改的 CHECK 约束，然后单击 **“修改”** 。  
  
3.  在 **“CHECK 约束”** 对话框中的 **“表设计器”** ，对 **“强制用于复制”** 选择 **“否”** 值。  
  
4.  单击 **“关闭”** 。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-disable-a-check-constraint-for-replication"></a>对复制禁用 CHECK 约束  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。 第一个示例创建包含一个 IDENTITY 列的表和表中的一个 CHECK 约束。 然后，该示例删除该约束，并通过指定 NOT FOR REPLICATION 子句重新创建约束。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE TABLE dbo.doc_exd (column_a int IDENTITY (1,1)   
    CONSTRAINT exd_check CHECK (column_a > 1))   
  
    ALTER TABLE dbo.doc_exd   
    DROP CONSTRAINT exd_check;   
    GO  
    ALTER TABLE dbo.doc_exd    
    ADD CONSTRAINT exd_check CHECK NOT FOR REPLICATION (column_a > 1);  
    ```  
  
 有关详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)。  
  
###  <a name="TsqlExample"></a>   
## <a name="see-also"></a>另请参阅  
 [指定架构选项](../../relational-databases/replication/publish/specify-schema-options.md)  
  
  
