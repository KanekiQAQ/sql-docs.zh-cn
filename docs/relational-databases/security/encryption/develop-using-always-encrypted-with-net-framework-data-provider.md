---
title: 配合使用 Always Encrypted 和 .NET Framework 数据提供程序进行开发 | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: security, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 827e509e-3c4f-4820-aa37-cebf0f7bbf80
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4771d3a0fbf49c2d8c87c96d99b3e387b392e3bb
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68049942"
---
# <a name="develop-using-always-encrypted-with-net-framework-data-provider"></a>配合使用 Always Encrypted 和 .NET Framework 数据提供程序进行开发
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

本文介绍如何使用 [始终加密](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 和 [用于 SQL Server 的 .NET Framework 数据提供程序](https://msdn.microsoft.com/library/kb9s9ks0(v=vs.110).aspx)来开发 .NET 应用程序。

始终加密允许客户端应用程序对敏感数据进行加密，并且永远不向 SQL Server 或 Azure SQL 数据库显示该数据或加密密钥。 启用了始终加密的驱动程序（例如用于 SQL Server 的 .NET Framework 数据提供程序）通过在客户端应用程序中以透明方式对敏感数据进行加密和解密来实现此目标。 该驱动程序自动确定哪些查询参数与敏感数据库列（使用始终加密进行保护）相对应，并对这些参数的值进行加密，然后再将数据传递到 SQL Server 或 Azure SQL 数据库。 同样，该驱动程序以透明方式对查询结果中从加密数据库列检索到的数据进行解密。 有关详细信息，请参阅 [始终加密（数据库引擎）](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)。


## <a name="prerequisites"></a>必备条件

- 在数据库中配置始终加密。 这涉及为选定数据库列预配始终加密密钥和设置加密。 如果还没有配置了始终加密的数据库，请按照 [始终加密入门](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_5)中的说明操作。
- 确保在开发计算机上安装 .NET Framework 4.6 版或更高版本。 有关详细信息，请参阅 [.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2(v=vs.110).aspx)。 还需确保将 .NET Framework 4.6 版或更高版本配置为开发环境中的目标 .NET Framework 版本。 如果使用的是 Visual Studio，请参阅[如何：面向 .NET Framework 的某个版本](https://msdn.microsoft.com/library/bb398202.aspx)。 

> [!NOTE]
> 特定 .NET Framework 版本对始终加密的支持级别有所不同。 有关详细信息，请参阅下面的“始终加密 API 参考”一节。 

## <a name="enabling-always-encrypted-for-application-queries"></a>为应用程序查询启用始终加密
若要对参数启用加密，以及对面向加密列的查询结果启用解密，最简单的方法是将“列加密设置”连接字符串关键字的值设置为“已启用”  。

以下是启用始终加密的连接字符串示例：
```
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
SqlConnection connection = new SqlConnection(connectionString);
```

以下是使用 SqlConnectionStringBuilder.ColumnEncryptionSetting 属性的等效示例。

```
SqlConnectionStringBuilder strbldr = new SqlConnectionStringBuilder();
strbldr.DataSource = "server63";
strbldr.InitialCatalog = "Clinic";
strbldr.IntegratedSecurity = true;
strbldr.ColumnEncryptionSetting = SqlConnectionColumnEncryptionSetting.Enabled;
SqlConnection connection = new SqlConnection(strbldr.ConnectionString);
```

还可以为单个查询启用始终加密。 请参阅下面的 **控制始终加密对性能的影响** 一节。
请注意，启用始终加密不足以成功实现加密或解密。 你还需要确保：
- 应用程序具有 *查看任意列主密钥定义* 和 *查看任意列加密密钥定义* 数据库权限，这是访问数据库中始终加密密钥的相关元数据所必需的权限。 有关详细信息，请参阅 [始终加密（数据库引擎）中的“权限”一节](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_7)。
- 应用程序可以访问用于保护列加密密钥的列主密钥，以便对查询到的数据库列加密。

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>检索和修改加密列中的数据

一旦为应用程序查询启用始终加密，就可以使用标准 ADO.NET API（请参阅 [在 ADO.NET 中检索和修改数据](https://msdn.microsoft.com/library/ms254937(v=vs.110).aspx)）或者 [System.Data.SqlClient Namespace](https://msdn.microsoft.com/library/kb9s9ks0(v=vs.110).aspx) 中定义的 [用于 SQL Server 的 .NET Framework 数据提供程序](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)API，来检索或修改加密数据库列中的数据。 假设应用程序具有所需的数据库权限，并且可以访问列主密钥，那么，用于 SQL Server 的 .NET Framework 数据提供程序将加密任何面向加密列的查询参数，并解密从返回 .NET 类型（对应于为数据库架构中的列设置的 SQL Server 数据类型）纯文本值的加密列中检索到的数据。
如果未启用始终加密，具有面向加密列的参数的查询将失败。 只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。 但是，用于 SQL Server 的 .NET Framework 数据提供程序不会尝试解密从加密列中检索到的任何值，并且应用程序将收到二进制加密数据（字节数组形式）。

下表概述了查询的行为，具体取决于是否启用了始终加密：

|查询特征 | 启用了始终加密，并且应用程序可以访问密钥和密钥元数据|启用了始终加密，但应用程序无法访问密钥或密钥元数据 | 禁用了始终加密|
|:---|:---|:---|:---|
| 具有面向加密列的参数的查询。 | 以透明方式加密参数值。 | 错误 | 错误|
| 从加密列中检索数据且没有面向加密列的参数的查询。| 以透明方式解密来自加密列的结果。 应用程序收到 .NET 数据类型（对应于为加密列配置的 SQL Server 类型）的纯文本值。 | 错误 | 不解密来自加密列的结果。 应用程序收到字节数组形式的加密值 (byte[])。 

以下示例说明如何检索和修改加密列中的数据。 这些示例假定目标表具有以下架构。 请注意，SSN 和 BirthDate 列已加密。


```
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1), 
 [SSN] [char](11) COLLATE Latin1_General_BIN2 
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC, 
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256', 
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL, 
 [BirthDate] [date] 
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED, 
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256', 
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="inserting-data-example"></a>插入数据示例

此示例向 Patients 表插入一行。 请注意以下事项：
- 对于示例代码中的加密，没有什么特定的注意事项。 用于 SQL Server 的 .NET Framework 数据提供程序会自动检测并加密面向加密列的 *paramSSN* 和 *paramBirthdate* 参数。 这使得加密操作对应用程序而言是透明的。 
- 插入到数据库列（包括加密列）中的值将作为 [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) 对象传递。 在将值发送到非加密列时， **SqlParameter** 是可选的（虽然强烈建议使用它，因为它有助于防止 SQL 注入），而在发送面向加密列的值时，它是必需的。 如果插入到 SSN 或 BirthDate 列中的值作为查询语句中嵌入的文本传递，查询将失败，因为用于 SQL Server 的 .NET Framework 数据提供程序无法确定目标加密列中的值，所以它不会对这些值加密。 因此，服务器会因为与加密列不兼容而拒绝它们。
- 面向 SSN 列的参数的数据类型将设置为映射到 char/varchar SQL Server 数据类型的 ANSI（非 Unicode）字符串。 如果该参数的类型设置为映射到 nchar/nvarchar 的 Unicode 字符串（字符串），查询将失败，因为始终加密不支持从加密的 nchar/nvarchar 值转换为加密的 char/varchar 值。 有关数据类型映射的信息，请参阅 [SQL Server 数据类型映射](/dotnet/framework/data/adonet/sql-server-data-type-mappings) 。
- 插入到 BirthDate 列中的参数的数据类型将使用 [SqlParameter.SqlDbType 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.sqldbtype.aspx)显式设置为目标 SQL Server 数据类型，而不依赖于使用 [SqlParameter.DbType 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.dbtype.aspx)时应用的从 .NET 类型到 SQL Server 数据类型的隐式映射。 默认情况下， [DateTime 结构](https://msdn.microsoft.com/library/system.datetime.aspx) 将映射到 datetime SQL Server 数据类型。 由于 BirthDate 列的数据类型是日期，而始终加密不支持将加密的日期时间值转换为加密的日期值，因此，使用默认映射将导致错误。 

```
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
using (SqlConnection connection = new SqlConnection(strbldr.ConnectionString))
{
   using (SqlCommand cmd = connection.CreateCommand())
   {
      cmd.CommandText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

      SqlParameter paramSSN = cmd.CreateParameter();
      paramSSN.ParameterName = @"@SSN";
      paramSSN.DbType = DbType.AnsiStringFixedLength;
      paramSSN.Direction = ParameterDirection.Input;
      paramSSN.Value = "795-73-9838";
      paramSSN.Size = 11;
      cmd.Parameters.Add(paramSSN);

      SqlParameter paramFirstName = cmd.CreateParameter();
      paramFirstName.ParameterName = @"@FirstName";
      paramFirstName.DbType = DbType.String;
      paramFirstName.Direction = ParameterDirection.Input;
      paramFirstName.Value = "Catherine";
      paramFirstName.Size = 50;
      cmd.Parameters.Add(paramFirstName);

      SqlParameter paramLastName = cmd.CreateParameter();
      paramLastName.ParameterName = @"@LastName";
      paramLastName.DbType = DbType.String;
      paramLastName.Direction = ParameterDirection.Input;
      paramLastName.Value = "Abel";
      paramLastName.Size = 50;
      cmd.Parameters.Add(paramLastName);

      SqlParameter paramBirthdate = cmd.CreateParameter();
      paramBirthdate.ParameterName = @"@BirthDate";
      paramBirthdate.SqlDbType = SqlDbType.Date;
      paramBirthdate.Direction = ParameterDirection.Input;
      paramBirthdate.Value = new DateTime(1996, 09, 10);
      cmd.Parameters.Add(paramBirthdate);

      cmd.ExecuteNonQuery();
   } 
}
```

### <a name="retrieving-plaintext-data-example"></a>检索纯文本数据示例

以下示例演示如何根据加密值筛选数据，以及从加密列中检索纯文本数据。 请注意以下事项：
- WHERE 子句中用于筛选 SSN 列的值需要使用 SqlParameter 进行传递，以便用于 SQL Server 的 .NET Framework 数据提供程序可以在将其发送到数据库之前以透明方式对其加密。
- 程序打印的所有值均为纯文本形式，因为用于 SQL Server 的 .NET Framework 数据提供程序将以透明方式解密从 SSN 和 BirthDate 列中检索到的数据。


> [!NOTE]
> 查询可以在列上执行相等比较（如果使用确定性加密对它们进行加密。 有关详细信息，请参阅 *始终加密（数据库引擎）* 的 [选择确定性或随机的加密](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)一节。

```
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
    
using (SqlConnection connection = new SqlConnection(strbldr.ConnectionString))
 {
    using (SqlCommand cmd = connection.CreateCommand())
 {

 cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN=@SSN";
 SqlParameter paramSSN = cmd.CreateParameter();
 paramSSN.ParameterName = @"@SSN";
 paramSSN.DbType = DbType.AnsiStringFixedLength;
 paramSSN.Direction = ParameterDirection.Input;
 paramSSN.Value = "795-73-9838";
 paramSSN.Size = 11;
 cmd.Parameters.Add(paramSSN);
 using (SqlDataReader reader = cmd.ExecuteReader())
 {
   if (reader.HasRows)
 {
 while (reader.Read())
 {
    Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
 }
```

### <a name="retrieving-encrypted-data-example"></a>检索加密数据示例

如果未启用始终加密，只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。

以下示例演示如何从加密列中检索二进制加密数据。 请注意以下事项：

- 由于未在连接字符串中启用始终加密，因此，查询将以字节数组的形式返回 SSN 和 BirthDate 的加密值（程序会将值转换为字符串）。
- 如果禁用始终加密，从加密列中检索数据的查询可以有参数，但前提是所有参数均不面向加密列。 上述查询按未在数据库中加密的 LastName 进行筛选。 如果查询按 SSN 或 BirthDate 进行筛选，则将失败。


```
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
                
using (SqlConnection connection = new SqlConnection(connectionString))
{
   connection.Open();
   using (SqlCommand cmd = connection.CreateCommand())
   {
      cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName";
      SqlParameter paramLastName = cmd.CreateParameter();
      paramLastName.ParameterName = @"@LastName";
      paramLastName.DbType = DbType.String;
      paramLastName.Direction = ParameterDirection.Input;
      paramLastName.Value = "Abel";
      paramLastName.Size = 50;
      cmd.Parameters.Add(paramLastName);
      using (SqlDataReader reader = cmd.ExecuteReader())
      {
         if (reader.HasRows)
         {
            while (reader.Read())
         {
         Console.WriteLine(@"{0}, {1}, {2}, {3}", BitConverter.ToString((byte[])reader[0]), reader[1], reader[2], BitConverter.ToString((byte[])reader[3]));
      }
   }
}
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>避免查询加密列时的常见问题

本节介绍从 .NET 应用程序查询加密列时的常见错误类别，以及有关如何避免这些错误的若干指导。

### <a name="unsupported-data-type-conversion-errors"></a>不支持的数据类型转换错误

始终加密支持对加密数据类型进行若干种转换。 有关受支持类型转换的详细列表，请参阅 [始终加密（数据库引擎）](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) 。 执行以下操作可避免数据类型转换错误：

- 设置面向加密列的参数的类型，以便参数的 SQL Server 数据类型与目标列的类型完全相同，或者支持将参数的 SQL Server 数据类型转换为列的目标类型。 可以使用 SqlParameter.SqlDbType 属性，强行根据需要将 .NET 数据类型映射到特定的 SQL Server 数据类型。
- 验证对于面向列的 decimal 和 numeric SQL Server 数据类型的参数，其精度和小数位数是否与为目标列配置的精度和小数位数相同。  
- 验证面向列的 datetime2、datetimeoffset 或 time SQL Server 数据类型的参数的精度是否不大于目标列的精度（在用于修改目标列中的值的查询中）。  

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>由于传递纯文本而非加密值而发生的错误

面向加密列的任何值都需要在应用程序内加密。 尝试插入/修改或者按纯文本值筛选加密列将导致如下错误：


```
System.Data.SqlClient.SqlException (0x80131904): Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', column_encryption_key_database_name = 'Clinic') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

为防止发生此类错误，请确保：
- 为面向加密列的应用程序查询（为特定查询的连接字符串或在 [SqlCommand](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx) 对象中）启用始终加密）。
- 使用 SqlParameter 发送面向加密列的数据。 以下示例显示了一个查询，该查询按文本/常量对加密列 (SSN) 进行错误筛选，而不是传递 SqlParameter 对象内的文本。 


```
using (SqlCommand cmd = connection.CreateCommand())
{
   cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
cmd.ExecuteNonQuery();
}
```

## <a name="working-with-column-master-key-stores"></a>使用列主密钥存储

若要对参数值进行加密或对查询结果中的数据进行解密，用于 SQL Server 的 .NET Framework 数据提供程序需要获取为目标列配置的列加密密钥。 列加密密钥以加密形式存储在数据库元数据中。 每个列加密密钥都有一个用于加密列加密密钥的相应列主密钥。 数据库元数据不会存储列主密钥 - 它只包含含有特定列主密钥的密钥存储的相关信息，以及该密钥在密钥存储中的位置。

为了获取列加密密钥的纯文本值，用于 SQL Server 的 .NET Framework 数据提供程序首先会获取列加密密钥和其相应列主密钥的相关元数据，然后使用元数据中的信息连接包含列主密钥的密钥存储，并对加密列的加密密钥进行解密。 用于 SQL Server 的 .NET Framework 数据提供程序使用列主密钥存储提供程序（一个派生自 [SqlColumnEncryptionKeyStoreProvider 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcolumnencryptionkeystoreprovider.aspx)的类实例）与密钥存储通信。


用于获取列加密密钥的过程：

1.  如果为查询启用了始终加密，则用于 SQL Server 的 .NET Framework 数据提供程序会以透明方式调用 **sys.sp_describe_parameter_encryption** 来检索面向加密列的参数的加密元数据（如果查询具有参数）。 对于查询结果中包含的加密数据，SQL Server 会自动附加加密元数据。 有关列主密钥的信息包括：
    - 密钥存储提供程序的名称，该提供程序可封装包含列主密钥的密钥存储。 
    - 指定列主密钥在密钥存储中的位置的密钥路径。
    
    有关列加密密钥的信息包括：

    - 列加密密钥的加密值。
    - 用于加密列加密密钥的算法的名称。
2.  用于 SQL Server 的 .NET Framework 数据提供程序使用列主密钥存储提供程序的名称在内部数据结构中查找提供程序对象（一个派生自 SqlColumnEncryptionKeyStoreProvider 类的类实例）。
3.  为了解密列加密密钥，用于 SQL Server 的 .NET Framework 数据提供程序会调用 SqlColumnEncryptionKeyStoreProvider.DecryptColumnEncryptionKey 方法（传递列主密钥路径、列加密密钥的加密值以及用于生成加密列加密密钥的加密算法的名称）。




### <a name="using-built-in-column-master-key-store-providers"></a>使用内置列主密钥存储提供程序

用于 SQL Server 的 .NET Framework 数据提供程序附带以下内置列主密钥存储提供程序，这些提供程序已使用特定的提供程序名称（用于查找该提供程序）进行预注册。


| 类 | 描述 | 提供程序（查找）名称 |
|:---|:---|:---|
|SqlColumnEncryptionCertificateStoreProvider 类| 用于 Windows 证书存储的提供程序。 | MSSQL_CERTIFICATE_STORE |
|[SqlColumnEncryptionCngProvider 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcolumnencryptioncngprovider.aspx) <br><br>**注意：** 此提供程序在 .NET Framework 4.6.1 及更高版本中可用。 |用于支持 [Microsoft 加密 API：下一代 (CNG) API](https://msdn.microsoft.com/library/windows/desktop/aa376210.aspx) 的密钥存储的提供程序。 这类存储通常是硬件安全模块 — 一种用于保护和管理数字密钥并提供加密处理的物理设备。  | MSSQL_CNG_STORE|
| [SqlColumnEncryptionCspProvider 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcolumnencryptioncspprovider.aspx)<br><br>**注意：** 此提供程序在 .NET Framework 4.6.1 或更高版本中可用。| 用于支持 [Microsoft 加密 API (CAPI)](https://msdn.microsoft.com/library/aa266944.aspx)的密钥存储的提供程序。 这类存储通常是硬件安全模块 — 一种用于保护和管理数字密钥并提供加密处理的物理设备。| MSSQL_CSP_PROVIDER |
  
你无需对应用程序代码进行任何更改便可使用这些提供程序，但请注意以下事项：

- 你（或你的 DBA）需要确保列主密钥元数据中配置的提供程序名称正确，并且列主密钥路径符合对于给定提供程序有效的密钥路径格式。 建议你使用诸如 SQL Server Management Studio 之类的工具来配置密钥，这类工具在发出 [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) 语句时会自动生成有效的提供程序名称和密钥路径。 有关详细信息，请参阅 [使用 SQL Server Management Studio 配置始终加密](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md) 和 [使用 PowerShell 配置始终加密](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)。
- 需要确保应用程序可以访问密钥存储中的密钥。 这可能涉及向应用程序授予对密钥和/或密钥存储的访问权限（具体取决于密钥存储），或执行其他特定于密钥存储的配置步骤。 例如，若要访问实现 CNG 或 CAPI 的密钥存储（例如硬件安全模块），需确保在应用程序计算机上安装用于为存储实现 CNG 或 CAPI 的库。 有关详细信息，请参阅 [创建并存储列主密钥 (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)。

### <a name="using-azure-key-vault-provider"></a>使用 Azure 密钥保管库提供程序

Azure 密钥保管库便于存储和管理用于始终加密的列主密钥（尤其是当应用程序在 Azure 中托管时）。 用于 SQL Server 的 .NET Framework 数据提供程序不包含用于 Azure 密钥保管库的内置列主密钥存储提供程序，但它将以 Nuget 程序包的形式提供，可与应用程序轻松集成。 有关详细信息，请参阅 [始终加密 — 使用数据加密保护 SQL 数据库中的敏感数据并将加密密钥存储在 Azure 密钥保管库中](https://azure.microsoft.com/documentation/articles/sql-database-always-encrypted-azure-key-vault/)。

### <a name="implementing-a-custom-column-master-key-store-provider"></a>实现自定义列主密钥存储提供程序

如果想要将列主密钥存储在现有提供程序不支持的密钥存储中，则可以通过扩展 [SqlColumnEncryptionCngProvider 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcolumnencryptioncngprovider.aspx) 并使用 [SqlConnection.RegisterColumnEncryptionKeyStoreProviders](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders.aspx) 方法进行注册来实现自定义提供程序。


```
public class MyCustomKeyStoreProvider : SqlColumnEncryptionKeyStoreProvider
    {
        public override byte[] EncryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] columnEncryptionKey)
        {
            // Logic for encrypting a column encrypted key.
        }
        public override byte[] DecryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] EncryptedColumnEncryptionKey)
        {
            // Logic for decrypting a column encrypted key.
        }
    }  
    class Program
    {
        static void Main(string[] args)
        {
            Dictionary\<string, SqlColumnEncryptionKeyStoreProvider> providers =
               new Dictionary\<string, SqlColumnEncryptionKeyStoreProvider>();
            providers.Add("MY_CUSTOM_STORE", customProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
            providers.Add(SqlColumnEncryptionCertificateStoreProvider.ProviderName, customProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers); 
       // ...
        }

    }
```
 
### <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>使用列主密钥存储提供程序进行编程密钥预配

在访问加密列时，用于 SQL Server 的 .NET Framework 数据提供程序将以透明方式查找并调用正确的列主密钥存储提供程序来解密列加密密钥。 通常情况下，普通的应用程序代码不会直接调用列主密钥存储提供程序。 不过，你可以显式实例化并调用一个提供程序，以编程方式预配和管理始终加密密钥：以便生成加密列加密密钥以及对列加密密钥解密（例如，在列主密钥轮替过程中）。 有关详细信息，请参阅 [Always Encrypted 密钥管理概述](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)。
请注意，仅当使用自定义密钥存储提供程序时，才有可能需要实现你自己的密钥管理工具。 使用存储于密钥存储（存在内置提供程序）或 Azure 密钥保管库中的密钥时，可以使用现有工具（如 SQL Server Management Studio 或 PowerShell）来管理和预配密钥。
以下示例说明如何生成列加密密钥并使用 [SqlColumnEncryptionCertificateStoreProvider 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider.aspx) 通过证书对密钥加密。


```
using System.Security.Cryptography;
static void Main(string[] args)
{
    byte[] EncryptedColumnEncryptionKey = GetEncryptedColumnEncryptonKey(); 
    Console.WriteLine("0x" + BitConverter.ToString(EncryptedColumnEncryptionKey).Replace("-", "")); 
    Console.ReadKey();
}

static byte[]  GetEncryptedColumnEncryptonKey()
{
    int cekLength = 32;
    String certificateStoreLocation = "CurrentUser";
    String certificateThumbprint = "698C7F8E21B2158E9AED4978ADB147CF66574180";
    // Generate the plaintext column encryption key.
    byte[] columnEncryptionKey = new byte[cekLength];
    RNGCryptoServiceProvider rngCsp = new RNGCryptoServiceProvider();
    rngCsp.GetBytes(columnEncryptionKey);

    // Encrypt the column encryption key with a certificate.
    string keyPath = String.Format(@"{0}/My/{1}", certificateStoreLocation, certificateThumbprint);
    SqlColumnEncryptionCertificateStoreProvider provider = new SqlColumnEncryptionCertificateStoreProvider();
    return provider.EncryptColumnEncryptionKey(keyPath, @"RSA_OAEP", columnEncryptionKey); 
}
```


## <a name="controlling-performance-impact-of-always-encrypted"></a>控制始终加密对性能的影响

始终加密是一种客户端加密技术，因此，大部分性能开销发生在客户端，而不是数据库中。 除加密和解密操作的成本之外，客户端上的其他性能开销来源包括：
- 额外往返数据库以检索查询参数的元数据。
- 调用列主密钥存储以访问列主密钥。

本节介绍用于 SQL Server 的 .NET Framework 数据提供程序中的内置性能优化，以及如何控制上述两个因素对性能的影响。

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>控制为了检索查询参数的元数据而往返的次数

如果为连接启用了始终加密，默认情况下，用于 SQL Server 的 .NET Framework 数据提供程序将为每个参数化查询调用 [sys.sp_describe_parameter_encryption](../../system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) ，并将查询语句（不带任何参数值）传递到 SQL Server。 **sys.sp_describe_parameter_encryption** 会分析查询语句，以了解是否有任何参数需要加密；如果有，则会针对每个需要加密的参数返回加密相关信息，以便用于 SQL Server 的 .NET Framework 数据提供程序对参数值加密。 以上行为可确保实现针对客户端应用程序的高级别透明性。 应用程序（和应用程序开发人员）不需要知道哪些查询在访问加密列，只需在 SqlParameter 对象中将面向加密列的值传递到用于 SQL Server 的 .NET Framework 数据提供程序即可。


### <a name="query-metadata-caching"></a>查询元数据缓存

在 .NET Framework 4.6.2 和更高版本中，用于 SQL Server 的 .NET Framework 数据提供程序会为每个查询语句缓存 **sys.sp_describe_parameter_encryption** 的结果。 因此，如果多次执行相同查询语句，则驱动程序仅调用 **sys.sp_describe_parameter_encryption** 一次。 查询语句的加密元数据缓存大大减少了从数据库提取元数据的性能开销。 缓存默认为启用状态。 可以通过将  [SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled.aspx) 设置为 false 来禁用参数元数据缓存，但是不建议这样做，除非是在如下所述的极少数情况下：

请考虑一个具有两个不同架构的数据库：s1 和 s2。 每个架构都包含一个具有相同名称的表：t。 除与加密相关的属性外，S1.t 和 s2.t 表的定义是相同的：名为 c 的列，在 s1.t 中未加密，在 s2.t 中已加密。 该数据库具有两个用户：u1 和 u2。 u1 用户的默认架构是 s1。 u2 的默认架构是 s2。 一个 .NET 应用程序打开与该数据库之间的两个连接，在一个连接上模拟 u1 用户，在另一个连接上模拟 u2 用户。 该应用程序在用于用户 u1 的连接上发送具有面向 c 列的参数的查询（该查询未指定架构，因此采用默认用户架构）。 接下来，该应用程序在用于 u2 用户的连接上发送相同查询。 如果启用了查询元数据缓存，则在第一个查询之后，缓存会使用指示 c 列（查询参数目标）未加密的元数据进行填充。 因为第二个查询具有相同的查询语句，所以会使用存储在缓存中的信息。 因此，驱动程序会在未加密参数的情况下发送查询（这是不正确的，因为目标列 s2.t.c 进行了加密），从而向服务器泄露参数的纯文本值。 服务器会检测该不兼容行，会强制驱动程序刷新缓存，因此该应用程序会以透明方式重新发送具有正确加密的参数值的查询。 在这种情况下，应禁用缓存以防止向服务器泄露敏感值。 




### <a name="setting-always-encrypted-at-the-query-level"></a>在查询级别设置始终加密

若要控制检索参数化查询的加密元数据时对性能的影响，可以为单个查询启用始终加密，而不是为连接设置始终加密。 这样一来，就可以确保仅针对你知道具有面向加密列的参数的查询调用 **sys.sp_describe_parameter_encryption** 。 但请注意，这样做会降低加密的透明度：如果更改数据库列的加密属性，可能需要更改应用程序代码，使其与架构更改保持一致。

> [!NOTE]
> 在查询级别上设置始终加密在 .NET 4.6.2 和更高版本中的性能优势比较有限，这些版本实现了参数加密元数据缓存。

若要控制单个查询的始终加密行为，需使用 [SqlCommand](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx) 的此构造函数和 [SqlCommandColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommandcolumnencryptionsetting.aspx)。 下面是一些有用的指导原则：
- 如果客户端应用程序通过数据库连接发送的大多数查询访问的是加密列，则可执行以下操作：
    - 将“列加密设置”  连接字符串关键字设置为“已启用”  。
    - 对于不访问任何加密列的单个查询，则设置 **SqlCommandColumnEncryptionSetting.Disabled**。 这将禁止调用 sys.sp_describe_parameter_encryption，同时禁止尝试对结果集中的任何值解密。
    - 对于其参数不需要加密但会从加密列检索数据的单个查询，则设置 **SqlCommandColumnEncryptionSetting.ResultSet** 。 这将禁止调用 sys.sp_describe_parameter_encryption 和参数加密。 查询将能够解密来自加密列的结果。
- 如果客户端应用程序通过数据库连接发送的大多数查询不访问加密列，则可执行以下操作：
    - 将“列加密设置”  连接字符串关键字设置为“已禁用”  。
    - 对于有参数需要加密的单个查询，则设置 **SqlCommandColumnEncryptionSetting.Enabled**。 这将允许调用 sys.sp_describe_parameter_encryption，同时允许对从加密列中检索到的任何查询结果解密。
    - 对于其参数不需要加密但会从加密列检索数据的查询，则设置 **SqlCommandColumnEncryptionSetting.ResultSet** 。 这将禁止调用 sys.sp_describe_parameter_encryption 和参数加密。 查询将能够解密来自加密列的结果。

在以下示例中，将对数据库连接禁用始终加密。 应用程序发出的查询有一个面向未加密的 LastName 列的参数。 该查询从已加密的 SSN 和 BirthDate 列中检索数据。 在这种情况下，不需要调用 sys.sp_describe_parameter_encryption 来检索加密元数据。 但是，需要启用查询结果解密，以便应用程序从两个加密列接收纯文本值。 使用 SqlCommandColumnEncryptionSetting.ResultSet 设置可以确保这一点。



```
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
using (SqlConnection connection = new SqlConnection(connectionString))
{
    connection.Open();
    using (SqlCommand cmd = new SqlCommand(@"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName",
connection, null, SqlCommandColumnEncryptionSetting.ResultSetOnly))
    {
        SqlParameter paramLastName = cmd.CreateParameter();
        paramLastName.ParameterName = @"@LastName";
        paramLastName.DbType = DbType.String;
        paramLastName.Direction = ParameterDirection.Input;
        paramLastName.Value = "Abel";
        paramLastName.Size = 50;
        cmd.Parameters.Add(paramLastName);
        using (SqlDataReader reader = cmd.ExecuteReader())
            {
               if (reader.HasRows)
               {
                  while (reader.Read())
                  {
                     Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
                  }
               }
            }
  } 
}
```


### <a name="column-encryption-key-caching"></a>列加密密钥缓存

为了减少对列加密密钥解密时调用列主密钥存储的次数，用于 SQL Server 的 .NET Framework 数据提供程序会将纯文本列加密密钥缓存在内存中。 从数据库元数据收到加密列的加密密钥值之后，驱动程序首先会尝试查找与加密密钥值对应的纯文本列加密密钥。 仅当在缓存中找不到加密列的加密密钥值时，驱动程序才会调用包含列主密钥的密钥存储。

> [!NOTE]
> 在 .NET Framework 4.6 和 4.6.1 中，永远不会收回缓存中的列加密密钥条目。 这意味着，对于某个给定的加密列加密密钥，驱动程序在应用程序生存期间只会访问密钥存储一次。

在 .NET Framework 4.6.2 和更高版本中，出于安全原因，会在可配置的生存时间间隔之后中逐出缓存项。 默认生存时间值是 2 小时。 如果你对应用程序中以纯文本形式缓存列加密密钥的时间长度具有更严格的安全要求，则可以使用 [SqlConnection.ColumnEncryptionKeyCacheTtl 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.columnencryptionkeycachettl.aspx)更改它。 


## <a name="enabling-additional-protection-for-a-compromised-sql-server"></a>对遭到入侵的 SQL Server 启用附加保护

默认情况下，用于 SQL Server 的 .NET Framework 数据提供程序  依赖于数据库系统（SQL Server 或 Azure SQL 数据库）来提供有关数据库中的哪些列进行加密以及如何加密的元数据。 加密元数据使用于 SQL Server 的 .NET Framework 数据提供程序可以加密查询参数并解密查询结果，而无需从应用程序进行任何输入，从而大大减少了应用程序中所需的更改量。 但是，如果 SQL Server 进程遭到入侵，并且攻击者篡改 SQL Server 发送给用于 SQL Server 的 .NET Framework 数据提供程序的元数据，则攻击者可能能够窃取敏感信息。 本部分介绍可帮助针对这种类型的攻击提供附加保护级别（以降低透明度为代价）的 API。 

### <a name="forcing-parameter-encryption"></a>强制实施参数加密 

在用于 SQL Server 的 .NET Framework 数据提供程序将参数化查询发送到 SQL Server 之前，它会要求 SQL Server（通过调用 [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)）分析查询语句并提供有关查询中的哪些参数应进行加密的信息。 遭到入侵的 SQL Server 实例可能会发送指示参数不是面向加密列的元数据（尽管事实是该列在数据库中会进行加密）来误导用于 SQL Server 的 .NET Framework 数据提供程序。 因此，用于 SQL Server 的 .NET Framework 数据提供程序不会加密参数值，会以纯文本形式将它发送到遭到入侵的 SQL Server 实例。

若要阻止这类攻击，应用程序可以将参数的 [SqlParameter.ForceColumnEncryption 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.forcecolumnencryption.aspx) 设置为 true。 这会导致用于 SQL Server 的 .NET Framework 数据提供程序引发异常（如果它从服务器收到的元数据指示参数不需要进行加密）。

请注意，虽然使用 **SqlParameter.ForceColumnEncryption 属性** 有助于提高安全性，不过它同样降低了面向客户端应用程序的加密透明度。 如果更新数据库架构以更改加密列集，则可能还需要进行应用程序更改。

下面的代码示例演示如何使用 **SqlParameter.ForceColumnEncryption 属性** 防止以纯文本形式将身份证号发送给数据库。 



```
SqlCommand cmd = _sqlconn.CreateCommand(); 

// Use parameterized queries to access Always Encrypted data. 
 
cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = @SSN;"; 

SqlParameter paramSSN = cmd.CreateParameter(); 
paramSSN.ParameterName = @"@SSN"; 
paramSSN.DbType = DbType.AnsiStringFixedLength; 
paramSSN.Direction = ParameterDirection.Input; 
paramSSN.Value = ssn; 
paramSSN.Size = 11; 
paramSSN.ForceColumnEncryption = true; 
cmd.Parameters.Add(paramSSN); 

SqlDataReader reader = cmd.ExecuteReader();
```
 

### <a name="configuring-trusted-column-master-key-paths"></a>配置可信列主密钥路径 

SQL Server 对于面向加密列的查询参数和从加密列检索的结果返回的加密元数据包含用于标识密钥存储以及密钥在密钥存储中的位置的列主密钥密钥路径。 如果 SQL Server 实例遭到入侵，则它可能会发送将用于 SQL Server 的 .NET Framework 数据提供程序定向到攻击者控制的位置的密钥路径。 对于需要应用程序进行身份验证的密钥存储，这可能会导致泄漏密钥存储凭据。 

若要阻止这类攻击，应用程序可以使用 [SqlConnection.ColumnEncryptionTrustedMasterKeyPaths 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths.aspx)为给定服务器指定可信密钥路径的列表。 如果用于 SQL Server 的 .NET Framework 数据提供程序收到处于可信密钥路径列表之外的密钥路径，它会引发异常。 

请注意，虽然设置可信密钥路径会提高应用程序的安全性，不过每当你轮换列主密钥时（每当列主密钥路径更改时），你都需要更改应用程序的代码或/和配置。 

下面的示例演示如何配置可信列主密钥路径：


```
// Configure trusted key paths to protect against fake key paths sent by a compromised SQL Server instance 
// First, create a list of trusted key paths for your server 
List<string> trustedKeyPathList = new List<string>(); 
trustedKeyPathList.Add("CurrentUser/my/425CFBB9DDDD081BB0061534CE6AB06CB5283F5Ea"); 

// Register the trusted key path list for your server 

SqlConnection.ColumnEncryptionTrustedMasterKeyPaths.Add(serverName, trustedKeyPathList);
```
 



## <a name="copying-encrypted-data-using-sqlbulkcopy"></a>使用 SqlBulkCopy 复制加密数据

使用 SqlBulkCopy，可以将已加密并且存储在某个表中的数据复制到另一个表，而无需对数据解密。 为此：

- 请确保目标表的加密配置与源表的配置完全相同。 特别是，两个表必须对相同的列加密，并且必须使用相同的加密类型和相同的加密密钥对列加密。 注意：如果任何目标列的加密方式与其相应的源列不同，你都不能在复制操作完成后对目标表中的数据进行解密。 数据将损坏。
- 配置数据库到源表和目标表的连接，而不启用始终加密。 
- 设置 AllowEncryptedValueModifications 选项（请参阅 [SqlBulkCopyOptions](https://msdn.microsoft.com/library/system.data.sqlclient.sqlbulkcopyoptions.aspx)）。 注意：指定 AllowEncryptedValueModifications 时需谨慎，因为这可能会导致损坏数据库，因为用于 SQL Server 的 .NET Framework 数据提供程序不会检查数据是否确实已加密，也不会检查是否使用与目标列相同的加密类型、算法和密钥对数据进行了正确加密。

请注意，AllowEncryptedValueModifications 选项在 .NET Framework 4.6.1 及更高版本中可用。

下面是将数据从一个表复制到另一个表的示例。 其中假定 SSN 和 BirthDate 列已加密。
        

```
static public void CopyTablesUsingBulk(string sourceTable, string targetTable)
{
   string sourceConnectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
   string targetConnectionString = "Data Source= server64; Initial Catalog=Clinic; Integrated Security=true";
   using (SqlConnection connSource = new SqlConnection(sourceConnectionString))
   {
      connSource.Open();
      using (SqlCommand cmd = new SqlCommand(string.Format("SELECT [PatientID], [SSN], [FirstName], [LastName], [BirthDate] FROM {0}", sourceTable), connSource))
      {
         using (SqlDataReader reader = cmd.ExecuteReader())
         {
            SqlBulkCopy copy = new SqlBulkCopy(targetConnectionString, SqlBulkCopyOptions.KeepIdentity | SqlBulkCopyOptions.AllowEncryptedValueModifications);
            copy.EnableStreaming = true;
            copy.DestinationTableName = targetTable;
            copy.WriteToServer(reader);
         }
      }
}
```


## <a name="always-encrypted-api-reference"></a>始终加密 API 参考

**命名空间:** [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)

**程序集：** System.Data（在 System.Data.dll 中）




|“属性”|描述|在以下 .NET 版本中引入
|:---|:---|:---
|[SqlColumnEncryptionCertificateStoreProvider 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider.aspx)|用于 Windows 证书存储区的密钥存储提供程序。|  4.6
|[SqlColumnEncryptionCngProvider 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcolumnencryptioncngprovider.aspx)|用于 Microsoft 下一代加密 API (CNG) 的密钥存储提供程序。|  4.6.1
|[SqlColumnEncryptionCspProvider 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcolumnencryptioncspprovider.aspx)|用于 Microsoft 基于 CAPI 的加密服务提供程序 (CSP) 的密钥存储提供程序。|4.6.1  
|[SqlColumnEncryptionKeyStoreProvider 类](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcolumnencryptionkeystoreprovider.aspx)|密钥存储提供程序的基类。|  4.6
|[SqlCommandColumnEncryptionSetting 枚举](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommandcolumnencryptionsetting.aspx)|为数据库连接启用加密和解密的设置。|4.6  
|[SqlConnectionColumnEncryptionSetting 枚举](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx)|控制各个查询的始终加密行为的设置。|4.6  
| [SqlConnectionStringBuilder.ColumnEncryptionSetting 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx)|获取并设置连接字符串中的始终加密。|4.6|
| [SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled.aspx) | 启用和禁用加密查询元数据缓存。 | 4.6.2
| [SqlConnection.ColumnEncryptionKeyCacheTtl 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.columnencryptionkeycachettl.aspx) | 获取和设置列加密密钥缓存中的项的生存时间。 | 4.6.2
|[SqlConnection.ColumnEncryptionTrustedMasterKeyPaths 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths.aspx)|允许你为数据库服务器设置受信任的密钥路径的列表。 如果在处理应用程序查询时，驱动程序收到不在列表上的密钥路径，则查询将失败。 此属性提供针对安全攻击的额外保护，这些攻击涉及到提供伪造密钥路径的受损 SQL Server，这可能导致泄露密钥存储凭据。|  4.6
|[SqlConnection.RegisterColumnEncryptionKeyStoreProviders 方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders.aspx)|允许你注册自定义密钥存储提供程序。 它是一个将密钥存储提供程序名称映射到密钥存储提供程序实现的字典。|  4.6
|[SqlCommand 构造函数 (String, SqlConnection, SqlTransaction, SqlCommandColumnEncryptionSetting)](https://msdn.microsoft.com/library/dn956511\(v=vs.110\).aspx)|允许你控制各个查询的始终加密行为。|  4.6
|[SqlParameter.ForceColumnEncryption 属性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.forcecolumnencryption.aspx)|强制实施参数加密。 如果 SQL Server 告知驱动程序参数不需加密，则使用该参数的查询会失败。 此属性提供针对安全攻击的额外保护，这些攻击涉及受损 SQL Server 向客户端提供不正确的加密元数据，这可能会导致数据泄漏。|4.6  
|新的 [连接字符串](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx) 关键字： `Column Encryption Setting=enabled`|启用或禁用针对连接的始终加密功能。| 4.6 
  

## <a name="see-also"></a>另请参阅

- [Always Encrypted（数据库引擎）](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [始终加密博客](https://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
- [SQL 数据库教程：使用 Always Encrypted 保护敏感数据](https://azure.microsoft.com/documentation/articles/sql-database-always-encrypted/)

















