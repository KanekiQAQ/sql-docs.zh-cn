---
title: 透明数据加密-并行数据仓库 |Microsoft Docs
description: 透明数据加密 (TDE) 并行数据仓库 (PDW) 执行实时 I/O 加密和解密的数据和事务日志文件的特殊的 PDW 日志文件。"
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 582c237819dab5f0a1e30e2bd4e27fe3cc9ae57f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67959986"
---
# <a name="transparent-data-encryption"></a>透明数据加密
您可以采取一些预防措施来帮助保护数据库的安全，如设计一个安全系统、加密机密资产以及在数据库服务器的周围构建防火墙。 但是，对于物理媒体 （如驱动器或备份磁带） 都被盗的情况下，恶意方可以只还原或附加数据库并浏览的数据。 一种解决方案是加密数据库中的敏感数据，并通过证书保护用于加密数据的密钥。 这可以防止任何没有密钥的人使用这些数据，但这种保护必须事先计划。  
  
*透明数据加密*(TDE) 执行实时 I/O 加密和解密的数据和事务日志文件和特殊 PDW 日志文件。 这种加密使用数据库加密密钥 (DEK)，该密钥存储在数据库引导记录中以供恢复时使用。 DEK 是使用 SQL Server PDW 的 master 数据库中存储的证书保护的对称密钥。 TDE 保护“处于休眠状态”的数据，即数据和日志文件。 它提供了遵从许多法律、法规和各个行业建立的准则的能力。 此功能使软件开发人员而无需更改现有应用程序使用 AES 和 3DES 加密算法来加密数据。  
  
> [!IMPORTANT]  
> TDE 不提供客户端和 PDW 之间传输的数据加密。 有关如何加密数据之间的客户端和 SQL Server PDW 的详细信息，请参阅[预配证书](provision-certificate.md)。  
>   
> TDE 不会加密数据移动时或在使用中。 SQL Server PDW 内的 PDW 组件之间的内部流量不会加密。 临时存储在内存缓冲区的数据没有加密。 若要缓解此风险，控制物理访问权限和连接到 SQL Server PDW。  
  
对数据库实施保护措施后，可以通过使用正确的证书还原此数据库。  
  
> [!NOTE]  
> 为 TDE 创建证书时，您应立即备份，以及相关联的私钥。 如果证书变为不可用，或者如果必须在另一台服务器上还原或附加数据库，则必须同时具有证书和私钥的备份，否则将无法打开该数据库。 即使不再对数据库启用 TDE，也应该保留加密证书。 即使数据库未加密，事务日志的某些部分仍可能保持受到保护，但在执行数据库的完整备份前，对于某些操作可能需要证书。 超过过期日期的证书仍可以用于通过 TDE 加密和解密数据。  
  
数据库文件的加密在页级别执行。 已加密数据库中的页在写入磁盘之前会进行加密，在读入内存时会进行解密。 TDE 不会增加已加密数据库的大小。  
  
下图显示了 TDE 加密密钥的层次结构：  
  
![显示的层次结构](media/tde-architecture.png "TDE_Architecture")  
  
## <a name="using-tde"></a>使用透明数据加密  
若要使用 TDE，请按以下步骤操作。 准备 SQL Server PDW 才能支持 TDE 时，只是一次，执行前三个步骤。  
  
1.  在 master 数据库中创建一个主密钥。  
  
2.  使用**sp_pdw_database_encryption**若要在 SQL Server PDW 上启用 TDE。 此操作以确保将来的临时数据的保护修改的临时数据库，如果有任何活动会话具有临时表时，尝试将失败。 **sp_pdw_database_encryption**开启 PDW 系统日志中的用户数据掩码。 (有关 PDW 系统日志中的用户数据屏蔽的详细信息，请参阅[sp_pdw_log_user_data_masking](../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)。)  
  
3.  使用[sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md)创建可以进行身份验证和写入存储证书的备份的共享的凭据。 如果预期的存储服务器已存在一个凭据，可以使用现有凭据。  
  
4.  在 master 数据库中创建受主密钥的证书。  
  
5.  备份到的存储共享证书。  
  
6.  在用户数据库中，创建数据库加密密钥并保护存储在 master 数据库中的证书。  
  
7.  使用`ALTER DATABASE`语句来加密使用 TDE 的数据库。  
  
下面的示例说明如何加密`AdventureWorksPDW2012`数据库使用一个名为证书`MyServerCert`、 SQL Server PDW 中创建。  
  
**第一个：启用 TDE 的 SQL Server PDW 上。** 此操作一次才有必要。  
  
