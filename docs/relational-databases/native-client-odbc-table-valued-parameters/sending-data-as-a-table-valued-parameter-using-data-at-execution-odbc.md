---
title: 将数据发送作为表值参数使用在执行数据 (ODBC) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), sending data to a stored procedure one row at a time
ms.assetid: 361e6442-34de-4cac-bdbd-e05f04a21ce4
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a3989c7543361be8dfc6807d11ccdc3e97b78e46
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68129186"
---
# <a name="sending-data-as-a-table-valued-parameter-using-data-at-execution-odbc"></a>使用执行时数据将数据作为表值参数发送 (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  它类似于[都在内存](../../relational-databases/native-client-odbc-table-valued-parameters/sending-data-as-a-table-valued-parameter-with-all-values-in-memory-odbc.md)过程中，但使用表值参数执行时数据。  
  
 有关演示表值参数的另一个示例，请参阅[使用表值参数&#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/use-table-valued-parameters-odbc.md)。  
  
 在此示例中，当调用 SQLExecute 或 SQLExecDirect 时，驱动程序将返回 SQL_NEED_DATA。 然后，应用程序反复调用 SQLParamData，直到驱动程序将返回 SQL_NEED_DATA 以外的值。 驱动程序将返回*ParameterValuePtr*来通知哪些参数请求的数据的应用程序。 在应用程序调用 SQLPutData 提供之前 SQLParamData 的后续调用的参数数据。 对于表值参数，对 SQLPutData 的调用指示它已准备好驱动程序 （在此示例中，始终为 1） 的行数。 表值的所有行已被传递给驱动程序，被调用 SQLPutData 以指示没有可用的行。  
  
 可以在表值行内使用执行时数据值。 SQLParamData 返回的值通知应用程序驱动程序需要使用哪个值。 与常规参数值，如 SQLPutData 可以调用一个或三次以上的字符或二进制表值列的值。 这样，应用程序可以分块传递较大值。  
  
 当针对表值调用 SQLPutData *DataPtr*用于 （在此示例中，始终为 1） 可用的行数。 *StrLen_or_IndPtr*必须始终为 0。 当传递了表值的所有行时，使用调用 SQLPutData *DataPtr*值为 0。  
  
## <a name="prerequisite"></a>先决条件  
 该过程假定已在服务器上执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)]：  
  
```  
create type TVParam as table(ProdCode integer, Qty integer)  
create procedure TVPOrderEntry(@CustCode varchar(5), @Items TVPParam,   
            @OrdNo integer output, @OrdDate datetime output)  
         as   
         set @OrdDate = GETDATE();  
         insert into TVPOrd (OrdDate, CustCode) values (@OrdDate, @CustCode) output OrdNo);   
         select @OrdNo = SCOPE_IDENTITY();   
         insert into TVPItem (OrdNo, ProdCode, Qty)  
select @OrdNo, @Items.ProdCode, @Items.Qty   
from @Items  
```  
  
## <a name="to-send-the-data"></a>发送数据  
  
1.  声明 SQL 参数的变量。 在本示例中，表值参数的缓冲区不必为数组；本示例每次传递一行数据。  
  
    ```  
    SQLRETURN r;  
  
    // Variables for SQL parameters:  
    SQLCHAR CustCode[6];  
    SQLCHAR *TVP = (SQLCHAR *) "TVPInParam";  
    SQLINTEGER ProdCode, Qty;  
    SQLINTEGER OrdNo;  
    char *OrdDate[23];  
    SQLCHAR *TVP = (SQLCHAR *) "TVParam";  
    SQLINTEGER ItemNo;  
    // Variables for indicator/length variables associated with parameters:  
    SQLLEN cbCustCode, cbTVP, cbProdCode, cbQty, cbOrdNo, cbOrdDate, cbItemNo;  
    // Token returned by SQLParamData to indicate which param data is needed for:  
    SQLPOINTER ParamId;  
    ```  
  
