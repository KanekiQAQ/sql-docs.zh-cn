---
title: 定义和修改列筛选器 | Microsoft Docs
ms.custom: ''
ms.date: 06/14/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- filters [SQL Server replication], column
- modifying filters, column
- modifying filters
- column filters [SQL Server replication]
ms.assetid: d7c3186a-9a8c-45d8-ab34-05beec4c26dd
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 8e0c26e32425f204f7dab29aa65c66f3a11f09d7
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68212120"
---
# <a name="define-and-modify-a-column-filter"></a>定义和修改列筛选器
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中定义和修改列筛选器。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
-   **定义和修改列筛选器，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Restrictions"></a> 限制和局限  
  
-   某些列无法筛选；有关详细信息，请参阅[筛选已发布数据](filter-published-data.md)。 如果在初始化订阅之后修改了列筛选器，则必须在更改后生成新的快照并重新初始化所有订阅。 有关属性更改要求的详细信息，请参阅[更改发布和项目属性](change-publication-and-article-properties.md)。  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可以在新建发布向导的 **“项目”** 页上定义列筛选器。 有关使用新建发布向导的详细信息，请参阅[创建发布](create-a-publication.md)。  
  
 在“发布属性 - \<发布>”对话框的“项目”页上定义和修改列筛选器。   有关发布和项目属性的详细信息，请参阅[查看和修改发布属性](view-and-modify-publication-properties.md)。  
  
#### <a name="to-define-a-column-filter"></a>定义列筛选器  
  
1.  在新建发布向导的 **“项目”** 页上，在 **“要发布的对象”** 窗格中展开要筛选的表。  
  
2.  清除要筛选的每个列旁边的复选框。  
  
#### <a name="to-modify-column-filtering"></a>修改列筛选  
  
1.  在“发布属性 - \<发布>”对话框的“项目”页上，在“要发布的对象”窗格中展开要筛选的表。     
  
2.  清除要筛选的每个列旁边的复选框，并确保选中每个应包含在项目中的列的复选框。  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="TsqlProcedure"></a> 使用 Transact-SQL  
 在创建表项目时，您可以定义要在项目中包含的列以及在定义项目后更改列。 您可以使用复制存储过程以编程的方式创建和修改已筛选的列。  
  
> [!NOTE]  
>  下列过程假定基础表未发生更改。 有关将数据定义语言 (DDL) 更改复制到已发布表的信息，请参阅[对发布数据库进行架构更改](make-schema-changes-on-publication-databases.md)。  
  
#### <a name="to-define-a-column-filter-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>为快照发布或事务发布中发布的项目定义列筛选器  
  
1.  定义要筛选的项目。 有关详细信息，请参阅 [Define an Article](define-an-article.md)。  
  
2.  在发布服务器的发布数据库中，执行 [sp_articlecolumn](/sql/relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql)。 这将定义要包括在项目中或要从项目中删除的列。  
  
    -   如果仅发布拥有许多列的表中的几个列，请对要添加的每个列执行一次 [sp_articlecolumn](/sql/relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql) 。 为 **@column** 指定列名称并将 **@operation** 的值指定为 **@operation** 中定义和修改列筛选器。  
  
    -   如果发布拥有许多列的表中的大部分列，请执行 [sp_articlecolumn](/sql/relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql)，同时将 **@column** 的值指定为 **@column** 指定列名称并将 **@operation** 的值指定为 **@operation** ，以添加所有列。 然后，对每个要排除的列执行一次 [sp_articlecolumn](/sql/relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql)，同时将 **@operation** 的值指定为 **@operation** 并为 **@column** 中定义和修改列筛选器。  
  
3.  在发布服务器的发布数据库中，执行 [sp_articleview](/sql/relational-databases/system-stored-procedures/sp-articleview-transact-sql)。 为 **@publication** 指定发布名称并为 **@article** 中定义和修改列筛选器。 这将为筛选的项目创建同步对象。  
  
#### <a name="to-change-a-column-filter-to-include-additional-columns-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>为快照发布或事务发布中发布的项目更改列筛选器以包括其他列  
  
1.  在发布服务器的发布数据库中，对要添加的每个列执行一次 [sp_articlecolumn](/sql/relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql) 。 为 **@column** 指定列名称并将 **@operation** 的值指定为 **@operation** 中定义和修改列筛选器。  
  
2.  在发布服务器的发布数据库中，执行 [sp_articleview](/sql/relational-databases/system-stored-procedures/sp-articleview-transact-sql)。 为 **@publication** 指定发布名称并为 **@article** 中定义和修改列筛选器。 如果发布拥有现有订阅，请将 **@change_active** 的值指定为 **@change_active** 中定义和修改列筛选器。 这将为已筛选的项目重新创建同步对象。  
  
3.  对发布重新运行快照代理作业以生成更新的快照。  
  
