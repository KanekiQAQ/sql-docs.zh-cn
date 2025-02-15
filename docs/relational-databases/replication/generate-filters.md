---
title: 生成筛选器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.generatefilters.f1
ms.assetid: be28515c-5d6d-467b-b933-d7c8d97a45b4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 341c6325064031b99eeb5d7b58c00dda686097db
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68128102"
---
# <a name="generate-filters"></a>生成筛选器
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  使用 **“生成筛选器”** 对话框，可以对合并发布中某一个表定义行筛选器；然后，复制会自动将筛选器扩展到通过外键关系相关的其他表。 例如，如果对 Customer 表定义筛选器，使其仅包含与法国客户有关的数据，则复制将扩展该筛选器，以便相关的 Orders 表和 Order Details 表仅包含与法国客户相关的信息。  
  
## <a name="options"></a>选项  
 使用此对话框可以创建表的行筛选器，包括三个步骤。 然后，将筛选器扩展到与筛选的表通过主键和外键关系相关的表。 例如，假设有三个表： **Customer**、 **SalesOrderHeader**和 **SalesOrderDetail**，其中 **Customer** 表与 **SalesOrderHeader**表之间有关系， **SalesOrderHeader** 表与 **SalesOrderDetail**表之间有关系，将行筛选器应用于 **Customer**表，则复制会将该筛选器扩展到 **SalesOrderHeader** 表和 **SalesOrderDetail**表。  
  
1.  **选择要筛选的表。**  
  
     从下拉列表框中选择表。 只有在 **“项目”** 页上所选择的表才会出现在列表框中。  
  
2.  **完成筛选语句以标识订阅服务器所要接收的表行。**  
  
     定义新的筛选语句。 **“列”** 列表框列出了要从 **“选择要筛选的表”** 中选择的表中发布的所有列。 **“筛选语句”** 文本区域包括默认的文本，其格式为：  
  
     `SELECT <published_columns> FROM [tableowner].[tablename] WHERE`  
  
     此文本无法更改；请使用标准的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法在 WHERE 关键字后键入筛选子句。  
  
    > [!IMPORTANT]  
    >  为提高性能，建议您不要在参数化行筛选子句中对列名应用函数，如 `LEFT([MyColumn]) = SUSER_SNAME()`。 如果在筛选子句中使用 HOST_NAME 并覆盖 HOST_NAME 值，则可能需要使用 CONVERT 转换数据类型。 有关此情况的最佳实践的详细信息，请参阅主题 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)表。  
  
3.  **指定将从此表接收数据的订阅数。**  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

     [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] and later versions only. Merge replication allows you to specify the type of partitions that are best suited to your data and application. If you select **A row from this table will go to only one subscription**, merge replication sets the nonoverlapping partitions option. Nonoverlapping partitions work in conjunction with precomputed partitions to improve performance, with nonoverlapping partitions minimizing the upload cost associated with precomputed partitions. The performance benefit of nonoverlapping partitions is more noticeable when the parameterized filters and join filters used are more complex. If you select this option, you must ensure that the data is partitioned in such a way that a row cannot be replicated to more than one Subscriber. For more information, see the section "Setting 'partition options'" in the topic [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 添加筛选器之后，请单击 **“确定”** 退出并关闭该对话框。 将对照 SELECT 子句中的表分析并运行指定的筛选器。 如果筛选语句有语法错误或其他问题，将会通知您编辑该筛选语句。  
  
 分析该语句之后，复制将创建必要的联接筛选器。 如果您尚未对运行此向导的发布服务器配置分发服务器，系统将会提示您进行配置。  
  
## <a name="see-also"></a>另请参阅  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [查看和修改发布属性](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [筛选已发布数据](../../relational-databases/replication/publish/filter-published-data.md)   
 [Join Filters](../../relational-databases/replication/merge/join-filters.md)   
 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [发布数据和数据库对象](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