```sql  
USE master;  
GO  
  
-- Create a database master key in the master database  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>';  
GO  
  
-- Enable encryption for PDW  
EXEC sp_pdw_database_encryption 1;  
GO  
  
-- Add a credential that can write to the share  
-- A credential created for a backup can be used if you wish  
EXEC sp_pdw_add_network_credentials 'SECURE_SERVER', '<domain>\<Windows_user>', '<password>';  
```  
  
**第二个：创建并备份 master 数据库中的证书。** 此操作才必须使用一次。 可以有一个单独的证书 （推荐），每个数据库也可以保护多个数据库使用一个证书。  
  
```sql  
-- Create certificate in master  
CREATE CERTIFICATE MyServerCert WITH SUBJECT = 'My DEK Certificate';  
GO  
  
-- Back up the certificate with private key  
BACKUP CERTIFICATE MyServerCert   
    TO FILE = '\\SECURE_SERVER\cert\MyServerCert.cer'  
    WITH PRIVATE KEY   
    (   
        FILE = '\\SECURE_SERVER\cert\MyServerCert.key',  
        ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>'   
    )   
GO  
```  
  
**最后一个：创建 DEK，并使用 ALTER DATABASE 来对用户数据库进行加密。** TDE 保护的每个数据库重复此操作。  
  
```sql  
USE AdventureWorksPDW2012;  
GO  
  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_128  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO  
  
ALTER DATABASE AdventureWorksPDW2012 SET ENCRYPTION ON;  
GO  
```  
  
加密和解密操作由 SQL Server 安排在后台线程中执行。 您可以查看这些操作在更高版本在本文中显示的列表中使用目录视图和动态管理视图的状态。  
  
> [!CAUTION]  
> 启用了 TDE 的数据库的备份文件也使用数据库加密密钥进行加密。 因此，当您还原这些备份时，用于保护数据库加密密钥的证书必须可用。 也就是说，除了备份数据库之外，您还要确保自己保留了服务器证书的备份以防数据丢失。 如果证书不再可用，将会导致数据丢失。  
  
## <a name="commands-and-functions"></a>命令和函数  
TDE 证书必须使用数据库主密钥加密才能被下列语句接受。  
  
下表提供了 TDE 命令和函数的链接和说明。  
  
|命令或函数|用途|  
|-----------------------|-----------|  
|[创建数据库加密密钥](../t-sql/statements/create-database-encryption-key-transact-sql.md)|创建一个用于加密数据库的密钥。|  
|[更改数据库加密密钥](../t-sql/statements/alter-database-encryption-key-transact-sql.md)|更改用于加密数据库的密钥。|  
|[删除数据库加密密钥](../t-sql/statements/drop-database-encryption-key-transact-sql.md)|删除用于加密数据库的密钥。|  
|[ALTER DATABASE](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw)|介绍用来启用 TDE 的 **ALTER DATABASE** 选项。|  
  
## <a name="catalog-views-and-dynamic-management-views"></a>目录视图和动态管理视图  
下表显示了 TDE 目录视图和动态管理视图。  
  
|目录视图或动态管理视图|用途|  
|-------------------------------------------|-----------|  
|[sys.databases](../relational-databases/system-catalog-views/sys-databases-transact-sql.md)|显示数据库信息的目录视图。|  
|[sys.certificates](../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)|显示数据库中的证书的目录视图。|  
|[sys.dm_pdw_nodes_database_encryption_keys](../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql.md)|为每个节点提供有关数据库和数据库加密状态中使用的加密密钥信息的动态管理视图。|  
  
## <a name="permissions"></a>权限  
如上表中所述，TDE 的每项功能和每个命令都有各自的权限要求。  
  
查看 TDE 所涉及的元数据需要`CONTROL SERVER`权限。  
  
## <a name="considerations"></a>注意事项  
当进行数据库加密操作的重新加密扫描时，将禁用对数据库的维护操作。  
  
您可以查找数据库加密使用的状态**sys.dm_pdw_nodes_database_encryption_keys**动态管理视图。 有关详细信息，请参阅*目录视图和动态管理视图*本文前面的部分。  
  
### <a name="restrictions"></a>限制  
在不允许执行下列操作`CREATE DATABASE ENCRYPTION KEY`， `ALTER DATABASE ENCRYPTION KEY`， `DROP DATABASE ENCRYPTION KEY`，或`ALTER DATABASE...SET ENCRYPTION`语句。  
  
-   删除数据库。  
  
-   使用`ALTER DATABASE`命令。  
  
-   启动数据库备份。  
  
-   正在启动数据库还原。  
  
下面的操作或条件将阻止`CREATE DATABASE ENCRYPTION KEY`， `ALTER DATABASE ENCRYPTION KEY`， `DROP DATABASE ENCRYPTION KEY`，或`ALTER DATABASE...SET ENCRYPTION`语句。  
  