4.  重新初始化订阅。 有关详细信息，请参阅 [重新初始化订阅](../reinitialize-subscriptions.md)。  
  
#### <a name="to-change-a-column-filter-to-remove-columns-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>为快照发布或事务发布中发布的项目更改列筛选器以删除列  
  
1.  在发布服务器的发布数据库中，对要删除的每个列执行一次 [sp_articlecolumn](/sql/relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql) 。 为 **@column** 指定列名称并将 **@operation** 的值指定为 **@operation** 中定义和修改列筛选器。  
  
2.  在发布服务器的发布数据库中，执行 [sp_articleview](/sql/relational-databases/system-stored-procedures/sp-articleview-transact-sql)。 为 **@publication** 指定发布名称并为 **@article** 中定义和修改列筛选器。 如果发布拥有现有订阅，请将 **@change_active** 的值指定为 **@change_active** 中定义和修改列筛选器。 这将为已筛选的项目重新创建同步对象。  
  
3.  对发布重新运行快照代理作业以生成更新的快照。  
  
4.  重新初始化订阅。 有关详细信息，请参阅 [重新初始化订阅](../reinitialize-subscriptions.md)。  
  
#### <a name="to-define-a-column-filter-for-an-article-published-in-a-merge-publication"></a>为合并复制中发布的项目定义列筛选器  
  
1.  定义要筛选的项目。 有关详细信息，请参阅 [定义项目](define-an-article.md)。  
  
2.  在发布服务器的发布数据库中，执行 [sp_mergearticlecolumn](/sql/relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql)。 这将定义要包括在项目中或要从项目中删除的列。  
  
    -   如果仅发布拥有许多列的表中的几个列，请对要添加的每个列执行一次 [sp_mergearticlecolumn](/sql/relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql) 。 为 **@column** 指定列名称并将 **@operation** 的值指定为 **@operation** 中定义和修改列筛选器。  
  
    -   如果发布拥有许多列的表中的大部分列，请执行 [sp_mergearticlecolumn](/sql/relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql)，同时将 **@column** 的值指定为 **@column** 指定列名称并将 **@operation** 的值指定为 **@operation** ，以添加所有列。 然后，对每个要排除的列执行一次 [sp_mergearticlecolumn](/sql/relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql)，同时将 **@operation** 的值指定为 **@operation** 并为 **@column** 中定义和修改列筛选器。  
  
#### <a name="to-change-a-column-filter-to-include-additional-columns-for-an-article-published-in-a-merge-publication"></a>为合并发布中发布的项目更改列筛选器以包括其他列  
  
1.  在发布服务器的发布数据库中，对要添加的每个列执行一次 [sp_mergearticlecolumn](/sql/relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql) 。 为 **@column** 指定列名称，将 **@operation** 的值指定为 **@operation** 指定列名称并将 **@change_active** 和 **@force_invalidate_snapshot** 的值都指定为 **@force_reinit_subscription** 中定义和修改列筛选器。  
  
2.  对发布重新运行快照代理作业以生成更新的快照。  
  
3.  重新初始化订阅。 有关详细信息，请参阅 [重新初始化订阅](../reinitialize-subscriptions.md)。  
  
#### <a name="to-change-a-column-filter-to-remove-columns-for-an-article-published-in-a-merge-publication"></a>为合并发布中发布的项目更改列筛选器以删除列  
  
1.  在发布服务器的发布数据库中，对要删除的每个列执行一次 [sp_mergearticlecolumn](/sql/relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql) 。 为 **@column** 指定列名称，将 **@operation** 的值指定为 **@operation** 指定列名称并将 **@change_active** 和 **@force_invalidate_snapshot** 的值都指定为 **@force_reinit_subscription** 中定义和修改列筛选器。  
  
2.  对发布重新运行快照代理作业以生成更新的快照。  
  
3.  重新初始化订阅。 有关详细信息，请参阅 [重新初始化订阅](../reinitialize-subscriptions.md)。  
  
###  <a name="TsqlExample"></a> 示例 (Transact-SQL)  
 在此事务复制示例中， `DaysToManufacture` 列将从基于 `Product` 表的项目中删除。  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../snippets/tsql/SQL15/replication/howto/tsql/createtranpub.sql#sp_addtranarticle)]  
  
 在此合并复制示例中， `CreditCardApprovalCode` 列将从基于 `SalesOrderHeader` 表的项目中删除。  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../snippets/tsql/SQL15/replication/howto/tsql/createmergepub.sql#sp_addmergearticle)]  
  
## <a name="see-also"></a>请参阅  
 [更改发布和项目属性](change-publication-and-article-properties.md)   
 [筛选已发布数据](filter-published-data.md)   
 [为合并复制筛选已发布数据](../merge/filter-published-data-for-merge-replication.md)  
  
  
