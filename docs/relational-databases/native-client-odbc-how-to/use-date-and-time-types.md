---
title: 使用日期和时间类型 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: a2aa5644-1e39-4d78-b149-0599d3502cda
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9de540ec1ba30ea4cfe4ff45bcbe3a203eb0ceeb
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67937446"
---
# <a name="use-date-and-time-types"></a>使用日期和时间类型
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  此示例显示如何对 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 中增加的日期/时间数据结构进行初始化。 随后准备输入值、绑定参数，并执行查询。 有关使用这些类型的详细信息，请参阅[日期和时间改进&#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="example"></a>示例  
 您将需要一个名为 DateTime 的 ODBC 数据源。 DateTime 的默认数据库应为 tempdb。 此数据源必须基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 的 ODBC 驱动程序。  
  
 如果您要将此示例构建为在 64 位操作系统上运行的 32 位应用程序并运行该示例，则必须使用 %windir%\SysWOW64\odbcad32.exe 中的 ODBC 管理器创建 ODBC 数据源。  
  
 此示例连接到您的计算机上默认的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 若要连接到命名实例，请更改 ODBC 数据源的定义以使用以下格式指定实例：server\namedinstance。 默认情况下，[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 将安装在命名实例中。  
  
 第一个 ( [!INCLUDE[tsql](../../includes/tsql-md.md)]) 代码列表创建此示例使用的表。  
  
 使用 odbc32.lib 和 user32.lib 编译第二个 (C++) 代码列表。 请确保您的 INCLUDE 环境变量包括含有 sqlncli.h 的目录。  
  
 第三个 ( [!INCLUDE[tsql](../../includes/tsql-md.md)]) 代码列表删除此示例使用的表。  
  
```sql
use tempdb  
GO  
  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'DateTimeTypes')  
DROP TABLE DateTimeTypes  
GO  
  
CREATE TABLE DateTimeTypes (datecol date, time2col time(7), datetime2col datetime2(7), datetimeoffsetcol datetimeoffset(7))  
GO  
```  
  
```cpp
// compile with: odbc32.lib user32.lib  
#include <windows.h>  
#include <Sqlext.h>  
#include <mbstring.h>  
#include <sqlncli.h>  
#include <stdio.h>  
  
#define MAX_DATA 1024  
#define MYSQLSUCCESS(rc) ( (rc == SQL_SUCCESS) || (rc == SQL_SUCCESS_WITH_INFO) )  
  
class direxec {  
   RETCODE rc;   // ODBC return code  
   HENV henv;   // Environment  
   HDBC hdbc;   // Connection Handle  
   HSTMT hstmt;   // Statement Handle  
   SQLHDESC hdesc;   // Descriptor handle  
   unsigned char szData[MAX_DATA];   // Returned Data Storage  
   SDWORD cbData;   // Output Length of data  
   unsigned char char_ds_name[SQL_MAX_DSN_LENGTH];   // Data Source Name  
  
   SQL_DATE_STRUCT date;   // date structure  
   SQL_SS_TIME2_STRUCT time2;   // time2 structure  
   SQL_TIMESTAMP_STRUCT datetime2;   // datetime2 structure  
   SQL_SS_TIMESTAMPOFFSET_STRUCT dateTimeOffset;   // datetimeoffset structure  
  
   SQLLEN cbdate;   // size of date structure  
   SQLLEN cbtime2;   // size of time structure  
   SQLLEN cbdatetime2;   // size of datetime2  
   SQLLEN cbtimestampoffset;   //size of dateTimeOffset  
  
public:  
   direxec();   // Constructor  
   void sqlconn();   // Allocate env, stat and conn  
  
   void sqldisconn();   // Free pointers to env, stat, conn and disconnect  
   void error_out();   // Display errors  
   void check_rc(RETCODE rc);   // Checks for success of the return code  
  
   void sqlinsert();   // Insert into the table  
};  
  
// Constructor initializes the string char_ds_name with the data source name and  
// initialize the data structures to with the date to be inserted.  
direxec::direxec() {  
   _mbscpy_s(char_ds_name, (const unsigned char *)"DateTime");  
  
   // Initialize the date structure  
   date.day = 12;  
   date.month = 10;  
   date.year = 2001;  
  
   // Initialize the time structure  
   time2.hour = 21;  
   time2.minute = 45;  
   time2.second = 52;  
   time2.fraction = 100  ;  
  
   // Initialize the datetime2 structure  
   datetime2.year = 2007;  
   datetime2.month = 12;  
   datetime2.day = 26;  
   datetime2.hour = 0;  
   datetime2.minute = 0;  
   datetime2.second = 0;  
   datetime2.fraction = 100;   
  
   // Initialize the timestampoffset structure  
   dateTimeOffset.year = 2007;  
   dateTimeOffset.month = 3;  
   dateTimeOffset.day = 11;  
   dateTimeOffset.hour = 2;  
   dateTimeOffset.minute = 30;  
   dateTimeOffset.second = 29;  
   dateTimeOffset.fraction = 200;  
   dateTimeOffset.timezone_hour = -8;  
   dateTimeOffset.timezone_minute = 0;  
  
   // Size of structures   
   cbdate = sizeof(SQL_DATE_STRUCT);  
   cbtime2 = sizeof(SQL_SS_TIME2_STRUCT);  
   cbdatetime2 = sizeof(SQL_TIMESTAMP_STRUCT);  
   cbtimestampoffset = sizeof(SQL_SS_TIMESTAMPOFFSET_STRUCT);  
  
}   // direxec  
  
// Allocate environment handles, connection handle, connect to data source, and allocate statement handle  
void direxec::sqlconn() {  
   // Allocate the environment handle  
   rc = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   check_rc(rc);  
  
   // Set the ODBC version to version 3  
   rc = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
   check_rc(rc);  
  
   // Allocate the database connection handle  
   rc = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   check_rc(rc);  
  
   // Connect to the database  
   rc = SQLConnect(hdbc, char_ds_name, SQL_NTS, NULL, 0, NULL, 0);  
   check_rc(rc);  
  
   // Allocate the statement handle  
   rc = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
   check_rc(rc);    
  
   // Allocate the descriptor handle  
   rc = rc = SQLAllocHandle(SQL_HANDLE_DESC, hdbc, &hdesc);  
   check_rc(rc);  
  
}   // direxec::sqlconn  
  
// Display error message from the DiagRecord  
void direxec::error_out() {  
   // String to hold the SQL State  
   unsigned char szSQLSTATE[10];  
  
   // Error code  
   SDWORD nErr;  
  
   // The error message  
   unsigned char msg[SQL_MAX_MESSAGE_LENGTH + 1];  
  
   // Size of the message  
   SWORD cbmsg;  
  
   // If hstmt is not null use that for getting the DiagRec  
   if (hstmt)  
      rc = SQLGetDiagRec(SQL_HANDLE_STMT, hstmt, 1, szSQLSTATE, &nErr, msg, sizeof(msg), &cbmsg);  
   // else get the diag record from the env  
   else  
      rc = SQLGetDiagRec(SQL_HANDLE_ENV, henv, 1, szSQLSTATE, &nErr, msg, sizeof(msg), &cbmsg);  
  
   // If the rc is successful, show the message using a message box  
   if ( rc == SQL_SUCCESS) {  
      char hold_err[100];  
      _itoa_s(nErr, hold_err, 100, 10);  
      _snprintf_s((char *)szData, MAX_DATA, MAX_DATA - 1, "%s" "%s" "%s" "%s" "%s" "%s" "%s" "%s",   
        "Error:", "\n", "SQLSTATE= ", szSQLSTATE, ", Native error=", hold_err, ", msg = ", msg);  
      MessageBox(NULL, (const char *)szData, "ODBC Error", MB_OK);  
   }  
}   // direxec::error_out  
  
// Checks the return code.  If failure, displays the error, free the memory and exits the program  
void direxec::check_rc(RETCODE rc) {  
   if (!MYSQLSUCCESS(rc)) {  
      error_out();  
      SQLFreeEnv(henv);  
      SQLFreeConnect(hdbc);  
      exit(-1);  
   }   
}   // direxec::check_rc  
  
// Function to insert dates into the table.  
void direxec::sqlinsert() {     
   rc = SQLPrepare(hstmt, (SQLCHAR *) "INSERT INTO DateTimeTypes (datecol, time2col, datetime2col, datetimeoffsetcol) VALUES (?, ?, ?, ?)", SQL_NTS);  
   check_rc(rc);  
  
   rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TYPE_DATE, 10, 0, &date, 0, &cbdate);  
   check_rc(rc);  
  
   rc = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_BINARY, SQL_SS_TIME2, 16, 7, &time2, 0, &cbtime2);  
   check_rc(rc);  
  
   rc = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_TIMESTAMP, SQL_TYPE_TIMESTAMP, 27, 7, &datetime2, 0, &cbdatetime2);  
   check_rc(rc);  
  
   rc = SQLBindParameter(hstmt, 4, SQL_PARAM_INPUT, SQL_C_BINARY, SQL_SS_TIMESTAMPOFFSET, 34, 7, &dateTimeOffset, 0, &cbtimestampoffset);  
   check_rc(rc);  
  
   rc = SQLExecute(hstmt);  
   check_rc(rc);  
}   // direxec::sqlinsert  
  
int main() {  
   direxec x;  
  
   // Allocate handles, and connect.  
   x.sqlconn();   
  
   // Insert all into the table  
   x.sqlinsert();  
}  
```  
  
```sql
USE tempdb  
GO  
  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'DateTimeTypes')  
DROP TABLE DateTimeTypes  
GO  
```  
  
  
