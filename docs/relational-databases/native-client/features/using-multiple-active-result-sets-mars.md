---
title: 使用多个活动结果集 (MARS) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, MARS
- SQLNCLI, MARS
- data access [SQL Server Native Client], MARS
- Multiple Active Result Sets
- SQL Server Native Client, MARS
- MARS [SQL Server]
- SQL Server Native Client ODBC driver, MARS
ms.assetid: ecfd9c6b-7d29-41d8-af2e-89d7fb9a1d83
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1248add51d33b46978230ffcb0467ec6ed25e976
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68076634"
---
# <a name="using-multiple-active-result-sets-mars"></a>使用多个活动的结果集 (MARS)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 在访问 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的应用程序中引入了对多重活动结果集 (MARS) 的支持。 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的早期版本中，数据库应用程序无法在单个连接上保持多个活动语句。 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 默认结果集时，应用程序必须先处理或取消从某一批处理生成的所有结果集，然后才能对该连接执行任何其他批处理。 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 引入了新的连接属性，该属性允许应用程序在每个连接上使用多个待定请求，具体而言，每个连接可以具有多个活动的默认结果集。  
  
 MARS 通过以下新功能简化了应用程序设计：  
  
-   应用程序可以同时打开多个默认结果集，并且交错读取它们。  
  
-   应用程序可以在默认结果集打开的同时执行其他语句（例如 INSERT、UPDATE、DELETE 和存储过程调用）。  
  
 下列指南对使用 MARS 的应用程序很有帮助：  
  
-   默认结果集应该用于使用单个 SQL 语句（SELECT、带 OUTPUT 的 DML、RECEIVE、READ TEXT 等）生成的短期或较小结果集。  
  
-   服务器游标应该用于使用单个 SQL 语句生成的长期或较大结果集。  
  
-   对于过程请求（不论它们是否返回结果）以及返回多个结果的批处理，应始终读取到它们的结果的末尾。  
  
-   尽可能使用 API 调用（而不是 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句）更改连接属性和管理事务。  
  
-   在 MARS 中，有多个批处理并发运行时禁止会话范围内的模拟。  
  
> [!NOTE]  
>  默认情况下，不启用 MARS 功能。 若要在使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 连接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时使用 MARS，必须在连接字符串内专门启用 MARS。 有关详细信息，请参阅本主题下文中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序等章节。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 不限制某个连接上的活动语句的数量。  
  
 不需要同时执行多个多语句批处理或存储过程的典型应用程序将受益于 MARS，且无需了解如何实现 MARS。 不过，具有较复杂要求的应用程序确实需要考虑到这一点。  
  
 MARS 支持在单一连接中交错执行多个请求。 即：它允许运行批处理，并且在执行过程中还允许执行其他请求。 不过请注意，MARS 是从交错执行而不是从并行执行的角度定义的。  
  
 MARS 基础结构允许以交错方式执行多个批处理，尽管只能在定义完善的时间点切换执行。 此外，多数语句必须在同一批处理内以原子方式运行。 语句的行返回到客户端，有时称为*收获点*，可以交错执行完成之前，虽然行发送到客户端，例如：  
  
-   SELECT  
  
-   FETCH  
  
-   RECEIVE  
  
 作为存储过程或批处理的一部分执行的任何其他语句必须运行完毕，之后才能切换到执行其他 MARS 请求。  
  
 多个批处理交错执行的确切方式受若干因素影响，很难预测包含收获点的多个批处理中的命令的确切执行顺序。 注意避免交错执行此类复杂批处理所产生的意外负面影响。  
  
 要避免这些问题，可使用 API 调用而不是 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句来管理连接状态（SET、USE）和事务（BEGIN TRAN、COMMIT、ROLLBACK），方法是不在同样包含收获点的多语句批处理中包括这些语句，以及通过使用或取消所有结果来顺序执行此类批处理。  
  
