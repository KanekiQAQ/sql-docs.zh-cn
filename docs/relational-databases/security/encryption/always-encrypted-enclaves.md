---
title: 具有安全 Enclave 的 Always Encrypted（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: e4ec4877b7433554ad1f2ef60fdb73ab485cbed7
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68043199"
---
# <a name="always-encrypted-with-secure-enclaves"></a>具有安全 Enclave 的 Always Encrypted
[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../../../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

 
具有安全 Enclave 的 Always Encrypted 为 [Always Encrypted](always-encrypted-database-engine.md) 功能提供其他功能。

SQL Server 2016 中引入的 Always Encrypted 可保护敏感数据的机密性免受 SQL Server 的恶意软件以及具有高度特权但未经授权  的用户的攻击。 具有高度特权但未经授权的用户包括 DBA、计算机管理员、云管理员，或可以合法访问服务器实例、硬件等但不应有权访问部分或全部实际数据的任何其他用户。 

到目前为止，Always Encrypted 通过在客户端加密数据并且从不允许数据或相应的加密密钥以纯文本形式显示在 SQL Server 引擎中来保护数据。 因此，数据库内的加密列上的功能受到严格限制。 SQL Server 可以对加密数据执行的唯一操作是相等比较（相等比较仅适用于确定性加密）。 数据库内不支持所有其他操作，包括加密操作（初始数据加密或密钥轮替）或富计算（例如，模式匹配）。 用户需要将数据移出数据库才能对客户端执行这些操作。

 具有安全 enclave 的 Always Encrypted 通过允许在服务器端对安全 enclave 内的纯文本数据进行计算来解决这些限制。 安全 enclave 是 SQL Server 进程内受保护的内存区域，并充当用于处理 SQL Server 引擎中的敏感数据的受信任执行环境。 安全 enclave 显示为托管计算机上的其余 SQL Server 和其他进程的黑盒。 无法从外部查看 enclave 内的任何数据或代码，即使采用调试程序也是如此。  


Always Encrypted 使用安全 enclave，如下图中所示：

![数据流](./media/always-encrypted-enclaves/ae-data-flow.png)



解析应用程序的查询时，SQL Server 引擎会确定查询是否包含对需要使用安全 enclave 的加密数据执行的任何操作。 对于需要访问安全 enclave 的查询：

- 客户端驱动程序向安全 enclave 发送操作所需的列加密密钥（通过安全通道）。 
- 然后，客户端驱动程序提交要执行的查询以及加密的查询参数。

在查询处理期间，不会在 SQL Server 引擎中以纯文本形式公开安全 enclave 之外的数据或列加密密钥。 SQL Server 引擎将加密列上的加密操作和计算委托给安全 enclave。 如果需要，安全 enclave 会解密查询参数和/或加密列中存储的数据，并执行所请求的操作。

## <a name="why-use-always-encrypted-with-secure-enclaves"></a>为何使用具有安全 enclave 的 Always Encrypted？

借助安全 enclave，Always Encrypted 可保护敏感数据的机密性，同时提供以下优势：

- **就地加密** – 针对敏感数据的加密操作（例如：初始数据加密或轮替列加密密钥）在安全 enclave 内执行，且不需要将数据移出数据库。 可以使用 ALTER TABLE Transact-SQL 语句执行就地加密，且不需要使用 SSMS 中的 Always Encrypted 向导或 Set-SqlColumnEncryption PowerShell cmdlet。

- **富计算（预览）** – 在安全 enclave 内支持针对加密列的操作，包括模式匹配（LIKE 谓词）和范围比较，这样会将 Always Encrypted 解锁到需要在数据库系统内执行此类计算的广泛应用和方案。

> [!IMPORTANT]
> 在 [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] 中，丰富计算的多个性能优化和错误处理增强正在待处理中，丰富计算当前默认处于禁用状态。 若要启用富计算，请参阅[启用富计算](configure-always-encrypted-enclaves.md#configure-a-secure-enclave)。

在 [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)][ 中，具有安全 enclave 的 Always Encrypted 使用](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/)基于虚拟化的安全 (VBS) 保护 Windows 中的内存 enclave（也称为虚拟安全模式或 VSM enclave）。

## <a name="secure-enclave-attestation"></a>安全 Enclave 证明

SQL Server 引擎内的安全 enclave 可以访问加密数据库列中存储的敏感数据以及相应的纯文本列加密密钥。 在向 SQL Server 提交涉及到 enclave 计算的查询之前，应用程序内的客户端驱动程序必须基于给定的技术（例如，VBS）验证安全 enclave 是否是正版 enclave，以及是否已签署 enclave 以在 enclave 内运行。 

验证 enclave 的过程称为“enclave 证明”  ，通常同时涉及应用程序内的客户端驱动程序和用于联系外部证明服务的 SQL Server。 证明过程的细节取决于 enclave 技术和证明服务。

SQL Server 支持 [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] 中的 VBS 安全 enclave 的证明过程是 Windows Defender System Guard 运行时证明，该证明将主机保护者服务 (HGS) 用作证明服务。 需要在你的环境中配置 HGS 并注册用于托管 HGS 中的 SQL Server 实例的计算机。 还必须使用 HGS 证明配置客户端应用程序或工具（例如，SQL Server Management Studio）。

## <a name="secure-enclave-providers"></a>安全 Enclave 提供程序

若要使用具有安全 enclave 的 Always Encrypted，应用程序必须使用支持该功能的客户端驱动程序。 在 [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] 中，应用程序必须使用 .NET Framework 4.7.2 和用于 SQL Server 的 .NET Framework 数据提供程序。 此外，.NET 应用程序还必须使用特定于 enclave 类型（例如，VBS）的安全 enclave 提供程序  和所使用的证明服务（例如，HGS）进行配置。 支持的 enclave 提供程序在 NuGet 包中单独装运，你需要将该包与你的应用程序集成。 Enclave 提供程序实施证明协议的客户端逻辑，用于使用给定类型的安全 enclave 建立安全通道。

## <a name="enclave-enabled-keys"></a>已启用 enclave 的密钥

具有安全 enclave 的 Always Encrypted 引入了已启用 enclave 的密钥的概念：

- **已启用 enclave 的列主密钥** – 具有数据库内的列主密钥元数据对象中指定的 ENCLAVE_COMPUTATIONS 属性的列主密钥。 列主密钥元数据对象还必须包含元数据属性的有效签名。
- **已启用 enclave 的列加密密钥** – 使用已启用 enclave 的列主密钥加密的列加密密钥。

当 SQL Server 引擎确定需要在安全 enclave 内执行查询中指定的操作时，SQL Server 引擎会请求客户端驱动程序共享使用安全 enclave 进行计算所需的列加密密钥。 客户端驱动程序只共享已启用 enclave（即使用已启用 enclave 的列主密钥进行加密）且已正确签名的列加密密钥。 否则，查询将失败。

## <a name="enclave-enabled-columns"></a>已启用 enclave 的列

已启用 enclave 的列是使用已启用 enclave 的列加密密钥加密的数据库列。 可用于已启用 enclave 的列的功能取决于列所使用的加密类型。

- 确定性加密  - 使用确定性加密的已启用 enclave 的列支持就地加密，但不支持安全 enclave 内的任何其他操作。 相等性比较受支持，但是通过比较enclave 外的已加密文本执行。  
- 随机加密  - 使用随机加密的已启用 enclave 的列支持就地加密以及安全 enclave 内的富计算。 支持的富计算是模式匹配和[比较运算符](../../../t-sql/language-elements/comparison-operators-transact-sql.md)，包括相等比较。

有关加密类型的详细信息，请参阅 [Always Encrypted 加密](always-encrypted-cryptography.md)。

下表总结了可用于加密列的功能，具体取决于列是否使用已启用 enclave 的列加密密钥以及加密类型。

| **运算**| **列未启用 enclave** |**列未启用 enclave**| **列已启用 enclave**  |**列已启用 enclave** |
|:---|:---|:---|:---|:---|
| | **随机加密**  | **确定性加密**     | **随机加密**      | **确定性加密**     |
| **就地加密** | 不支持  | 不支持   | 是否支持         | 是否支持    |
| **相等比较**   | 不支持 | 支持（在 enclave 之外） | 支持（在 enclave 内） | 支持（在 enclave 之外） |
| **超出相等的比较运算符** | 不支持  | 不支持   | 是否支持      | 不支持     |
| **LIKE**    | 不支持      | 不支持    | 是否支持     | 不支持    |

就地加密包括对 enclave 内的以下操作的支持：

- 初始加密现有列中存储的数据。
- 重新加密列中的现有数据，例如：
  
  - 轮替列加密密钥（使用新密钥重新加密列）。
  - 更改加密类型。  

- 解密加密列中存储的数据（将该列转换为纯文本列）。

加密操作中涉及的列加密密钥（或密钥）必须已启用 enclave 才能使用就地加密：

- 初始加密：所加密的列的列加密密钥必须已启用 enclave。
- 重新加密：当前列加密密钥和目标列加密密钥（如果不同于当前密钥）都必须已启用 enclave。
- 解密：列的当前列加密密钥必须已启用 enclave。

### <a name="indexes-on-enclave-enabled-columns-using-randomized-encryption"></a>使用随机加密且已启用 enclave 的列上的索引
可以在使用随机加密且已启用 enclave 的列上创建非聚集索引，从而加快丰富查询的运行速度。 为了确保使用随机加密进行加密的列上的索引不会泄露敏感数据，同时它可用于处理 enclave 内的查询，索引数据结构（B 树）中的键值根据纯文本值进行加密和排序。 当 SQL Server 引擎中的查询执行器使用加密列上的索引在 enclave 内执行计算时，它搜索索引来查找列中存储的特定值。 每个搜索都可能涉及多个比较。 查询执行器将所有比较都委托给 enclave，它解密列中存储的值以及要比较的加密索引键值，以纯文本形式执行比较，并向执行器返回比较结果。 有关 SQL Server 中的索引工作原理的一般信息（而不是 Always Encrypted 专用信息），请参阅[描述的聚集索引和非聚集索引](../../indexes/clustered-and-nonclustered-indexes-described.md)。

由于使用随机加密且已启用 enclave 的列上的索引会存储加密索引键值，而值是根据纯文本进行排序，因此 SQL Server 引擎必须将 enclave 用于任何涉及创建或更新索引的操作，包括：
- 创建或重新生成索引。
- 在包含索引列/加密列的表中插入、更新或删除行，这些操作触发向索引插入索引键和/或从索引中删除索引键。
- 运行涉及检查索引完整性的 DBCC 命令（例如，[DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) 或 [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md)）。
- 数据库恢复（例如，在 SQL Server 出现故障并重启后），前提是 SQL Server 需要撤消对索引的任何更改（如需了解更多详情，请参阅下文）。

以上所有操作都要求，enclave 必须包含索引列的列加密密钥，这样才能解密索引键。 通常情况下，enclave 可以通过下面两种方式之一获取列加密密钥：

- **直接从对索引调用了操作的客户端应用程序获取**，如上面的简介中所述。 这种方法要求，应用程序或用户必须有权访问保护索引列的列主密钥和列加密密钥。 应用程序必须连接到数据库，并为连接启用 Always Encrypted。
- **从列加密密钥缓存获取。** enclave 将用于旧查询的密钥存储在内部缓存（位于 enclave 内，因此无法从外部访问）中。 如果应用程序对索引触发操作，但未直接提供必需的列加密密钥，enclave 便会在缓存中查找密钥。 如果 enclave 在缓存中找到密钥，就会用它完成操作。 使用这种方法，DBA 无需有权访问加密密钥或纯文本数据，即可管理索引，并执行特定数据清理操作（如从加密列上有索引的表中删除行）。 这种方法要求，应用程序必须连接到数据库，但未为连接启用 Always Encrypted。

仍不支持在使用随机加密但未启用 enclave 的列上创建索引。

#### <a name="database-recovery"></a>数据库恢复

如果 SQL Server 实例出现故障，它的数据库可能处于以下状态：数据文件可能包含来自未完成事务的一些修改。 启动后，此实例运行[数据库恢复](../../logs/the-transaction-log-sql-server.md#recovery-of-all-incomplete-transactions-when--is-started)过程，其中涉及回滚在事务日志中找到的所有未完成事务，以确保维护数据库完整性。 如果未完成事务对索引进行了任何更改，也需要撤消这些更改。 例如，可能需要删除或重新插入索引中的一些键值。

> [!IMPORTANT]
> Microsoft 强烈建议，先为数据库启用[加速数据库恢复 (ADR)](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr)，再  在使用随机加密进行加密且已启用 enclave 的列上创建首个索引。

如果使用遵循 [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) 恢复模式的[传统数据库恢复过程](https://docs.microsoft.com/azure/sql-database/sql-database-accelerated-database-recovery#the-current-database-recovery-process)，SQL Server 必须等到应用程序向 enclave 提供列的列加密密钥，才能撤消对索引的更改，而这可能需要很长时间才能完成。 ADR 大大减少了因无法从 enclave 内的缓存获取列加密密钥而必须延迟的撤消操作数。 因此，它通过最大限度地降低新事务受阻的可能性，极大地提高了数据库可用性。 启用 ADR 后，SQL Server 可能仍需要使用列加密密钥来完成旧数据版本清理，但是作为不影响数据库可用性或用户事务的后台任务来完成。 不过，错误日志中可能会显示错误消息，指明清理操作因缺少列加密密钥而失败。

### <a name="indexes-on-enclave-enabled-columns-using-deterministic-encryption"></a>使用确定性加密且已启用 enclave 的列上的索引

使用确定性加密的列上的索引是根据已加密文本（而不是纯文本）进行排序，无论列是否已启用 enclave。

## <a name="security-considerations"></a>需要考虑的安全性因素

下面的安全注意事项适用于含安全 enclave 的 Always Encrypted。

- enclave 内数据的安全性取决于证明协议和证明服务。 因此，必须确保证明服务及其强制执行的证明策略是由受信任的管理员管理。 此外，证明服务通常还支持各种策略和证明协议，其中一些对 enclave 及其环境执行最小验证，旨在用于测试和开发目的。 请严格遵循证明服务专用指南，以确保对生产部署使用建议的配置和策略。 
- 如果结合使用随机加密和已启用 enclave 的 CEK 来加密列，可能会导致列中存储的数据顺序泄露，因为此类列支持范围比较。 例如，如果包含员工薪金的加密列有索引，恶意 DBA 可以通过扫描此索引来查找最高加密薪金值，并确定薪金最高的员工（假设员工姓名未加密）。 
- 如果使用 Always Encrypted 来防止 DBA 未经授权地访问敏感数据，请不要将列主密钥或列加密密钥与 DBA 共享。 借助 enclave 内的列加密密钥缓存，DBA 无需拥有对密钥的直接访问权限，即可管理加密列上的索引。

## <a name="considerations-for-alwayson-and-database-migration"></a>AlwaysOn 和数据库迁移的注意事项

配置支持使用 enclave 的查询所需的 AlwaysOn 可用性组时，必须确保在可用性组中托管数据库的所有 SQL Server 实例都支持含安全 enclave 的 Always Encrypted，并且已配置 enclave。 如果主数据库支持 enclave，但次要副本不支持，任何尝试使用含安全 enclave 的 Always Encrypted 功能的查询都会失败。

如果数据库使用含安全 enclave 的 Always Encrypted 功能，同时你在未配置 enclave 的 SQL Server 实例上还原它的备份文件，还原操作会成功，且不依赖 enclave 的所有功能都可用。 不过，所有使用 enclave 功能的后续查询都会失败，使用随机加密且已启用 enclave 的列上的索引也会失效。  在未配置 enclave 的实例上附加使用含安全 enclave 的 Always Encrypted 的数据库时，亦是如此。

如果数据库包含使用随机加密且已启用 enclave 的列上的索引，请务必先在数据库中启用[加速数据库恢复 (ADR)](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr)，再创建数据库备份。 ADR 可确保数据库（包括索引）在你还原数据库后立即可用。 有关详细信息，请参阅[数据库恢复](#database-recovery)。

使用 bacpac 文件迁移数据库时，必须确保先删除使用随机加密且已启用 enclave 的列上的所有索引，再创建 bacpac 文件。

## <a name="known-limitations"></a>已知的限制

安全 enclave 可增强 Always Encrypted 功能。 已启用 enclave 的列现在支持以下功能  ：

- 就地加密操作。
- 使用随机加密进行加密的列上的模式匹配 (LIKE) 和比较运算符。
    > [!NOTE]
    > 使用包含 BIN2 排序顺序的排序规则（即 BIN2 排序规则）的字符串列支持上面的操作。 对于使用非 BIN2 排序规则的字符串列，可以使用随机加密和已启用 enclave 的列加密密钥来加密列。 但是，可用于此类列的唯一新功能是就地加密。
- 在使用随机加密的列上创建非聚集索引。
- 计算列使用表达式，这些表达式包含使用随机加密的列上的 LIKE 谓词和比较运算符。

[功能详细信息](always-encrypted-database-engine.md#feature-details)列出的适用于不含安全 enclave 的 Always Encrypted 的其他所有限制（上述增强功能未予以解决）也适用于含安全 enclave 的 Always Encrypted。

下面介绍了含安全 enclave 的 Always Encrypted 的专属限制：

- 无法在使用随机加密且已启用 enclave 的列上创建聚集索引。
- 使用随机加密且已启用 enclave 的列无法作为主键列，并且无法被外键约束或唯一键约束引用。
- 不支持在使用随机加密且已启用 enclave 的列上执行哈希联接和合并联接。 仅支持执行嵌套循环联接（使用索引（若有））。
- 如果查询中的 LIKE 运算符或比较运算符包含使用以下数据类型（加密后成为大型对象）之一的查询参数，查询会忽略索引，并执行表扫描。
    - nchar[n] 和 nvarchar[n]（如果 n 大于 3967 的话）。
    - char[n]、varchar[n]、binary[n]、varbinary[n]（如果 n 大于 7935 的话）。
- 就地加密操作无法与列元数据的其他任何更改合并，更改同一代码页内的排序规则以及更改为 Null 性除外。 例如，在一个 ALTER TABLE 或 ALTER COLUMN Transact-SQL 语句中，既无法加密、重新加密或解密列，也无法更改列的数据类型。 请单独使用两个语句。
- 不支持将已启用 enclave 的密钥用于内存中表内的列。
- 用于存储已启用 enclave 的列主密钥的唯一受支持密钥存储是 Windows 证书存储和 Azure 密钥保管库。

以下限制虽适用于 [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)]，但正在计划予以解决：

- 不支持为使用随机加密且已启用 enclave 的列创建统计信息。
- 支持具有安全 enclave 的 Always Encrypted 的唯一客户端驱动程序是 .NET Framework 4.7.2 中用于 SQL Server 的 .NET Framework 数据提供程序 (ADO.NET)。 不支持 ODBC/JDBC。
- 具有安全 enclave 的 Always Encrypted 的工具支持当前不完整。 具体而言：
  - 不支持导入/导出包含已启用 enclave 的密钥的数据库。
  - 若要通过 ALTER TABLE Transact-SQL 语句触发就地加密操作，需要使用 SSMS 中的查询窗口发出该语句，或者可以自行编写可发出该语句的程序。 SqlServer PowerShell 模块中的 Set-SqlColumnEncryption cmdlet 和 SQL Server Management Studio 中的 Always Encrypted 向导尚不支持就地加密 - 这两种工具当前将数据移出数据库以执行加密操作，即使用于这些操作的列加密密钥已启用 enclave 也是如此。
- 不支持检查索引完整性或更新索引的 DBCC 命令。
- 在通过 CREATE TABLE 创建表时，在加密列上创建索引。 需要通过 CREATE INDEX 单独在加密列上创建索引。

## <a name="next-steps"></a>后续步骤

- 在 SSMS 中设置测试环境并尝试使用具有安全 enclave 的 Always Encrypted 功能，请参阅[教程：通过 SSMS 开始使用具有安全 enclave 的 Always Encrypted](../tutorial-getting-started-with-always-encrypted-enclaves.md)。
- 若要详细了解如何使用含安全 enclave 的 Always Encrypted，请参阅[配置含安全 enclave 的 Always Encrypted](configure-always-encrypted-enclaves.md)。