2.  绑定参数。 *ColumnSize*为 1，这意味着每次传递最多一行。  
  
    ```  
    // Bind parameters for call to TVPOrderEntryByRow.  
    r = SQLBindParameter(hstmt, 1, SQL_C_CHAR, SQL_PARAM_INPUT,SQL_VARCHAR, 5, 0, CustCode, sizeof(CustCode), &cbCustCode);  
  
    // 2 - Items TVP  
    r = SQLBindParameter(hstmt,   
        2,         // ParameterNumber  
        SQL_C_DEFAULT,   // InputOutputType  
        SQL_PARAM_INPUT,   // ValueType   
        SQL_SS_TABLE,   // Parametertype  
        1,         // ColumnSize: For a table-valued parameter this the row array size.  
        0,         // DecimalDigits: For a table-valued parameter this is always 0.   
        TVP,      // ParameterValuePtr: For a table-valued parameter this is the type name of the TVP,  
             //      and also a token returned by SQLParamData.  
        SQL_NTS,      // BufferLength: For a table-valued parameter this is the length of the type name or SQL_NTS.  
        &cbTVP);      // StrLen_or_IndPtr: For a table-valued parameter this is the number of rows input and output.  
  
    // 3 - OrdNo output  
    r = SQLBindParameter(hstmt, 3, SQL_PARAM_OUTPUT, SQL_C_LONG, SQL_INTEGER, 0, 0, &OrdNo,  
        sizeof(SQLINTEGER), &cbOrdNo);  
    // 4- OrdDate output  
    r = SQLBindParameter(hstmt, 4, SQL_PARAM_OUTPUT, SQL_C_CHAR, SQL_TYPE_TIMESTAMP, 23, 3, &OrdDate,  
        sizeof(OrdDate), &cbOrdDate);  
    ```  
  
3.  绑定表值参数的列。  
  
    ```  
    // Bind the table-valued parameter columns.  
    // First set focus on param 2  
    r = SQLSetStmtAttr(hstmt, SQL_SOPT_SS_PARAM_FOCUS, (SQLPOINTER) 2, SQL_IS_INTEGER);  
  
    // ProdCode  
    r = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 0, 0, &ProdCode,  
        sizeof(SQLINTEGER), &cbProdCode);  
    // Qty  
    r = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 0, 0, &Qty,   
       sizeof(SQLINTEGER), &cbQty);  
  
    // Reset param focus  
    r = SQLSetStmtAttr(hstmt, SQL_SOPT_SS_PARAM_FOCUS, (SQLPOINTER) 0, SQL_IS_INTEGER);  
    ```  
  
4.  初始化参数。 本示例将表值参数的大小设置为 SQL_DATA_AT_EXEC，而非行计数。  
  
    ```  
    // Initialze the TVP for row streaming.  
    cbTVP = SQL_DATA_AT_EXEC;  
  
    // Populate non-data-at-exec parameters.  
    strcpy_s((char *) CustCode ,sizeof(CustCode), "CUST1"); cbCustCode = SQL_NTS;  
    ```  
  
5.  调用该过程。 SQLExecDirect 将返回 SQL_NEED_DATA，因为表值参数是执行时数据参数。  
  
    ```  
    // Call the procedure  
    r = SQLExecDirect(hstmt, (SQLCHAR *) "{call TVPOrderEntry(?, ?, ?, ?)}",SQL_NTS);  
    ```  
  
