---
title: CREATE WORKLOAD GROUP (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/05/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- WORKLOAD GROUP
- WORKLOAD_GROUP_TSQL
- CREATE WORKLOAD GROUP
- CREATE_WORKLOAD_GROUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE WORKLOAD GROUP statement
ms.assetid: d949e540-9517-4bca-8117-ad8358848baa
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 2528d770b1ad7e08898deb8e34350ccb3ad1ccfc
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67948092"
---
# <a name="create-workload-group-transact-sql"></a>CREATE WORKLOAD GROUP (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

创建资源调控器工作负荷组并将工作负荷组与资源调控器资源池关联。 不是 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的所有版本都提供资源调控器。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)。

## <a name="syntax"></a>语法

```
CREATE WORKLOAD GROUP group_name
[ WITH
    ( [ IMPORTANCE = { LOW | MEDIUM | HIGH } ]
      [ [ , ] REQUEST_MAX_MEMORY_GRANT_PERCENT = value ]
      [ [ , ] REQUEST_MAX_CPU_TIME_SEC = value ]
      [ [ , ] REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value ]
      [ [ , ] MAX_DOP = value ]
      [ [ , ] GROUP_MAX_REQUESTS = value ] )
 ]
[ USING {
    [ pool_name | "default" ]
    [ [ , ] EXTERNAL external_pool_name | "default" ] ]
    } ]
[ ; ]
```

## <a name="arguments"></a>参数

group_name 是工作负荷组的用户定义名称  。 group_name 由字母数字组成，最多可包含 128 个字符，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中必须是唯一的，并且必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则  。

IMPORTANCE = { LOW | MEDIUM | HIGH } 指定工作负荷组中某个请求的相对重要性  。 重要性为下列值之一，默认值为 MEDIUM：

- LOW
- MEDIUM（默认值）
- HIGH

> [!NOTE]
> 在内部，每个重要性设置都存储为用于计算的一个数字。

IMPORTANCE 对资源池而言是局部性的；同一资源池内重要性不同的工作负荷组会相互影响，但不会影响其他资源池中的工作负荷组。

REQUEST_MAX_MEMORY_GRANT_PERCENT = value 指定单个请求可从池中获取的最大内存量  。 value 是相对于 MAX_MEMORY_PERCENT 指定的资源池大小的百分比  。 value 是 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 以下版本的整数，并且自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 起上下浮动（默认设置为 25）  。 value 的允许范围是 1 到 100  。

> [!NOTE]
> 指定的量指的只是查询执行授予内存。
>
> - 将 value 设置为 0 可以阻止在用户定义的工作负荷组中运行具有 SORT 和 HASH MATCH 操作的查询  。 > - 建议不要将值设置为大于 70，这是因为如果正在运行其他并发查询，则服务器可能无法保留足够的空闲内存  。 可能最终会导致查询超时错误 8645。
> - 如果查询内存要求超过了此参数指定的限制，服务器会执行以下操作：
>
> 对于用户定义的工作负荷组，服务器会尝试降低查询的并行度，直到内存要求降到限制范围以内，或直到并行度等于 1。 如果查询内存要求仍然大于限制值，则会发生错误 8657。
>
> 对于内部和默认工作负荷组，服务器会允许查询获取必需的内存。
>
> 请注意，如果服务器没有足够的物理内存，则这两种情况都会出现超时错误 8645。

REQUEST_MAX_CPU_TIME_SEC = value 指定请求可以使用的最长 CPU 时间（以秒为单位）  。 value 必须为 0 或一个正整数  。 value 的默认设置为 0，也就是说无限制  。

