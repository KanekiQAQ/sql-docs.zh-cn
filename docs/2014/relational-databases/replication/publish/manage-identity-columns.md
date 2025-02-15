---
title: 管理标识列 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- identity values [SQL Server replication]
- merge replication [SQL Server replication], identity range management
- publishing [SQL Server replication], identity columns
- transactional replication, identity range management
- identity columns [SQL Server], replication
ms.assetid: 98892836-cf63-494a-bd5d-6577d9810ddf
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 12233854049cf83f809a434b0e28dd4fb06d8d65
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68212102"
---
# <a name="manage-identity-columns"></a>管理标识列
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中管理标识列。 在将订阅服务器插入操作复制回发布服务器时，必须对标识列进行管理，以免在订阅服务器和发布服务器上分配相同的标识值。 复制可自动管理标识范围，或者您可以选择手动处理标识范围管理。  有关由复制提供的标识范围管理选项的信息，请参阅[复制标识列](replicate-identity-columns.md)。  
  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Recommendations"></a> 建议  
  
-   在多个发布中发布表时，必须为这些发布指定相同的标识范围管理选项。 有关详细信息，请参阅[发布数据和数据库对象](publish-data-and-database-objects.md)中的“在多个发布中发布表”。  
  
-   要创建一个可在多个表中使用的自动递增数字或者可以从应用程序中调用而不引用任何表的自动递增数字，请参阅[序列号](../../sequence-numbers/sequence-numbers.md)。  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 可在新建发布向导的“项目属性 - \<项目>”对话框的“属性”选项卡上指定标识列管理选项。   有关使用此向导的详细信息，请参阅[创建发布](create-a-publication.md)。 在新建发布向导中：  
  
-   如果在 **“发布类型”** 页上选择 **“合并发布”** 或 **“带有更新订阅的事务发布”** ，那么请选择自动或手动标识范围管理（建议使用默认值自动）。 在发布表后不能修改该属性，但可以修改其他相关属性。  
  
-   如果选择其他的发布类型，则应将标识范围管理设置为手动。  
  
 在“项目属性 - \<项目>”对话框的“属性”选项卡上指定标识范围和阈值，该对话框可以在“发布属性 - \<发布>”对话框中找到。    有关访问此对话框的详细信息，请参阅 [View and Modify Publication Properties](view-and-modify-publication-properties.md)。  
  
#### <a name="to-specify-an-identity-column-management-option"></a>指定标识列管理选项  
  
1.  如果发布服务器运行的是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之前的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]版本，请在新建发布向导的 **“发布类型”** 页上，选择 **“合并发布”** 或 **“带有更新订阅的事务发布”** 。  
  
2.  在 **“项目”** 页上，选择一个包含标识列的表。  
  
3.  单击 **“项目属性”** ，再单击 **“设置突出显示的表项目的属性”** 。  
  
4.  在“项目属性 - \<项目>”对话框的“属性”选项卡的“标识范围管理”部分，将“自动管理标识范围”属性设置为“自动”或“手动”（对于运行 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 或更高版本的发布服务器），或者设置为“True”或“False”（对于运行 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 之前的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本的发布服务器）。          
  
5.  如果在步骤 4 中选择了 **“自动”** 或 **True** ，请输入下表中列出的选项值。 有关如何使用这些设置的详细信息，请参阅[复制标识列](replicate-identity-columns.md)中的“指定标识范围”部分。  
  
    |Option|ReplTest1|描述|  
    |------------|-----------|-----------------|  
    |**发布服务器范围大小**|表示范围大小的整数值（例如 20000）。|请参阅[复制标识列](replicate-identity-columns.md)中的“指定标识范围”部分。|  
    |**订阅服务器范围大小**|表示范围大小的整数值（例如 10000）。|请参阅[复制标识列](replicate-identity-columns.md)中的“指定标识范围”部分。|  
    |**范围阈值百分比**|表示百分比阈值的整数值（例如，90 相当于 90%）。|指定新标识范围之前在节点上使用的总标识值的百分比。<br /><br /> 注意:必须指定此值，但它仅用于：使用排队更新订阅的订阅服务器以及运行 [!INCLUDE[ssEW](../../../includes/ssew-md.md)] 或以前版本的其他 SQL Server 版本的合并发布的订阅服务器。 有关详细信息，请参阅[复制标识列](replicate-identity-columns.md)中的“指定标识范围”部分。|  
    |**下一个范围的起始值**|整数值。 只读。|下一个范围的起始值。 例如，如果当前范围是 5001-6000，此起始值就是 6001。|  
    |**最大标识值**|整数值。 只读。|标识列的最大值。 由列的基本数据类型决定。|  
    |**增量**|整数值。 只读。|对于每个插入操作，标识列中的数字应增加或减少的量：通常设置为 1。|  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-modify-identity-ranges-and-thresholds-after-a-table-is-published"></a>发布表后修改标识范围和阈值  
  
