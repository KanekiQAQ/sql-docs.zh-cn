---
title: 教程：在使用随机加密且已启用 enclave 的列上创建并使用索引 | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: 9865be1d006e10271295ae4dda731eb33331dbda
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68126800"
---
# <a name="tutorial-creating-and-using-indexes-on-enclave-enabled-columns-using-randomized-encryption"></a>教程：在使用随机加密且已启用 enclave 的列上创建并使用索引
[!INCLUDE [tsql-appliesto-ssver15-xxxx-xxxx-xxx](../../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

本教程将介绍如何在使用[含安全 enclave 的 Always Encrypted](encryption/always-encrypted-enclaves.md) 中支持的随机加密且已启用 enclave 的列上创建和使用索引。 它将介绍：

- 如何在有权访问保护列的密钥（列主密钥和列加密密钥）时创建索引。
- 如何在无权访问保护列的密钥时创建索引。

## <a name="prerequisites"></a>必备条件

本教程是[教程：通过 SSMS 开始使用含安全 enclave 的 Always Encrypted](./tutorial-getting-started-with-always-encrypted-enclaves.md) 的延续。 请务必先完成上一教程，再执行以下步骤。

## <a name="step-1-enable-accelerated-database-recovery-adr-in-your-database"></a>第 1 步：在数据库中启用加速数据库恢复 (ADR)

Microsoft 强烈建议，先在数据库中启用 ADR，再在使用随机加密且已启用 enclave 的列上创建首个索引。 请参阅[含安全 enclave 的 Always Encrypted](./encryption/always-encrypted-enclaves.md) 中的[数据库恢复](./encryption/always-encrypted-enclaves.md##database-recovery)部分。

1. 关闭在上一教程中使用的任何 SSMS 实例。 这会关闭已打开的数据库连接（启用 ADR 的必需操作）。
1. 以 sysadmin 身份打开新 SSMS 实例，并连接到 SQL Server 实例，无需  为数据库连接启用 Always Encrypted。
    1. 启动 SSMS。
    1. 在“连接到服务器”  对话框中，指定服务器名称，选择身份验证方法，并指定凭据。
    1. 单击“选项 >>”  ，并选择“Always Encrypted”  选项卡。
    1. 务必取消  选中“启用 Always Encrypted (列加密)”  复选框。
    1. 选择“连接”  。
1. 打开新查询窗口，并执行以下语句来启用 ADR。

   ```sql
   ALTER DATABASE ContosoHR SET ACCELERATED_DATABASE_RECOVERY = ON;
   ```

## <a name="step-2-create-and-test-an-index-without-role-separation"></a>第 2 步：在不使用角色分隔的情况下创建和测试索引

这一步将在加密列上创建和测试索引。 你将是一个同时具有以下两个角色的用户：管理数据库的 DBA 和有权访问保护数据的密钥的数据所有者。

1. 打开新 SSMS 实例，并连接到已  为数据库连接启用 Always Encrypted 的 SQL Server 实例。
   1. 启动新 SSMS 实例。
   1. 在“连接到服务器”  对话框中，指定服务器名称，选择身份验证方法，并指定凭据。
   1. 单击“选项 >>”  ，并选择“Always Encrypted”  选项卡。
   1. 选中“启用 Always Encrypted (列加密)”  复选框，并指定 enclave 证明 URL（例如，ht<span>tp://<   span>hgs.bastion.local/Attestation）。
   1. 选择“连接”  。
   1. 如果系统提示启用 Always Encrypted 查询参数化，请单击“启用”  。
1. 如果系统没有提示启用 Always Encrypted 参数化，请验证它是否已启用。
   1. 在 SSMS 的主菜单中，选择“工具”  。
   2. 选择“选项...”  。
   3. 导航到“查询执行”   > “SQL Server”   > “高级”  。
   4. 务必选中“启用 Always Encrypted 参数化”  。
   5. 选择“确定”  。
1. 打开查询窗口，并执行下面的语句，以加密“Employees”  表中的“LastName”  列。 后面的步骤将介绍如何在此列上创建并使用索引。

   ```sql
   USE [ContosoHR];
   GO   
   ALTER TABLE [dbo].[Employees]
   ALTER COLUMN [LastName] [nvarchar](50) COLLATE Latin1_General_BIN2 
   ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
   GO   
   ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
   GO
   ```

1. 在“LastName”  列上创建索引。 由于你是在已启用 Always Encrypted 的情况下连接到数据库，因此 SSMS 内部的客户端驱动程序以透明方式将 CEK1  （保护“LastName”  列的列加密密钥）提供给创建索引所需的 enclave。

   ```sql
   USE [ContosoHR];
   GO

   CREATE INDEX IX_LastName ON [Employees] ([LastName])
   INCLUDE ([EmployeeID], [FirstName], [SSN], [Salary]);
   GO
   ```

1. 对“LastName”  列运行丰富查询，并验证 SQL Server 是否在执行查询时使用索引。
   1. 在同一或新查询窗口中，确保工具栏上的“包括实时查询统计信息”  按钮处于启用状态。
   1. 执行下面的查询。

       ```sql
       USE [ContosoHR];
       GO

       DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
       SELECT * FROM [dbo].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
       GO
      ```

   1. 在查询窗口底部的“实时查询统计信息”  选项卡中，观察查询是否使用索引。

## <a name="step-3-create-an-index-with-role-separation"></a>步骤 3：在使用角色分隔的情况下创建索引

在这一步中，你将假装为两个不同的用户，在加密列上创建索引。 一个用户是需要创建索引但无权访问密钥的 DBA。 另一个用户是有权访问密钥的数据所有者。

1. 使用未  启用 Always Encrypted 的 SSMS 实例来执行下面的语句，以删除“LastName”  列上的索引。

   ```sql
   USE [ContosoHR];
   GO

   DROP INDEX IX_LastName ON [Employees]; 
   GO
   ```

1. 以数据所有者（或有权访问密钥的应用程序）身份，使用 CEK1  填充 enclave 内的缓存。

   > [!NOTE]
   > 除非你在**第 2 步：在不使用角色分隔的情况下创建和测试索引**后重启了 SQL Server 实例，否则此为冗余步骤，因为缓存中已有 CEK1  。 之所以添加它是为了展示数据所有者如何向 enclave 提供密钥（如果 enclave 中尚无的话）。

   1. 在已  启用 Always Encrypted 的 SSMS 实例中，在查询窗口中执行下面的语句。 此语句将所有已启用 enclave 的列加密密钥都发送到 enclave。 有关详细信息，请参阅 [sp_enclave_send_keys](../system-stored-procedures/sp-enclave-send-keys-sql.md)。

        ```sql
        USE [ContosoHR];
        GO

        EXEC sp_enclave_send_keys;
        GO
        ```

   1. 作为上述存储过程的替代方法，可以运行对“LastName”  列使用 enclave 的 DML 查询。 这只会使用 CEK1  填充 enclave。

        ```sql
        USE [ContosoHR];
        GO

        DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
        SELECT * FROM [dbo].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
        GO
        ```

1. 以 DBA 身份创建索引。
    1. 在未  启用 Always Encrypted 的 SSMS 实例中，在查询窗口中执行下面的语句。

        ```sql
        USE [ContosoHR];
        GO

        CREATE INDEX IX_LastName ON [Employees] ([LastName])
        INCLUDE ([EmployeeID], [FirstName], [SSN], [Salary]);
        GO
        ```

1. 以数据所有者身份对“LastName”  列运行丰富查询，并验证 SQL Server 是否在执行查询时使用索引。
   1. 在已  启用 Always Encrypted 的 SSMS 实例中，选择现有查询窗口或打开新查询窗口，并确保工具栏上的“包括实时查询统计信息”  按钮处于启用状态。
   1. 执行下面的查询。 

        ```sql
        USE [ContosoHR];
        GO

        DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
        SELECT * FROM [dbo].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
        GO
        ```

   1. 在查询窗口底部的“实时查询统计信息”  中，观察查询是否使用索引。

## <a name="next-steps"></a>后续步骤

- 若要了解含安全 enclave 的 Always Encrypted 的其他用例，请参阅[配置含安全 enclave 的 Always Encrypted](encryption/configure-always-encrypted-enclaves.md)。
