---
title: 表值参数 (OLE DB) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, table-valued parameters
- table-valued parameters (OLE DB)
ms.assetid: 4298b73d-615b-4d28-9843-03b4d5fc489e
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4292022496d19ed80ff1cde71d5fbd4a35a68c9e
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68069613"
---
# <a name="table-valued-parameters-ole-db"></a>表值参数 (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  本节介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口中的表值参数支持。 有关其他概述信息，请参阅[表值参数&#40;SQL Server Native Client&#41;](../../relational-databases/native-client/features/table-valued-parameters-sql-server-native-client.md)。 有关示例，请参阅[使用表值参数&#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)。  
  
## <a name="remarks"></a>备注  
 现在，可向服务器发送多行数据作为带参数集的过程的参数（如 ICommand::Execute 中的 DBPARAMS 参数）  。 使用参数集时，该参数集中的每个元素都必须通过单独的远程过程调用 (RPC) 请求发送到服务器。 表值参数提供类似的功能，但可以与服务器更好地集成。 这可以减少 RPC 请求数，并在服务器上启用基于集的操作。  
  
 中支持表值参数[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 访问接口作为 OLE DB**行集**对象。 任何**行集**使用者可以提供对象 (即，客户端应用程序使用[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 访问接口) 作为表值参数的参数的占位符。 表值参数的处理方式与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 参数类型相似。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口提供创建、发现、规范、绑定和架构接口。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [创建表值参数行集](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameter-rowset-creation.md)  
  
-   [表值参数类型发现](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameter-type-discovery.md)  
  
-   [执行包含表值参数的命令](../../relational-databases/native-client-ole-db-table-valued-parameters/executing-commands-containing-table-valued-parameters.md)  
  
-   [向表值参数中插入数据](../../relational-databases/native-client-ole-db-table-valued-parameters/inserting-data-into-table-valued-parameters.md)  
  
-   [为 OLE DB 表值参数更改的架构行集](../../relational-databases/native-client-ole-db-table-valued-parameters/schema-rowsets-changed-for-ole-db-table-valued-parameters.md)  
  
-   [OLE DB 表值参数类型支持](../../relational-databases/native-client-ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support.md)  
  
-   [OLE DB 表值参数类型支持（方法）](../../relational-databases/native-client-ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-methods.md)  
  
-   [OLE DB 表值参数类型支持（属性）](../../relational-databases/native-client-ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-properties.md)  
  
## <a name="see-also"></a>请参阅  
 [SQL Server 本机客户端&#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)   
 [使用表值参数 (OLE DB)](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