> [!NOTE]
> 默认情况下，如果超过最长时间，Resource Governor 并不会阻止继续发出请求。 但会生成一个事件。 有关详细信息，请参阅 [CPU Threshold Exceeded 事件类](../../relational-databases/event-classes/cpu-threshold-exceeded-event-class.md)。
> [!IMPORTANT]
> 从 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP2 和 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3 开始以及使用[跟踪标志 2422](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 时，Resource Governor 将在超出最大时间时终止请求。

REQUEST_MEMORY_GRANT_TIMEOUT_SEC = value 指定查询等待内存授予（工作缓冲区内存）变为可用状态的最长时间（以秒为单位）  。 value 必须为 0 或一个正整数  。 value 的默认设置为 0，表示使用基于查询开销的内部计算来确定最长时间  。

> [!NOTE]
> 查询并不总是在达到内存授予超时的时候失败。 仅当有太多并发查询运行时，查询才失败。 否则，查询只能获取最小内存授予，从而导致查询性能下降。

MAX_DOP = value 指定并行请求的最大并行度 (DOP)  。 value 必须为 0 或一个正整数  。 value 的允许范围为 0 到 64  。 value 的默认设置为 0，表示使用全局设置  。 按如下方式处理 MAX_DOP：

- MAX_DOP 作为查询提示时，在未超出工作负荷组 MAX_DOP 时保持有效。 如果 MAXDOP 查询提示值超出使用资源调控器配置的值，则数据库引擎使用资源调控器 MAXDOP 值。
- 作为查询提示的 MAX_DOP 始终会覆盖 sp_configure 'max degree of parallelism'。
- 工作负荷组 MAX_DOP 覆盖 sp_configure 'max degree of parallelism'。
- 如果在编译时将查询标记为串行，则在运行时无法更改回并行，不论工作负荷组或 sp_configure 如何设置。
- 配置 DOP 后，只能在授予内存不足时降低它。 工作负荷组重新配置在授予内存队列中等待时不可见。

GROUP_MAX_REQUESTS = value 指定在工作负荷组中允许执行的同时请求最大数  。 value 必须为 0 或一个正整数  。 value 的默认设置为 0，表示允许的请求数不限  。 当达到最大并发请求数时，该组中的用户可以登录但置于等待状态，直至并发请求数降到指定值之下。

USING { pool_name | "default" } 将工作负荷组与由 pool_name 标识的用户定义的资源池关联起来    。 这实际上是将工作负荷组放入资源池中。 如果没有提供 pool_name，或如果没有使用 USING 参数，将工作负荷组放入预定义的资源调控器默认池  。

"default" 是保留字，并且在与 USING 一起使用时，必须使用引号 ("") 引起来或用方括号 ([]) 括起来。

> [!NOTE]
> 预定义工作负荷组和资源池都使用小写名称，例如“default”。 对于使用区分大小写排序规则的服务器，应当注意这一点。 使用不区分大小写排序规则的服务器（例如 SQL_Latin1_General_CP1_CI_AS）会将“default”和“Default”视为相同。

EXTERNAL external_pool_name | "default" 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]）  。

工作负荷组可以指定一个外部资源池。 可定义一个工作负荷组并关联 2 个池：

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工作负荷和查询的资源池
- 外部进程的外部资源池。 有关详细信息，请参阅 [sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)。

## <a name="remarks"></a>Remarks

使用 `REQUEST_MEMORY_GRANT_PERCENT` 时，允许索引创建操作使用比最初授予的工作区内存更多的工作区内存以提高性能。 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中的资源调控器支持这种特殊的处理方法。 然而，最初授予及任何其他内存授予都受资源池和工作负荷组设置的限制。

### <a name="index-creation-on-a-partitioned-table"></a>对已分区表创建索引

对非对齐的已分区表创建索引所占用的内存与涉及的分区数成正比。 如果所需的内存总量超过 Resource Governor 工作负荷组设置规定的每个查询的限制 `REQUEST_MAX_MEMORY_GRANT_PERCENT`，则可能无法执行此索引创建。 由于 "default" 工作负荷组允许查询超过每个查询的限制，并在开始时使用所需的最低内存，因此，如果 "default" 资源池配置了足够多的内存总量以运行此类查询，用户或许能够在 "default" 工作负荷组中运行相同的索引创建    。

## <a name="permissions"></a>权限

需要 `CONTROL SERVER` 权限。

## <a name="example"></a>示例

- 创建名为 newReports 的工作负荷组

它使用资源调控器默认设置并位于资源调控器默认池中。 该示例指定了 `default` 池，但这并非必需的。

```sql
CREATE WORKLOAD GROUP newReports
    USING "default" ;
GO
```

## <a name="see-also"></a>另请参阅

- [ALTER 工作负荷组 (Transact-SQL)](../../t-sql/statements/alter-workload-group-transact-sql.md)
- [DROP 工作负荷组 (Transact-SQL)](../../t-sql/statements/drop-workload-group-transact-sql.md)
- [创建资源池 (Transact-SQL)](../../t-sql/statements/create-resource-pool-transact-sql.md)
- [ALTER RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-resource-pool-transact-sql.md)
- [DROP RESOURCE POOL (Transact-SQL)](../../t-sql/statements/drop-resource-pool-transact-sql.md)
- [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)
