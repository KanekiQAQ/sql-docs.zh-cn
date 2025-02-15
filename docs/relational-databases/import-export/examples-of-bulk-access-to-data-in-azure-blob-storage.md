---
title: 批量访问 Azure Blob 存储中数据的示例 | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], from Azure blob storage
- Azure blob storage, bulk import to SQL Server
- BULK INSERT, Azure blob storage
- OPENROWSET, Azure blob storage
ms.assetid: f7d85db3-7a93-400e-87af-f56247319ecd
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2017||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7bbf0e39b5187f91495f36624ca133d02b87f764
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68035786"
---
# <a name="examples-of-bulk-access-to-data-in-azure-blob-storage"></a>批量访问 Azure Blob 存储中数据的示例
[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md)]

`BULK INSERT` 和 `OPENROWSET` 语句可以直接访问 Azure Blob 存储中的文件。 下面的示例使用 CSV（逗号分隔值）文件（名为 `inv-2017-01-19.csv`）中的数据，该文件存储在容器（名为 `Week3`）中，该容器存储在存储帐户（名为 `newinvoices`）中。 可以使用格式文件的路径，但这些示例不包含该路径。 

从 SQL Server 批量访问 Azure Blob 存储至少需要 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 版本 1.1。

> [!IMPORTANT]
>  blob 上容器和文件的所有路径都是 `CASE SENSITIVE`。 如果不正确，它可能会返回错误，如“无法进行大容量加载。 文件 "file.csv" 不存在或你没有文件访问权限。”
> "


## <a name="create-the-credential"></a>创建凭据   
   
下面的所有示例都需要一个引用共享访问签名的数据库范围凭据。   

> [!IMPORTANT]
>  必须借助一个使用 `SHARED ACCESS SIGNATURE` 标识的数据库范围凭据创建外部数据源。 若要为存储帐户创建共享访问签名，请查看 Azure 门户中存储帐户属性页上的“共享访问签名”属性。  有关共享访问签名的详细信息，请参阅[使用共享访问签名 (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1)。 有关凭据的详细信息，请参阅 [CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)。  
 
使用必须属于 `SHARED ACCESS SIGNATURE` 的 `IDENTITY` 创建数据库范围凭据。 使用 Azure 门户中的机密。 例如：  

```sql
CREATE DATABASE SCOPED CREDENTIAL UploadInvoices  
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
SECRET = 'QLYMgmSXMklt%2FI1U6DcVrQixnlU5Sgbtk1qDRakUBGs%3D';
```


## <a name="accessing-data-in-a-csv-file-referencing-an-azure-blob-storage-location"></a>访问引用 Azure Blob 存储位置的 CSV 文件中的数据   
下面的示例使用指向名为 `newinvoices` 的 Azure 存储帐户的外部数据源。   
```sql
CREATE EXTERNAL DATA SOURCE MyAzureInvoices
    WITH  (
        TYPE = BLOB_STORAGE,
        LOCATION = 'https://newinvoices.blob.core.windows.net', 
        CREDENTIAL = UploadInvoices  
    );
```   

然后，`OPENROWSET` 语句将容器名称 (`week3`) 添加到文件说明。 该文件命名为 `inv-2017-01-19.csv`。
```sql     
SELECT * FROM OPENROWSET(
   BULK  'week3/inv-2017-01-19.csv',
   DATA_SOURCE = 'MyAzureInvoices',
   SINGLE_CLOB) AS DataFile;
```

使用 `BULK INSERT` 时，请使用容器和文件说明：

```sql
BULK INSERT Colors2
FROM 'week3/inv-2017-01-19.csv'
WITH (DATA_SOURCE = 'MyAzureInvoices',
      FORMAT = 'CSV'); 
```

## <a name="accessing-data-in-a-csv-file-referencing-a-container-in-an-azure-blob-storage-location"></a>访问引用 Azure Blob 存储位置中某个容器的 CSV 文件中的数据   

下面的示例使用指向 Azure 存储帐户中某个容器（名为 `week3`）的外部数据源。   
```sql
CREATE EXTERNAL DATA SOURCE MyAzureInvoicesContainer
    WITH  (
        TYPE = BLOB_STORAGE,
        LOCATION = 'https://newinvoices.blob.core.windows.net/week3', 
        CREDENTIAL = UploadInvoices  
    );
```  
  
`OPENROWSET` 语句不会在文件说明中包含该容器名称：
```sql
SELECT * FROM OPENROWSET(
   BULK  'inv-2017-01-19.csv',
   DATA_SOURCE = 'MyAzureInvoicesContainer',
   SINGLE_CLOB) AS DataFile;
```   

使用 `BULK INSERT` 时，请不要在文件说明中使用该容器名称： 

```sql
BULK INSERT Colors2
FROM 'inv-2017-01-19.csv'
WITH (DATA_SOURCE = 'MyAzureInvoicesContainer',
      FORMAT = 'CSV'); 
```

## <a name="see-also"></a>另请参阅   

[CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)   
[CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md)   
[BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md)   
[OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)   

