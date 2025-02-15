---
title: 执行异步操作 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- initialization [SQL Server Native Client]
- database connections [SQL Server Native Client]
- data access [SQL Server Native Client], asynchronous operations
- connections [SQL Server Native Client]
- asynchronous operations [SQL Server Native Client]
- rowsets [SQL Server], initializing
- SQLNCLI, asynchronous operations
- SQL Server Native Client, asynchronous operations
ms.assetid: 8fbd84b4-69cb-4708-9f0f-bbdf69029bcc
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9702fd9ece40171b528fb7a9e5fb0e0f2a67d16a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68016649"
---
# <a name="performing-asynchronous-operations"></a>执行异步操作
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 允许应用程序执行异步数据库操作。 异步处理将使方法能够立即返回，而不会阻塞调用线程。 这使得多线程机制能提供更强大的功能和灵活性，而不需要开发人员显式创建线程或处理同步。 应用程序将在初始化数据库连接时或初始化由执行命令所生成的结果时请求异步处理。  
  
## <a name="opening-and-closing-a-database-connection"></a>打开和关闭数据库连接  
 使用时[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序，用于以异步方式初始化数据源对象的应用程序可以设置 DBPROP_INIT_ASYNCH 属性，然后调用中的 DBPROPVAL_ASYNCH_INITIALIZE 位**Idbinitialize:: Initialize**。 设置此数据时，提供程序立即通过对 Initialize 的调用返回值。如果操作立即完成，则返回 S_OK；如果初始化以异步方式继续进行，则返回 DB_S_ASYNCHRONOUS  。 应用程序可以查询**IDBAsynchStatus**或[ISSAsynchStatus](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)接口的数据源对象，然后再调用**IDBAsynchStatus::GetStatus**或[Issasynchstatus:: Waitforasynchcompletion](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md)若要获取的初始化状态。  
  
 此外，SSPROP_ISSAsynchStatus 属性已添加到 DBPROPSET_SQLSERVERROWSET 属性集。 支持 **ISSAsynchStatus** 接口的提供程序必须使用值 VARIANT_TRUE 实现此属性。  
  
 可调用 IDBAsynchStatus::Abort 或 [ISSAsynchStatus::Abort](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-abort-ole-db.md) 来取消 Initialize 异步调用   。 使用者必须显式请求异步数据源初始化。 否则，必须等到数据源对象完全初始化之后，IDBInitialize::Initialize 才返回值  。  
  
> [!NOTE]  
>  用于连接池的数据源对象不能调用**ISSAsynchStatus**接口中[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序。 ISSAsynchStatus 接口不对入池数据源对象公开  。  
>   
>  如果应用程序显式强制使用游标引擎，则 IOpenRowset::OpenRowset 和 IMultipleResults::GetResult 不支持异步处理   。  
>   
>  此外，远程处理代理/存根 dll （在 MDAC 2.8) 不能调用**ISSAsynchStatus**接口中[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]本机客户端。 ISSAsynchStatus 接口不通过远程公开  。  
>   
>  服务组件不支持 ISSAsynchStatus  。  
  
## <a name="execution-and-rowset-initialization"></a>执行和行集初始化  
 被设计成以异步方式打开由执行命令所生成的结果的应用程序可以设置 DBPROP_ROWSET_ASYNCH 属性中的 DBPROPVAL_ASYNCH_INITIALIZE 位。 在调用 IDBInitialize::Initialize、ICommand::Execute、IOpenRowset::OpenRowset 或 IMultipleResults::GetResult 之前设置该位时，必须将 riid 参数设置为 IID_IDBAsynchStatus、IID_ISSAsynchStatus 或 IID_IUnknown      。  
  
 该方法立即返回值，且 ppRowset 设置为行集上的请求接口。如果行集初始化立即完成，则返回 S_OK；如果行集继续异步初始化，则返回 DB_S_ASYNCHRONOUS  。 有关[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序，此接口只能**IDBAsynchStatus**或**ISSAsynchStatus**。 在行集完全初始化之前，该接口的行为如同处于挂起状态一样，而且对 IID_IDBAsynchStatus 或 IID_ISSAsynchStatus 以外的接口调用 QueryInterface 可能返回 E_NOINTERFACE    。 除非使用者显式请求异步处理，否则行集将以同步方式执行初始化。 如果 IDBAsynchStaus::GetStatus 或 ISSAsynchStatus::WaitForAsynchCompletion 返回的内容指示异步操作已完成，则请求的所有接口均可用   。 这不一定意味着行集已完全填充，但它是完整的，且功能齐全。  
  
 即使执行的命令未返回行集，它仍会立即返回支持 IDBAsynchStatus 的对象  。  
  
 如果需要获得由异步命令执行所产生的多个结果，则应当：  
  
-   在执行命令之前，设置 DBPROP_ROWSET_ASYNCH 属性的 DBPROPVAL_ASYNCH_INITIALIZE 位。  
  
-   调用 ICommand::Execute，并请求 IMultipleResults   。  
  
 然后，可通过 QueryInterface 查询多个结果接口，从而获得 IDBAsynchStatus 和 ISSAsynchStatus 接口    。  
  
 该命令完成后执行时， **IMultipleResults**可用作正常，并且从同步的情况下的一个例外：可能返回 DB_S_ASYNCHRONOUS，在这种情况下**IDBAsynchStatus**或**ISSAsynchStatus**可用于确定何时完成该操作。  
  
## <a name="examples"></a>示例  
 在以下示例中，应用程序调用非阻止方法，执行一些其他处理，然后返回以处理结果。 ISSAsynchStatus::WaitForAsynchCompletion 等待内部事件对象，直到异步执行操作完成，或超过了 dwMilisecTimeOut 指定的时间   。  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
  
DBPROPSET CmdPropset[1];  
DBPROP CmdProperties[1];  
  
CmdPropset[0].rgProperties = CmdProperties;  
CmdPropset[0].cProperties = 1;  
CmdPropset[0].guidPropertySet = DBPROPSET_ROWSET;  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
CmdProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
  
hr = pICommandProps->SetProperties(1, CmdPropset);  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if ( hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 ISSAsynchStatus::WaitForAsynchCompletion 等待内部事件对象，直到异步执行操作完成，或传递了 dwMilisecTimeOut 值   。  
  
 以下示例显示有多个结果集的异步处理：  
  
```  
DBPROP CmdProperties[1];  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_IMultipleResults,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pIMultipleResults);  
  
// Use GetResults for ISSAsynchStatus.  
hr = pIMultipleResults->GetResult(IID_ISSAsynchStatus, (void **) &pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if (hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 若要阻止阻塞，客户端可以检查异步操作的运行状态，如以下示例所示：  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);   
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   do{  
      // Do some work...  
      hr = pISSAsynchStatus->GetStatus(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN, NULL, NULL, &ulAsynchPhase, NULL);  
   }while (DBASYNCHPHASE_COMPLETE != ulAsynchPhase)  
   if SUCCEEDED(hr)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
   }  
   pIDBAsynchStatus->Release();  
}  
```  
  
 以下示例演示如何才能取消当前正在运行的异步操作：  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work...  
   hr = pISSAsynchStatus->Abort(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN);  
}  
```  
  
## <a name="see-also"></a>请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [行集属性和行为](../../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
