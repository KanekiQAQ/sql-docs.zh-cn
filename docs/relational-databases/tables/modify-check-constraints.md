---
title: 修改 CHECK 约束 | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- CHECK constraints, modifying
- modifying constraints
- constraints [SQL Server], check
- constraints [SQL Server], modifying
ms.assetid: f22daef8-e350-40ef-8ff0-b5f87d1d9e56
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 35ab525e1420d5e8db2a63f4892d8e17a3673158
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68139618"
---
# <a name="modify-check-constraints"></a>修改 CHECK 约束
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  当您希望更改约束表达式或更改对特定条件启用或禁用约束的选项时，可通过使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中修改 CHECK 约束。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **使用以下工具修改 CHECK 约束：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Security"></a> Security  
  
####  <a name="Permissions"></a> 权限  
 需要对表的 ALTER 权限。  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-modify-a-check-constraint"></a>修改 CHECK 约束  
  
1.  在“对象资源管理器”  中，右键单击包含 CHECK 约束的表，然后选择“设计”  。  
  
2.  在“表设计器”菜单上，单击“CHECK 约束…”   。  
  
3.  在 **“CHECK 约束”** 对话框中，在 **“选定的 CHECK 约束”** 下选择要编辑的约束。  
  
4.  完成下表中的相应操作：  
  
    |若要|需要遵循的步骤|  
    |--------|------------------------|  
    |编辑约束表达式|在 **“表达式”** 字段中键入新的表达式。|  
    |重命名约束|在 **“名称”** 字段中键入新的名称。|  
    |将该约束应用于现有数据|选择 **“在创建或启用时检查现有数据”** 选项。|  
    |向表中添加新数据或更新表中现有数据时禁用该约束。|清除 **“对 INSERT 和 UPDATE 强制约束”** 选项。|  
    |当复制代理在表中插入或更新数据时，禁用该约束。|清除 **“强制用于复制”** 选项。|  
  
    > [!NOTE]  
    >  对于 CHECK 约束，有些数据库具有不同的功能。  
  
5.  单击 **“关闭”** 。  
  
6.  在“文件”  菜单上，单击“保存”  以保存表名  。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **修改 CHECK 约束**  
  
 必须首先删除现有的 `CHECK` 约束，然后使用新定义重新创建，才能使用 [!INCLUDE[tsql](../../includes/tsql-md.md)]修改 `CHECK` 约束。 有关详细信息，请参阅 [删除 CHECK 约束](../../relational-databases/tables/delete-check-constraints.md) 和 [创建 CHECK 约束](../../relational-databases/tables/create-check-constraints.md)。  
  
###  <a name="TsqlExample"></a>  
