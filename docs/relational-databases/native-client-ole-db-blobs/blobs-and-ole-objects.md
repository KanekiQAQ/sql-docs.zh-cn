---
title: Blob 和 OLE 对象 |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- BLOBs, OLE objects
- BLOBs
- storage object [OLE DB]
- SQL Server Native Client OLE DB provider, BLOBs
- large data, OLE objects
ms.assetid: 767fa2f6-9cd2-436f-add5-e760bed29a58
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d6f2a73f44d762a2d17eccc8cf82570b07933426
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68128893"
---
# <a name="blobs-and-ole-objects"></a>BLOB 和 OLE 对象
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口公开**ISequentialStream**接口以支持使用者访问[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **ntext**，**文本**，**图像**， **varchar （max)** ， **nvarchar （max)** ， **varbinary （max)** ，和 xml 数据类型作为二进制大型对象 (Blob). 通过对 ISequentialStream 执行 Read 方法，使用者可以用便于管理的方式成块检索大量数据   。  
  
 有关演示此功能的示例，请参阅[大型数据集&#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/set-large-data-ole-db.md)。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序可以使用使用者实现**IStorage**接口时使用者提供的接口指针的取值函数中绑定的数据修改。  
  
 对于大值数据类型[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 访问接口会检查的中的类型大小假设**IRowset**和 DDL 接口。 包含的列**varchar**， **nvarchar**，并**varbinary**最大大小设置为不受限制的数据类型将通过架构行集和接口表示为 ISLONG返回列数据类型。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口公开**varchar （max)** ， **varbinary （max)** 并**nvarchar （max)** DBTYPE_STR、 DBTYPE_BYTES 和 DBTYPE_ 类型WSTR 分别。  
  
 若要使用这些类型应用程序具有以下选项：  
  
-   绑定为字节（DBTYPE_STR、DBTYPE_BYTES 和 DBTYPE_WSTR）。 如果缓冲区不够大，将发生截断，这与早期版本中的这些类型完全相同（但是现在可以使用更大的值）。  
  
-   绑定为字节并指定 DBTYPE_BYREF。  
  
-   绑定为 DBTYPE_IUNKNOWN 并使用流处理。  
  
 如果绑定为 DBTYPE_IUNKNOWN，则使用 ISequentialStream 流功能。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序支持输出参数绑定为 DBTYPE_IUNKNOWN 对于大值数据类型，以利于以下方案，其中的存储的过程返回这些数据类型为返回值，并将公开作为 DBTYPE_IUNKNOWN向客户端。  
  
## <a name="storage-object-limitations"></a>存储对象限制  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口可以支持只有一个打开的存储对象。 尝试打开多个存储对象（以获取对多个 ISequentialStream 接口指针的引用）返回 DBSTATUS_E_CANTCREATE  。  
  
-   在[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序，DBPROP_BLOCKINGSTORAGEOBJECTS 只读属性的默认值为 VARIANT_TRUE。 这指示如果存储对象处于活动状态，某些方法（存储对象上的方法除外）将失败，并返回 E_UNEXPECTED。  
  
-   由使用者实现的存储对象提供数据的长度必须进行到已知[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序时创建引用存储对象的行取值函数。 使用者必须在用于创建取值函数的 DBBINDING 结构中绑定一个长度指示符。  
  
-   如果行包含多个大型数据值，且 DBPROP_ACCESSORDER 不为 DBPROPVAL_AO_RANDOM，使用者必须使用[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 访问接口游标支持行集来检索行数据或处理所有大型数据值之前正在检索其他行值。 如果 DBPROP_ACCESSORDER 为 DBPROPVAL_AO_RANDOM， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序将所有 xml 数据类型都缓存为二进制大型对象 (Blob)，以便它可以按任意顺序访问。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [获取大型数据](../../relational-databases/native-client-ole-db-blobs/getting-large-data.md)  
  
-   [设置大型数据](../../relational-databases/native-client-ole-db-blobs/setting-large-data.md)  
  
-   [BLOB 输出参数的流支持](../../relational-databases/native-client-ole-db-blobs/streaming-support-for-blob-output-parameters.md)  
  
## <a name="see-also"></a>请参阅  
 [SQL Server 本机客户端&#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)   
 [使用大值类型](../../relational-databases/native-client/features/using-large-value-types.md)  
  
  
