---
title: 在 JDBC 驱动程序中使用 Always Encrypted |Microsoft Docs
ms.custom: ''
ms.date: 07/11/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
author: MightyPen
ms.author: genemi
ms.openlocfilehash: f19878f73397b9146765fecd879dad07ebb73dc3
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67916453"
---
# <a name="using-always-encrypted-with-the-jdbc-driver"></a>对 JDBC 驱动程序使用 Always Encrypted
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

本页提供有关如何使用[Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)和用于 SQL Server 的 Microsoft JDBC Driver 6.0 (或更高版本) 开发 Java 应用程序的信息。

Always Encrypted 允许客户端对敏感数据进行加密，并且永远不向 SQL Server 或 Azure SQL 数据库显示该数据或加密密钥。 启用了 Always Encrypted 的驱动程序（例如用于 SQL Server 的 Microsoft JDBC Driver 6.0 或更高版本）通过在客户端应用程序中以透明方式对敏感数据进行加密和解密来实现此行为。 驱动程序自动确定哪些查询参数与 Always Encrypted 数据库列相对应, 并在将这些参数的值发送到 SQL Server 或 Azure SQL 数据库之前对这些参数的值进行加密。 同样，该驱动程序以透明方式对查询结果中从加密数据库列检索到的数据进行解密。 有关详细信息, 请参阅 JDBC 驱动程序的[Always Encrypted (数据库引擎)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)和[Always Encrypted API 参考](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)。

