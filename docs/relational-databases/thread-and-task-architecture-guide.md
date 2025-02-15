---
title: 线程和任务体系结构指南 | Microsoft Docs
ms.custom: ''
ms.date: 11/13/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, thread and task architecture
- thread and task architecture guide
ms.assetid: 925b42e0-c5ea-4829-8ece-a53c6cddad3b
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5dd4aa4c3beb769509884c6ebb75fd8c82c1c8ae
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68058127"
---
# <a name="thread-and-task-architecture-guide"></a>线程和任务体系结构指南
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

线程是操作系统功能，允许将应用程序逻辑分到几个并发的执行路径中。 当复杂应用程序包含可同时执行的多个任务时，此功能非常有用。 

操作系统执行应用程序实例时，它将创建一个单元（称为进程）来管理该实例。 此进程包含一个执行线程。 它是由应用程序代码执行的一系列编程指令。 例如，如果一个简单应用程序具有一组可串行执行的指令，则整个应用程序只有一个执行路径或线程。 更复杂的应用程序可能有几个任务，这些任务可以一前一后地执行，而不是串行执行。 通过为每个任务启动独立的进程，应用程序可以实现串行操作。 但是，启动进程会消耗大量资源。 而应用程序可启动独立的线程。 相对来说，这将消耗较少的资源。 而且，可以独立于与某进程关联的其他线程来安排每个线程的执行。

线程使复杂的应用程序能够更有效地利用 CPU，即使在只有一个 CPU 的计算机上也是如此。 如果只有一个 CPU，则每次只能执行一个线程。 如果一个线程执行不使用 CPU 的长时间运行的操作（如磁盘读/写操作），则第一个操作完成之前可以执行另一个线程。 通过在其他线程等待操作完成的同时执行线程，应用程序可以最大限度地利用 CPU。 对于大量占用磁盘 I/O 的多用户应用程序（如数据库服务器），这尤其有效。 具有多个微处理器 (CPU) 的计算机可以同时在每个 CPU 上执行一个线程。 例如，如果某计算机有八个 CPU，则它可以同时执行八个线程。

## <a name="sql-server-batch-or-task-scheduling"></a>SQL Server 批处理或任务计划

### <a name="allocating-threads-to-a-cpu"></a>为 CPU 分配线程
默认情况下，每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例启动每个线程，操作系统根据负载从计算机上的 CPU 中的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例分发线程。 如果已在操作系统级别启用了进程关联，则操作系统会将每个线程分配给特定的 CPU。 与之相反，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] 将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工作线程分配给在处理器 (CPU) 之间平均分发线程的计划程序。
    
为了执行多任务处理，例如当多个应用程序访问同一组处理器时，操作系统有时会在不同处理器之间移动进程线程。 虽然从操作系统方面而言，这种活动是高效的，但是在高系统负荷的情况下，该活动会降低 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的性能，因为每个处理器缓存都会不断地重新加载数据。 如果将各个处理器分配给特定线程，则通过消除处理器的重新加载需要以及减少处理器之间的线程迁移（因而减少上下文切换），可以提高在这些条件下的性能；线程与处理器之间的这种关联称为“处理器关联”。 如果已经启用了关联，则操作系统会将每个线程分配给一个特定的 CPU。 

[关联掩码选项](../database-engine/configure-windows/affinity-mask-server-configuration-option.md)通过使用 [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md) 进行设置。 如果未设置关联掩码，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例会在未应用掩码的计划程序之间平均分配工作线程。

> [!CAUTION]
> 请勿在操作系统中配置 CPU 关联后，还在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中配置关联掩码。 这些设置实现的效果相同，如果配置不一致，则可能会得到意外的结果。 有关详细信息，请参阅[关联掩码选项](../database-engine/configure-windows/affinity-mask-server-configuration-option.md)。

