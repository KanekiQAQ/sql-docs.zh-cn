---
title: 稀疏列支持 (OLE DB) |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 918574b3-c62e-4937-9e5f-37310dedc8f9
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fd4a9b05a0d650e5b518f695c2f9ca2c296568ef
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68031876"
---
# <a name="sparse-columns-support-ole-db"></a>稀疏列支持 (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  本主题提供有关 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 对稀疏列的支持的信息。 有关稀疏列的详细信息，请参阅[SQL Server Native Client 中的稀疏列支持](../../../relational-databases/native-client/features/sparse-columns-support-in-sql-server-native-client.md)。 例如，请参阅[显示稀疏列的列和目录元数据 &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-how-to/display-column-and-catalog-metadata-for-sparse-columns-ole-db.md)。  
  
## <a name="ole-db-statement-metadata"></a>OLE DB 语句元数据  
 从 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 开始，可以使用新的 DBCOLUMNFLAGS 标记值 DBCOLUMNFLAGS_SS_ISCOLUMNSET。 应当为是 column_set 值的列设置该值  。 可以通过检索 DBCOLUMNFLAGS 标记*dwFlags* icolumnsinfo:: Getcolumnsinfo 和 icolumnsrowset:: Getcolumnsrowset 返回的行集的 DBCOLUMN_FLAGS 列的参数。  
  
## <a name="ole-db-catalog-metadata"></a>OLE DB 目录元数据  
 其他两个特定于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的列已添加到 DBSCHEMA_COLUMNS 中。  
  
|列名|数据类型|值/注释|  
|-----------------|---------------|---------------------|  
|SS_IS_SPARSE|DBTYPE_BOOL|如果该列是稀疏列，它将具有值 VARIANT_TRUE；否则，为 VARIANT_FALSE。|  
|SS_IS_COLUMN_SET|DBTYPE_BOOL|如果该列是稀疏 column_set 列，则它具有值 VARIANT_TRUE；否则，为 VARIANT_FALSE  。|  
  
 还添加了其他两个架构行集。 这些行集具有与 DBSCHEMA_COLUMNS 相同的结构，但返回不同内容。 不管 column_set 成员身份是什么，DBSCHEMA_COLUMNS_EXTENDED 都将返回所有列  。 DBSCHEMA_SPARSE_COLUMN_SET 仅返回属于稀疏 column_set 的成员的列  。  
  
## <a name="ole-db-datatypecompatibility-behavior"></a>OLE DB DataTypeCompatibility 行为  
 与行为**DataTypeCompatibility = 80** （在连接字符串中） 是符合[!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)]客户端，按如下所示：  
  
-   新架构行集是不可见的，并且在架构行集中没有它们的行。  
  
-   COLUMNS 行集中的新列是不可见的。  
  
-   不为 column_set 列设置 DBCOLUMNFLAGS_SS_ISCOLUMNSET  。  
  
-   为 column_set 列设置 DBCOMPUTEMODE_NOTCOMPUTED  。  
  
## <a name="ole-db-support-for-sparse-columns"></a>OLE DB 对稀疏列的支持  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 中修改了以下 OLE DB 接口以支持稀疏列：  
  
|类型或成员函数|描述|  
|-----------------------------|-----------------|  
|IColumnsInfo::GetColumnsInfo|为 dwFlags 中的 column_set 列设置了新的 DBCOLUMNFLAGS 标记值 DBCOLUMNFLAGS_SS_ISCOLUMNSET   。<br /><br /> 为 column_set 列设置了 DBCOLUMNFLAGS_WRITE  。|  
|IColumsRowset::GetColumnsRowset|为 DBCOLUMN_FLAGS 中的 column_set 列设置了新的 DBCOLUMNFLAGS 标记值 DBCOLUMNFLAGS_SS_ISCOLUMNSET  。<br /><br /> 对于 column_set 列，将 DBCOLUMN_COMPUTEMODE 设置为 DBCOMPUTEMODE_DYNAMIC  。|  
|IDBSchemaRowset::GetSchemaRowset|DBSCHEMA_COLUMNS 返回两个新列：SS_IS_COLUMN_SET 和 SS_IS_SPARSE。<br /><br /> DBSCHEMA_COLUMNS 仅返回不属于 column_set 成员的列  。<br /><br /> 已添加两个新的架构行集：DBSCHEMA_COLUMNS_EXTENDED 都将返回所有列的稀疏性无论**column_set**成员身份。 DBSCHEMA_SPARSE_COLUMN_SET 仅返回属于 column_set 成员的列  。 这些新行集具有与 DBSCHEMA_COLUMNS 相同的列和限制。|  
|IDBSchemaRowset::GetSchemas|IDBSchemaRowset::GetSchemas 将新行集 DBSCHEMA_COLUMNS_EXTENDED 和 DBSCHEMA_SPARSE_COLUMN_SET 的 GUID 包括在可用架构行集的列表中。|  
|ICommand::Execute|如果使用从表中选择 \*，它将返回不属于稀疏 column_set 成员的所有列，外加一个 XML 列（如果有），其中包含属于稀疏 column_set 成员的所有非 NULL 列的值     。|  
|IOpenRowset::OpenRowset|Iopenrowset:: Openrowset 使用 icommand:: Execute，与相同的列的行集返回与**选择\*** 同一个表的查询。|  
|ITableDefinition|对于稀疏列或 column_set 列，该接口没有更改  。 必须进行架构修改的应用程序必须直接执行正确的 [!INCLUDE[tsql](../../../includes/tsql-md.md)]。|  
  
## <a name="see-also"></a>请参阅  
 [SQL Server Native Client (OLE DB)](../../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
  