1.  在“发布属性 - \<发布>”对话框的“项目”页上，选择一个具有标识列的表。    
  
2.  单击 **“项目属性”** ，再单击 **“设置突出显示的表项目的属性”** 。  
  
3.  在“项目属性 - \<项目>”对话框的“属性”选项卡的“标识范围管理”部分中，输入一个或多个以下属性的值：    **“发布服务器范围大小”、“订阅服务器范围大小”和“范围阈值百分比”**   。  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  在“发布属性 - \<发布>”对话框中单击“确定”。    
  
##  <a name="TsqlProcedure"></a> 使用 Transact-SQL  
 创建项目时，可使用复制存储过程来指定标识范围管理选项。  
  
#### <a name="to-enable-automatic-identity-range-management-when-defining-articles-for-a-transactional-publication"></a>定义事务发布的项目时启用自动标识范围管理  
  
1.  在发布服务器上，对发布数据库执行 [sp_addarticle](/sql/relational-databases/system-stored-procedures/sp-addarticle-transact-sql)。 如果要发布的源表具有标识列，则在为 **@threshold** 分配新标识范围之前，应将 **@identityrangemanagementoption** 的值指定为 **@pub_identity_range** ，指定分配给发布服务器的标识值范围 **@identity_range** ，指定分配给每个订阅服务器的标识值范围 **@threshold** 中管理标识列。 有关如何定义项目的详细信息，请参阅[定义项目](define-an-article.md)。  
  
    > [!NOTE]  
    >  确保标识列的数据类型足够大，从而能够支持分配到所有订阅服务器的标识总范围。  
  
#### <a name="to-disable-automatic-identity-range-management-when-defining-articles-for-a-transactional-publication"></a>定义事务发布的项目时禁用自动标识范围管理  
  
1.  在发布服务器上，对发布数据库执行 [sp_addarticle](/sql/relational-databases/system-stored-procedures/sp-addarticle-transact-sql)。 将 **@identityrangemanagementoption** 分配新标识范围之前，应将 **@identityrangemanagementoption** 中管理标识列。 有关如何定义项目的详细信息，请参阅[定义项目](define-an-article.md)。  
  
2.  将范围分配到订阅服务器上的标识项目列，以避免在更新订阅服务器时产生冲突。 有关详细信息，请参阅[复制标识列](replicate-identity-columns.md)主题中的“为手动标识范围管理分配范围”部分。  
  
#### <a name="to-enable-automatic-identity-range-management-when-defining-articles-for-a-merge-publication"></a>定义合并发布的项目时启用自动标识范围管理  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergearticle](/sql/relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql)。 如果要发布的源表具有标识列，则在为 **@threshold** 分配新标识范围之前，应将 **@identityrangemanagementoption** 的值指定为 **@pub_identity_range** ，指定为分配给服务器订阅的标识值范围 **@identity_range** ，指定分配给每个订阅服务器的标识值范围 **@threshold** 中管理标识列。 有关何时分配新标识范围的详细信息，请参阅[复制标识列](replicate-identity-columns.md)主题中的“分配标识范围”。 有关如何定义项目的详细信息，请参阅[定义项目](define-an-article.md)。  
  
    > [!NOTE]  
    >  确保标识列的数据类型足够大，从而能够支持分配到所有订阅服务器（尤其是具有服务器订阅的订阅服务器）的标识总范围。  
  
