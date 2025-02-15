---
title: 使用增强的日期和时间功能 (OLE DB) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 50f98cab-8c80-43c5-bc9a-5d2f95f67f17
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 14790203925575ef08f2ab84e2b1f355d970448d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68110011"
---
# <a name="use-enhanced-date-and-time-features-ole-db"></a>使用日期和时间增强功能 (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  此示例显示如何使用 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 中引入的日期/时间功能。 此示例使用四个新日期和时间类型 (**日期**，**时间**， **datetime2**，以及**datetimeoffset**) 以执行命令参数和检索行集结果。  
  
 此示例要求使用 AdventureWorks 示例数据库，其可从 [Microsoft SQL Server 示例和社区项目](https://go.microsoft.com/fwlink/?LinkID=85384)主页下载。  
  
> [!IMPORTANT]  
>  请尽可能使用 Windows 身份验证。 如果 Windows 身份验证不可用，请在运行时提示用户输入其凭据。 不要将凭据存储在一个文件中。 如果必须保存凭据，应当用 [Win32 crypto API](https://go.microsoft.com/fwlink/?LinkId=64532)（Win32 加密 API）加密它们。  
  
## <a name="example"></a>示例  
 第一个 ([!INCLUDE[tsql](../../includes/tsql-md.md)]) 代码列表创建此示例要使用的存储过程。  
  
 使用 ole32.lib 和 oleaut32.lib 编译并执行第二个 (C++) 代码列表。 此应用程序连接到您的计算机上默认的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 在某些 Windows 操作系统上，您需要将 (localhost) 或 (local) 更改为您的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。 若要连接到命名实例，请将连接字符串从 L"(local)" 更改为 L"(local)\\\name"，其中 name 是命名实例。 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express 安装在命名实例中。 请确保您的 INCLUDE 环境变量包括含有 sqlncli.h 的目录。  
  
 第三个 ([!INCLUDE[tsql](../../includes/tsql-md.md)]) 代码列表删除此示例使用的存储过程。  
  
```  
CREATE PROCEDURE sp_datetimetypes  
   @date_param DATE OUTPUT,   
   @time_param TIME(7) OUTPUT,  
   @datetime2_param DATETIME2(7) OUTPUT,  
   @datetimeoffset_param DATETIMEOFFSET(7) OUTPUT  
AS   
SELECT @date_param, @time_param, @datetime2_param, @datetimeoffset_param  
GO  
```  
  
```  
// compile with: ole32.lib oleaut32.lib  
#define WIN32_LEAN_AND_MEAN  
#include <stdio.h>  
#include <tchar.h>  
#include <sqlncli.h>  
#include "oledberr.h"  
#include <stddef.h>  
  
static const DBLENGTH sc_cchStrLen = 64;  
  
struct DateTimeTypeEx {  
   DBSTATUS status;  
   DBLENGTH length;  
   union {  
      DBTIMESTAMP ts;  
      DBDATE d;  
      DBTIME2 t;  
      DBTIMESTAMPOFFSET tso;  
      WCHAR wstr[sc_cchStrLen];  
      CHAR str[sc_cchStrLen];  
   };  
};  
  
struct TypePrecScalEx {  
   DBTYPE wType;  
   BYTE bPrec;  
   BYTE bScale;  
   WCHAR wszTypeName[64];  
   DBLENGTH ulSize;  
};  
  
HRESULT InitializeAndEstablishConnection (IUnknown ** ppUnkDBInitialize)  {  
   HRESULT hr = S_OK;  
   IDBProperties * pIDBProperties = NULL;  
   IDBInitialize * pIDBInitialize = NULL;  
   DBPROPSET rgInitPropSet[1] = {0};  
   DBPROP InitProperties[4] = {0};  
  
   *ppUnkDBInitialize = NULL;  
  
   // Initialize the COM library.  
   CoInitialize(NULL);  
  
   // Obtain access to the SQL Server Native Client OLE DB provider.  
   hr = CoCreateInstance(CLSID_SQLNCLI11, NULL, CLSCTX_INPROC_SERVER, IID_IDBInitialize, (void **) &pIDBInitialize);  
  
   // Initialize property values needed to establish connection.  
   for (unsigned int i = 0 ; i < 4 ; i++)   
      VariantInit(&InitProperties[i].vValue);  
  
   // Server name.  
   // See DBPROP structure for more information on InitProperties  
   InitProperties[0].dwPropertyID = DBPROP_INIT_DATASOURCE;  
   InitProperties[0].vValue.vt = VT_BSTR;  
   InitProperties[0].vValue.bstrVal= SysAllocString(L"localhost");  
   InitProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[0].colid = DB_NULLID;  
  
   // Database.  
   InitProperties[1].dwPropertyID = DBPROP_INIT_CATALOG;  
   InitProperties[1].vValue.vt = VT_BSTR;  
   InitProperties[1].vValue.bstrVal = SysAllocString(L"AdventureWorks");  
   InitProperties[1].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[1].colid = DB_NULLID;  
  
   // Username (login).  
   InitProperties[2].dwPropertyID = DBPROP_AUTH_INTEGRATED;  
   InitProperties[2].vValue.vt = VT_BSTR;  
   InitProperties[2].vValue.bstrVal = SysAllocString(L"SSPI");  
   InitProperties[2].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[2].colid = DB_NULLID;  
   InitProperties[3].dwOptions = DBPROPOPTIONS_REQUIRED;  
   InitProperties[3].colid = DB_NULLID;  
  
   // Construct the DBPROPSET structure(rgInitPropSet). The DBPROPSET structure is used to pass   
   // an array of DBPROP structures (InitProperties) to the SetProperties method.  
   rgInitPropSet[0].guidPropertySet = DBPROPSET_DBINIT;  
   rgInitPropSet[0].cProperties = 4;  
   rgInitPropSet[0].rgProperties = InitProperties;  
  
   // Set initialization properties.  
   hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);  
   hr = pIDBProperties->SetProperties(1, rgInitPropSet);   
   pIDBProperties->Release();  
  
   // Establish the connection to the data source.  
   hr = pIDBInitialize->Initialize();     
  
   if (S_OK == hr)  
      *ppUnkDBInitialize = pIDBInitialize;  
  
   return hr;  
}  
  
//   This function creates an OLE DB Session object from the given DataSource object.  
HRESULT myCreateSession(IUnknown * pUnkDataSource, REFIID riid, IUnknown ** ppUnkSession) {  
   HRESULT hr;  
   IDBCreateSession * pIDBCreateSession = NULL;  
  
   // Create a session object from a data source object  
   hr = pUnkDataSource->QueryInterface(IID_IDBCreateSession, (void**)&pIDBCreateSession);  
   hr = pIDBCreateSession->CreateSession(NULL, riid, ppUnkSession);     
  
   if (pIDBCreateSession)  
      pIDBCreateSession->Release();  
  
   return hr;  
}  
  
// This function takes an IUnknown pointer on a Session object and attempts to create a Command object  
// object using the Session's IDBCreateCommand interface. Since this interface is optional, this may fail.  
HRESULT myCreateCommand(IUnknown * pUnkSession, IUnknown ** ppUnkCommand) {  
   HRESULT hr;  
   IDBCreateCommand * pIDBCreateCommand = NULL;  
  
   // Attempt to create a Command object from the Session object  
   hr = pUnkSession->QueryInterface(IID_IDBCreateCommand, (void**)&pIDBCreateCommand);     
   hr = pIDBCreateCommand->CreateCommand(NULL, IID_ISSCommandWithParameters, ppUnkCommand);     
  
   if (pIDBCreateCommand)  
      pIDBCreateCommand->Release();  
  
   return hr;  
}  
  
// This function takes an IUnknown pointer on a Command object and creates a new Rowset object as follows:  
// * Sets the given properties on the Command object. These properties will be  
//   applied by the provider to any Rowset created by this Command.  
// * Sets the given command text for the Command.  
// * Executes the command to create a new Rowset object.  
HRESULT myExecuteCommand(ISSCommandWithParameters * pUnkCommand, ULONG cPropSets, DBPROPSET* rgPropSets, IUnknown ** ppUnkRowset) {  
   HRESULT hr;  
   ICommandText * pICommandText = NULL;  
   ICommandProperties * pICommandProperties = NULL;  
   IAccessor * pIAccessor = NULL;  
   IAccessor * pIAccessorRowset = NULL;  
   HACCESSOR hAccessor = NULL;  
   HACCESSOR hAccessorWstr = NULL;  
   DBCOUNTITEM cRowsObtained = 0;  
   HROW * rghRow = NULL;  
  
   static const DBCOUNTITEM sc_cCol = 4;  
   static const DB_UPARAMS sc_cParams = sc_cCol;  
   static const DBROWCOUNT sc_cRows = 1;  
  
   // Bindings and values  
   DateTimeTypeEx rgDttEx[sc_cCol] = {0};  
   DBBINDING rgBindings[sc_cCol] = {0};  
   DBBINDSTATUS rgStatus[sc_cCol] = {0};  
   DBBINDING rgBindingsWstr[sc_cCol] = {0};  
  
   TypePrecScalEx rgTpsEx[sc_cCol] = {   
      DBTYPE_DBDATE, 0, 0, L"DATE", sizeof(DBDATE),  
      DBTYPE_DBTIME2, 0, 4, L"TIME", sizeof(DBTIME2),  
      DBTYPE_DBTIMESTAMP, 0, 4, L"DATETIME2", sizeof(DBTIMESTAMP),  
      DBTYPE_DBTIMESTAMPOFFSET, 0, 4, L"DATETIMEOFFSET", sizeof(DBTIMESTAMPOFFSET)    
   };  
  
   // Command, bindings, and param values  
   WCHAR * pwszCommandText = L"{call sp_datetimetypes(?,?,?,?)}";  
   DB_UPARAMS rgParamOrdinals[sc_cParams] = {0};  
   DBPARAMBINDINFO rgParamBindInfo[sc_cParams] = {0};  
   DBPARAMS myDBPARAMS = {0};  
  
   // Set param ordinals  
   for (unsigned int i = 0; i < sc_cParams; i++)  
      rgParamOrdinals[i] = i + 1;  
  
   // Set param bind info  
   for (unsigned int i = 0; i < sc_cParams; i++) {  
      rgParamBindInfo[i].pwszDataSourceType = rgTpsEx[i].wszTypeName;  
      rgParamBindInfo[i].bPrecision = rgTpsEx[i].bPrec;  
      rgParamBindInfo[i].bScale = rgTpsEx[i].bScale;  
      rgParamBindInfo[i].dwFlags = DBPARAMFLAGS_ISINPUT | DBPARAMFLAGS_ISOUTPUT | DBPARAMFLAGS_ISNULLABLE;  
      rgParamBindInfo[i].pwszName = NULL;  
      rgParamBindInfo[i].ulParamSize = rgTpsEx[i].ulSize;  
   }  
  
   // Prepare bindings  
   for (unsigned int i = 0; i < sc_cParams; i++) {  
      rgBindings[i].iOrdinal = i+1;  
      rgBindings[i].obValue =  (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, t);  
      rgBindings[i].obLength = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, length);  
      rgBindings[i].obStatus = (i * sizeof(DateTimeTypeEx));  
      rgBindings[i].pTypeInfo = NULL;  
      // rgBindings[i].pObject not used  
      rgBindings[i].pBindExt = NULL;  
      rgBindings[i].dwPart = DBPART_VALUE | DBPART_LENGTH | DBPART_STATUS;  
      rgBindings[i].dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
      rgBindings[i].eParamIO = DBPARAMIO_INPUT | DBPARAMIO_OUTPUT;  
      rgBindings[i].cbMaxLen = 0;   // ignored for fixed length types  
      rgBindings[i].dwFlags = 0;  
      rgBindings[i].wType = rgTpsEx[i].wType;  
      rgBindings[i].bPrecision = rgTpsEx[i].bPrec;  
      rgBindings[i].bScale = rgTpsEx[i].bScale;  
  
      rgBindingsWstr[i].iOrdinal = i+1;  
      rgBindingsWstr[i].obValue = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, t);  
      rgBindingsWstr[i].obLength = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, length);  
      rgBindingsWstr[i].obStatus = (i * sizeof(DateTimeTypeEx));  
      rgBindingsWstr[i].pTypeInfo = NULL;  
      // rgBindingsWstr[i].pObject not used  
      rgBindingsWstr[i].pBindExt = NULL;  
      rgBindingsWstr[i].dwPart = DBPART_VALUE | DBPART_LENGTH | DBPART_STATUS;  
      rgBindingsWstr[i].dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
      rgBindingsWstr[i].eParamIO = DBPARAMIO_NOTPARAM ;  
      rgBindingsWstr[i].cbMaxLen = sizeof(WCHAR)*sc_cchStrLen;   
      rgBindingsWstr[i].dwFlags = 0;  
      rgBindingsWstr[i].wType = DBTYPE_WSTR;  
      rgBindingsWstr[i].bPrecision = 0;  
      rgBindingsWstr[i].bScale = 0;  
   }  
  
   // Create accessors  
   hr = pUnkCommand->QueryInterface(IID_IAccessor, (void**)&pIAccessor);  
   hr =  pIAccessor->CreateAccessor (DBACCESSOR_PARAMETERDATA, sc_cCol, rgBindings, 0, &hAccessor, rgStatus);   
  
   myDBPARAMS.cParamSets = 1;  
   myDBPARAMS.pData = rgDttEx;  
   myDBPARAMS.hAccessor = hAccessor;  
  
   // Set the properties on the Command object  
   hr = pUnkCommand->QueryInterface(IID_ICommandProperties, (void**)&pICommandProperties);     
   hr = pICommandProperties->SetProperties(cPropSets, rgPropSets);     
  
   // Set the text for this Command, using the default command text  
   // dialect. All providers that support commands must support this  
   // dialect and providers that support SQL must be able to recognize  
   // an SQL command as SQL when this dialect is specified  
   hr = pUnkCommand->QueryInterface(IID_ICommandText, (void**)&pICommandText);  
   hr = pICommandText->SetCommandText(DBGUID_DEFAULT, pwszCommandText);  
  
   // Set value in preparation for executing command  
   rgDttEx[0].length = sizeof(DBDATE);  
   rgDttEx[0].d.year = 1979;  
   rgDttEx[0].d.month = 4;  
   rgDttEx[0].d.day = 1;  
  
   rgDttEx[1].length = sizeof(DBTIME2);  
   rgDttEx[1].t.hour = 2;  
   rgDttEx[1].t.minute = 3;  
   rgDttEx[1].t.second = 5;  
   rgDttEx[1].t.fraction = 800000;  
  
   rgDttEx[2].length = sizeof(DBTIMESTAMP);  
   rgDttEx[2].ts.year = 1776;  
   rgDttEx[2].ts.month = 7;  
   rgDttEx[2].ts.day = 4;  
   rgDttEx[2].ts.hour = 21;  
   rgDttEx[2].ts.minute = 39;  
   rgDttEx[2].ts.second = 59;  
   rgDttEx[2].ts.fraction = 5000000;  
  
   rgDttEx[3].length = sizeof(DBTIMESTAMPOFFSET);  
   rgDttEx[3].tso.year = 1979;  
   rgDttEx[3].tso.month = 4;  
   rgDttEx[3].tso.day = 1;  
   rgDttEx[3].tso.hour = 2;  
   rgDttEx[3].tso.minute = 3;  
   rgDttEx[3].tso.second = 5;  
   rgDttEx[3].tso.fraction = 0;  
   rgDttEx[3].tso.timezone_hour = -5;  
   rgDttEx[3].tso.timezone_minute = 0;  
  
   // Add parameter info  
   hr = pUnkCommand->SetParameterInfo(sc_cParams, rgParamOrdinals, rgParamBindInfo);     
  
   // Execute the Command. User could enter a non-row returning command, so we check for  
   // that and return failure to prevent the display of the non-existant rowset by the caller  
   hr = pICommandText->Execute(NULL, IID_IRowset, &myDBPARAMS, NULL, ppUnkRowset);     
  
   if (FAILED(hr) || !*ppUnkRowset) {  
      printf("\nThe command executed, but did not " \  
         "return a rowset.\nNo rowset will be displayed.\n");  
      hr = E_FAIL;        
      // exit(0);  
   }  
   else  
   {  
      // Create accessor for rowset  
      hr = (*ppUnkRowset)->QueryInterface(IID_IAccessor, (void**)&pIAccessorRowset);  
      hr =  pIAccessorRowset->CreateAccessor (DBACCESSOR_ROWDATA, sc_cCol, rgBindingsWstr, 0, &hAccessorWstr, rgStatus);   
      hr = ((IRowset*)*ppUnkRowset)->GetNextRows(DB_NULL_HCHAPTER, 0, sc_cRows, &cRowsObtained, &rghRow);  
      hr = ((IRowset*)*ppUnkRowset)->GetData(rghRow[0], hAccessorWstr, rgDttEx);     
  
      printf ("Data returned from sp_datetimetypes -------------\n%32ls%32ls%32ls%32ls\n",   
         rgDttEx[0].wstr, rgDttEx[1].wstr, rgDttEx[2].wstr, rgDttEx[3].wstr);  
   }  
   if (pICommandText)  
      pICommandText->Release();  
  
   if (pICommandProperties)  
      pICommandProperties->Release();  
  
   if (pIAccessor)  
      pIAccessor->Release();  
  
   if (pIAccessorRowset)  
      pIAccessorRowset->Release();  
  
   return hr;  
}  
  
HRESULT myTableDefinition (IUnknown * pUnkDataSource, DBID *  pTableID, IUnknown ** ppUnkRowset) {  
   HRESULT hr;  
   ITableDefinition * pITableDefinition = NULL;  
   HACCESSOR hAccessor = NULL;  
  
   static const DBORDINAL sc_cCol = 4;  
  
   DBCOLUMNDESC rgColumnDescs[sc_cCol];  
  
   pTableID->eKind = DBKIND_NAME;  
   pTableID->uName.pwszName = L"datetimetypestable";  
  
   rgColumnDescs[0].pwszTypeName = L"date";  
   rgColumnDescs[0].pTypeInfo = NULL;  
   rgColumnDescs[0].rgPropertySets = NULL;  
   rgColumnDescs[0].pclsid = NULL;  
   rgColumnDescs[0].cPropertySets = 0;  
   rgColumnDescs[0].ulColumnSize = 0;  
   rgColumnDescs[0].dbcid.eKind = DBKIND_NAME;  
   rgColumnDescs[0].dbcid.uName.pwszName = L"datecol";  
   rgColumnDescs[0].wType = DBTYPE_DBDATE;  
   rgColumnDescs[0].bPrecision = 0;  
   rgColumnDescs[0].bScale = 0;  
  
   rgColumnDescs[1].pwszTypeName = L"time";  
   rgColumnDescs[1].pTypeInfo = NULL;  
   rgColumnDescs[1].rgPropertySets = NULL;  
   rgColumnDescs[1].pclsid = NULL;  
   rgColumnDescs[1].cPropertySets = 0;  
   rgColumnDescs[1].ulColumnSize = 0;  
   rgColumnDescs[1].dbcid.eKind = DBKIND_NAME;  
   rgColumnDescs[1].dbcid.uName.pwszName = L"timecol";  
   rgColumnDescs[1].wType = DBTYPE_DBTIME2;  
   rgColumnDescs[1].bPrecision = 0;  
   rgColumnDescs[1].bScale = 7;  
  
   rgColumnDescs[2].pwszTypeName = L"datetime2";  
   rgColumnDescs[2].pTypeInfo = NULL;  
   rgColumnDescs[2].rgPropertySets = NULL;  
   rgColumnDescs[2].pclsid = NULL;  
   rgColumnDescs[2].cPropertySets = 0;  
   rgColumnDescs[2].ulColumnSize = 0;  
   rgColumnDescs[2].dbcid.eKind = DBKIND_NAME;  
   rgColumnDescs[2].dbcid.uName.pwszName = L"datetime2col";  
   rgColumnDescs[2].wType = DBTYPE_DBTIMESTAMP;  
   rgColumnDescs[2].bPrecision = 0;  
   rgColumnDescs[2].bScale = 7;  
  
   rgColumnDescs[3].pwszTypeName = L"datetimeoffset";  
   rgColumnDescs[3].pTypeInfo = NULL;  
   rgColumnDescs[3].rgPropertySets = NULL;  
   rgColumnDescs[3].pclsid = NULL;  
   rgColumnDescs[3].cPropertySets = 0;  
   rgColumnDescs[3].ulColumnSize = 0;  
   rgColumnDescs[3].dbcid.eKind = DBKIND_NAME;  
   rgColumnDescs[3].dbcid.uName.pwszName = L"datetimeoffsetcol";  
   rgColumnDescs[3].wType = DBTYPE_DBTIMESTAMPOFFSET;  
   rgColumnDescs[3].bPrecision = 0;  
   rgColumnDescs[3].bScale = 7;  
  
   hr = pUnkDataSource->QueryInterface(IID_ITableDefinition, (void**)&pITableDefinition);     
   hr = pITableDefinition->CreateTable(NULL, pTableID, sc_cCol, rgColumnDescs, IID_IRowset, 0, NULL, &pTableID, ppUnkRowset);   
  
   // Ignore the case where the table already exists  
   if (DB_E_DUPLICATETABLEID == hr)  
      hr = S_OK;  
  
   if (pITableDefinition)  
      pITableDefinition->Release();  
  
   return hr;  
}  
  
HRESULT myTableChange(IOpenRowset * pIOpenRowset, DBID * pTableID, IUnknown ** ppUnkRowset) {  
   HRESULT hr;  
   IRowsetChange * pIRowsetChange = NULL;  
   IAccessor * pIAccessor = NULL;  
   HACCESSOR hAccessor = NULL;  
   HROW hRow = NULL;  
  
   static const DBCOUNTITEM sc_cCol = 4;  
  
   // Bindings and values  
   DateTimeTypeEx rgDttEx[sc_cCol] = {0};  
   DBBINDING rgBindings[sc_cCol] = {0};  
   DBBINDSTATUS rgStatus[sc_cCol] = {0};  
  
   TypePrecScalEx rgTpsEx[sc_cCol] = {   
      DBTYPE_DBDATE, 0, 0, L"DATE", sizeof(DBDATE),  
      DBTYPE_DBTIME2, 0, 7, L"TIME", sizeof(DBTIME2),  
      DBTYPE_DBTIMESTAMP, 0, 7, L"DATETIME2", sizeof(DBTIMESTAMP),  
      DBTYPE_DBTIMESTAMPOFFSET, 0, 7, L"DATETIMEOFFSET", sizeof(DBTIMESTAMPOFFSET)    
   };  
  
   // Open IRowsetChange interface on existing table  
   hr = pIOpenRowset->OpenRowset(NULL, pTableID, NULL, IID_IRowsetChange, 0, NULL, (IUnknown**)&pIRowsetChange);     
  
   // Prepare bindings  
   for (unsigned int i = 0; i < sc_cCol; i++) {  
      rgBindings[i].iOrdinal = i+1;  
      rgBindings[i].obValue = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, t);       
      rgBindings[i].obLength = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, length);  
      rgBindings[i].obStatus = (i * sizeof(DateTimeTypeEx));  
  
      rgBindings[i].pTypeInfo = NULL;  
      // rgBindings[i].pObject not used  
      rgBindings[i].pBindExt = NULL;  
      rgBindings[i].dwPart = DBPART_VALUE | DBPART_LENGTH | DBPART_STATUS;  
      rgBindings[i].dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
      rgBindings[i].eParamIO = DBPARAMIO_NOTPARAM ;  
      rgBindings[i].cbMaxLen = 0;   // ignored for fixed length types (sizeof(DBTIMESTAMP))  
      rgBindings[i].dwFlags = 0;  
      rgBindings[i].wType = rgTpsEx[i].wType;  
      rgBindings[i].bPrecision = rgTpsEx[i].bPrec;  
      rgBindings[i].bScale = rgTpsEx[i].bScale;  
   }  
  
   // Create accessors  
   hr = pIRowsetChange->QueryInterface(IID_IAccessor, (void**)&pIAccessor);  
   hr =  pIAccessor->CreateAccessor (DBACCESSOR_ROWDATA, sc_cCol, rgBindings, 0, &hAccessor, rgStatus);   
  
   // Set value in preparation for inserting a new row  
   rgDttEx[0].length = sizeof(DBDATE);  
   rgDttEx[0].d.year = 1979;  
   rgDttEx[0].d.month = 4;  
   rgDttEx[0].d.day = 1;  
  
   rgDttEx[1].length = sizeof(DBTIME2);  
   rgDttEx[1].t.hour = 2;  
   rgDttEx[1].t.minute = 3;  
   rgDttEx[1].t.second = 5;  
   rgDttEx[1].t.fraction = 800000;  
  
   rgDttEx[2].length = sizeof(DBTIMESTAMP);  
   rgDttEx[2].ts.year = 1776;  
   rgDttEx[2].ts.month = 7;  
   rgDttEx[2].ts.day = 4;  
   rgDttEx[2].ts.hour = 21;  
   rgDttEx[2].ts.minute = 39;  
   rgDttEx[2].ts.second = 59;  
   rgDttEx[2].ts.fraction = 5000000;  
  
   rgDttEx[3].length = sizeof(DBTIMESTAMPOFFSET);  
   rgDttEx[3].tso.year = 1979;  
   rgDttEx[3].tso.month = 4;  
   rgDttEx[3].tso.day = 1;  
   rgDttEx[3].tso.hour = 2;  
   rgDttEx[3].tso.minute = 3;  
   rgDttEx[3].tso.second = 5;  
   rgDttEx[3].tso.fraction = 0;  
   rgDttEx[3].tso.timezone_hour = -5;  
   rgDttEx[3].tso.timezone_minute = 0;  
  
   hr = pIRowsetChange->InsertRow(NULL, hAccessor, rgDttEx, &hRow);     
  
   if (pIRowsetChange)  
      pIRowsetChange->Release();  
  
   if (pIAccessor)  
      pIAccessor->Release();  
  
   return hr;  
}  
  
HRESULT myTableRead(IOpenRowset * pIOpenRowset, DBID * pTableID, IUnknown ** ppUnkRowset) {  
   HRESULT  hr;  
   IRowset * pIRowset = NULL;  
   IAccessor * pIAccessor = NULL;  
   HACCESSOR hAccessor = NULL;  
   HACCESSOR hAccessorStr = NULL;  
   HACCESSOR hAccessorWstr = NULL;  
   static const DBROWCOUNT sc_cRows = 1;  
   HROW * rghRow = NULL;  
   DBCOUNTITEM cRowsObtained = 0;  
  
   // static const DBORDINAL sc_cCol = 4;  
   static const DBCOUNTITEM sc_cCol = 4;  
  
   // Bindings and values  
   DateTimeTypeEx rgDttEx[sc_cCol] = {0};  
   DBBINDING rgBindings[sc_cCol] = {0};  
   DBBINDSTATUS rgStatus[sc_cCol] = {0};  
   DBBINDING rgBindingsStr[sc_cCol] = {0};  
   DBBINDING rgBindingsWstr[sc_cCol] = {0};  
  
   TypePrecScalEx rgTpsEx[sc_cCol] = {   
      DBTYPE_DBDATE, 0, 0, L"DATE", sizeof(DBDATE),  
      DBTYPE_DBTIME2, 0, 0, L"TIME", sizeof(DBTIME2),  
      DBTYPE_DBTIMESTAMP, 0, 0, L"DATETIME2", sizeof(DBTIMESTAMP),  
      DBTYPE_DBTIMESTAMPOFFSET, 0, 0, L"DATETIMEOFFSET", sizeof(DBTIMESTAMPOFFSET)  
   };  
  
   printf ("%d = sizeof(DateTimeTypeEx)\n", sizeof(DateTimeTypeEx));  
  
   // Open IRowsetChange interface on existing table  
   hr = pIOpenRowset->OpenRowset(NULL, pTableID, NULL, IID_IRowset, 0, NULL, (IUnknown**)&pIRowset);     
  
   // Prepare bindings  
   for (unsigned int i = 0; i < 4; i++) {  
      rgBindings[i].iOrdinal = i+1;  
      rgBindingsStr[i].iOrdinal = i+1;  
      rgBindingsWstr[i].iOrdinal = i+1;  
  
      rgBindings[i].obValue = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, t);       
      rgBindings[i].obLength = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, length);  
      rgBindingsStr[i].obValue = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, t);    
      rgBindingsWstr[i].obValue = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, t);;  
      rgBindings[i].obLength = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, length);  
      rgBindingsStr[i].obLength = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, length);  
      rgBindingsWstr[i].obLength = (i * sizeof(DateTimeTypeEx)) + offsetof(DateTimeTypeEx, length);  
      rgBindings[i].obStatus = (i * sizeof(DateTimeTypeEx));  
      rgBindingsStr[i].obStatus = (i * sizeof(DateTimeTypeEx));  
      rgBindingsWstr[i].obStatus = (i * sizeof(DateTimeTypeEx));  
      rgBindings[i].pTypeInfo = NULL;  
      rgBindingsStr[i].pTypeInfo = NULL;  
      rgBindingsWstr[i].pTypeInfo = NULL;  
      // rgBindings[i].pObject not used  
      rgBindings[i].pBindExt = NULL;  
      rgBindingsStr[i].pBindExt = NULL;  
      rgBindingsWstr[i].pBindExt = NULL;  
      rgBindings[i].dwPart = DBPART_VALUE | DBPART_LENGTH | DBPART_STATUS;  
      rgBindingsStr[i].dwPart = DBPART_VALUE | DBPART_LENGTH | DBPART_STATUS;  
      rgBindingsWstr[i].dwPart = DBPART_VALUE | DBPART_LENGTH | DBPART_STATUS;  
      rgBindings[i].dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
      rgBindingsStr[i].dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
      rgBindingsWstr[i].dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
      rgBindings[i].eParamIO = DBPARAMIO_NOTPARAM ;  
      rgBindingsStr[i].eParamIO = DBPARAMIO_NOTPARAM ;  
      rgBindingsWstr[i].eParamIO = DBPARAMIO_NOTPARAM ;  
      rgBindings[i].cbMaxLen = 0;   // ignored for fixed length types  
      rgBindingsStr[i].cbMaxLen = sc_cchStrLen;   
      rgBindingsWstr[i].cbMaxLen = sizeof(WCHAR)*sc_cchStrLen;   
      rgBindings[i].dwFlags = 0;  
      rgBindingsStr[i].dwFlags = 0;  
      rgBindingsWstr[i].dwFlags = 0;  
      rgBindings[i].wType = rgTpsEx[i].wType;  
      rgBindingsStr[i].wType = DBTYPE_STR;  
      rgBindingsWstr[i].wType = DBTYPE_WSTR;  
      rgBindings[i].bPrecision = rgTpsEx[i].bPrec;  
      rgBindingsStr[i].bPrecision = 0;  
      rgBindingsWstr[i].bPrecision = 0;  
      rgBindings[i].bScale = rgTpsEx[i].bScale;  
      rgBindingsStr[i].bScale = 0;  
      rgBindingsWstr[i].bScale = 0;  
   }  
  
   // Create accessors  
   hr = pIRowset->QueryInterface(IID_IAccessor, (void**)&pIAccessor);  
   hr =  pIAccessor->CreateAccessor (DBACCESSOR_ROWDATA, sc_cCol, rgBindings, 0, &hAccessor, rgStatus);   
   hr =  pIAccessor->CreateAccessor (DBACCESSOR_ROWDATA, sc_cCol, rgBindingsStr, 0, &hAccessorStr, rgStatus);   
   hr =  pIAccessor->CreateAccessor (DBACCESSOR_ROWDATA, sc_cCol, rgBindingsWstr, 0, &hAccessorWstr, rgStatus);   
   hr = pIRowset->GetNextRows(DB_NULL_HCHAPTER, 0, sc_cRows, &cRowsObtained, &rghRow);  
   hr = pIRowset->GetData(rghRow[0], hAccessor, rgDttEx);  
   hr = pIRowset->GetData(rghRow[0], hAccessorStr, rgDttEx);  
   hr = pIRowset->GetData(rghRow[0], hAccessorWstr, rgDttEx);     
  
   if (pIRowset)  
      pIRowset->Release();  
  
   if (pIAccessor)  
      pIAccessor->Release();  
  
   return hr;  
}  
  
int main() {  
   HRESULT hr = S_OK;  
   IDBInitialize * pIDBInitialize = NULL;  
   IDBCreateSession * pIDBCreateSession = NULL;  
   ISSCommandWithParameters * pIDBCreateCommand = NULL;  
   IOpenRowset * pIOpenRowset = NULL;  
   IRowset * pIRowset = NULL;  
   DBID tableID = {0};  
  
   hr = InitializeAndEstablishConnection((IUnknown**)&pIDBInitialize);  
   hr = myCreateSession((IUnknown*)pIDBInitialize, IID_IDBCreateCommand, (IUnknown**)&pIDBCreateSession);  
   hr = myTableDefinition(pIDBCreateSession, &tableID, (IUnknown**)&pIRowset);  
   hr = myCreateSession((IUnknown*)pIDBInitialize, IID_IOpenRowset, (IUnknown**)&pIOpenRowset);  
   hr = myTableChange(pIOpenRowset, &tableID, (IUnknown**)&pIRowset);  
   hr = myTableRead(pIOpenRowset, &tableID, (IUnknown**)&pIRowset);  
   hr = myCreateCommand((IUnknown*)pIDBCreateSession, (IUnknown**)&pIDBCreateCommand);  
   hr = myExecuteCommand(pIDBCreateCommand,  0, NULL, (IUnknown**)&pIRowset);  
  
   if (pIDBInitialize)  
      pIDBInitialize->Release();  
  
   if (pIDBCreateSession)  
      pIDBCreateSession->Release();  
  
   if (pIDBCreateCommand)  
      pIDBCreateCommand->Release();  
  
   if (pIRowset)  
      pIRowset->Release();  
}  
```  
  
```  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'sp_datetimetypes')  
     DROP PROCEDURE sp_datetimetypes  
```  
  
  