当服务器上连接有大量客户端时，线程池有助于优化性能。 一般情况下，会为每个查询请求创建一个单独的操作系统线程。 但是，当到服务器的连接达到数以百计时，为每个查询请求使用一个线程会占用大量的系统资源。 [最大工作线程数选项](..//database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)使 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 可以为更大数量的查询请求创建一个工作线程池，从而提高性能。 

### <a name="using-the-lightweight-pooling-option"></a>使用 lightweight pooling 选项
切换线程上下文的开销可能不是很大。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的大多数实例不会发现轻型池选项设置为 0 或 1 时性能有何差别。 只有运行在具有下列特征的计算机上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例可能从[轻型池](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)受益：    
* 大型多 CPU 服务器
* 所有 CPU 都以接近最大容量运行
* 存在高级别的上下文切换

如果将轻型池的值设为 1，则可能会略微提高这些系统的性能。

> [!IMPORTANT]
> 请勿使用纤程模式计划日常操作。 这会抑制上下文切换优点的正常发挥，从而降低性能，因为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的某些组件在纤程模式下不能正常工作。 有关详细信息，请参阅[轻型池](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)。

## <a name="thread-and-fiber-execution"></a>线程和纤程的执行
Microsoft Windows 使用从 1 到 31 的数值优先级系统计划线程的执行优先级。 零是保留值，专供操作系统使用。 当有多个线程等待执行时，Windows 将分派优先级最高的线程。

默认情况下，每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的优先级都是 7，这个优先级称为正常优先级。 此默认值为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 线程提供了足够高的优先级以获得足够的 CPU 资源，而不会对其他应用程序造成负面影响。 

[优先级提升](../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md)配置选项可用于将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例的线程优先级提高到 13。 此优先级称为高优先级。 此设置为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 线程提供了一个比其他大多数应用程序高的优先级。 因此，通常当 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 线程准备好运行时便获得分派，而不会被其他应用程序的线程抢先。 这可以在服务器只运行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例而未运行其他应用程序时提高性能。 但是，如果 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中发生占用大量内存的操作，其他应用程序便不可能有足够高的优先级而抢先于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 线程。 

如果在一台计算机上运行多个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例，而只为其中的某些实例打开了 priority boost 选项，那么任何以正常优先级运行的实例的性能都将受到负面影响。 而且，如果打开了优先级提升选项，服务器上的其他应用程序和组件的性能将会降低。 因此，必须在严格控制下使用此选项。

## <a name="hot-add-cpu"></a>热添加 CPU
热添加 CPU 是指能够动态向运行中的系统添加 CPU。 添加 CPU 时，可以通过添加新硬件来进行物理添加，或者通过联机硬件分区进行逻辑添加，或者通过虚拟化层进行虚拟添加。 从 [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] 开始，[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持热添加 CPU。

热添加 CPU 的要求：  
* 要求使用支持热添加 CPU 的硬件。
* 要求使用 Windows Server 2008 Datacenter 的 64 位版本或 Windows Server 2008 Enterprise Edition for Itanium-Based Systems 操作系统。
* 要求具有 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise。
* [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 无法配置为使用软 NUMA。 有关软 NUMA 的详细信息，请参阅 [Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md)。

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不会在添加 CPU 后自动开始使用它们。 这可以防止 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用可能是为其他用途而添加的 CPU。 添加 CPU 后，请执行 [RECONFIGURE](../t-sql/language-elements/reconfigure-transact-sql.md) 语句以便 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 将新 CPU 识别为可用资源。

> [!NOTE]
> 如果配置了 [affinity64 掩码](../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) ，则必须修改 affinity64 掩码以使用新 CPU。
 
## <a name="best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus"></a>在具有超过 64 个 CPU 的计算机上运行 SQL Server 的最佳做法

### <a name="assigning-hardware-threads-with-cpus"></a>给硬件线程分配 CPU
不要使用 affinity mask 和 affinity64 mask 服务器配置选项来将处理器绑定到特定线程。 这些选项限制为 64 个 CPU。 请改用 [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md) 的 SET PROCESS AFFINITY 选项。

### <a name="managing-the-transaction-log-file-size"></a>管理事务日志文件大小
不要依赖于自动增长来增加事务日志文件的大小。 增加事务日志必须是一个串行的过程。 扩展日志可能会阻止事务继续写操作，直到完成日志扩展。 请通过将文件大小设置为足够支持环境中典型工作负荷的值来预分配日志文件的空间。

### <a name="setting-max-degree-of-parallelism-for-index-operations"></a>设置索引操作的最大并行度
可以通过暂时将数据库的恢复模式设置为大容量日志恢复模式或简单恢复模式，以在具有许多 CPU 的计算机上改进索引操作（如创建或重新创建索引）的性能。 这些索引操作可以导致重大的日志活动和日志争用，从而影响 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 所做的最佳并行度 (DOP) 选择。

此外，请考虑调整这些操作的最大并行度 (MAXDOP) 服务器配置选项  。 以下准则基于内部测试，可以作为一般性建议。 您应尝试几个不同的 MAXDOP 设置来确定自己环境的最佳设置：

* 对于完整恢复模式，将最大并行度选项的值设置为 8 或更小。   
* 对于大容量日志恢复模式或简单恢复模式，应考虑将最大并行度选项的值设置为大于 8。   
* 对于配置了 NUMA 的服务器，最大并行度不应超过分配给每个 NUMA 节点的 CPU 数目。 这是因为查询更可能使用 1 个 NUMA 节点的本地内存，这可以缩短内存访问时间。  
* 对于启用了超线程且在 2009 年或更早（改进超线程功能之前）制造的服务器，MAXDOP 值不应超过物理处理器（而不是逻辑处理器）的数目。

有关最大并行度选项的详细信息，请参阅[配置最大并行度服务器配置选项](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。

### <a name="setting-the-maximum-number-of-worker-threads"></a>设置最大工作线程数
应始终将最大工作线程数设置为超过最大并行度设置。 工作线程数必须始终设置为至少是服务器上存在的 CPU 数目的七倍。 有关详细信息，请参阅 [配置最大工作线程数选项](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)。

### <a name="using-sql-trace-and-sql-server-profiler"></a>使用 SQL 跟踪和 SQL Server Profiler
不建议在生产环境中使用 SQL 跟踪和 SQL Profiler。 运行这些工具的系统开销也会随着 CPU 的数目的增加而增加。 如果您必须在生产环境中使用 SQL 跟踪，请将跟踪事件的数目限制为最少。 请在负荷下仔细探查和测试每个跟踪事件，并且避免使用显著影响性能的事件组合。

### <a name="setting-the-number-of-tempdb-data-files"></a>设置 tempdb 数据文件的数目
通常，tempdb 数据文件的数目应与 CPU 的数目匹配。 但是，通过仔细考虑 tempdb 的并发需要，可以减少数据库管理。 例如，如果一个系统具有 64 个 CPU 并且通常只有 32 个查询使用 tempdb，则将 tempdb 文件的数目增加到 64 将不会提高性能。

### <a name="sql-server-components-that-can-use-more-than-64-cpus"></a>可以使用超过 64 个 CPU 的 SQL Server 组件
下表列出了 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 组件，并指示它们是否可以使用超过 64 个 CPU。

|进程名称   |可执行程序 |是否可使用超过 64 个 CPU |  
|----------|----------|----------|  
|SQL Server 数据库引擎 |Sqlserver.exe  |是 |  
|Reporting Services |Rs.exe |否 |  
|Analysis Services  |As.exe |否 |  
|Integration Services   |Is.exe |否 |  
|Service Broker |Sb.exe |否 |  
|全文搜索   |Fts.exe    |否 |  
|SQL Server 代理   |Sqlagent.exe   |否 |  
|SQL Server Management Studio   |Ssms.exe   |否 |  
|SQL Server 安装程序   |Setup.exe  |否 |  