#### <a name="to-disable-automatic-identity-range-management-when-defining-articles-for-a-merge-publication"></a>定义合并发布的项目时禁用自动标识范围管理  
  
1.  在发布服务器上，对发布数据库执行 [sp_addmergearticle](/sql/relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql)。 为 **@identityrangemanagementoption** 指定下列值之一：  
  
    -   **manual** - 必须手动分配标识范围以更新订阅服务器。  
  
    -   **none** - 不将发布服务器上的标识列定义为订阅服务器上的标识列。  
  
     有关如何定义项目的详细信息，请参阅[定义项目](define-an-article.md)。  
  
2.  将范围分配到订阅服务器上的标识项目列，以避免在更新订阅服务器时产生冲突。  
  
#### <a name="to-change-automatic-identity-range-management-settings-for-an-existing-article-in-a-snapshot-or-transactional-publication"></a>为快照发布或事务发布中现有的项目更改自动标识范围管理设置  
  
1.  在发布服务器上，对发布数据库执行 [sp_helparticle](/sql/relational-databases/system-stored-procedures/sp-helparticle-transact-sql) ，并注意结果集中 **identityrangemanagementoption** 的值。 如果该值为 **0**，则未启用自动标识范围管理。  
  
2.  如果结果集中 **identityrangemanagementoption** 的值为 **1**，则按下面的方法更改设置：  
  
    -   若要更改已分配的标识范围，请在发布服务器上对发布数据库执行 [sp_changearticle](/sql/relational-databases/system-stored-procedures/sp-changearticle-transact-sql) 。 将 **@property** 在 **identity_range** 分配新标识范围之前，应将 **@property** ，并为 **@value** 中管理标识列。  
  
    -   若要更改用于分配新范围的阈值，在发布服务器上对发布数据库执行 [sp_changearticle](/sql/relational-databases/system-stored-procedures/sp-changearticle-transact-sql) 。 将 **@property** 分配新标识范围之前，应将 **@property** ，并为 **@value** 中管理标识列。  
  
#### <a name="to-change-automatic-identity-range-management-settings-for-an-existing-article-in-a-merge-publication"></a>为合并发布中的现有项目更改自动标识范围管理设置  
  
1.  在发布服务器上，对发布数据库执行 [sp_helpmergearticle](/sql/relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql) ，并注意结果集中 **identity_support** 的值。 如果该值为 **0**，则未启用自动标识范围管理。  
  
2.  如果结果集中 **identity_support** 的值为 **1**，则按下面的方法更改设置：  
  
    -   若要更改已分配的标识范围，在发布服务器上对发布数据库执行 [sp_changemergearticle](/sql/relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql) 。 将 **@property** 在 **identity_range** 分配新标识范围之前，应将 **@property** ，并为 **@value** 中管理标识列。  
  
    -   若要更改用于分配新范围的阈值，在发布服务器上对发布数据库执行 [sp_changemergearticle](/sql/relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql) 。 将 **@property** 分配新标识范围之前，应将 **@property** ，并为 **@value** 中管理标识列。 有关何时分配新标识范围的详细信息，请参阅[复制标识列](replicate-identity-columns.md)主题中的“分配标识范围”。  
  
    -   若要禁用自动标识范围管理，在发布服务器上对发布数据库执行 [sp_changemergearticle](/sql/relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql) 。 将 **identityrangemanagementoption** 分配新标识范围之前，应将 **@property** ，并将 **@identityrangemanagementoption** 在 **none** 分配新标识范围之前，应将 **@value** 中管理标识列。  
  
## <a name="see-also"></a>请参阅  
 [Peer-to-Peer Transactional Replication](../transactional/peer-to-peer-transactional-replication.md)   
 [Replication System Stored Procedures Concepts](../concepts/replication-system-stored-procedures-concepts.md)   
 [复制标识列](replicate-identity-columns.md)  
  
  