6.  提供执行时数据参数的数据。 当返回 SQLParamData *ParameterValuePtr*对于表值参数，该应用程序必须做好列下一步的行或表值的行。 然后在应用程序调用使用 SQLPutData *DataPtr*设置为可用 （在此示例中，1） 的行数和*StrLen_or_IndPtr*设置为 0。  
  
    ```  
    // Check if parameter data is required, and get the first parameter ID token  
    if (r == SQL_NEED_DATA) {  
        r = SQLParamData(hstmt, &ParamId);  
    }  
  
    // Supply parameter row data.  
    int rowNum = 0;  
    while (r == SQL_NEED_DATA) {  
        if (ParamId == TVP) {  
       switch (rowNum) {  
           case 0: // Supply data for 1st row  
          // Populate input table-valued parameter row constituent columns.  
          ProdCode = 1215;   cbProdCode = sizeof(SQLINTEGER);   
          Qty = 5;      cbQty = sizeof(SQLINTEGER);  
          // Returning 1 for StrLenOrIndPtr indicates that a row is available.  
          r = SQLPutData(hstmt, (SQLPOINTER) 1, 1);  
          rowNum++;  
          break;  
  
           case 1: // Supply data for the second row.  
          // Populate another table-valued parameter row as above.  
          ProdCode = 1017;   cbProdCode = sizeof(SQLINTEGER);   
          // This time supply Qty through SQLPutData.  
          Qty = 0;      cbQty = SQL_DATA_AT_EXEC;   
          r = SQLPutData(hstmt, (SQLPOINTER) 1, 1);  
          rowNum++;  
          break;  
  
        default:  
          // Passing 0 in StrLenOrIndPtr indicates that no more table-valued parameter rows are available.  
          r = SQLPutData(hstmt, (SQLPOINTER) 1, 0);  
          break;  
           }  
        }  
        else {  
           if (ParamId == &Qty) {  
          Qty = 2;  
          // For a character or binary parameter, SQLPutData could be called  
          // multiple times to pass the value in pieces.  
          SQLPutData(hstmt, &Qty, sizeof(SQLINTEGER));  
           }  
       }  
       // Signal that parameter data is available, and get the token for   
       // the next parameter.  
       r = SQLParamData(hstmt, &ParamId);  
        }  
    }  
    ```  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  
 此示例演示您可以使用行流式处理，每次调用 SQLPutData 对 ODBC TVP，类似于如何使用 BCP.exe 将数据加载到数据库的一个行。  
  
 在构建此示例之前，请更改连接字符串中的服务器名称。  
  
 此示例使用默认数据库。 在运行此示例之前，请在您将使用的数据库中运行以下命令：  
  
```  
create table MCLOG (  
   biSeqNo bigint,   
   iSeries int,   
   bmRestData varbinary(max)  
)  
go  
  
-- Table type definition  
create type MCLOGType   
   as table(biSeqNo bigint, iSeries int, bmRestData varbinary(max) )  
go  
  
-- Insert procedure  
create procedure MCLOGInsert (@TableVariable MCLOGType READONLY)  
   as  
   insert into MCLog(biSeqNo,  iSeries, bmRestData)   
   select biSeqNo, iSeries, bmRestData from @TableVariable    
go  
```  
  
### <a name="code"></a>代码  
  