-   `ALTER DATABASE`执行命令。  
  
-   正在进行任何数据备份。  
  
当创建数据库文件时，如果启用了 TDE，则即时文件初始化功能不可用。  
  
### <a name="areas-not-protected-by-tde"></a>不能通过 TDE 保护的区域  
TDE 不会保护外部表。  
  
TDE 不会保护诊断会话。 用户应注意不到具有敏感参数在诊断会话中使用的查询。 一旦不再需要应删除披露敏感信息的诊断会话。  
  
放置在 SQL Server PDW 内存中时，由 TDE 保护的数据进行解密。 在设备上会出现某些问题时，创建内存转储。 转储文件表示内容的内存在的问题出现时，可以包含未加密的格式中的敏感数据。 与他人共享之前，应查看内存转储的内容。  
  
Master 数据库不受 TDE。 Master 数据库不包含用户数据，但它包含信息，例如登录名。  
  
### <a name="transparent-data-encryption-and-transaction-logs"></a>透明数据加密与事务日志  
允许数据库使用 TDE 的效果的清零虚拟事务日志，以强制将下一步虚拟事务日志的剩余部分。 这可以保证在数据库设置为加密后事务日志中不会留有明文。 可以通过查看每个 PDW 节点上找到的日志文件加密状态`encryption_state`中的列`sys.dm_pdw_nodes_database_encryption_keys`视图，如本例所示：  
  
```sql  
WITH dek_encryption_state AS   
(  
    SELECT ISNULL(db_map.database_id, dek.database_id) AS database_id, encryption_state  
    FROM sys.dm_pdw_nodes_database_encryption_keys AS dek  
        INNER JOIN sys.pdw_nodes_pdw_physical_databases AS node_db_map  
           ON dek.database_id = node_db_map.database_id AND dek.pdw_node_id = node_db_map.pdw_node_id  
        LEFT JOIN sys.pdw_database_mappings AS db_map  
            ON node_db_map .physical_name = db_map.physical_name  
        INNER JOIN sys.dm_pdw_nodes AS nodes  
            ON nodes.pdw_node_id = dek.pdw_node_id  
    WHERE dek.encryptor_thumbprint <> 0x  
)  
SELECT TOP 1 encryption_state  
       FROM dek_encryption_state  
       WHERE dek_encryption_state.database_id = DB_ID('AdventureWorksPDW2012 ')  
       ORDER BY (CASE encryption_state WHEN 3 THEN -1 ELSE encryption_state END) DESC;  
```  
  
所有在数据库加密密钥更改前写入事务日志的数据都将使用之前的数据库加密密钥加密。  
  
### <a name="pdw-activity-logs"></a>PDW 活动日志  
SQL Server PDW 维护一组用于故障排除的日志。 （请注意，这不是事务日志、 SQL Server 错误日志或 Windows 事件日志。）这些 PDW 活动日志可以包含以明文形式，其中一些可以包含用户数据的完整语句。 典型的示例包括**插入**并**更新**语句。 用户数据屏蔽可以显式打开或关闭使用**sp_pdw_log_user_data_masking**。 启用对 SQL Server PDW 加密会自动打开 PDW 活动日志中的用户数据的屏蔽以便保护它们。 **sp_pdw_log_user_data_masking**还可用于屏蔽语句时不使用 TDE，但不是建议使用该因为极大地减少了 Microsoft 支持团队能够分析问题。  
  
### <a name="transparent-data-encryption-and-the-tempdb-system-database"></a>透明数据加密与 tempdb 系统数据库  
Tempdb 系统数据库进行加密时使用启用加密[sp_pdw_database_encryption](../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)。 这是必需的任何数据库才能使用 TDE。 这可能会产生性能影响未加密数据库的 SQL Server PDW 的同一实例上。  
  
## <a name="key-management"></a>密钥管理  
数据库加密密钥 (DEK) 受存储在 master 数据库中的证书。 这些证书是受保护的主数据库的数据库主密钥 (DMK)。 将在没有需要由服务主密钥 (SMK) 以用于 TDE 进行保护。  
  
系统可以访问的密钥，而无需人工干预，（例如提供密码）。 如果证书不可用，系统将输出一个错误，直到可访问正确的证书无法解密 DEK。  
  