> [!NOTE]  
>  启用 MARS 时，启动手动或隐式事务的批处理或存储过程必须完成该事务，之后批处理才能退出。 如果不是这样，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将在批处理完成时回滚该事务所做的所有更改。 这种事务由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 作为批范围的事务管理。 这是 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 新引入的事务类型，用于在启用 MARS 时支持使用功能良好的现有存储过程。 有关批范围的事务的详细信息，请参阅[Transaction 语句&#40;TRANSACT-SQL&#41;](~/t-sql/statements/statements.md)。  
  
 有关从 ADO 使用 MARS 的示例，请参阅[使用 SQL Server Native Client 使用 ADO](../../../relational-databases/native-client/applications/using-ado-with-sql-server-native-client.md)。  
  
## <a name="in-memory-oltp"></a>内存中 OLTP  
 内存中 OLTP 支持 MARS 使用查询和本机编译存储的过程。 MARS 支持从多个查询，而无需完全检索每个结果集发送请求以从新的结果集中提取行之前的请求数据。 若要成功读取从多个打开的结果集必须使用 MARS 启用连接。  
  
 默认情况下禁用 MARS，因此你必须通过添加显式启用`MultipleActiveResultSets=True`到连接字符串。 下面的示例演示如何连接到 SQL Server 的实例并指定启用 MARS:  
  
```  
Data Source=MSSQL; Initial Catalog=AdventureWorks; Integrated Security=SSPI; MultipleActiveResultSets=True  
```  
  
 MARS 使用内存中 OLTP 是实质上是 MARS 相同 SQL 引擎的其余部分。 下面列出了差异，使用 MARS 中内存优化表和本机编译存储的过程时。  
  
 **火星人与内存优化表**  
  
 基于磁盘和内存优化表时使用 MARS 启用连接之间的差异如下：  
  
-   两个语句可以修改相同的目标对象中的数据，但如果它们都试图修改同一记录写 / 写冲突将导致新的操作失败。 但是，如果这两种操作修改不同记录，操作会成功。  
  
-   每个语句在快照隔离下运行，因此新的操作，无法看到由现有语句所做的更改。 即使在同一事务下执行的并发语句的 SQL 引擎创建批处理级事务的每个语句是相互隔离的。 但是，批处理级事务仍然绑定在一起以便回滚的一个批处理级事务会影响同一个批处理中的其他的。  
  
-   在用户事务中不允许 DDL 操作，因此，它们将立即失败。  
  
 **MARS 和本机编译的存储过程**  
  
 本机编译存储的过程可以启用 MARS 的连接中运行，并且仅当遇到 yield 点时，才可以产生执行转移到另一个语句。 将 yield 点要求 SELECT 语句，这是本机编译的存储过程可以生成执行转移到另一个语句中的唯一语句。 如果 SELECT 语句不存在则不会生成的过程中，它将其他语句开始之前运行完成。  
  
 **MARS 和内存中 OLTP 事务**  
  
 由语句和交错执行的原子块所做的更改是相互隔离的。 例如，如果一个语句或原子块进行一些更改，然后生成执行转移到另一个语句，新的语句将看不由第一个语句所做的更改。 此外，当第一条语句继续执行时，它不会由任何其他语句所做的任何更改。 语句将只能看到完成并提交该语句前的更改。  
  
 可以使用 BEGIN TRANSACTION 语句在当前用户事务中启动新的用户事务-这是支持仅在互操作模式中，因此，仅从 T-SQL 语句，名为 BEGIN TRANSACTION，不能从在本机编译存储过程。您可以创建了一个保存点在事务中使用 SAVE TRANSACTION 或对事务的 API 调用。Save(save_point_name) 回滚到保存点。 此功能还会启用仅从 T-SQL 语句，不能从在本机编译存储的过程。  
  
 **MARS 和列存储索引**  
  
 SQL Server （从 2016年开始） 具有列存储索引支持 MARS。 SQL Server 2014 将 MARS 用于具有列存储索引的表的只读连接。    但是，SQL Server 2014 不支持将 MARS 用于具有列存储索引的表中的并发数据操作语言 (DML) 操作。 发生这种情况时，SQL Server 会终止连接并中止事务。   SQL Server 2012 具有只读列存储索引和 MARS 不适用于它们。  
  
## <a name="sql-server-native-client-ole-db-provider"></a>SQL Server Native Client OLE DB 访问接口  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口支持通过添加 SSPROP_INIT_MARSCONNECTION 数据源初始化属性，它在 DBPROPSET_SQLSERVERDBINIT 属性集中实现 MARS。 此外，还添加了新的连接字符串关键字 MarsConn  。 它接受 **，则返回 true**或**false**值;**false**是默认值。  
  
 数据源属性 DBPROP_MULTIPLECONNECTIONS 默认为 VARIANT_TRUE。 这意味着访问接口将生成多个连接以支持多个并发命令和行集对象。 如果启用了 MARS，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]本机客户端可以支持多个命令和行集对象上单个连接，因此默认情况下 MULTIPLE_CONNECTIONS 设置为 VARIANT_FALSE。  
  
 有关 DBPROPSET_SQLSERVERDBINIT 属性集对所做的增强功能的详细信息，请参阅[初始化和授权属性](../../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md)。  
  
### <a name="sql-server-native-client-ole-db-provider-example"></a>SQL Server Native Client OLE DB 访问接口示例  
 在此示例中，使用创建数据源对象[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]本机 OLE DB 访问接口，并且启用 MARS 使用 DBPROPSET_SQLSERVERDBINIT 属性集在创建会话对象之前。  
  