```  
#define UNICODE  
#define _UNICODE  
#define _SQLNCLI_ODBC_  
  
#include <windows.h>  
#include <tchar.h>  
#include <sqlext.h>  
#include "sqlncli.h"  
  
// link to sqlncli11.lib  
  
#define SUCCESS(x) ( \  
   !((x) & 0xFFFE) \  
   )  
  
#define CHKRC(stmt) { \  
   rc = (stmt); \  
   if (!SUCCESS(rc)) { \  
      _tprintf(_T(#stmt) _T(" failed with rc = %ld\r\n"), rc); \  
      goto EXIT; \  
   } \  
};  
  
void PrintError(SQLSMALLINT HandleType, SQLHANDLE Handle) {  
   RETCODE rc = SQL_SUCCESS;  
   SQLTCHAR szSqlState[6];  
   SQLTCHAR szMessage[1024];  
   SQLSMALLINT i = 1;  
   SQLSMALLINT msgLen = 0;  
   SQLINTEGER NativeError;  
  
   i = 1;  
   while ( (rc = SQLGetDiagRec(HandleType, Handle, i, szSqlState, &NativeError, szMessage, sizeof(szMessage)/sizeof(SQLTCHAR), &msgLen)) != SQL_NO_DATA) {  
      if (!SUCCESS(rc))  
         break;  
      szMessage[msgLen] = 0;  
      szSqlState[5] = 0;  
      _tprintf(_T("SQLState=%s, NativeError=%ld, Message=%s\r\n"), szSqlState, NativeError, szMessage);  
      i++;  
   }  
}  
  
int main() {  
   RETCODE rc = SQL_SUCCESS;  
   HENV henv = SQL_NULL_HENV;  
   HDBC hdbc = SQL_NULL_HDBC;  
   SQLHSTMT hstmt = SQL_NULL_HSTMT;  
   SQLTCHAR * pszConnection = _T("DRIVER={SQL Server Native Client 10.0};Server=your_servername;Trusted_Connection=Yes;");  
  
   // insert one TVP parameter  
   SQLTCHAR * pszInsertStmt = _T("{call MCLOGInsert(?)}");  
   SQLLEN cbParamLength;  
   SQLULEN cMaxRows = 3;  
  
   CHKRC(SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HENV, &henv));  
   CHKRC(SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0));  
   CHKRC(SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc));  
   CHKRC(SQLSetConnectAttr(hdbc, SQL_ATTR_LOGIN_TIMEOUT,reinterpret_cast<SQLPOINTER>(60),SQL_IS_UINTEGER));  
   CHKRC(SQLDriverConnect(hdbc, NULL, pszConnection, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT));  
   CHKRC(SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt));  
   CHKRC(SQLPrepare(hstmt, pszInsertStmt, SQL_NTS));  
  
   // Bind the first parameter  
   CHKRC(SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_DEFAULT, SQL_SS_TABLE, cMaxRows, 0, (SQLPOINTER)1, 0, &cbParamLength));  
   // If the stored procedure is executed as T-SQL ("exec sp_insert ?, ?"), you will supply the type name.  
   // CHKRC(SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_DEFAULT, SQL_SS_TABLE, cMaxRows, 0, (SQLPOINTER)lpszTVPParamType, SQL_NTS, &cbParamLengths));  
  
   // bind TVP columns  
   CHKRC(SQLSetStmtAttr(hstmt, SQL_SOPT_SS_PARAM_FOCUS, (SQLPOINTER)1, SQL_IS_INTEGER));  
  
   // for each TVP column, you can define an array to send more than one row for each SQLPutData call.  
   LONGLONG llSeqNo;  
   SQLLEN cbSeqNo = sizeof(LONGLONG);  
   LONG lSeries;  
   SQLLEN cbSeries = sizeof(LONG);  
   BYTE rgbRestData[2048];  
   SQLLEN cbRestData = SQL_DATA_AT_EXEC;  
   SQLUSMALLINT iColumn = 1;  
  
   // Bind biSeqNo   
   CHKRC(SQLBindParameter(hstmt, iColumn, SQL_PARAM_INPUT, SQL_C_SBIGINT, SQL_BIGINT, sizeof(LONGLONG), 0, (SQLPOINTER)&llSeqNo, sizeof(llSeqNo), &cbSeqNo));  
  
   // Bind iSeries   
   iColumn++;  
   CHKRC(SQLBindParameter(hstmt, iColumn, SQL_PARAM_INPUT, SQL_C_SLONG, SQL_INTEGER, sizeof(LONG), 0, (SQLPOINTER)&lSeries, sizeof(lSeries), &cbSeries));  
  
   // Bind bmRestData   
   iColumn++;  
   CHKRC(SQLBindParameter(hstmt, iColumn, SQL_PARAM_INPUT, SQL_C_BINARY, SQL_VARBINARY, 0, 0, (SQLPOINTER)rgbRestData, 0, &cbRestData));  
   CHKRC(SQLSetStmtAttr(hstmt, SQL_SOPT_SS_PARAM_FOCUS, (SQLPOINTER)0, SQL_IS_INTEGER));  
  
   // Set cbParamLength to SQL_DATA_AT_EXEC to indicate the TVP parameter is bound as DAE.  
   cbParamLength = SQL_DATA_AT_EXEC;  
   rc = SQLExecute(hstmt);  
  
   if (rc == SQL_NEED_DATA) {  
      SQLPOINTER ptr = NULL;  
      SQLULEN cRows = 0;  
  
      rc = ::SQLParamData(hstmt, &ptr);  
  
      while (rc == SQL_NEED_DATA) {  
         if (ptr == (SQLPOINTER)1) {  
            // it is the TVP parameter  
            if (cRows == cMaxRows) {  
               // We finish sending the last row already.  
               CHKRC(::SQLPutData(hstmt, NULL, 0));  
            }  
            else {  
               // StrLen_or_IndPtr can be changed to SQL_DATA_AT_EXEC or to a byte length before sending  
               // the actual TVP rows. SQL_DATA_AT_EXEC means send DAE data.  
               llSeqNo = cRows;  
               cbSeqNo = sizeof(LONGLONG);   // send as bound TVP column  
               lSeries = cRows + 100;  
               cbSeries = sizeof(LONG);   // send as bound TVP column  
               cbRestData = SQL_DATA_AT_EXEC;   // send as DAE TVP column  
               CHKRC(::SQLPutData(hstmt, (SQLPOINTER)1, 1));  
               cRows++;  
            }  
         }  
         else if (ptr == (SQLPOINTER)rgbRestData)  
            // varbinary(max) column.  Send data in parts.  
            for ( int i = 0 ; i < 3 ; i++ ) {  
               // Obtain the data in part from somewhere, here we just set all bytes to 'a'.  
               ::memset(rgbRestData, 'a', sizeof(rgbRestData));  
               CHKRC(::SQLPutData(hstmt, (SQLPOINTER)rgbRestData, sizeof(rgbRestData)));  
            }  
         else   
            // handling other DAE parameters, but in our case, we don't have other DAE parameters.  
            goto EXIT;  
         rc = ::SQLParamData(hstmt, &ptr);  
      }  
   }  
  
   if (hstmt)  
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
   if (hdbc) {  
      SQLDisconnect(hdbc);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
   }  
   if (henv)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
  
EXIT:  
   if (!SUCCESS(rc)) {  
      if (hstmt)  
         PrintError(SQL_HANDLE_STMT, hstmt);  
      if (hdbc)  
         PrintError(SQL_HANDLE_DBC, hdbc);  
      if(henv)  
         PrintError(SQL_HANDLE_ENV, henv);  
   }  
}  
```  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  
 此示例演示您可以使用行流式处理，每次调用 SQLPutData 对 ODBC TVP，类似于如何使用 BCP.exe 将数据加载到数据库的多个行。  
  
 在构建此示例之前，请更改连接字符串中的服务器名称。  
  
 此示例使用默认数据库。 在运行此示例之前，请在您将使用的数据库中运行以下命令：  
  
