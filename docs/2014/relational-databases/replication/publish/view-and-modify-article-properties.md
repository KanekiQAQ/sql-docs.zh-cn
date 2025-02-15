---
title: 查看和修改项目属性 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changearticle
- sp_helparticle
- viewing replication properties
- sp_changemergearticle
- sp_helpmergearticle
- modifying replication properties, articles
- articles [SQL Server replication], modifying
- articles [SQL Server replication], properties
ms.assetid: e71831fa-3d39-4e4a-9706-4d3a497082cc
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 22355696fda59f984e21b72e1070aaa69cba9b97
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68199252"
---
# <a name="view-and-modify-article-properties"></a>查看和修改项目属性
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]或复制管理对象 (RMO) 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中查看和修改项目属性。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [建议](#Recommendations)  
  
-   **查看和修改项目属性，使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [复制管理对象 (RMO)](#RMOProcedure)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Restrictions"></a> 限制和局限  
  
-   创建复制后，有些属性便不可以再进行修改，如果该发布存在订阅，则其他属性也无法再进行修改。 不能进行修改的属性将显示为只读。  
  
###  <a name="Recommendations"></a> 建议  
  
-   创建发布之后，某些属性更改要求新的快照。 如果发布具有多个订阅，某些更改还会要求重新初始化所有订阅。 有关详细信息，请参阅[更改发布和项目属性](change-publication-and-article-properties.md)和[向现有发布添加项目和从中删除项目](add-articles-to-and-drop-articles-from-existing-publications.md)。  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在“发布属性 - \<发布>”对话框（在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 和复制监视器中可用）中查看和修改项目属性。  有关启动复制监视器的信息，请参阅[启动复制监视器](../monitor/start-the-replication-monitor.md)。  
  
-   **“常规”** 页，包含发布名称和说明、数据库名称、发布类型以及订阅过期设置。  
  
-   **“项目”** 页，对应于新建发布向导中的 **“项目”** 页。 使用此页可添加和删除项目，以及更改项目的属性和列筛选。  
  
-   **“筛选行”** 页，对应于新建发布向导中的 **“筛选表行”** 页。 使用此页可添加、编辑和删除所有发布类型的静态行筛选器，以及添加、编辑和删除合并发布的参数化行筛选器和联接筛选器。  
  
-   **“快照”** 页，使您可以指定快照的格式和位置、快照是否压缩，以及应用快照前后要运行的脚本。  
  
-   **“FTP 快照”** 页（适用于快照和事务发布，以及运行 SQL Server 2005 之前版本的发布服务器的合并发布），使您可以指定订阅服务器是否可以通过文件传输协议 (FTP) 下载快照文件。  
  
-   **“FTP 快照和 Internet”** 页（适用于运行 SQL Server 2005 或更高版本的发布服务器的合并发布），使您可以指定订阅服务器是否可以通过 FTP 下载快照文件，以及订阅服务器是否可以通过 HTTPS 对订阅进行同步。  
  
-   **“订阅选项”** 页，使您可以设置多个应用于所有订阅的选项。 这些选项会随着发布类型而有所不同。  
  
-   **“发布访问列表”** 页，使您可以指定可以访问发布的登录名和组。  
  
-   **“代理安全性”** 页，使您可以访问用于运行下列代理并连接复制拓扑中计算机的帐户设置：所有发布的快照代理、所有事务发布的日志读取器代理以及允许排队更新订阅的事务发布的队列读取器代理。  
  
-   **“数据分区”** 页（适用于来自运行 SQL Server 2005 或更高版本的发布服务器的合并发布），使您可以指定在快照不可用时使用参数化筛选器的发布的订阅服务器是否可以请求快照。 它还允许一次性或按循环计划生成一个或多个分区的快照。  
  
#### <a name="to-view-and-modify-article-properties"></a>查看和修改项目属性  
  
1.  在“发布属性 - \<发布>”对话框的“项目”页上，选择一个项目，然后单击“项目属性”。     
  
2.  选择要将更改应用于哪些项目属性：  
  
    -   单击“设置突出显示的 \<对象类型> 项目的属性”以启动“项目属性 - \<对象名>”对话框；在此对话框中进行的属性更改仅应用于在“项目”页上的对象窗格中突出显示的对象。     
  
    -   单击“设置所有 \<对象类型> 项目的属性”以启动“所有 \<对象类型> 项目的属性”对话框；在此对话框中进行的属性更改应用于“项目”页上的对象窗格中该类型的所有对象，包括尚未选择进行发布的对象。     
  
        > [!NOTE]  
        >  在“所有 \<对象类型> 项目的属性”对话框中进行的属性更改会重写以前在“项目属性 - \<对象名>”对话框中进行的任何更改。   例如，若要为某对象类型的所有项目设置一些默认值，但还希望为单个对象设置一些属性，请首先设置所有项目的默认值。 然后再设置单个对象的属性。  
  
3.  根据需要修改属性，然后单击 **“确定”** 。  
  
4.  在“发布属性 - \<发布>”对话框中单击“确定”。    
  
##  <a name="TsqlProcedure"></a> 使用 Transact-SQL  
 可以使用复制存储过程以编程方式修改项目以及返回其属性。 使用的存储过程取决于项目所属的发布的类型。  
  
#### <a name="to-view-the-properties-of-an-article-belonging-to-a-snapshot-or-transactional-publication"></a>查看属于快照发布或事务发布的项目的属性  
  
1.  执行 [sp_helparticle](/sql/relational-databases/system-stored-procedures/sp-helparticle-transact-sql)，为 **@publication** 参数指定发布名称，并为 **@article** 参数指定项目的名称。 如果未指定 **@article** ，将返回该发布中所有项目的信息。  
  
2.  为表项目执行 [sp_helparticlecolumns](/sql/relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql) 可列出基表中可用的所有列。  
  
#### <a name="to-modify-the-properties-of-an-article-belonging-to-a-snapshot-or-transactional-publication"></a>修改属于快照发布或事务发布的项目的属性  
  
1.  执行 [sp_changearticle](/sql/relational-databases/system-stored-procedures/sp-changearticle-transact-sql)，指定 **@property** 参数中要更改的项目属性，并为 **@value** 参数指定项目的名称。  
  
    > [!NOTE]  
    >  如果更改需要生成新的快照，则还必须为 **@force_invalidate_snapshot** 指定值 **@force_invalidate_snapshot** ，并且如果更改需要初始化订阅服务器，则还必须为 **@force_invalidate_snapshot** 指定值 **@force_reinit_subscription** 。 有关更改时需要新快照或重新初始化的属性的详细信息，请参阅[更改发布和项目属性](change-publication-and-article-properties.md)。  
  
#### <a name="to-view-the-properties-of-an-article-belonging-to-a-merge-publication"></a>查看属于合并发布的项目的属性  
  
1.  执行 [sp_helpmergearticle](/sql/relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql)，为 **@publication** 参数指定发布名称，并为 **@article** 参数指定项目的名称。 如果未指定这些参数，将返回为发布或发布服务器中所有项目的信息。  
  
2.  为表项目执行 [sp_helpmergearticlecolumn](/sql/relational-databases/system-stored-procedures/sp-helpmergearticlecolumn-transact-sql) 可列出基表中可用的所有列。  
  
#### <a name="to-modify-the-properties-of-an-article-belonging-to-a-merge-publication"></a>修改属于合并发布的项目的属性  
  
1.  执行 [sp_changemergearticle](/sql/relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql)，指定 **@property** 参数中要更改的项目属性，并为 **@value** 参数指定项目的名称。  
  
    > [!NOTE]  
    >  如果更改需要生成新的快照，则还必须为 **@force_invalidate_snapshot** 指定值 **@force_invalidate_snapshot** ，并且如果更改需要初始化订阅服务器，则还必须为 **@force_invalidate_snapshot** 指定值 **@force_reinit_subscription** 。 有关更改时需要新快照或重新初始化的属性的详细信息，请参阅[更改发布和项目属性](change-publication-and-article-properties.md)。  
  
###  <a name="TsqlExample"></a> 示例 (Transact-SQL)  
 此事务复制示例返回了已发布项目的属性。  
  
 [!code-sql[HowTo#sp_helptranarticle](../../../snippets/tsql/SQL15/replication/howto/tsql/changetranart.sql#sp_helptranarticle)]  
  
 此事务复制示例更改了已发布项目的架构选项。  
  
 [!code-sql[HowTo#sp_changetranarticle](../../../snippets/tsql/SQL15/replication/howto/tsql/changetranart.sql#sp_changetranarticle)]  
  
 此合并复制示例返回了已发布项目的属性。  
  
 [!code-sql[HowTo#sp_helpmergearticle](../../../snippets/tsql/SQL15/replication/howto/tsql/changemergeart.sql#sp_helpmergearticle)]  
  
 此合并复制示例更改了发布项目的冲突检测设置。  
  
 [!code-sql[HowTo#sp_changemergearticle](../../../snippets/tsql/SQL15/replication/howto/tsql/changemergeart.sql#sp_changemergearticle)]  
  
##  <a name="RMOProcedure"></a> 使用复制管理对象 (RMO)  
 可以使用复制管理对象 (RMO) 以编程方式修改项目以及访问其属性。 用于查看或修改项目属性的 RMO 类取决于项目所属的发布的类型。  
  
#### <a name="to-view-or-modify-properties-of-an-article-that-belongs-to-a-snapshot-or-transactional-publication"></a>查看或修改属于快照发布或事务发布的项目的属性  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建 <xref:Microsoft.SqlServer.Replication.TransArticle> 类的实例。  
  
3.  设置 <xref:Microsoft.SqlServer.Replication.Article.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>和 <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> 属性。  
  
4.  为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置步骤 1 中的连接。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 `false`，则说明步骤 3 中的项目属性定义不正确或此项目不存在。  
  
6.  （可选）若要更改属性，请为可以设置的 <xref:Microsoft.SqlServer.Replication.TransArticle> 属性中的一个设置新值。  
  
7.  （可选）如果已将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> 的值指定为 `true`，则调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 方法以在服务器上提交更改。 如果将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> 的值指定为 `false`（默认值），则会将更改立即发送到服务器。  
  
#### <a name="to-view-or-modify-properties-of-an-article-that-belongs-to-a-merge-publication"></a>查看或修改属于合并发布的项目的属性  
  
1.  使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 类创建与发布服务器的连接。  
  
2.  创建 <xref:Microsoft.SqlServer.Replication.MergeArticle> 类的实例。  
  
3.  设置 <xref:Microsoft.SqlServer.Replication.Article.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>和 <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> 属性。  
  
4.  为 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 属性设置步骤 1 中的连接。  
  
5.  调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法获取该对象的属性。 如果此方法返回 `false`，则说明步骤 3 中的项目属性定义不正确或此项目不存在。  
  
6.  （可选）若要更改属性，请为可以设置的 <xref:Microsoft.SqlServer.Replication.MergeArticle> 属性中的一个设置新值。  
  
7.  （可选）如果已将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> 的值指定为 `true`，则调用 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 方法以在服务器上提交更改。 如果将 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> 的值指定为 `false`（默认值），则会将更改立即发送到服务器。  
  
###  <a name="PShellExample"></a> 示例 (RMO)  
 此示例更改一个合并项目来指定该项目所使用的业务逻辑处理程序。  
  
 [!code-csharp[HowTo#rmo_ChangeMergeArticle_BLH](../../../snippets/csharp/SQL15/replication/howto/cs/rmotestevelope.cs#rmo_changemergearticle_blh)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeMergeArticle_BLH](../../../snippets/visualbasic/SQL15/replication/howto/vb/rmotestenv.vb#rmo_vb_changemergearticle_blh)]  
  
## <a name="see-also"></a>请参阅  
 [为合并项目实现业务逻辑处理程序](../implement-a-business-logic-handler-for-a-merge-article.md)   
 [发布数据和数据库对象](publish-data-and-database-objects.md)   
 [更改发布和项目属性](change-publication-and-article-properties.md)   
 [Replication System Stored Procedures Concepts](../concepts/replication-system-stored-procedures-concepts.md)   
 [Advanced Merge Replication Conflict Detection and Resolution](../merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
  
