---
title: CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/28/2018
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DATABASE SCOPED CREDENTIAL
- DATABASE_SCOPED_CREDENTIAL_TSQL
- SCOPED_TSQL
- CREATE_DATABASE_SCOPED_CREDENTIAL
- CREATE_DATABASE_SCOPED_CREDENTIAL_TSQL
- SCOPED_CREDENTIAL_TSQL
- SCOPED_CREDENTIAL
helpviewer_keywords:
- DATABASE SCOPED CREDENTIAL statement
- credentials [SQL Server], DATABASE SCOPED CREDENTIAL statement
ms.assetid: fe830577-11ca-44e5-953b-2d589d54d045
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=aps-pdw-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1c5bd7fef54ee28993472bd6b3f4e4df3bba739f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68060985"
---
# <a name="create-database-scoped-credential-transact-sql"></a>CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  创建数据库凭据。 数据库凭据不会映射到服务器登录或数据库用户。 只要数据库在执行需要访问权限的操作，数据库就可使用凭据访问外部位置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
 
CREATE DATABASE SCOPED CREDENTIAL credential_name   
WITH IDENTITY = 'identity_name'  
    [ , SECRET = 'secret' ]  
  
```  
  
## <a name="arguments"></a>参数  
 credential_name   
 指定创建的数据库范围凭据的名称。 credential_name 不能以数字符号 (#) 开头  。 系统凭据以 ## 开头。  
  
 IDENTITY **='** _identity\_name_ **'**  
 指定从服务器外部进行连接时要使用的帐户名称。 要使用共享密钥从 Azure Blob 存储导入文件，标识名称必须是 `SHARED ACCESS SIGNATURE`。 若要将数据加载到 SQL DW，任何有效的值均可用于标识。 有关共享访问签名的详细信息，请参阅[使用共享访问签名 (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1)。  
  
 SECRET = 'secret'     
 指定发送身份验证所需的机密内容。 需要 `SECRET` 才可从 Azure Blob 存储导入文件。 若要从 Azure Blob 存储加载到 SQL DW 或并行数据仓库，Secret 必须是 Azure 存储密钥。  
> [!WARNING]
>  SAS 密钥值可以“?”（问号）开头。 使用 SAS 密钥时，必须删除前导“?”。 否则会阻止操作。  
  
## <a name="remarks"></a>Remarks  
 数据库范围凭据是一个记录，其中包含连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以外的资源所需的身份验证信息。 多数凭据包括一个 Windows 用户和一个密码。  
  
 创建数据库范围凭据之前，数据库必须具有主密钥用于保护凭据。 有关详细信息，请参阅 [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md)。  
  
 当 IDENTITY 为 Windows 用户时，机密内容可以是密码。 机密内容使用服务主密钥进行加密。 如果重新生成服务主密钥，则使用新的服务主密钥重新加密机密内容。  
   
 可在 [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md) 目录视图中查看有关数据库范围凭据的信息。  
  
 
 以下是数据库范围凭据的一些应用：  
  
- [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 使用数据库范围凭据访问非公共 Azure blob 存储或 Kerberos 保护的使用 PolyBase 的 Hadoop 群集。 有关详细信息，请参阅 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)。  

- [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] 使用数据库范围凭据访问使用 PolyBase 的非公共 Azure blob 存储。 有关详细信息，请参阅 [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)。
  
- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 将数据库范围凭据用于其全局查询功能。 这是跨多个数据库分片查询的能力。  
  
- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 使用数据库范围凭据将扩展事件文件写入 Azure blob 存储。  
  
- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 将数据库范围凭据用于弹性池。 有关详细信息，请参阅[使用弹性数据库控制爆炸式增长](https://azure.microsoft.com/documentation/articles/sql-database-elastic-pool/)  

- [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 使用数据库范围凭据访问 Azure blob 存储的数据。 有关详细信息，请参阅[批量访问 Azure Blob 存储中数据的示例](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md)。 
  
## <a name="permissions"></a>权限  
 需要针对数据库的 CONTROL 权限  。  
  
## <a name="examples"></a>示例  
### <a name="a-creating-a-database-scoped-credential-for-your-application"></a>A. 为应用程序创建数据库范围凭据。
 以下示例创建名为 `AppCred` 的数据库范围凭据。 数据库范围凭据包含 Windows 用户 `Mary5` 和一个密码。  
  
```sql  
-- Create a db master key if one does not already exist, using your own password.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';  
  
-- Create a database scoped credential.  
CREATE DATABASE SCOPED CREDENTIAL AppCred WITH IDENTITY = 'Mary5',   
    SECRET = '<EnterStrongPasswordHere>';  
GO  
```  

### <a name="b-creating-a-database-scoped-credential-for-a-shared-access-signature"></a>B. 为共享访问签名创建数据库范围凭据。   
以下示例创建的数据库范围凭据可用于创建可执行批量操作（例如 [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) 和 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)）的[外部数据源](../../t-sql/statements/create-external-data-source-transact-sql.md)。 共享访问签名不能与 SQL Server、APS 或 SQL DW 中的 PolyBase一起使用。
```sql
CREATE DATABASE SCOPED CREDENTIAL MyCredentials  
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
SECRET = 'QLYMgmSXMklt%2FI1U6DcVrQixnlU5Sgbtk1qDRakUBGs%3D';
```
  
### <a name="c-creating-a-database-scoped-credential-for-polybase-connectivity-to-azure-data-lake-store"></a>C. 为到 Azure Data Lake Store 的 PolyBase 连接创建数据库范围凭据。  
以下示例创建的数据库范围凭据可用于创建可以由 SQL 数据仓库中的 PolyBase 使用的[外部数据源](../../t-sql/statements/create-external-data-source-transact-sql.md)。

Azure Data Lake Store 使用 Azure Active Directory 应用程序进行服务到服务身份验证。
请先[创建 AAD 应用程序](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)，并记录 client_id、OAuth_2.0_Token_EndPoint 和密钥，然后再尝试创建数据库范围凭据。

```sql
CREATE DATABASE SCOPED CREDENTIAL ADL_User
WITH
    IDENTITY = '<client_id>@\<OAuth_2.0_Token_EndPoint>'
    SECRET = '<key>'
;
```  
  
  
  
## <a name="more-information"></a>详细信息  
 [凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [ALTER DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/alter-database-scoped-credential-transact-sql.md)   
 [DROP DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/drop-database-scoped-credential-transact-sql.md)   
 [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sys.credentials (Transact-SQL)](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)  
  
  
