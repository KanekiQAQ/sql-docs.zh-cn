---
title: 配置快照属性（复制 Transact-SQL 编程）| Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server replication], properties
ms.assetid: 978d150f-8971-458a-ab2b-3beba5937b46
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d7d71a8622496d89c18ddb8ab68fdc5272ae4d61
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67907955"
---
# <a name="configure-snapshot-properties-replication-transact-sql-programming"></a>配置快照属性（复制 Transact-SQL 编程）
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  可以使用复制存储过程以编程方式定义和修改快照属性，使用的存储过程取决于发布的类型。  
  
### <a name="to-configure-snapshot-properties-when-creating-a-snapshot-or-transactional-publication"></a>在创建快照发布或事务发布时配置快照属性  
  
1.  在发布服务器上，执行 [sp_addpublication](../../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)。 为 **@publication** 指定发布名称，将 **@repl_freq** 的值指定为 **snapshot** 或 **@repl_freq** ，并指定一个或多个下列与快照相关的参数：  
  
    -   **@alt_snapshot_folder** - 如果此发布的快照可从某位置访问，而不是或者也能从快照的默认文件夹访问，则指定相应路径。    
    -   **@compress_snapshot** - 如果备用快照文件夹内的快照文件是 **CAB 文件格式的压缩文件，则将值指定为** true [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 。    
    -   **@pre_snapshot_script** - 指定在初始快照应用之前的初始化过程中将在订阅服务器上执行的 **.sql** 文件的文件名和完整路径。    
    -   **@post_snapshot_script** - 指定在初始快照应用之前的初始化过程中将在订阅服务器上执行的 **.sql** 文件的文件名和完整路径。    
    -   **@snapshot_in_defaultfolder** - 如果备用快照文件夹内的快照文件是 **false** 。  
  
     有关创建发布的详细信息，请参阅 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
### <a name="to-configure-snapshot-properties-when-creating-a-merge-publication"></a>创建合并发布时配置快照属性  
  
1.  在发布服务器上，执行 [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)。 为 **@publication** 指定发布名称，将 **@repl_freq** 的值指定为 **snapshot** 或 **@repl_freq** ，并指定一个或多个下列与快照相关的参数：  
  
    -   **@alt_snapshot_folder** - 如果此发布的快照可从某位置访问，而不是或者也能从快照的默认文件夹访问，则指定相应路径。    
    -   **@compress_snapshot** - 如果备用快照文件夹内的快照文件是 **CAB 文件格式的压缩文件，则将值指定为** 。   
    -   **@pre_snapshot_script** - 指定在初始快照应用之前的初始化过程中将在订阅服务器上执行的 **.sql** 文件的文件名和完整路径。    
    -   **@post_snapshot_script** - 指定在初始快照应用之前的初始化过程中将在订阅服务器上执行的 **.sql** 文件的文件名和完整路径。    
    -   **@snapshot_in_defaultfolder** - 如果备用快照文件夹内的快照文件是 **false** 。  
  
2.  有关创建发布的详细信息，请参阅 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)。  
  
### <a name="to-modify-snapshot-properties-of-an-existing-snapshot-or-transactional-publication"></a>修改现有快照发布或事务发布的快照属性  
  
1.  在发布服务器上，对发布数据库执行 [sp_changepublication](../../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)。 将 **@force_invalidate_snapshot** 或 **@force_invalidate_snapshot** ，并为 **@property** 指定下列值之一：  
  
    -   **alt_snapshot_folder** - 也为 **@value** 。    
    -   **compress_snapshot** - 也将 **CAB 文件格式的压缩文件，则将值指定为** 的值指定为 **false** 或 **@value** ，以指示备用快照文件夹内的快照文件是否为 CAB 文件格式的压缩文件。    
    -   **pre_snapshot_script** - 也为 **@value** 指定在初始快照应用之前的初始化过程中将在订阅服务器上执行的 **.sql** 文件的文件名和完整路径。    
    -   **post_snapshot_script** - 也为 **@value** 指定在初始快照应用之前的初始化过程中将在订阅服务器上执行的 **.sql** 文件的文件名和完整路径。    
    -   **snapshot_in_defaultfolder** - 也将值指定为 **true** 或 **false** ，以指示快照是否仅在非默认位置可用。  
  
2.  （可选）在发布服务器上，对发布数据库执行 [sp_changepublication_snapshot](../../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)。 指定 **@publication** 以及将要更改的一个或多个计划或安全凭据参数。  
  
    > [!IMPORTANT]  
    >  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
3.  在命令提示符处运行 [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md) 或启动快照代理作业以生成新的快照。 有关详细信息，请参阅 [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
### <a name="to-modify-snapshot-properties-of-an-existing-merge-publication"></a>修改现有合并发布的快照属性  
  
1.  在发布服务器上，对发布数据库执行 [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)。 将 **@force_invalidate_snapshot** 或 **@force_invalidate_snapshot** ，并为 **@property** 指定下列值之一：  
  
    -   **alt_snapshot_folder** - 也为 **@value** 。    
    -   **compress_snapshot** - 也将 **CAB 文件格式的压缩文件，则将值指定为** 的值指定为 **false** 或 **@value** ，以指示备用快照文件夹内的快照文件是否为 CAB 文件格式的压缩文件。    
    -   **pre_snapshot_script** - 也为 **@value** 指定在初始快照应用之前的初始化过程中将在订阅服务器上执行的 **.sql** 文件的文件名和完整路径。    
    -   **post_snapshot_script** - 也为 **@value** 指定在初始快照应用之前的初始化过程中将在订阅服务器上执行的 **.sql** 文件的文件名和完整路径。    
    -   **snapshot_in_defaultfolder** - 也将值指定为 **true** 或 **false** ，以指示快照是否仅在非默认位置可用。  
  
2.  在命令提示符处运行 [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md) 或启动快照代理作业以生成新的快照。 有关详细信息，请参阅 [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)。  
  
## <a name="example"></a>示例  
 此示例创建一个使用备用快照文件夹和压缩快照的发布。  
  
 [!code-sql[HowTo#sp_mergealtsnapshot](../../../relational-databases/replication/codesnippet/tsql/configure-snapshot-prope_1.sql)]  
  
## <a name="see-also"></a>另请参阅  
 [修改快照选项](../../../relational-databases/replication/snapshot-options.md)   
 [在应用快照之前和之后执行脚本](../../../relational-databases/replication/snapshot-options.md#execute-scripts-before-and-after-snapshot-is-applied)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [通过 FTP 传输快照](../../../relational-databases/replication//publish/deliver-a-snapshot-through-ftp.md)   
 [更改发布和项目属性](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)  
  
  