在将数据库从一个设备移到另一个，证书用于保护其 DEK 必须先还原到目标服务器上。 然后可以像往常一样还原数据库。 详细信息，请参阅标准的 SQL Server 文档，在[将受 TDE 保护的数据库移动到另一个 SQL Server](https://technet.microsoft.com/library/ff773063.aspx)。  
  
只要使用它们的数据库备份也应保留用于加密 Dek 的证书。 证书备份必须包括证书私钥，因为不带私钥的证书不能用于数据库还原。 这些证书的专用密钥备份存储在单独的文件，必须为证书还原提供有密码保护。  
  
## <a name="restoring-the-master-database"></a>还原 master 数据库  
可以使用还原了 master 数据库**DWConfig**，作为灾难恢复的一部分。  
  
-   如果控制节点未发生更改，则如果从中获取主数据库的备份在相同且不会更改设备上还原了 master 数据库，将在没有和所有证书都将无需执行其他操作可读。  
  
-   如果在不同的设备上还原了 master 数据库或控制节点后主数据库的备份发生了更改，需要执行其他步骤将以重新生成 DMK。  
  
    1.  打开 DMK:  
  
        ```sql  
        OPEN MASTER KEY DECRYPTION BY PASSWORD = '<password>';  
        ```  
  
    2.  添加通过 SMK 加密：  
  
        ```sql  
        ALTER MASTER KEY   
            ADD ENCRYPTION BY SERVICE MASTER KEY;  
        ```  
  
    3.  重新启动设备。  
  
## <a name="upgrade-and-replacing-virtual-machines"></a>升级和替换为虚拟机  
如果设备在其执行升级或替换 VM 上存在 DMK，必须作为参数提供 DMK 密码。  
  
升级操作的示例。 替换为`**********`DMK 密码。  
  
`setup.exe /Action=ProvisionUpgrade ... DMKPassword='**********'`  
  
要替换为虚拟机的操作的示例。  
  
`setup.exe /Action=ReplaceVM ... DMKPassword='**********'`  
  
在升级期间，如果用户数据库进行加密，并且不提供在没有密码，则升级操作将失败。 在替换期间如果 DMK 存在时，未提供正确的密码操作将跳过 DMK 恢复步骤。 将替换 VM 操作结束时完成所有其他步骤，但该操作将报告结束时失败指示的其他步骤是必需。 在安装日志中 (位于 **\ProgramData\Microsoft\Microsoft SQL Server Parallel Data Warehouse\100\Logs\Setup\\< 时间戳 > \Detail-Setup** )，结尾附近将显示以下警告。  
  
`*** WARNING \*\*\* DMK is detected in master database, but could not be recovered automatically! The DMK password was either not provided or is incorrect!`
  
在 PDW 中手动执行这些语句，并在此之后重新启动设备，以便恢复 DMK:  
  
```sql
OPEN MASTER KEY DECRYPTION BY PASSWORD = '<DMK password>';  
ALTER MASTER KEY ADD ENCRYPTION BY SERVICE MASTER KEY;  
```
  
使用中的步骤**还原 master 数据库**段落恢复数据库，并重新启动设备。  
  
如果将在没有之前已存在，但未恢复操作后，查询数据库时将引发以下错误消息。  
  
```sql
Msg 110806;  
A distributed query failed: Database '<db_name>' cannot be opened due to inaccessible files or insufficient memory or disk space. See the SQL Server errorlog for details.
```  
  
## <a name="performance-impact"></a>性能影响  
数据，、 存储方式，以及 SQL Server PDW 上的工作负荷活动的类型的类型而异的 TDE 的性能影响。 当 TDE 保护的 I/O 读取然后对数据进行解密或加密和则写入数据 CPU 密集型活动和时其他 CPU 密集型活动发生在同一时间将具有更大的成果。 因为 TDE 加密`tempdb`，TDE 可能会影响未加密的数据库的性能。 若要获取准确估计的性能，应使用数据和查询活动测试整个系统。  
  
## <a name="related-content"></a>相关内容  
以下链接包含有关 SQL Server 管理加密的一般信息。 这些文章可帮助您了解 SQL Server 加密功能，但这些文章没有特定于 SQL Server PDW 的信息，他们将讨论 SQL Server PDW 中不存在的功能。  
  
-   [SQL Server 加密](../relational-databases/security/encryption/sql-server-encryption.md)  
  
-   [加密层次结构](../relational-databases/security/encryption/encryption-hierarchy.md)  
  
-   [SQL Server 和数据库加密密钥](../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  

  
## <a name="see-also"></a>请参阅  
[ALTER DATABASE](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw)  
[CREATE MASTER KEY](../t-sql/statements/create-master-key-transact-sql.md)  
[创建数据库加密密钥](../t-sql/statements/create-database-encryption-key-transact-sql.md)  
[BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)  
[sp_pdw_database_encryption](../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)  
[sp_pdw_database_encryption_regenerate_system_keys](../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)  
[sp_pdw_log_user_data_masking](../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
[sys.certificates](../relational-databases/system-catalog-views/sys-certificates-transact-sql.md)  
[sys.dm_pdw_nodes_database_encryption_keys](../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql.md)  
  