## <a name="prerequisites"></a>必备条件
- 请确保在开发计算机上安装了 SQL Server 的 Microsoft JDBC Driver 6.0 (或更高版本)。 
- 下载并安装 Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files。  请确保阅读包含在 zip 文件中的自述文件，以获取安装说明和导出/导入问题的相关详细信息。  

    - 如果使用 mssql-jdbc-X.X.X.jre7.jar 或 sqljdbc41.jar，则可以从 [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 7 下载](https://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html)下载策略文件

    - 如果使用 mssql-jdbc-X.X.X.jre8.jar 或 sqljdbc42.jar，则可以从 [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 8 下载](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html)下载策略文件

    - 如果使用 mssql-jdbc-X. x. x. x. x. x. x. x. x. x. jre9, 则无需下载策略文件。 Java 9 中的区域策略默认为不受限制的加密强度。

## <a name="working-with-column-master-key-stores"></a>使用列主密钥存储
若要对加密列的数据进行加密或解密, SQL Server 维护列加密密钥。 列加密密钥以加密形式存储在数据库元数据中。 每个列加密密钥都有一个用于加密列加密密钥的相应列主密钥。 数据库元数据不包含列主密钥。 这些密钥仅由客户端持有。 但是, 数据库元数据包含有关列主密钥相对于客户端的存储位置的信息。 例如, 数据库元数据可能会说, 持有列主密钥的密钥存储是 Windows 证书存储, 用于加密和解密的特定证书位于 Windows 证书存储区中的特定路径。 如果客户端有权访问 Windows 证书存储中的证书, 则可以获取证书。 然后, 可以使用证书来解密列加密密钥。 然后, 可以使用该加密密钥对使用该列加密密钥的加密列的数据进行解密或加密。

Microsoft JDBC Driver for SQL Server 使用列主密钥存储提供程序与密钥存储进行通信, 该提供程序是派生自**SQLServerColumnEncryptionKeyStoreProvider**的类的实例。

### <a name="using-built-in-column-master-key-store-providers"></a>使用内置列主密钥存储提供程序
Microsoft JDBC Driver for SQL Server 附带以下内置列主密钥存储提供程序。 其中一些提供程序使用特定提供程序名称 (用于查找提供程序) 预先注册, 某些提供程序需要其他凭据或显式注册。

| 类                                                 | 描述                                        | 提供程序（查找）名称  | 是否已预注册？ |
| :---------------------------------------------------- | :------------------------------------------------- | :---------------------- | :----------------- |
| **SQLServerColumnEncryptionAzureKeyVaultProvider**    | 用于 Azure Key Vault 的密钥存储的提供程序。 | AZURE_KEY_VAULT         | 否                 |
| **SQLServerColumnEncryptionCertificateStoreProvider** | 用于 Windows 证书存储的提供程序。      | MSSQL_CERTIFICATE_STORE | 是                |
| **SQLServerColumnEncryptionJavaKeyStoreProvider**     | Java 密钥存储的提供程序                   | MSSQL_JAVA_KEYSTORE     | 是                |

对于预注册的密钥存储提供程序, 无需对应用程序代码进行任何更改即可使用这些提供程序, 但请注意以下事项:

- 你（或你的 DBA）需要确保列主密钥元数据中配置的提供程序名称正确，并且列主密钥路径符合对于给定提供程序有效的密钥路径格式。 建议你使用诸如 SQL Server Management Studio 之类的工具来配置密钥，这类工具在发出 CREATE COLUMN MASTER KEY (Transact-SQL) 语句时会自动生成有效的提供程序名称和密钥路径。
- 确保应用程序可以访问密钥存储中的密钥。 此任务可能涉及向应用程序授予对密钥和/或密钥存储的访问权限（具体取决于密钥存储），或执行其他特定于密钥存储的配置步骤。 例如, 若要使用 SQLServerColumnEncryptionJavaKeyStoreProvider, 需要在连接属性中提供密钥存储的位置和密码。 

后面的部分将更详细地介绍所有这些密钥存储提供程序。 只需实现一个密钥存储提供程序即可使用 Always Encrypted。

### <a name="using-azure-key-vault-provider"></a>使用 Azure 密钥保管库提供程序
Azure Key Vault 便于存储和管理用于 Always Encrypted 的列主密钥（尤其是当应用程序在 Azure 中托管时）。 Microsoft JDBC Driver for SQL Server 包括内置提供程序 SQLServerColumnEncryptionAzureKeyVaultProvider, 适用于在 Azure Key Vault 中存储密钥的应用程序。 此提供程序的名称为 AZURE_KEY_VAULT。 为了使用 Azure Key Vault 存储提供程序, 应用程序开发人员需要在 Azure Key Vault 中创建保管库和密钥, 并在 Azure Active Directory 中创建应用注册。 必须为已注册的应用程序授予 Get、解密、加密、解包密钥、环绕键, 并验证为创建用于 Always Encrypted 的密钥保管库定义的访问策略中的权限。 若要详细了解如何设置密钥保管库并创建列主密钥, 请参阅[Azure Key Vault-逐步执行](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/)操作和[在 Azure Key Vault 中创建列主密钥](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault)。

对于此页上的示例, 如果已通过使用 SQL Server Management Studio 创建了基于 Azure Key Vault 的列主密钥和列加密密钥, 则用来重新创建该 T-sql 脚本的 T-sql 脚本可能与此示例类似于其自己的特定**KEY_PATH**和**ENCRYPTED_VALUE**:

```sql
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',
    KEY_PATH = N'https://<MyKeyValutName>.vault.azure.net:443/keys/Always-Encrypted-Auto1/c61f01860f37302457fa512bb7e7f4e8'
)

CREATE COLUMN ENCRYPTION KEY [MyCEK]
WITH VALUES
(
    COLUMN_MASTER_KEY = [MyCMK],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 0x01BA000001680074507400700073003A002F002F006400610076006...
)
```

若要使用 Azure Key Vault, 客户端应用程序需要实例化 SQLServerColumnEncryptionAzureKeyVaultProvider, 并将其注册到驱动程序中。

下面是初始化 SQLServerColumnEncryptionAzureKeyVaultProvider 的示例:  

```java
SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider = new SQLServerColumnEncryptionAzureKeyVaultProvider(clientID, clientKey);
```

**clientID**是 Azure Active Directory 实例中的应用注册的应用程序 ID。 **clientKey**是在该应用程序下注册的密钥密码, 它提供对 AZURE KEY VAULT 的 API 访问。

在应用程序创建 SQLServerColumnEncryptionAzureKeyVaultProvider 的实例后, 应用程序必须使用 SQLServerConnection () 方法向驱动程序注册实例。 强烈建议使用默认查找名称 AZURE_KEY_VAULT 注册实例, 该名称可通过调用 SQLServerColumnEncryptionAzureKeyVaultProvider () API 来获取。 使用默认名称, 你可以使用 SQL Server Management Studio 或 PowerShell 等工具来预配和管理 Always Encrypted 密钥 (这些工具使用默认名称来生成用于列主密钥的元数据对象)。 下面的示例演示如何注册 Azure Key Vault 提供程序。 有关 SQLServerConnection Sqlserverconnection.registercolumnencryptionkeystoreproviders () 方法的详细信息, 请参阅[JDBC Driver ALWAYS ENCRYPTED API 参考](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)。

```java
Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
keyStoreMap.put(akvProvider.getName(), akvProvider);
SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
```

> [!IMPORTANT]
>  如果使用 Azure Key Vault 密钥存储提供程序, 则 JDBC 驱动程序的 Azure Key Vault 实现依赖于这些库 (来自 GitHub), 这些库必须包含在你的应用程序中:
>
>  [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java)
>
>  [azure-activedirectory-library-for-java 库](https://github.com/AzureAD/azure-activedirectory-library-for-java)
>
> 有关如何在 Maven 项目中包含这些依赖项的示例, 请参阅[使用 Apache Maven 下载 ADAL4J 和 AKV 依赖项](https://github.com/Microsoft/mssql-jdbc/wiki/Download-ADAL4J-And-AKV-Dependencies-with-Apache-Maven)

### <a name="using-windows-certificate-store-provider"></a>使用 Windows 证书存储提供程序
SQLServerColumnEncryptionCertificateStoreProvider，可以用于在 Windows 证书存储区中存储列主密钥。 使用 SQL Server Management Studio (SSMS) Always Encrypted 向导或其他受支持的工具, 在数据库中创建列主密钥和列加密密钥定义。 同一向导可用于在 Windows 证书存储中生成自签名证书, 该证书可用作始终加密数据的列主密钥。 有关列主密钥和列加密密钥 T-sql 语法的详细信息, 请参阅[CREATE COLUMN MASTER key](../../t-sql/statements/create-column-master-key-transact-sql.md)和[CREATE column encryption key](../../t-sql/statements/create-column-encryption-key-transact-sql.md) 。

SQLServerColumnEncryptionCertificateStoreProvider 的名称为 MSSQL_CERTIFICATE_STORE, 可由 provider 对象的 getName () API 查询。 它由驱动程序自动注册, 并且无需更改任何应用程序即可无缝使用。

对于此页上的示例, 如果已使用 SQL Server Management Studio 创建了基于 Windows 证书存储的列主密钥和列加密密钥, 则用来重新创建该 T-sql 脚本的 KEY_ 可能与此示例类似: **路径**和**ENCRYPTED_VALUE**:

```sql
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',
    KEY_PATH = N'CurrentUser/My/A2A91F59C461B559E4D962DA9D2BC6131B32CB91'
)

CREATE COLUMN ENCRYPTION KEY [MyCEK]
WITH VALUES
(
    COLUMN_MASTER_KEY = [MyCMK],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 0x016E000001630075007200720065006E0074007500730065007200...
)
```

> [!IMPORTANT]
> 尽管本文中的其他密钥存储提供程序可在该驱动程序支持的所有平台上使用, 但 JDBC 驱动程序的 SQLServerColumnEncryptionCertificateStoreProvider 实现仅在 Windows 操作系统上可用。 它依赖于驱动程序包中提供的 sqljdbc_auth。 若要使用此提供程序，请将 sqljdbc_auth.dll 文件复制计算机中 Windows 系统路径下的 JDBC 驱动程序安装目录中。 也可以设置 java.library.path 系统属性来指定 sqljdbc_auth.dll 的目录。 如果您运行 32 位的 Java 虚拟机 (JVM)，则使用 x86 文件夹中的 sqljdbc_auth.dll 文件，即使操作系统是 x64 版本也不例外。 如果您在 x64 处理器上运行 64 位 JVM，则使用 x64 文件夹中的 sqljdbc_auth.dll 文件。 例如，如果使用的是 32 位 JVM，并且 JDBC 驱动程序安装在默认目录中，可以在 Java 应用程序启动时使用以下虚拟机 (VM) 参数来指定 DLL 的位置：`-Djava.library.path=C:\Microsoft JDBC Driver <version> for SQL Server\sqljdbc_<version>\enu\auth\x86`

### <a name="using-java-key-store-provider"></a>使用 Java 密钥存储提供程序
JDBC 驱动程序附带 Java 密钥存储的内置密钥存储提供程序实现。 如果连接字符串中存在**keyStoreAuthentication**连接字符串属性, 并将其设置为 "JavaKeyStorePassword", 则驱动程序将自动实例化并注册 Java 密钥存储的提供程序。 Java 密钥存储提供程序的名称为 MSSQL_JAVA_KEYSTORE。 还可以使用 SQLServerColumnEncryptionJavaKeyStoreProvider. getName () API 查询此名称。 

有三个连接字符串属性, 客户端应用程序可以使用这些属性指定驱动程序对 Java 密钥存储进行身份验证所需的凭据。 驱动程序基于连接字符串中这三个属性的值初始化提供程序。

**keyStoreAuthentication:** 标识要使用的 Java 密钥存储。 使用 Microsoft JDBC Driver 6.0 和更高版本 SQL Server, 只能通过此属性向 Java 密钥存储进行身份验证。 对于 Java 密钥存储, 此属性的值必须`JavaKeyStorePassword`为。

**keyStoreLocation:** 存储列主密钥的 Java 密钥存储文件的路径。 路径包含密钥存储文件名。

**keyStoreSecret:** 用于密钥存储以及密钥的机密/密码。 对于使用 Java 密钥存储, 密钥库和密钥密码必须相同。

下面是在连接字符串中提供这些凭据的示例:

```java
String connectionUrl = "jdbc:sqlserver://<server>:<port>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;keyStoreAuthentication=JavaKeyStorePassword;keyStoreLocation=<path_to_the_keystore_file>;keyStoreSecret=<keystore_key_password>";
```

还可以使用 SQLServerDataSource 对象获取或设置这些设置。 有关详细信息, 请参阅[JDBC Driver ALWAYS ENCRYPTED API 参考](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)。

当这些凭据存在于连接属性中时, JDBC 驱动程序会自动实例化 SQLServerColumnEncryptionJavaKeyStoreProvider。

### <a name="creating-a-column-master-key-for-the-java-key-store"></a>为 Java 密钥存储创建列主密钥
SQLServerColumnEncryptionJavaKeyStoreProvider 可以与 JKS 或 PKCS12 密钥存储类型一起使用。 若要创建或导入要与此提供程序一起使用的密钥, 请使用 Java [keytool](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html)实用程序。 密钥必须与密钥存储本身具有相同的密码。 下面的示例演示如何使用 keytool 实用工具创建公钥及其关联的私钥:

```
keytool -genkeypair -keyalg RSA -alias AlwaysEncryptedKey -keystore keystore.jks -storepass mypassword -validity 360 -keysize 2048 -storetype jks
```

此命令创建一个公钥, 并将其包装在 x.509 自签名证书中, 该证书存储在密钥存储 "jks" 及其关联的私钥中。 密钥存储中的此条目由别名 "AlwaysEncryptedKey" 标识。

下面是使用 PKCS12 存储类型的相同示例:

```
keytool -genkeypair -keyalg RSA -alias AlwaysEncryptedKey -keystore keystore.pfx -storepass mypassword -validity 360 -keysize 2048 -storetype pkcs12 -keypass mypassword
```

如果密钥存储的类型为 PKCS12, 则 keytool 实用程序不会提示输入密钥密码, 密钥密码需要与-keypass 选项一起提供, 因为 SQLServerColumnEncryptionJavaKeyStoreProvider 要求密钥存储和密钥具有相同的权限.

你还可以使用 .pfx 格式从 Windows 证书存储中导出证书, 并将其与 SQLServerColumnEncryptionJavaKeyStoreProvider 一起使用。 还可以将导出的证书作为 JKS 密钥存储类型导入到 Java 密钥存储。

创建 keytool 项后, 在数据库中创建列主密钥元数据, 这需要密钥存储提供程序名称和密钥路径。 有关如何创建列主密钥元数据的详细信息, 请参阅[CREATE COLUMN MASTER key](../../t-sql/statements/create-column-master-key-transact-sql.md)。 对于 SQLServerColumnEncryptionJavaKeyStoreProvider, 密钥路径只是密钥的别名, SQLServerColumnEncryptionJavaKeyStoreProvider 的名称为 "MSSQL_JAVA_KEYSTORE"。 你还可以使用 SQLServerColumnEncryptionJavaKeyStoreProvider 类的 getName () 公共 API 查询此名称。 

用于创建列主密钥的 T-sql 语法为:

```sql
CREATE COLUMN MASTER KEY [<CMK_name>]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_JAVA_KEYSTORE',
    KEY_PATH = N'<key_alias>'
)
```

对于上面创建的 "AlwaysEncryptedKey", 列主密钥定义为:

```sql
CREATE COLUMN MASTER KEY [MyCMK]
WITH
(
    KEY_STORE_PROVIDER_NAME = N'MSSQL_JAVA_KEYSTORE',
    KEY_PATH = N'AlwaysEncryptedKey'
)
```

> [!NOTE]
> 内置 SQL Server management Studio 功能无法为 Java 密钥存储创建列主密钥定义。 T-sql 命令必须以编程方式使用。

### <a name="creating-a-column-encryption-key-for-the-java-key-store"></a>为 Java 密钥存储创建列加密密钥
使用 Java 密钥存储中的列主密钥时, 不能使用 SQL Server Management Studio 或任何其他工具来创建列加密密钥。 客户端应用程序必须使用 SQLServerColumnEncryptionJavaKeyStoreProvider 类以编程方式创建列加密密钥。 有关详细信息, 请参阅[使用列主密钥存储提供程序进行编程密钥预配](#using-column-master-key-store-providers-for-programmatic-key-provisioning)。

### <a name="implementing-a-custom-column-master-key-store-provider"></a>实现自定义列主密钥存储提供程序
如果想要将列主密钥存储在现有提供程序不支持的密钥存储中，则可以通过扩展 SQLServerColumnEncryptionKeyStoreProvider 类并使用 SQLServerConnection.registerColumnEncryptionKeyStoreProviders() 方法进行注册来实现自定义提供程序。

```java
public class MyCustomKeyStore extends SQLServerColumnEncryptionKeyStoreProvider{  
    private String name = "MY_CUSTOM_KEYSTORE";

    public void setName(String name)
    {
        this.name = name;
    }

    public String getName()
    {
        return name;
    }

    public byte[] encryptColumnEncryptionKey(String masterKeyPath, String encryptionAlgorithm, byte[] plainTextColumnEncryptionKey)
    {
        // Logic for encrypting the column encryption key
    }

    public byte[] decryptColumnEncryptionKey(String masterKeyPath, String encryptionAlgorithm, byte[] encryptedColumnEncryptionKey)
    {
        // Logic for decrypting the column encryption key
    }
}
```

注册提供程序：

```java
SQLServerColumnEncryptionKeyStoreProvider storeProvider = new MyCustomKeyStore();
Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
keyStoreMap.put(storeProvider.getName(), storeProvider);
SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
```

## <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>使用列主密钥存储提供程序进行编程密钥预配
在访问加密列时，Microsoft JDBC Driver for SQL Server 将以透明方式查找并调用正确的列主密钥存储提供程序来解密列加密密钥。 通常情况下，普通的应用程序代码不会直接调用列主密钥存储提供程序。 不过, 您可以通过编程方式实例化并调用提供程序来预配和管理 Always Encrypted 密钥。 例如, 可以执行此步骤来生成加密的列加密密钥, 并将列加密密钥作为部分列主密钥循环进行解密。 有关详细信息，请参阅 [Always Encrypted 密钥管理概述](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)。

如果使用自定义密钥存储提供程序，可能需要实现你自己的密钥管理工具。 使用 Windows 证书存储或 Azure Key Vault 中存储的密钥时, 可以使用现有工具 (如 SQL Server Management Studio 或 PowerShell) 来管理和预配密钥。 使用存储在 Java 密钥存储中的密钥时, 需要以编程方式预配密钥。 下面的示例演示如何使用 SQLServerColumnEncryptionJavaKeyStoreProvider 类通过存储在 Java 密钥存储中的密钥来加密密钥。

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionJavaKeyStoreProvider;
import com.microsoft.sqlserver.jdbc.SQLServerColumnEncryptionKeyStoreProvider;
import com.microsoft.sqlserver.jdbc.SQLServerException;

/**
 * This program demonstrates how to create a column encryption key programmatically for the Java Key Store.
 */
public class AlwaysEncrypted {
    // Alias of the key stored in the keystore.
    private static String keyAlias = "<provide key alias>";

    // Name by which the column master key will be known in the database.
    private static String columnMasterKeyName = "MyCMK";

    // Name by which the column encryption key will be known in the database.
    private static String columnEncryptionKey = "MyCEK";

    // The location of the keystore.
    private static String keyStoreLocation = "C:\\Dev\\Always Encrypted\\keystore.jks";

    // The password of the keystore and the key.
    private static char[] keyStoreSecret = "********".toCharArray();

    /**
     * Name of the encryption algorithm used to encrypt the value of the column encryption key. The algorithm for the system providers must be
     * RSA_OAEP.
     */
    private static String algorithm = "RSA_OAEP";

    public static void main(String[] args) {
        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;";

        try (Connection connection = DriverManager.getConnection(connectionUrl);
                Statement statement = connection.createStatement();) {

            // Instantiate the Java Key Store provider.
            SQLServerColumnEncryptionKeyStoreProvider storeProvider = new SQLServerColumnEncryptionJavaKeyStoreProvider(keyStoreLocation,
                    keyStoreSecret);

            byte[] encryptedCEK = getEncryptedCEK(storeProvider);

            /**
             * Create column encryption key For more details on the syntax, see:
             * https://docs.microsoft.com/sql/t-sql/statements/create-column-encryption-key-transact-sql Encrypted column encryption key first needs
             * to be converted into varbinary_literal from bytes, for which byteArrayToHex() is used.
             */
            String createCEKSQL = "CREATE COLUMN ENCRYPTION KEY "
                    + columnEncryptionKey
                    + " WITH VALUES ( "
                    + " COLUMN_MASTER_KEY = "
                    + columnMasterKeyName
                    + " , ALGORITHM =  '"
                    + algorithm
                    + "' , ENCRYPTED_VALUE =  0x"
                    + byteArrayToHex(encryptedCEK)
                    + " ) ";
            statement.executeUpdate(createCEKSQL);
            System.out.println("Column encryption key created with name : " + columnEncryptionKey);
        }
        // Handle any errors that may have occurred.
        catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static byte[] getEncryptedCEK(SQLServerColumnEncryptionKeyStoreProvider storeProvider) throws SQLServerException {
        String plainTextKey = "You need to give your plain text";

        // plainTextKey has to be 32 bytes with current algorithm supported
        byte[] plainCEK = plainTextKey.getBytes();

        // This will give us encrypted column encryption key in bytes
        byte[] encryptedCEK = storeProvider.encryptColumnEncryptionKey(keyAlias, algorithm, plainCEK);

        return encryptedCEK;
    }

    public static String byteArrayToHex(byte[] a) {
        StringBuilder sb = new StringBuilder(a.length * 2);
        for (byte b : a)
            sb.append(String.format("%02x", b).toUpperCase());
        return sb.toString();
    }
}
```

## <a name="enabling-always-encrypted-for-application-queries"></a>为应用程序查询启用始终加密
若要对参数启用加密，以及对面向加密列的查询结果启用解密，最简单的方法是将 columnEncryptionSetting 连接字符串关键字的值设置为“已启用”   。

下面的连接字符串是在 JDBC 驱动程序中启用 Always Encrypted 的示例:

```java
String connectionUrl = "jdbc:sqlserver://<server>:<port>;user=<user>;password=<password>;databaseName=<database>;columnEncryptionSetting=Enabled;";
SQLServerConnection connection = (SQLServerConnection) DriverManager.getConnection(connectionUrl);
```

下面的代码是使用 SQLServerDataSource 对象的等效示例:

```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName("<server>");
ds.setPortNumber(<port>);
ds.setUser("<user>");
ds.setPassword("<password>");
ds.setDatabaseName("<database>");
ds.setColumnEncryptionSetting("Enabled");
SQLServerConnection con = (SQLServerConnection) ds.getConnection();
```

还可以为单个查询启用始终加密。 有关详细信息，请参阅[控制 Always Encrypted 对性能的影响](#controlling-the-performance-impact-of-always-encrypted)。 启用 Always Encrypted 不足以成功实现加密或解密。 你还需要确保：
- 应用程序具有 *查看任意列主密钥定义* 和 *查看任意列加密密钥定义* 数据库权限，这是访问数据库中始终加密密钥的相关元数据所必需的权限。 有关详细信息，请参阅 [Always Encrypted（数据库引擎）中的权限](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions)。
- 应用程序可以访问用于保护列加密密钥的列主密钥，以便对查询到的数据库列加密。 若要使用 Java 密钥存储提供程序, 需要在连接字符串中提供其他凭据。 有关详细信息, 请参阅[使用 Java 密钥存储提供程序](#using-java-key-store-provider)。

### <a name="configuring-how-javasqltime-values-are-sent-to-the-server"></a>配置如何将 java.sql.Time 值发送到服务器
sendTimeAsDatetime  连接属性用于配置将 java.sql.Time 值发送到服务器的方式。 如果设置为 false, 则将时间值作为 SQL Server 时间类型发送。 如果设置为 true, 则将时间值作为 datetime 类型发送。 如果对时间列进行了加密, 则**sendTimeAsDatetime**属性必须为 false, 因为加密列不支持从时间转换为日期时间。 另请注意, 默认情况下, 此属性为 true, 因此, 在使用加密时间列时, 必须将其设置为 false。 否则, 驱动程序将引发异常。 从驱动程序版本6.0 开始, SQLServerConnection 类有两种方法可用于以编程方式配置此属性的值:
 
* public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)
* public boolean getSendTimeAsDatetime()

有关此属性的详细信息, 请参阅[配置如何将 .Java 值发送到服务器](configuring-how-java-sql-time-values-are-sent-to-the-server.md)。

### <a name="configuring-how-string-values-are-sent-to-the-server"></a>配置字符串值如何发送到服务器
**SendStringParametersAsUnicode**连接属性用于配置将字符串值发送到 SQL Server 的方式。 如果设置为 true，则 String 参数将以 Unicode 格式发送到服务器。 如果设置为 false, 则以非 Unicode 格式 (例如 ASCII 或 MBCS) 发送字符串参数, 而不是 Unicode。 此属性的默认值为 true。 如果启用了 Always Encrypted 并对 char/varchar/varchar (max) 列进行了加密, 则**sendStringParametersAsUnicode**的值必须设置为 false。 如果将此属性设置为 true, 则当解密包含 Unicode 字符的加密 char/varchar/varchar (max) 列中的数据时, 驱动程序将引发异常。 有关此属性的详细信息, 请参阅[设置连接属性](../../connect/jdbc/setting-the-connection-properties.md)。
  
## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>检索和修改加密列中的数据
为应用程序查询启用 Always Encrypted 后, 你可以使用标准 JDBC Api 来检索或修改加密数据库列中的数据。 如果你的应用程序具有所需的数据库权限, 并且可以访问列主密钥, 则驱动程序将对所有面向加密列的查询参数进行加密, 并对从加密列中检索到的数据进行解密。

如果未启用 Always Encrypted，具有面向加密列的参数的查询将失败。 只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。 但是，驱动程序不会尝试解密从加密列中检索到的任何值，并且应用程序将收到二进制加密数据（字节数组形式）。

下表概述了查询的行为，具体取决于是否启用了 Always Encrypted：

| 查询特征                                                                           | 启用了始终加密，并且应用程序可以访问密钥和密钥元数据                                                                                                                        | 启用了 Always Encrypted，但应用程序无法访问密钥或密钥元数据 | 禁用了始终加密                                                                                        |
| :--------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------ |
| 具有面向加密列的参数的查询。                                           | 以透明方式加密参数值。                                                                                                                                                           | 错误                                                                             | 错误                                                                                                               |
| 从加密列中检索数据且没有面向加密列的参数的查询。 | 以透明方式解密来自加密列的结果。 应用程序收到 JDBC 数据类型（对应于为加密列配置的 SQL Server 类型）的纯文本值。 | 错误                                                                             | 不解密来自加密列的结果。 应用程序收到字节数组形式的加密值 (byte[])。 |

### <a name="inserting-and-retrieving-encrypted-data-examples"></a>插入和检索加密数据示例

以下示例说明如何检索和修改加密列中的数据。 这些示例假定目标表具有以下架构和已加密的 SSN 和出生列。 如果已配置名为 "MyCMK" 的列主密钥和名为 "MyCEK" 的列加密密钥 (如前面的 "密钥存储提供程序" 部分中所述), 则可以使用以下脚本创建该表:

```sql
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = MyCEK) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = MyCEK) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

对于每个 Java 代码示例, 需要在所述位置插入密钥存储特定代码。

如果使用的是 Azure Key Vault 密钥存储提供程序:

```java
    String clientID = "<Azure Application ID>";
    String clientKey = "<Azure Application API Key Password>";
    SQLServerColumnEncryptionAzureKeyVaultProvider akvProvider = new SQLServerColumnEncryptionAzureKeyVaultProvider(clientID, clientKey);
    Map<String, SQLServerColumnEncryptionKeyStoreProvider> keyStoreMap = new HashMap<String, SQLServerColumnEncryptionKeyStoreProvider>();
    keyStoreMap.put(akvProvider.getName(), akvProvider);
    SQLServerConnection.registerColumnEncryptionKeyStoreProviders(keyStoreMap);
    String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;";
```

如果使用的是 Windows 证书存储密钥存储提供程序:

```java
    String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;";
```

如果使用的是 Java Key Store 密钥存储提供程序:

```java
    String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;columnEncryptionSetting=Enabled;keyStoreAuthentication=JavaKeyStorePassword;keyStoreLocation=<path to jks or pfx file>;keyStoreSecret=<keystore secret/password>";
```

### <a name="inserting-data-example"></a>插入数据示例

此示例向 Patients 表插入一行。 请注意以下各项：

- 对于示例代码中的加密，没有什么特定的注意事项。 Microsoft JDBC Driver for SQL Server 会自动检测并加密目标为加密列的参数。 这种行为使得加密操作对应用程序而言是透明的。
- 插入到数据库列中的值 (包括加密列) 使用 SQLServerPreparedStatement 作为参数进行传递。 在将值发送到非加密列时，使用参数是可选的（虽然强烈建议使用它，因为它有助于防止 SQL 注入），而在发送面向加密列的值时，它是必需的。 如果插入到加密列中的值作为嵌入在查询语句中的文本传递, 则查询将失败, 因为驱动程序无法确定目标加密列中的值, 也不会对值进行加密。 因此，服务器会因为与加密列不兼容而拒绝它们。
- 程序打印的所有值均为纯文本形式，因为 Microsoft JDBC Driver for SQL Server 将以透明方式解密从加密列中检索到的数据。
- 如果要使用 WHERE 子句执行查找, 则需要将 WHERE 子句中使用的值作为参数传递, 以便驱动程序可以在将其发送到数据库之前对其进行透明加密。 在下面的示例中, 将 SSN 作为参数传递, 但 LastName 将作为文本传递, 因为 LastName 未加密。
- 用于定向到 SSN 列的参数的 setter 方法为 setString (), 该方法映射到 char/varchar SQL Server 数据类型。 如果对于此参数，所用的 setter 方法是 setNString()，该方法可映射到 nchar/nvarchar，查询将失败，因为 Always Encrypted 不支持从加密的 nchar/nvarchar 值转换为加密的 char/varchar 值。

```java
// <Insert keystore-specific code here>
try (Connection sourceConnection = DriverManager.getConnection(connectionUrl);
        PreparedStatement insertStatement = sourceConnection.prepareStatement("INSERT INTO [dbo].[Patients] VALUES (?, ?, ?, ?)")) {
    insertStatement.setString(1, "795-73-9838");
    insertStatement.setString(2, "Catherine");
    insertStatement.setString(3, "Abel");
    insertStatement.setDate(4, Date.valueOf("1996-09-10"));
    insertStatement.executeUpdate();
    System.out.println("1 record inserted.\n");
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="retrieving-plaintext-data-example"></a>检索纯文本数据示例

以下示例演示如何根据加密值筛选数据，以及从加密列中检索纯文本数据。 请注意以下各项：

- WHERE 子句中用于筛选 SSN 列的值需要以参数形式进行传递，以便 Microsoft JDBC Driver for SQL Server 可以在将其发送到数据库之前以透明方式对其加密。
- 程序打印的所有值均为纯文本形式，因为 Microsoft JDBC Driver for SQL Server 将以透明方式解密从 SSN 和 BirthDate 列中检索到的数据。

> [!NOTE]
> 如果使用确定性加密加密列，查询可以对其执行相等比较。 有关详细信息，请参阅[在 Always Encrypted（数据库引擎）中选择确定性加密或随机加密](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption)。

```java
// <Insert keystore-specific code here>
try (Connection connection = DriverManager.getConnection(connectionUrl);
        PreparedStatement selectStatement = connection
                .prepareStatement("\"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN = ?;\"");) {
    selectStatement.setString(1, "795-73-9838");
    ResultSet rs = selectStatement.executeQuery();
    while (rs.next()) {
        System.out.println("SSN: " + rs.getString("SSN") + ", FirstName: " + rs.getString("FirstName") + ", LastName:"
                + rs.getString("LastName") + ", Date of Birth: " + rs.getString("BirthDate"));
    }
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="retrieving-encrypted-data-example"></a>检索加密数据示例

如果未启用 Always Encrypted，只要查询没有面向加密列的参数，就仍然可以从加密列中检索数据。

以下示例说明如何从加密列中检索二进制加密数据。 请注意以下各项：

- 由于未在连接字符串中启用 Always Encrypted，因此，查询将以字节数组的形式返回 SSN 和 BirthDate 的加密值（程序会将值转换为字符串）。
- 如果禁用始终加密，从加密列中检索数据的查询可以有参数，但前提是所有参数均不面向加密列。 以下查询按未在数据库中加密的 LastName 进行筛选。 如果查询按 SSN 或 BirthDate 进行筛选，则将失败。

```java
try (Connection sourceConnection = DriverManager.getConnection(connectionUrl);
        PreparedStatement selectStatement = sourceConnection
                .prepareStatement("SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE LastName = ?;");) {

    selectStatement.setString(1, "Abel");
    ResultSet rs = selectStatement.executeQuery();
    while (rs.next()) {
        System.out.println("SSN: " + rs.getString("SSN") + ", FirstName: " + rs.getString("FirstName") + ", LastName:"
                + rs.getString("LastName") + ", Date of Birth: " + rs.getString("BirthDate"));
    }
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>避免查询加密列时的常见问题

本节介绍从 Java 应用程序查询加密列时的常见错误类别，以及有关如何避免这些错误的若干指导。

### <a name="unsupported-data-type-conversion-errors"></a>不支持的数据类型转换错误

始终加密支持对加密数据类型进行若干种转换。 有关受支持类型转换的详细列表，请参阅 [Always Encrypted（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)。 下面介绍可以执行哪些操作来避免数据类型转换错误。 请确保：

- 传递面向加密列的参数的值时, 请使用正确的 setter 方法。 请确保参数的 SQL Server 数据类型与目标列的类型完全相同, 或者支持将参数的 SQL Server 数据类型转换为列的目标类型。 API 方法已添加到 SQLServerPreparedStatement、SQLServerCallableStatement 和 SQLServerResultSet 类, 用于传递与特定 SQL Server 数据类型对应的参数。 例如, 如果未对列进行加密, 则可以使用 setTimestamp () 方法将参数传递给 datetime2 或日期时间列。 但在加密列时, 必须使用表示数据库中列的类型的确切方法。 例如, 使用 setTimestamp () 将值传递给已加密的 datetime2 列, 并使用 setDateTime () 将值传递给加密的日期时间列。 有关新 Api 的完整列表, 请参阅[JDBC Driver ALWAYS ENCRYPTED API 参考](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)。
- 对于面向列的 decimal 和 numeric SQL Server 数据类型的参数，其精度和小数位数与为目标列配置的精度和小数位数相同。 已将 API 方法添加到 SQLServerPreparedStatement、SQLServerCallableStatement 和 SQLServerResultSet 类, 以接受精度和小数位数以及表示 decimal 和 numeric 数据类型的参数/列的数据值。 有关新的/重载 Api 的完整列表, 请参阅[JDBC Driver ALWAYS ENCRYPTED API 参考](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)。  
- 用于修改目标列的值的查询中的目标列的小数部分精度/小数位数不大于目标列的值为 datetime2、datetimeoffset 或 SQL Server time 的参数的秒的小数部分精度/小数位数. 已将 API 方法添加到 SQLServerPreparedStatement、SQLServerCallableStatement 和 SQLServerResultSet 类, 以接受秒的小数部分精度/小数位数以及表示这些数据类型的参数的数据值。 有关新的/重载 Api 的完整列表, 请参阅[JDBC Driver ALWAYS ENCRYPTED API 参考](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)。

### <a name="errors-due-to-incorrect-connection-properties"></a>由于连接属性不正确而导致的错误

本部分介绍如何正确配置连接设置以使用 Always Encrypted 数据。 因为加密数据类型支持有限转换, 所以在使用加密列时, **sendTimeAsDatetime**和**sendStringParametersAsUnicode**连接设置需要正确的配置。 请确保：

- 将数据插入到加密时间列时, [sendTimeAsDatetime](setting-the-connection-properties.md)连接设置设置为 false。 有关详细信息, 请参阅[配置如何将 .Java 值发送到服务器](configuring-how-java-sql-time-values-are-sent-to-the-server.md)。
- 将数据插入到加密 char/varchar/varchar (max) 列时, [sendStringParametersAsUnicode](setting-the-connection-properties.md)连接设置设置为 true (或保留为默认值)。

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>由于传递纯文本而非加密值而发生的错误

面向加密列的任何值都需要在应用程序内加密。 尝试插入/修改或者按纯文本值筛选加密列将导致如下错误：

```java
com.microsoft.sqlserver.jdbc.SQLServerException: Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'MyCEK', column_encryption_key_database_name = 'ae') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

为防止发生此类错误，请确保：

- 为面向加密列的应用程序查询（为连接字符串或特定查询）启用 Always Encrypted。
- 您可以使用预定义的语句和参数发送针对加密列的数据。 以下示例显示了一个查询，该查询按文本/常量对加密列 (SSN) 进行错误筛选，而不是以参数形式传递内部文本。 此查询将失败:

```java
ResultSet rs = connection.createStatement().executeQuery("SELECT * FROM Customers WHERE SSN='795-73-9838'");
```

## <a name="force-encryption-on-input-parameters"></a>对输入参数强制加密

强制加密功能在使用 Always Encrypted 时强制执行参数的加密。 如果使用强制加密并且 SQL Server 告知驱动程序参数不需加密，则使用该参数的查询会失败。 此属性提供针对安全攻击的额外保护，这些攻击涉及受损 SQL Server 向客户端提供不正确的加密元数据，这可能会导致数据泄漏。 SQLServerPreparedStatement 和 SQLServerCallableStatement 类中的 set * 方法和 SQLServerResultSet 类中\*的 update 方法将重载, 以接受布尔参数以指定强制加密设置。 如果此参数的值为 false, 则驱动程序将不会对参数强制进行加密。 如果将 "强制加密" 设置为 true, 则仅当目标列已加密, 并且在连接或语句上启用 Always Encrypted 时, 才会发送查询参数。 使用此属性将提供额外的安全层, 确保驱动程序不会错误地将数据作为纯文本发送到 SQL Server。

有关 SQLServerPreparedStatement 和 SQLServerCallableStatement 方法的详细信息, 这些方法是通过强制加密设置重载的, 请参阅[JDBC Driver ALWAYS ENCRYPTED API 参考](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)  

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>控制 Always Encrypted 对性能的影响

Always Encrypted 是一种客户端加密技术，因此，大部分性能开销发生在客户端，而不是数据库中。 除加密和解密操作的成本之外，客户端上的其他性能开销来源包括：

- 额外往返数据库以检索查询参数的元数据。
- 调用列主密钥存储以访问列主密钥。

本节介绍 Microsoft JDBC Driver for SQL Server 中的内置性能优化，以及如何控制上述两个因素对性能的影响。

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>控制为了检索查询参数的元数据而往返的次数

如果为连接启用了 Always Encrypted，默认情况下，提供程序将为每个参数化查询调用 [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) ，并将查询语句（不带任何参数值）传递到 SQL Server。 [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) 会分析查询语句，以了解是否有任何参数需要加密；如果有，则会针对每个需要加密的参数返回加密相关信息，以便驱动程序对参数值加密。 此行为可确保实现针对客户端应用程序的高级别透明性。 只要应用程序使用参数将针对加密列的值传递给驱动程序, 应用程序 (和应用程序开发人员) 不需要知道哪些查询访问加密列。

### <a name="setting-always-encrypted-at-the-query-level"></a>在查询级别设置始终加密

若要控制检索参数化查询的加密元数据时对性能的影响，可以为单个查询启用 Always Encrypted，而不是为连接设置 Always Encrypted。 这样一来，就可以确保仅针对你知道具有面向加密列的参数的查询调用 sys.sp_describe_parameter_encryption。 但请注意，这样做会降低加密的透明度：如果更改数据库列的加密属性，可能需要更改应用程序代码，使其与架构更改保持一致。

若要控制单个查询的 Always Encrypted 行为, 需要通过传递 SQLServerStatementColumnEncryptionSetting (指定在读取和写入数据时将发送和接收数据的方式) 来配置单个语句对象。该特定语句的加密列。 下面是一些有用的指导原则：

- 如果客户端应用程序通过数据库连接发送的大多数查询访问的是加密列，则可使用以下指南：

    - 将“columnEncryptionSetting”连接字符串关键字设置为“已启用”   。
    - 对于不访问任何加密列的单个查询，则设置 SQLServerStatementColumnEncryptionSetting.Disabled。 此设置将禁止调用 sys.sp_describe_parameter_encryption，同时禁止尝试对结果集中的任何值解密。
    - 对于其参数不需要加密但会从加密列检索数据的单个查询，则设置 SQLServerStatementColumnEncryptionSetting.ResultSet。 此设置将禁止调用 sys.sp_describe_parameter_encryption 和参数加密。 查询将能够解密来自加密列的结果。

- 如果客户端应用程序通过数据库连接发送的大多数查询不访问加密列，则可使用以下指南：

    - 将“columnEncryptionSetting”连接字符串关键字设置为“已禁用”   。
    - 对于有参数需要加密的单个查询，则设置 SQLServerStatementColumnEncryptionSetting.Enabled。 此设置将允许调用 sys.sp_describe_parameter_encryption，同时允许对从加密列中检索到的任何查询结果解密。
    - 对于其参数不需要加密但会从加密列检索数据的查询，则设置 SQLServerStatementColumnEncryptionSetting.ResultSet。 此设置将禁止调用 sys.sp_describe_parameter_encryption 和参数加密。 查询将能够解密来自加密列的结果。

SQLServerStatementColumnEncryptionSetting 设置不能用于绕过加密, 并获得对纯文本数据的访问权限。 有关如何在语句中配置列加密的详细信息, 请参阅[JDBC Driver ALWAYS ENCRYPTED API 参考](../../connect/jdbc/always-encrypted-api-reference-for-the-jdbc-driver.md)。  

在以下示例中，将对数据库连接禁用 Always Encrypted。 应用程序发出的查询有一个面向未加密的 LastName 列的参数。 该查询从已加密的 SSN 和 BirthDate 列中检索数据。 在这种情况下，不需要调用 sys.sp_describe_parameter_encryption 来检索加密元数据。 但是，需要启用查询结果解密，以便应用程序从两个加密列接收纯文本值。 SQLServerStatementColumnEncryptionSetting 设置用于确保。

```java
// Assumes the same table definition as in Section "Retrieving and modifying data in encrypted columns"
// where only SSN and BirthDate columns are encrypted in the database.
String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<database>;user=<user>;password=<password>;" 
        + "keyStoreAuthentication=JavaKeyStorePassword;"
        + "keyStoreLocation=<keyStoreLocation>" 
        + "keyStoreSecret=<keyStoreSecret>;";

String filterRecord = "SELECT FirstName, LastName, SSN, BirthDate FROM " + tableName + " WHERE LastName = ?";

try (SQLServerConnection connection = (SQLServerConnection) DriverManager.getConnection(connectionUrl);
        PreparedStatement selectStatement = connection.prepareStatement(filterRecord, ResultSet.TYPE_FORWARD_ONLY, ResultSet.CONCUR_READ_ONLY,
                connection.getHoldability(), SQLServerStatementColumnEncryptionSetting.ResultSetOnly);) {

    selectStatement.setString(1, "Abel");
    ResultSet rs = selectStatement.executeQuery();
    while (rs.next()) {
        System.out.println("First name: " + rs.getString("FirstName"));
        System.out.println("Last name: " + rs.getString("LastName"));
        System.out.println("SSN: " + rs.getString("SSN"));
        System.out.println("Date of Birth: " + rs.getDate("BirthDate"));
    }
}
// Handle any errors that may have occurred.
catch (SQLException e) {
    e.printStackTrace();
}
```

### <a name="column-encryption-key-caching"></a>列加密密钥缓存

为了减少对列加密密钥解密时调用列主密钥存储的次数，Microsoft JDBC Driver for SQL Server 会将纯文本列加密密钥缓存在内存中。 从数据库元数据收到加密列的加密密钥值之后，驱动程序首先会尝试查找与加密密钥值对应的纯文本列加密密钥。 仅当在缓存中找不到加密列的加密密钥值时，驱动程序才会调用包含列主密钥的密钥存储。

可以使用 SQLServerConnection 类中的 API setColumnEncryptionKeyCacheTtl () 为缓存中的列加密密钥条目配置生存时间值。 缓存中的列加密密钥条目的默认生存时间值为两小时。 若要禁用缓存, 请使用值0。 若要设置任何生存时间值, 请使用以下 API:

```java
SQLServerConnection.setColumnEncryptionKeyCacheTtl (int columnEncryptionKeyCacheTTL, TimeUnit unit)
```

例如, 若要将生存时间值设置为10分钟, 请使用:

```java
SQLServerConnection.setColumnEncryptionKeyCacheTtl (10, TimeUnit.MINUTES)
```

仅支持使用 DAYS、HOURS、MINUTES 或 SECONDS 作为时间单位。  

## <a name="copying-encrypted-data-using-sqlserverbulkcopy"></a>使用 SQLServerBulkCopy 复制加密数据

使用 SQLServerBulkCopy，可以将已加密并且存储在某个表中的数据复制到另一个表，而无需对数据解密。 为此：

- 请确保目标表的加密配置与源表的配置完全相同。 特别是，两个表必须对相同的列加密，并且必须使用相同的加密类型和相同的加密密钥对列加密。 如果任何目标列的加密方式与其相应的源列不同，你都不能在复制操作完成后对目标表中的数据进行解密。 数据将损坏。
- 配置数据库到源表和目标表的连接，而不启用 Always Encrypted。
- 设置 allowEncryptedValueModifications 选项。 有关详细信息, 请参阅对[JDBC 驱动程序使用大容量复制](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md)。

> [!NOTE]
> 指定 AllowEncryptedValueModifications 时需谨慎，因为该选项可能会导致损坏数据库，因为用于 Microsoft JDBC Driver for SQL Server 不会检查数据是否确实已加密，也不会检查是否使用与目标列相同的加密类型、算法和密钥对数据进行了正确加密。

## <a name="see-also"></a>另请参阅

[始终加密（数据库引擎）](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
