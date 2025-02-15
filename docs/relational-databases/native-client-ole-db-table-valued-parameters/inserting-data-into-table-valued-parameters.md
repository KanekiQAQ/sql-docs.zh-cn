---
title: 将数据插入到表值参数 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, inserting data into
ms.assetid: 9c1a3234-4675-40d3-b473-8df06208f880
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eb8e32b0008cba846679e7704a3a9ab9cef6e93d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68103441"
---
# <a name="inserting-data-into-table-valued-parameters"></a>向表值参数中插入数据
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口支持两种用于使用者指定表值参数行的数据模型： 推送模型和请求模型。 提供演示请求模型的示例；请参阅 [SQL Server 数据编程示例](https://msftdpprodsamples.codeplex.com/)。  
  
> [!NOTE]  
>  表值参数列要么必须在所有行中具有非默认值，要么必须在所有行中具有默认值。 不能在某些行中具有默认值，而在其他行中不具有默认值。 因此，在表值参数绑定中，表值参数行集列数据仅允许状态值 DBSTATUS_S_ISNULL 和 DBSTATUS_S_OK。 DBSTATUS_S_DEFAULT 将导致失败，而绑定的状态值将设置为 DBSTATUS_E_BADSTATUS。  
  
## <a name="push-model-loads-all-table-valued-paremeter-data-in-memory"></a>推送模型（在内存中加载所有表值参数数据）  
 推送模型类似于使用参数集（也即，ICommand::Execute 中的 DBPARAMS 参数）。 仅当在未对 IRowset 接口执行自定义实现的情况下使用表值参数行集对象时，才使用推送模型。 当表值参数行集中的行数较少且预计不会给应用程序带来过量内存压力时，建议使用推送模型。 这比请求模型更简单，因为它向使用者应用程序要求的功能不会超过典型 OLE DB 应用程序中当前常用的功能。  
  
 使用者应在执行命令之前向访问接口提供所有表值参数数据。 为提供数据，使用者为每个表值参数填充一个表值参数行集对象。 表值参数行集对象显示行集 Insert、Set 和 Delete 操作，使用者将使用这些操作来操作表值参数数据。 访问接口在执行时将从该表值参数行集对象中提取数据。  
  
 当向使用者提供表值参数行集对象时，使用者可以将其作为行集对象处理。 使用者可以使用 icolumnsinfo:: Getcolumninfo 或 icolumnsrowset:: Getcolumnsrowset 接口方法获取每个列 （类型、 最大长度、 精度和小数位数） 的类型信息。 然后，使用者创建取值函数，以便为数据指定绑定。 下一步是将数据行插入表值参数行集。 这可以通过使用 irowsetchange:: Insertrow。 Irowsetchange:: Setdata 或 IRowsetChange::DeleteRows 还可对表值参数行集对象如果您必须操作数据。 表值参数行集对象将进行引用计数，这与流对象类似。  
  
 如果使用 icolumnsrowset:: Getcolumnsrowset，则将对结果列对行集对象的 irowset:: Getnextrows、 irowset:: Getdata 和 irowset:: Releaserows 方法的后续调用。  
  
 之后[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 访问接口开始执行命令，将从表值参数行集对象中提取的表值参数值，并将其发送到服务器。  
  
 推送模型对于使用者的工作量要求最少，但占用的内存高于请求模型，原因在于在执行时所有表值参数数据都必须位于内存中。  
  
## <a name="pull-model-obtaining-table-valued-parameter-data-on-demand-from-the-consumer"></a>请求模型（从使用者按需获得表值参数数据）  
 请求模型用于两种应用场景：  
  
-   使行成为流数据。  
  
-   如果其他访问接口中的行集正用作表值参数值。  
  
 在请求模型中，使用者按需为访问接口提供数据。 如果应用程序具有许多数据插入操作，并且内存中的表值参数行集数据将导致过量的内存访问，则使用此方法。 如果使用多个 OLE DB 访问接口，则借助于使用者请求模型，使用者可以将任何行集对象作为表值参数值提供。  
  
 为了使用请求模型，使用者必须提供其自己的对于行集对象的实现。 使用表值参数行集 (CLSID_ROWSET_TVP) 拉取模型，使用者则需要聚合表值参数行集公开的对象，该提供程序通过 ITableDefinitionWithConstraints::CreateTableWithConstraints 方法或 iopenrowset:: Openrowset 方法。 使用者对象应只覆盖 IRowset 接口实现。 您必须覆盖以下函数：  
  
-   IRowset::GetNextRows  
  
-   IRowset::AddRefRows  
  
-   IRowset::GetData  
  
-   IRowset::ReleaseRows  
  
-   IRowset::RestartPosition  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口将一次从使用者行集对象中读取一行或多行，以便在表值参数中支持流行为。 例如，用户可能在磁盘上（而非内存中）具有表值参数行集数据，当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口要求时，可能实现从磁盘读取数据的功能。  
  
 使用者将对其数据格式传送[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB Provider iaccessor:: Createaccessor 使用表值参数行集对象。 当从使用者缓冲区读取数据时，访问接口将验证所有可写入和非默认列是否至少可用于一个取值函数句柄，并使用相应的句柄来读取列数据。 为了避免混乱，在表值参数行集列与绑定之间应具有一对一的对应关系。 与同一列的重复绑定将导致错误。 此外，每个访问器都必须具有*iOrdinal* DBBindings 的序列中的成员。 对于 IRowset::GetData 的调用数将与每行的取值函数数量相同，调用的顺序将基于 iOrdinal 值的顺序（从低值到高值）  。  
  
 访问接口应实现由表值参数行集对象显示的大多数接口。 使用者将使用最少的接口实现行集对象 (IRowset)。 由于盲聚合 (blind aggregation)，因此，表值参数行集对象将实现剩下的所必需的行集对象接口。  
  
 对于任何其他行集对象（如对于任何 OLE DB 访问接口获得的行集对象），使用者提供的行集必须按照 OLE DB 规范要求实现所有必需的行集对象接口。  
  
 在执行时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口将回调行集对象以提取行并读取列数据。  
  
## <a name="see-also"></a>请参阅  
 [表值参数 (OLE DB)](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [使用表值参数 (OLE DB)](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