```  
#include <sqlncli.h>  
  
IDBInitialize *pIDBInitialize = NULL;  
IDBCreateSession *pIDBCreateSession = NULL;  
IDBProperties *pIDBProperties = NULL;  
  
// Create the data source object.  
hr = CoCreateInstance(CLSID_SQLNCLI10, NULL,  
   CLSCTX_INPROC_SERVER,  
   IID_IDBInitialize,   
    (void**)&pIDBInitialize);  
  
hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void**)&pIDBProperties);  
  
// Set the MARS property.  
DBPROP rgPropMARS;  
  
// The following is necessary since MARS is off by default.  
rgPropMARS.dwPropertyID = SSPROP_INIT_MARSCONNECTION;  
rgPropMARS.dwOptions = DBPROPOPTIONS_REQUIRED;  
rgPropMARS.dwStatus = DBPROPSTATUS_OK;  
rgPropMARS.colid = DB_NULLID;  
V_VT(&(rgPropMARS.vValue)) = VT_BOOL;  
V_BOOL(&(rgPropMARS.vValue)) = VARIANT_TRUE;  
  
// Create the structure containing the properties.  
DBPROPSET PropSet;  
PropSet.rgProperties = &rgPropMARS;  
PropSet.cProperties = 1;  
PropSet.guidPropertySet = DBPROPSET_SQLSERVERDBINIT;  
  
// Get an IDBProperties pointer and set the initialization properties.  
pIDBProperties->SetProperties(1, &PropSet);  
pIDBProperties->Release();  
  
// Initialize the data source object.  
hr = pIDBInitialize->Initialize();  
  
//Create a session object from a data source object.  
IOpenRowset * pIOpenRowset = NULL;  
hr = IDBInitialize->QueryInterface(IID_IDBCreateSession, (void**)&pIDBCreateSession));  
hr = pIDBCreateSession->CreateSession(  
   NULL,             // pUnkOuter  
   IID_IOpenRowset,  // riid  
  &pIOpenRowset ));  // ppSession  
  
// Create a rowset with a firehose mode cursor.  
IRowset *pIRowset = NULL;  
DBPROP rgRowsetProperties[2];  
  
// To get a firehose mode cursor request a   
// forward only read only rowset.  
rgRowsetProperties[0].dwPropertyID = DBPROP_IRowsetLocate;  
rgRowsetProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
rgRowsetProperties[0].dwStatus = DBPROPSTATUS_OK;  
rgRowsetProperties[0].colid = DB_NULLID;  
VariantInit(&(rgRowsetProperties[0].vValue));  
rgRowsetProperties[0].vValue.vt = VARIANT_BOOL;  
rgRowsetProperties[0].vValue.boolVal = VARIANT_FALSE;  
  
rgRowsetProperties[1].dwPropertyID = DBPROP_IRowsetChange;  
rgRowsetProperties[1].dwOptions = DBPROPOPTIONS_REQUIRED;  
rgRowsetProperties[1].dwStatus = DBPROPSTATUS_OK;  
rgRowsetProperties[1].colid = DB_NULLID;  
VariantInit(&(rgRowsetProperties[1].vValue));  
rgRowsetProperties[1].vValue.vt = VARIANT_BOOL;  
rgRowsetProperties[1].vValue.boolVal = VARIANT_FALSE;  
  
DBPROPSET rgRowsetPropSet[1];  
rgRowsetPropSet[0].rgProperties = rgRowsetProperties  
rgRowsetPropSet[0].cProperties = 2  
rgRowsetPropSet[0].guidPropertySet = DBPROPSET_ROWSET;  
  
hr = pIOpenRowset->OpenRowset (NULL,  
   &TableID,  
   NULL,  
   IID_IRowset,  
   1,  
   rgRowsetPropSet  
   (IUnknown**)&pIRowset);  
```  
  
## <a name="sql-server-native-client-odbc-driver"></a>SQL Server Native Client ODBC 驱动程序  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序添加到通过支持 MARS [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)并[SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md)函数。 添加了 SQL_COPT_SS_MARS_ENABLED 以接受 SQL_MARS_ENABLED_YES 或 SQL_MARS_ENABLED_NO，默认值为 SQL_MARS_ENABLED_NO。 此外，新的连接字符串关键字**Mars_Connection**，如已添加。 它接受值 "yes" 或 "no"；默认值为 "no"。  
  
### <a name="sql-server-native-client-odbc-driver-example"></a>SQL Server Native Client ODBC 驱动程序示例  
 在此示例中， **SQLSetConnectAttr**函数用于在调用之前启用 MARS **SQLDriverConnect**函数以连接数据库。 建立连接后，两个**SQLExecDirect**函数调用以在同一连接上创建两个单独的结果集。  
  
```  
#include <sqlncli.h>  
  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_MARS_ENABLED, SQL_MARS_ENABLED_YES, SQL_IS_UINTEGER);  
SQLDriverConnect(hdbc, hwnd,   
   "DRIVER=SQL Server Native Client 10.0;  
   SERVER=(local);trusted_connection=yes;", SQL_NTS, szOutConn,   
   MAX_CONN_OUT, &cbOutConn, SQL_DRIVER_COMPLETE);  
  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt1);  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt2);  
  
// The 2nd execute would have failed with connection busy error if  
// MARS were not enabled.  
SQLExecDirect(hstmt1, L"SELECT * FROM Authors", SQL_NTS);  
SQLExecDirect(hstmt2, L"SELECT * FROM Titles", SQL_NTS);  
  
// Result set processing can interleave.  
SQLFetch(hstmt1);  
SQLFetch(hstmt2);  
```  
  
## <a name="see-also"></a>请参阅  
 [SQL Server Native Client 功能](../../../relational-databases/native-client/features/sql-server-native-client-features.md)   
 [使用 SQL Server 默认结果集](../../../relational-databases/native-client-odbc-cursors/implementation/using-sql-server-default-result-sets.md)  
  
  
