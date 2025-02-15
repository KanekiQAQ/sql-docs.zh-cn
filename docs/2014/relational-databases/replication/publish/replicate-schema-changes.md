---
title: 复制架构更改 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], schema changes
- schemas [SQL Server replication], replicating changes
ms.assetid: c09007f0-9374-4f60-956b-8a87670cd043
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 1a2c275604d9c74699eeb2b3c77a90e2d819fb6c
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68199328"
---
# <a name="replicate-schema-changes"></a>复制架构更改
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中复制架构更改。  
  
 如果对发布的项目进行以下架构更改，则会默认将其传播到 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅服务器：  
  
-   ALTER TABLE  
  
-   ALTER VIEW  
  
-   ALTER PROCEDURE  
  
-   ALTER FUNCTION  
  
-   ALTER TRIGGER  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
-   **复制架构更改，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Restrictions"></a> 限制和局限  
  
-   ALTER TABLE …DROP COLUMN 语句将始终复制到所有其订阅包含要被删除的列的订阅服务器，即使禁用对架构更改的复制也是如此。  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 如果不想复制发布的架构更改，请在“发布属性 - \<发布>”对话框中禁用对架构更改的复制。  有关访问此对话框的详细信息，请参阅 [View and Modify Publication Properties](view-and-modify-publication-properties.md)。  
  
#### <a name="to-disable-replication-of-schema-changes"></a>禁用对架构更改的复制  
  
1.  在“发布属性 - \<发布>”对话框的“订阅选项”页上，将“复制架构更改”属性值设置为“False”。      
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
     若要仅传播特定的架构更改，请在架构更改前将此属性设置为 **True** ，然后在更改后将其设置为 **False** 。 相反，若要传播大多数架构更改，而不是一个给定更改，请在架构更改前将此属性设置为 **“False”** ，然后在更改后将其设置为 **“True”** 。  
  
##  <a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程来指定是否复制这些架构更改。 您使用的存储过程取决于发布的类型。  
  
#### <a name="to-create-a-snapshot-or-transactional-publication-that-does-not-replicate-schema-changes"></a>创建不复制架构更改的快照发布或事务发布  
  
1.  在发布服务器上，对发布数据库执行 [sp_addpublication &#40;Transact-SQL&#41;](/sql/relational-databases/system-stored-procedures/sp-addpublication-transact-sql)，并将 **@replicate_ddl** 的值指定为 **0**。 有关详细信息，请参阅 [Create a Publication](create-a-publication.md)。  
  
#### <a name="to-create-a-merge-publication-that-does-not-replicate-schema-changes"></a>创建不复制架构更改的合并发布  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergepublication &#40;Transact-SQL&#41;](/sql/relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql)，并将 **@replicate_ddl** 的值指定为 **0**。 有关详细信息，请参阅 [Create a Publication](create-a-publication.md)。  
  
#### <a name="to-temporarily-disable-replicating-schema-changes-for-a-snapshot-or-transactional-publication"></a>为快照发布或事务发布暂时禁用复制架构更改  
  
1.  对于包含架构更改复制的发布，执行 [sp_changepublication &#40;Transact-SQL&#41;](/sql/relational-databases/system-stored-procedures/sp-changepublication-transact-sql)，将 **@property** 的值指定为 **replicate_ddl** 并将 **@value** 的值指定为 **0**。  
  
2.  对已发布对象执行 DDL 命令。  
  
3.  （可选）通过执行 [sp_changepublication &#40;Transact-SQL&#41;](/sql/relational-databases/system-stored-procedures/sp-changepublication-transact-sql) 并将 **@property** 的值指定为 **replicate_ddl** 且将 **@value** 的值指定为 **1** 来重新启用架构更改复制。  
  
#### <a name="to-temporarily-disable-replicating-schema-changes-for-a-merge-publication"></a>为合并发布暂时禁用复制架构更改  
  
1.  对于包含架构更改复制的发布，执行 [sp_changemergepublication &#40;Transact-SQL&#41;](/sql/relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql)，将 **@property** 的值指定为 **replicate_ddl** 并将 **@value** 的值指定为 **0**。  
  
2.  对已发布对象执行 DDL 命令。  
  
3.  （可选）通过执行 [sp_changemergepublication &#40;Transact-SQL&#41;](/sql/relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql) 并将 **@property** 的值指定为 **replicate_ddl** 且将 **@value** 的值指定为 **1** 来重新启用架构更改复制。  
  
## <a name="see-also"></a>请参阅  
 [对发布数据库进行架构更改](make-schema-changes-on-publication-databases.md)   
 [对发布数据库进行架构更改](make-schema-changes-on-publication-databases.md)  
  
  
