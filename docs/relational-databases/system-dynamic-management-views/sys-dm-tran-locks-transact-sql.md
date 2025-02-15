---
title: sys.dm_tran_locks (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_tran_locks
- sys.dm_tran_locks
- sys.dm_tran_locks_TSQL
- dm_tran_locks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_locks dynamic management view
ms.assetid: f0d3b95a-8a00-471b-9da4-14cb8f5b045f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 32140a9ef5b1a8965876c4ee30fe72559ba8b7ce
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68262617"
---
# <a name="sysdmtranlocks-transact-sql"></a>sys.dm_tran_locks (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  返回 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中有关当前活动的锁管理器资源的信息。 向锁管理器发出的已授予锁或正等待授予锁的每个当前活动请求分别对应一行。  
  
 结果集中的列大体分为两组：资源组和请求组。 资源组说明正在进行锁请求的资源，请求组说明锁请求。  
  
> [!NOTE]  
> 若要调用此项从[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]或[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]，使用名称**sys.dm_pdw_nodes_tran_locks**。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**resource_type**|**nvarchar(60)**|表示资源类型。 该值可以是下列值之一：数据库、 文件、 对象、 页、 密钥、 范围、 RID、 应用程序、 元数据、 HOBT 或 ALLOCATION_UNIT。|  
|**resource_subtype**|**nvarchar(60)**|表示的子类型**resource_type**。 从技术角度而言，可以在未持有父类型的非子类型化锁的情况下获取子类型锁。 不同的子类型之间以及与非子类型化的父类型之间都不会发生冲突。 并非所有资源类型都有子类型。|  
|**resource_database_id**|**int**|此资源位于其范围之内的数据库的 ID。 由锁管理器处理的所有资源均按该数据库 ID 划分范围。|  
|**resource_description**|**nvarchar(256)**|资源的说明，其中只包含从其他资源列中无法获取的信息。|  
|**resource_associated_entity_id**|**bigint**|数据库中与资源相关联的实体的 ID。 该值可以是对象 ID、Hobt ID 或分配单元 ID，具体视资源类型而定。|  
|**resource_lock_partition**|**Int**|已分区锁资源的锁分区 ID。 对于未分区锁资源，该值为 0。|  
|**request_mode**|**nvarchar(60)**|请求的模式。 对于已授予的请求，为已授予模式；对于等待请求，为正在请求的模式。|  
|**request_type**|**nvarchar(60)**|请求类型。 该值为 LOCK。|  
|**request_status**|**nvarchar(60)**|该请求的当前状态。 可能的值有 GRANTED、CONVERT、WAIT、LOW_PRIORITY_CONVERT、LOW_PRIORITY_WAIT 或 ABORT_BLOCKERS。 有关低优先级的等待和中止阻塞程序的详细信息，请参阅*low_priority_lock_wait*一部分[ALTER INDEX &#40;TRANSACT-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)。|  
|**request_reference_count**|**smallint**|返回同一请求程序已请求该资源的近似次数。|  
|**request_lifetime**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**request_session_id**|**int**|当前拥有该请求的会话 ID。 对于分布式事务和绑定事务，拥有请求的会话 ID 可能不同。 该值为 -2 时，指示该请求属于孤立的分布式事务。 该值为 -3 时，指示请求属于延迟的恢复事务，例如因其回滚未能成功完成而延迟恢复该回滚的事务。|  
|**request_exec_context_id**|**int**|当前拥有该请求的进程的执行上下文 ID。|  
|**request_request_id**|**int**|当前拥有该请求的进程的请求 ID（批处理 ID）。 每当事务的多个活动的结果集 (MARS) 连接更改时，该值便会更改。|  
|**request_owner_type**|**nvarchar(60)**|拥有请求的实体类型。 锁管理器请求可由各种实体所拥有。 可能的值有：<br /><br /> TRANSACTION = 请求由事务所有。<br /><br /> CURSOR = 请求由游标所有。<br /><br /> SESSION = 请求由用户会话所有。<br /><br /> SHARED_TRANSACTION_WORKSPACE = 请求由事务工作区的共享部分所有。<br /><br /> EXCLUSIVE_TRANSACTION_WORKSPACE = 请求由事务工作区的排他部分所有。<br /><br /> NOTIFICATION_OBJECT = 请求由内部 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件所有。 此组件已经请求锁管理器在有其他组件等待获取锁时进行通知。 FileTable 功能是使用此值的一个组件。<br /><br /> **注意：** 工作空间在内部用于登记的会话持有锁。|  
|**request_owner_id**|**bigint**|此请求的特定所有者的 ID。<br /><br /> 当事务是请求的所有者时，此值包含事务 ID。<br /><br /> 当 FileTable 是请求的所有者**request_owner_id**具有以下值之一。<br /><br /> <br /><br /> -4:一个 FileTable 已获取数据库锁。<br /><br /> -3:一个 FileTable 已获取表锁。<br /><br /> 其他值：此值表示的文件句柄。 此值还显示为**fcb_id**动态管理视图[sys.dm_filestream_non_transacted_handles &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md)。|  
|**request_owner_guid**|**uniqueidentifier**|此请求的特定所有者的 GUID。 该值仅供分布式事务使用，在该事务中，该值与事务的 MS DTC GUID 相对应。|  
|**request_owner_lockspace_id**|**nvarchar(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] 该值表示请求程序的锁空间 ID。 锁空间 ID 确定两个请求程序是否相互兼容以及在两者冲突的模式下是否可以向其授予锁。|  
|**lock_owner_address**|**varbinary(8)**|用于跟踪该请求的内部数据结构的内存地址。 可以联接此列使用**resource_address**中的列**sys.dm_os_waiting_tasks**。|  
|**pdw_node_id**|**int**|**适用于**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]， [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> <br /><br /> 对于此分布的节点标识符。|  
  
## <a name="permissions"></a>权限

上[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]，需要`VIEW SERVER STATE`权限。   
上[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]高级层，需要`VIEW DATABASE STATE`数据库中的权限。 上[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]标准版和基本层，需要**服务器管理员**或**Azure Active Directory 管理员**帐户。   
 
## <a name="remarks"></a>备注  
 已授予请求状态指示已将资源上的锁授予请求程序。 等待请求指示尚未授予请求。 返回下列等待请求类型**request_status**列：  
  
-   转换请求状态指示已向请求程序授予对资源的请求，并且请求程序当前正在等待升级到要授予的初始请求。  
  
-   等待请求状态指示请求程序当前未持有对资源的已授予请求。  
  
 因为**sys.dm_tran_locks**填充从内部锁管理器数据结构，维护此信息不会添加到常规的额外开销处理。 具体化该视图需要访问锁管理器的内部数据结构。 这可能会略微影响服务器中的常规处理。 这些影响应该很难察觉，并且应该只会影响频繁使用的资源。 由于该视图中的数据与活动的锁管理器状态相对应，因此该数据可能会随时更改，并且在获取和释放锁时会相应地添加和删除行。 该视图不包含历史信息。  
  
 仅当所有资源组列都相等时，才对同一资源执行两个请求。  
  
 您可以使用下列工具控制读取操作的锁定：  
  
-   SET TRANSACTION ISOLATION LEVEL 用于指定会话的锁定级别。 有关详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。  
  
-   锁定表提示用于指定在 FROM 子句中表的单个引用的锁定级别。 有关语法和限制，请参阅[表提示&#40;TRANSACT-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md)。  
  
 使用一个会话 ID 运行的资源可以有多个已授予锁。 正在运行一个会话下的不同实体可以拥有同一资源上的锁和中显示的信息**request_owner_type**并**request_owner_id**的列返回的**sys.dm_tran_locks**。 如果多个实例的相同**request_owner_type**存在， **request_owner_id**列用来区分每个实例。 对于分布式事务， **request_owner_type**并**request_owner_guid**列将显示不同的实体的信息。  
  
 例如，会话 S1 拥有共享的锁上**Table1**; 并事务 T1，会话 S1 下运行，还在拥有共享的锁**Table1**。 在这种情况下， **resource_description**返回的列**sys.dm_tran_locks**将显示同一资源的两个实例。 **Request_owner_type**列将显示为会话，事务作为另一个实例。 此外， **resource_owner_id**列将具有不同的值。  
  
 在一个会话下运行的多个游标无法区分，被视为一个实体。  
  
 与会话 ID 值没有关联的分布式事务是孤立事务，向该事务分配的会话 ID 值为 -2。 有关详细信息，请参阅 [KILL (Transact-SQL)](../../t-sql/language-elements/kill-transact-sql.md)。  
  
## <a name="resource-details"></a>资源详细信息  
 下表列出了在中表示的资源**resource_associated_entity_id**列。  
  
|资源类型|资源说明|Resource_associated_entity_id|  
|-------------------|--------------------------|--------------------------------------|  
|DATABASE|表示数据库。|不适用|  
|FILE|表示数据库文件。 此文件可以是数据文件，也可以是日志文件。|不适用|  
|OBJECT|表示数据库对象。 此对象可以是数据表、视图、存储过程、扩展存储过程或任何具有对象 ID 的对象。|对象 ID|  
|PAGE|表示数据文件中的单页。|HoBt ID。 此值对应于**sys.partitions.hobt_id**。 PAGE 资源并不总是有 HoBt ID，因为 HoBt ID 是可由调用方提供的额外信息，而有些调用方不能提供该信息。|  
|KEY|表示索引中的一行。|HoBt ID。 此值对应于**sys.partitions.hobt_id**。|  
|EXTENT|表示数据文件区。 区是由八个连续页构成的组。|不适用|  
|RID|表示堆中的物理行。|HoBt ID。 此值对应于**sys.partitions.hobt_id**。 RID 资源并不总是有 HoBt ID，因为 HoBt ID 是可由调用方提供的额外信息，而有些调用方不能提供该信息。|  
|APPLICATION|表示指定了应用程序的资源。|不适用|  
|METADATA|表示元数据信息。|不适用|  
|HOBT|表示堆或 B 树。 它们是基本访问路径结构。|HoBt ID。 此值对应于**sys.partitions.hobt_id**。|  
|ALLOCATION_UNIT|表示一组相关页，如索引分区。 每个分配单元都包含一个索引分配映射 (IAM) 链。|分配单元 ID。 此值对应于**sys.allocation_units.allocation_unit_id**。|  
  
 下表列出了与每个资源类型相关联的子类型。  
  
|ResourceSubType|同步|  
|---------------------|------------------|  
|ALLOCATION_UNIT.BULK_OPERATION_PAGE|用于批处理操作的预先分配的页。|  
|ALLOCATION_UNIT.PAGE_COUNT|在延迟删除操作期间的分配单元页计数统计信息。|  
|DATABASE.BULKOP_BACKUP_DB|数据库备份与大容量操作。|  
|DATABASE.BULKOP_BACKUP_LOG|数据库日志备份与大容量操作。|  
|DATABASE.CHANGE_TRACKING_CLEANUP|更改跟踪清除任务。|  
|DATABASE.CT_DDL|数据库和表级更改跟踪 DDL 操作。|  
|DATABASE.CONVERSATION_PRIORITY|Service Broker 会话优先级操作，如 CREATE BROKER PRIORITY。|  
|DATABASE.DDL|数据定义语言 (DDL) 操作与文件组操作（如删除）。|  
|DATABASE.ENCRYPTION_SCAN|TDE 加密同步。|  
|DATABASE.PLANGUIDE|计划引导同步。|  
|DATABASE.RESOURCE_GOVERNOR_DDL|资源调控器操作的 DDL 操作，例如 ALTER RESOURCE POOL。|  
|DATABASE.SHRINK|数据库收缩操作。|  
|DATABASE.STARTUP|用于数据库启动同步。|  
|FILE.SHRINK|文件收缩操作。|  
|HOBT.BULK_OPERATION|下列隔离级别下的优化堆大容量加载操作与并发扫描：快照、未提交读和使用行版本控制的已提交读。|  
|HOBT.INDEX_REORGANIZE|堆或索引重组操作。|  
|OBJECT.COMPILE|存储过程编译。|  
|OBJECT.INDEX_OPERATION|索引操作。|  
|OBJECT.UPDSTATS|表的统计信息更新。|  
|METADATA.ASSEMBLY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_CLR_NAME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_TOKEN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASYMMETRIC_KEY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_ACTIONS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_SPECIFICATION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AVAILABILITY_GROUP|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CERTIFICATE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CHILD_INSTANCE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_FRAGMENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_ROWSET|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_RECV|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_SEND|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_GROUP|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_PRIORITY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CREDENTIAL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CRYPTOGRAPHIC_PROVIDER|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATA_SPACE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE_PRINCIPAL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_SESSION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_WITNESS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_PRINCIPAL_SID|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT_WEBMETHOD|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_COLUMN|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_CATALOG|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_INDEX|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_STOPLIST|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEX_EXTENSION_SCHEME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEXSTATS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INSTANTIATED_TYPE_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.MESSAGE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.METADATA_CACHE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PARTITION_FUNCTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PASSWORD_POLICY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PERMISSIONS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE_SCOPE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME_HASH|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.REMOTE_SERVICE_BINDING|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ROUTE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SCHEMA|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_CACHE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_DESCRIPTOR|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SEQUENCE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_EVENT_SESSIONS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_PRINCIPAL|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_BROKER_GUID|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_CONTRACT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_MESSAGE_TYPE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.STATS|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SYMMETRIC_KEY|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.USER_TYPE|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COLLECTION|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COMPONENT|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_INDEX_QNAME|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 下表提供的格式**resource_description**每种资源类型的列。  
  
|Resource|格式|描述|  
|--------------|------------|-----------------|  
|DATABASE|不适用|数据库 ID 已推出**resource_database_id**列。|  
|FILE|<file_id>|此资源所表示的文件 ID。|  
|OBJECT|<object_id>|此资源所表示的对象 ID。 此对象可以是任何对象中列出**sys.objects**，而不仅仅是一个表。|  
|PAGE|< file_id >: < page_in_file >|表示此资源所表示的页的文件和页 ID。|  
|KEY|<hash_value>|表示行中由此资源表示的键列的哈希。|  
|EXTENT|< file_id >: < page_in_files >|表示此资源所表示的区的文件和页 ID。 区 ID 与区中的第一页的页 ID 相同。|  
|RID|< file_id >: < page_in_file >: < row_on_page >|表示此资源所表示的行的页 ID 和行 ID。 请注意，如果关联的对象 ID 为 99，则此资源表示 IAM 链的第一个 IAM 页上的八个混合页槽之一。|  
|APPLICATION|\<DbPrincipalId >:\<最多 32 个字符 >:(< hash_value >)|表示用于划分此应用程序锁资源范围的数据库主体的 ID。 还包含与此应用程序锁资源相对应的资源字符串，最多包含其中的 32 个字符。 在某些情况下，因不再提供完整字符串而只能显示 2 个字符。 只有在恢复过程中重新获取的应用程序锁处于数据库恢复期间才会发生此行为。 哈希值表示与此应用程序锁资源相对应的完整资源字符串的哈希。|  
|HOBT|不适用|HoBt ID 是作为包含**resource_associated_entity_id**。|  
|ALLOCATION_UNIT|不适用|分配单元 ID 是作为包含**resource_associated_entity_id**。|  
|METADATA.ASSEMBLY|assembly_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_CLR_NAME|$qname_id = Q|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSEMBLY_TOKEN|assembly_id = A, $token_id|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ASSYMMETRIC_KEY|asymmetric_key_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT|audit_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_ACTIONS|device_id = D, major_id = M|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AUDIT_SPECIFICATION|audit_specification_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.AVAILABILITY_GROUP|availability_group_id = A|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CERTIFICATE|certificate_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CHILD_INSTANCE|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_FRAGMENT|object_id = O , compressed_fragment_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.COMPRESSED_ROW|object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_RECV|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSTATION_ENDPOINT_SEND|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_GROUP|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CONVERSATION_PRIORITY|conversation_priority_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CREDENTIAL|credential_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.CRYPTOGRAPHIC_PROVIDER|provider_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATA_SPACE|data_space_id = D|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE|database_id = D|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DATABASE_PRINCIPAL|principal_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_SESSION|database_id = D|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_MIRRORING_WITNESS|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.DB_PRINCIPAL_SID|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT|endpoint_id = E|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ENDPOINT_WEBMETHOD|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_CATALOG|fulltext_catalog_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_INDEX|object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_COLUMN|object_id = O, column_id = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.EXPR_HASH|object_id = O, $hash = H|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_CATALOG|fulltext_catalog_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_INDEX|object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.FULLTEXT_STOPLIST|fulltext_stoplist_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEX_EXTENSION_SCHEME|index_extension_id = I|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INDEXSTATS|object_id = O, index_id or stats_id = I|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.INSTANTIATED_TYPE_HASH|user_type_id = U, hash = H|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.MESSAGE|message_id = M|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.METADATA_CACHE|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PARTITION_FUNCTION|function_id = F|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PASSWORD_POLICY|principal_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PERMISSIONS|class = C|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.PLAN_GUIDE|plan_guide_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA. PLAN_GUIDE_HASH|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA. PLAN_GUIDE_SCOPE|scope_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME|$qname_id = Q|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.QNAME_HASH|$qname_scope_id = Q, $qname_hash = H|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.REMOTE_SERVICE_BINDING|remote_service_binding_id = R|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.ROUTE|route_id = R|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SCHEMA|schema_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_CACHE|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SECURITY_DESCRIPTOR|sd_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SEQUENCE|$seq_type = S, object_id = O|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER|server_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_EVENT_SESSIONS|event_session_id = E|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVER_PRINCIPAL|principal_id = P|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE|service_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_BROKER_GUID|$hash = H1:H2:H3|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_CONTRACT|service_contract_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SERVICE_MESSAGE_TYPE|message_type_id = M|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.STATS|object_id = O, stats_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.SYMMETRIC_KEY|symmetric_key_id = S|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.USER_TYPE|user_type_id = U|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COLLECTION|xml_collection_id = X|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_COMPONENT|xml_component_id = X|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|METADATA.XML_INDEX_QNAME|object_id = O, $qname_id = Q|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
 以下 Xevent 与分区相关**交换机**和联机索引重新生成。 有关语法的信息，请参阅[ALTER TABLE &#40;TRANSACT-SQL&#41; ](../../t-sql/statements/alter-table-transact-sql.md)并[ALTER INDEX &#40;-&#41;](../../t-sql/statements/alter-index-transact-sql.md)。  
  
-   lock_request_priority_state  
  
-   process_killed_by_abort_blockers  
  
-   ddl_with_wait_at_low_priority  
  
 现有 XEvent **progress_report_online_index_operation**联机索引操作已通过添加扩展**partition_number**并**partition_id**。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-sysdmtranlocks-with-other-tools"></a>A. 将 sys.dm_tran_locks 与其他工具一起使用  
 以下示例处理更新操作被另一个事务阻塞的情况。 通过使用**sys.dm_tran_locks**和其他工具，提供了有关锁定资源信息。  
  
```sql  
USE tempdb;  
GO  
  
-- Create test table and index.  
CREATE TABLE t_lock  
    (  
    c1 int, c2 int  
    );  
GO  
  
CREATE INDEX t_lock_ci on t_lock(c1);  
GO  
  
-- Insert values into test table  
INSERT INTO t_lock VALUES (1, 1);  
INSERT INTO t_lock VALUES (2,2);  
INSERT INTO t_lock VALUES (3,3);  
INSERT INTO t_lock VALUES (4,4);  
INSERT INTO t_lock VALUES (5,5);  
INSERT INTO t_lock VALUES (6,6);  
GO  
  
-- Session 1  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  
  
BEGIN TRAN  
    SELECT c1  
        FROM t_lock  
        WITH(holdlock, rowlock);  
  
-- Session 2  
BEGIN TRAN  
    UPDATE t_lock SET c1 = 10  
```  
  
 下面的查询将显示锁信息。 值`<dbid>`应替换为**database_id**从**sys.databases**。  
  
```sql  
SELECT resource_type, resource_associated_entity_id,  
    request_status, request_mode,request_session_id,  
    resource_description   
    FROM sys.dm_tran_locks  
    WHERE resource_database_id = <dbid>  
```  
  
 下面的查询使用前一个查询中的 `resource_associated_entity_id` 返回对象信息。 必须在连接到包含此对象的数据库时执行此查询。  
  
```  
SELECT object_name(object_id), *  
    FROM sys.partitions  
    WHERE hobt_id=<resource_associated_entity_id>  
```  
  
 下面的查询将显示阻塞信息。  
  
```sql  
SELECT   
        t1.resource_type,  
        t1.resource_database_id,  
        t1.resource_associated_entity_id,  
        t1.request_mode,  
        t1.request_session_id,  
        t2.blocking_session_id  
    FROM sys.dm_tran_locks as t1  
    INNER JOIN sys.dm_os_waiting_tasks as t2  
        ON t1.lock_owner_address = t2.resource_address;  
```  
  
 通过回滚事务来释放资源。  
  
```sql  
-- Session 1  
ROLLBACK;  
GO  
  
-- Session 2  
ROLLBACK;  
GO  
```  
  
### <a name="b-linking-session-information-to-operating-system-threads"></a>B. 将会话信息链接到操作系统线程  
 下面的示例返回将会话 ID 与 Windows 线程 ID 相关联的信息。 可以在 Windows 性能监视器中监视线程的性能。 该查询不返回当前正在休眠的会话 ID。  
  
```sql  
SELECT STasks.session_id, SThreads.os_thread_id  
    FROM sys.dm_os_tasks AS STasks  
    INNER JOIN sys.dm_os_threads AS SThreads  
        ON STasks.worker_address = SThreads.worker_address  
    WHERE STasks.session_id IS NOT NULL  
    ORDER BY STasks.session_id;  
GO  
```  
  
## <a name="see-also"></a>请参阅  
 [sys.dm_tran_database_transactions &#40;TRANSACT-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md)   
 [动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与事务相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