```  
create table MCLOG (  
   biSeqNo bigint,   
   iSeries int,   
   bmRestData varbinary(max)  
)  
go  
  
-- Table type definition  
create type MCLOGType   
   as table(biSeqNo bigint, iSeries int, bmRestData varbinary(max) )  
go  
  
-- Insert procedure  
create procedure MCLOGInsert (@TableVariable MCLOGType READONLY)  
   as  
   insert into MCLog(biSeqNo,  iSeries, bmRestData)   
   select biSeqNo, iSeries, bmRestData from @TableVariable    
go  
```  
  
### <a name="code"></a>代码  
  
```  
#define UNICODE  
#define _UNICODE  
#define _SQLNCLI_ODBC_  
  
#include <windows.h>  
#include <tchar.h>  
#include <sqlext.h>  
#include "sqlncli.h"  
  
// link to sqlncli11.lib  
  
#define SUCCESS(x) ( \  
   !((x) & 0xFFFE) \  
   )  
  
#define CHKRC(stmt) { \  
   rc = (stmt); \  
   if (!SUCCESS(rc)) { \  
      _tprintf(_T(#stmt) _T(" failed with rc = %ld\r\n"), rc); \  
      goto EXIT; \  
   } \  
};  
  
void PrintError(SQLSMALLINT HandleType, SQLHANDLE Handle) {  
   RETCODE rc = SQL_SUCCESS;  
   SQLTCHAR szSqlState[6];  
   SQLTCHAR szMessage[1024];  
   SQLSMALLINT i = 1;  
   SQLSMALLINT msgLen = 0;  
   SQLINTEGER NativeError;  
  
   i = 1;  
   while ( (rc = SQLGetDiagRec(HandleType, Handle, i, szSqlState, &NativeError, szMessage, sizeof(szMessage)/sizeof(SQLTCHAR), &msgLen)) != SQL_NO_DATA) {  
      if (!SUCCESS(rc))  
         break;  
      szMessage[msgLen] = 0;  
      szSqlState[5] = 0;  
      _tprintf(_T("SQLState=%s, NativeError=%ld, Message=%s\r\n"), szSqlState, NativeError, szMessage);  
      i++;  
   }  
}  
  
int main() {  
   RETCODE rc = SQL_SUCCESS;  
   HENV henv = SQL_NULL_HENV;  
   HDBC hdbc = SQL_NULL_HDBC;  
   SQLHSTMT hstmt = SQL_NULL_HSTMT;  
   SQLTCHAR * pszConnection = _T("DRIVER={SQL Server Native Client 10.0};Server=MyServer;Trusted_Connection=Yes;");  
  
   // insert one TVP parameter  
   SQLTCHAR * pszInsertStmt = _T("{call MCLOGInsert(?)}");  
   SQLLEN cbParamLength;  
   SQLULEN cMaxRows = 9;  
  
   CHKRC(SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HENV, &henv));  
   CHKRC(SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0));  
  
   CHKRC(SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc));  
   CHKRC(SQLSetConnectAttr( hdbc, SQL_ATTR_LOGIN_TIMEOUT, reinterpret_cast<SQLPOINTER>(60), SQL_IS_UINTEGER));  
   CHKRC(SQLDriverConnect( hdbc, NULL, pszConnection, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT));   
   CHKRC(SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt));  
   CHKRC(SQLPrepare(hstmt, pszInsertStmt, SQL_NTS));  
  
   // Bind the first parameter  
   CHKRC(SQLBindParameter( hstmt, 1, SQL_PARAM_INPUT, SQL_C_DEFAULT, SQL_SS_TABLE, cMaxRows, 0, (SQLPOINTER)1, 0, &cbParamLength));  
  
   /*  
   // If the stored procedure is executed as T-SQL ("exec sp_insert ?, ?"), then, supply the type name.  
   CHKRC(SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_DEFAULT, SQL_SS_TABLE, cMaxRows, 0, (SQLPOINTER)lpszTVPParamType, SQL_NTS, &cbParamLengths));  
   */  
  
   // bind TVP columns.  
   CHKRC(SQLSetStmtAttr( hstmt, SQL_SOPT_SS_PARAM_FOCUS, (SQLPOINTER)1, SQL_IS_INTEGER));   
  
   // For the first and the second TVP columns (bigint, int), always send them as bound.   
   // For the third column varbinary(max), either send them as bound or DAE.  
   const size_t ARRAY_SIZE = 3;  
   LONGLONG llSeqNo[ARRAY_SIZE];  
   SQLLEN cbSeqNo[ARRAY_SIZE] = {sizeof(LONGLONG), sizeof(LONGLONG), sizeof(LONGLONG)};  
   LONG lSeries[ARRAY_SIZE];  
   SQLLEN cbSeries[ARRAY_SIZE] = {sizeof(LONG), sizeof(LONG), sizeof(LONG)};  
   BYTE rgbRestData[ARRAY_SIZE][2048];  
   SQLLEN cbRestData[ARRAY_SIZE] = {sizeof(rgbRestData[0]), sizeof(rgbRestData[0]), sizeof(rgbRestData[0])};  
   SQLUSMALLINT iColumn = 1;  
  
   // Bind biSeqNo   
   CHKRC(SQLBindParameter( hstmt, iColumn, SQL_PARAM_INPUT, SQL_C_SBIGINT, SQL_BIGINT, sizeof(LONGLONG), 0, (SQLPOINTER)&llSeqNo, sizeof(llSeqNo[0]), cbSeqNo));  
  
   // Bind iSeries   
   iColumn++;  
   CHKRC(SQLBindParameter( hstmt, iColumn, SQL_PARAM_INPUT, SQL_C_SLONG, SQL_INTEGER, sizeof(LONG), 0, (SQLPOINTER)&lSeries, sizeof(lSeries[0]), cbSeries));  
  
   // Bind bmRestData   
   iColumn++;  
   CHKRC(SQLBindParameter(hstmt, iColumn, SQL_PARAM_INPUT, SQL_C_BINARY, SQL_VARBINARY, 0, 0, (SQLPOINTER)rgbRestData, sizeof(rgbRestData[0]), cbRestData));  
  
   CHKRC(SQLSetStmtAttr(hstmt, SQL_SOPT_SS_PARAM_FOCUS, (SQLPOINTER)0, SQL_IS_INTEGER));  
  
   // Set cbParamLength to SQL_DATA_AT_EXEC to indicate the TVP parameter is bound as DAE.  
   cbParamLength = SQL_DATA_AT_EXEC;  
   rc = SQLExecute(hstmt);  
  
   if (rc == SQL_NEED_DATA) {  
      SQLPOINTER ptr = NULL;  
      SQLUINTEGER cRows = 0;  
  
      rc = ::SQLParamData(hstmt, &ptr);  
  
      while (rc == SQL_NEED_DATA) {  
         if (ptr == (SQLPOINTER)1) {  
            // it is the TVP parameter  
            if (cRows >= cMaxRows) {  
               // We finish sending the last row already.  
               CHKRC(::SQLPutData(hstmt, NULL, 0));  
            }  
            else {  
               // Obtaining row data from somewhere. In this case we will fill 3 rows.  
               for (size_t i = 0; i < ARRAY_SIZE; i++) {  
                  llSeqNo[i] = cRows + i + 1;  
                  lSeries[i] = llSeqNo[i] * 10;  
  
                  // Now fill the varbinary(max) column.  Assume that the even row can't be fit into   
                  // the buffer provided as send them as DAE.  
                  if (!((cRows + i) % 2)) {  
                     // SQL_DATA_AT_EXEC means send DAE data.  
                     cbRestData[i] = SQL_DATA_AT_EXEC;  
                  }  
                  else {  
                     // data can fit into the buffer, then copy the data to the buffer directly.  
                     cbRestData[i] = 100;  
                     ::memset(&rgbRestData[i], 'b', cbRestData[i]);  
                  }  
               }  
               CHKRC(::SQLPutData(hstmt, (SQLPOINTER)1, ARRAY_SIZE));  
               cRows += ARRAY_SIZE;  
            }  
         }  
         else if ((SQLPOINTER)&rgbRestData[0] <= ptr && ptr <= (SQLPOINTER)&rgbRestData[ARRAY_SIZE-1]) {  
            // it is varbinary(max) column  
            // Send data in parts.  
            for (int i = 0; i < 3; i++) {  
               // Obtain the data in part from somewhere, here we just set all bytes to 'a'.  
               ::memset(ptr, 'a', sizeof(rgbRestData[0]));  
               CHKRC(::SQLPutData(hstmt, (SQLPOINTER)ptr, sizeof(rgbRestData[0])));  
            }  
         }  
         else {  
            // handling other DAE parameters, but in our case, we don't have other DAE parameters.  
            goto EXIT;  
         }  
         rc = ::SQLParamData(hstmt, &ptr);  
      }  
   }  
  
EXIT:  
   if (!SUCCESS(rc)) {  
      if (hstmt)   
         PrintError(SQL_HANDLE_STMT, hstmt);  
      if (hdbc)  
         PrintError(SQL_HANDLE_DBC, hdbc);  
      if(henv)  
         PrintError(SQL_HANDLE_ENV, henv);  
   }  
  
   if (hstmt)  
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
   if (hdbc) {  
      SQLDisconnect(hdbc);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
   }  
   if (henv)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="see-also"></a>请参阅  
 [ODBC 表值参数编程示例](https://msdn.microsoft.com/library/3f52b7a7-f2bd-4455-b79e-d015fb397726)  
  
  
