---
title: 结果集示例 |Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: reference
ms.assetid: a0590ba6-3856-4731-bb29-87b0a1c1b795
author: mashamsft
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 1dd5cec5623cfca499fcd4d1eb1ce93faec1dd36
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62782140"
---
# <a name="result-set-sample"></a>结果集示例
  有时，在通读查询结果时能够执行命令（而不需要打开新的连接并将所有结果读入内存）很有用。 ADO.NET 2.0 中的多个活动的结果集 (MARS) 功能就是一种能够帮助您实现以上操作的技术。 目前，用于服务器端编程的进程内的提供程序不能实现 MARS。 若要消除此限制，可以使用服务器端游标。 此示例说明如何使用服务器端游标解决对服务器端编程缺少 MARS 支持的问题。  
  
> [!CAUTION]  
>  使用服务器端游标要消耗大量的服务器资源，并且有时会阻碍 Microsoft SQL Server 中的查询优化器增强查询的性能。 因此，可以考虑重新编写代码来尽可能使用 `JOINs`。  
  
 除了可以在结果集中前后移动以外，此类的 API 与数据读取器相似；并且即使结果集处于打开状态，也可以在连接时发出其他命令。此实现过程经过较大程度的简化，可令该示例易于理解。 更有效的实现将提取多行来避免提取的每行的数据库周转。 与用查询的所有结果填充数据集比较，使用此类可以大大减少内存需求量，这对于服务器端编程是非常重要的。 此示例还说明如何使用“允许部分受信任的调用方”属性来指示结果集程序集是可以安全地从其他程序集进行调用的库。 与使用不安全的权限注册调用程序集相比，此方法稍微复杂一些，但要安全许多。 该方法更安全的原因是：它将调用程序集注册为安全的程序集，调用程序集将限制相关资源脱离服务器并防止对服务器的完整性造成损害。  
  
## <a name="prerequisites"></a>先决条件  
 若要创建和运行此项目，必须安装下列软件：  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express。 可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express 文档和示例[网站](https://go.microsoft.com/fwlink/?LinkId=31046)免费获取 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]开发人员[网站](https://go.microsoft.com/fwlink/?linkid=62796)提供的 AdventureWorks 数据库  
  
-   .NET Framework SDK 2.0 或更高版本，或 Microsoft Visual Studio 2005 或更高版本。 您可以免费获取 .NET Framework SDK。  
  
-   此外，还必须满足以下条件：  
  
-   使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例必须已启用 CLR 集成。  
  
-   若要启用 CLR 集成，请执行以下步骤：  
  
    #### <a name="enabling-clr-integration"></a>启用 CLR 集成  
  
    -   执行以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 命令：  
  
     `sp_configure 'clr enabled', 1`  
  
     `GO`  
  
     `RECONFIGURE`  
  
     `GO`  
  
    > [!NOTE]  
    >  若要启用 CLR，您必须具有 `ALTER SETTINGS` 服务器级别权限，`sysadmin` 和 `serveradmin` 固定服务器角色的成员隐式拥有该权限。  
  
-   必须在您使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上安装 AdventureWorks 数据库。  
  
-   如果你不是要使用的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的管理员，必须请管理员授予你 **CreateAssembly** 权限，才能完成安装。  
  
## <a name="building-the-sample"></a>生成示例  
  
#### <a name="create-and-run-the-sample-by-using-the-following-instructions"></a>按照以下说明创建和运行该示例：  
  
1.  打开 Visual Studio 或 .NET Framework 命令提示符。  
  
2.  如有必要，为您的示例创建目录。 对于此示例，我们将使用 C:\MySample。  
  
3.  在命令提示符处执行以下命令，创建强名称密钥文件。  
  
## <a name="sample-code"></a>示例代码  
 下面是此示例的代码列表。  
  
 此代码用于库 `ResultSet.`  
  
 C#  
  
```  
#region Using directives  
using System;  
using System.Collections;  
using System.Collections.Generic;  
using System.Text;  
using System.Data;  
using System.Data.Common;  
using System.Data.Sql;  
using System.Data.SqlTypes;  
using System.Data.SqlClient;  
using System.IO;  
using System.Globalization;  
using Microsoft.SqlServer.Server;  
using System.Runtime.Serialization;  
using System.Reflection;  
using System.Runtime.CompilerServices;  
#endregion  
  
[assembly: AssemblyVersion("1.0.*")]  
[assembly: System.CLSCompliant(true)]  
[assembly: System.Runtime.InteropServices.ComVisible(false)]  
[assembly: System.Security.Permissions.EnvironmentPermission(System.Security.Permissions.SecurityAction.RequestMinimum)]  
[assembly: System.Security.AllowPartiallyTrustedCallers]  
  
// A class which permits browsing of results while executing other commands by using server-side cursors.  
  
   // This is the base class for exceptions raised in the Adventure Works Cycles Storefront application.  
   [Serializable]  
   public class ResultSetException : System.Exception {  
      public ResultSetException() {}  
      public ResultSetException(string message) : base(message) {}  
      public ResultSetException(String message, Exception innerException) : base(message, innerException) {}  
      protected ResultSetException(SerializationInfo info, StreamingContext context) : base(info, context) {}  
   }  
  
   // This exception is raised when a user tries to register an email address which has already been taken.  
   [Serializable]  
   public class InvalidStateException : ResultSetException {  
      public InvalidStateException() {}  
      public InvalidStateException(string message) : base(message) {}  
      public InvalidStateException(String message, Exception innerException) : base(message, innerException) {}  
      protected InvalidStateException(SerializationInfo info, StreamingContext context) : base(info, context) {}  
   }  
  
   // This class is used to provide an API similar to a SQL data reader except that it is possible  
   // to navigate through any part of the result set.  It is also possible to execute commands while  
   // this class is still "open" even though the CLR in-proc provider does not support MARS at this time.  
   // This implementation is highly simplified to make the sample easy to understand.  A better implementation  
   // would fetch multiple rows to avoid a database turnaround per row fetched.  
   // Using this class can have a significantly smaller memory footprint than filling a dataset with   
   // all the results of a query which is very important for server side programming.  
  
   public class ResultSet : IDataReader, IDataRecord, IEnumerable, IDisposable {  
      // Which database to access  
      private SqlConnection connection;  
  
      // A cache or a single row's worth of data   
      private Microsoft.SqlServer.Server.SqlDataRecord results;  
  
      //A cache of whether the columns of the current cached row are set to the default value for the column  
      private int visibleFieldCount;  
  
      private string cursorName = string.Format(CultureInfo.InvariantCulture, "[{0}]", Guid.NewGuid().ToString());  
  
      internal Microsoft.SqlServer.Server.SqlDataRecord CurrentRecord {  
         get {  
            EnsureState(StateValues.read);  
            return results;  
         }  
      }  
  
      // Which database to access.  The connection passed to the setter must be open.  
  
      public SqlConnection Connection {  
         get {  
            return connection;  
         }  
  
         set {  
            if (value == null)  
               throw new ArgumentNullException("value");  
            if (state != StateValues.created && state != StateValues.initialized)  
               throw new InvalidStateException("Cannot alter select command after opening the result set.");  
            if (value.State != ConnectionState.Open)  
               throw new InvalidStateException("Connection must be opened prior to setting result set connection value.");  
            connection = value;  
            if (selectCommand != null && connection != null)  
               state = StateValues.initialized;  
            else  
               state = StateValues.created;  
         }  
      }  
  
      private string selectCommand;  //What query to execute against the database  
  
      public string SelectCommand {  
         get {  
            return selectCommand;  
         }  
  
         set {  
            if (state != StateValues.created && state != StateValues.initialized)  
               throw new InvalidStateException("Cannot alter select command after opening the result set.");  
            selectCommand = value;  
            if (selectCommand != null && connection != null)  
               state = StateValues.initialized;  
            else  
               state = StateValues.created;  
         }  
      }  
  
      private bool isScrollable = true;  
  
      public bool IsScrollable {  
         get {  
            return isScrollable;  
         }  
  
         set {  
            if (state != StateValues.created && state != StateValues.initialized)  
               throw new InvalidStateException("Cannot alter IsScrollable property after opening the result set.");  
            isScrollable = value;  
         }  
      }  
  
      // The possible phases this object can go through from creation through termination.  
      private enum StateValues {  
         created,    //Result set is instantiated but connection and select command are not set  
         initialized, //Result set is instantiated and connection and select command are set  
         opened, //The cursor has been created for browsing data  
         read, //At least one row of data has been read and cached.  
         closed  
      }; // The cursor has been closed and deallocated, and the connection is closed.   
      // No further operations are possible.  
  
      // What the current phase is  
      private StateValues state = StateValues.created;  
  
      // It is possible to create the result set with this constructor and then set the connection  
      // and select command using the public properties.  
      public ResultSet() {  
  
      }  
  
      // Or you can create the result set all at once using this form of the constructor.  
      // [System.CLSCompliant(false)]  
      public ResultSet(SqlConnection connection, string selectCommand) {  
         this.Connection = connection;  
         this.SelectCommand = selectCommand;  
         state = StateValues.initialized;  
      }  
  
      // By default the result set is scrollable, but you can create a forward-only  
      // result set using this constructor and specifying false for isScrollable.  
      // [System.CLSCompliant(false)]  
      public ResultSet(SqlConnection connection, string selectCommand, bool isScrollable) {  
         this.Connection = connection;  
         this.SelectCommand = selectCommand;  
         this.IsScrollable = isScrollable;  
         state = StateValues.initialized;  
      }  
  
      /// <summary>  
      /// This method should be called just before the result set is to be used to pull data.    
      /// The open doesn't happen as part of creation as server resources are being consumed  
      /// while the result set is open.  Be sure to close an open result set as soon as possible  
      /// to release server side resources.  
      /// </summary>  
  
      public void Open() {  
         switch (state) {  
            case StateValues.created:  
               throw new InvalidStateException("Cannot open result set until it is properly initialized");  
            case StateValues.opened:  
            case StateValues.read:  
               throw new InvalidStateException("Result set already open.");  
            case StateValues.closed:  
               throw new InvalidStateException("Cannot reopen closed result set.");  
            default:  
               break;  
         }  
         if (state == StateValues.initialized) {  
            //There are many possible cursor options.  We only allow control of scrollability.  
            //To keep the implementation simple we only permit read operations.  
            string scrollMode = (isScrollable) ? "SCROLL" : "FORWARD_ONLY";  
            //Caller must ensure that any user input which is part of selectCommand is free from  
            //injection attacks.  
            SqlCommand cmd = connection.CreateCommand();  
            cmd.CommandText = string.Format(CultureInfo.InvariantCulture, "DECLARE {0} CURSOR GLOBAL {1} READ_ONLY  FOR {2}; OPEN {0};",  
                cursorName, scrollMode, selectCommand);  
            cmd.ExecuteNonQuery();  
            state = StateValues.opened;  
         }  
      }  
  
      #region IDataReader Members  
  
      /// <summary>  
      /// Free all server and client side resources being consumed for this result set.  Most operations  
      /// are no longer valid after calling this method on the result set.  
      /// </summary>  
  
      public void Close() {  
         switch (state) {  
            case StateValues.created:  
            case StateValues.initialized:  
               throw new InvalidStateException("Cannot close unopened result set.");  
            case StateValues.closed:  
               throw new InvalidStateException("Result set already closed.");  
            case StateValues.opened:  
            case StateValues.read:  
               SqlCommand cmd = connection.CreateCommand();  
               cmd.CommandText =  
                   string.Format(CultureInfo.InvariantCulture, "CLOSE {0}; DEALLOCATE {0};", cursorName);  
               try {  
                  cmd.ExecuteNonQuery();  
               }  
               finally {  
                  results = null;  
                  state = StateValues.closed;  
               }  
               break;  
            default:  
               throw new InvalidStateException(  
                   string.Format(CultureInfo.InvariantCulture,  
                   "Unknown result set state {0}", state.ToString()));  
         }  
  
      }  
  
      /// <summary>  
      /// This implementation does not support multi-dimensional data  
      /// </summary>  
      /// <value>Always zero</value>  
      public int Depth {  
         get {  
            EnsureState(StateValues.read);  
            return 0;  
         }  
      }  
  
      /// <summary>  
      /// Normally this would return a data table with schema information about the data being returned.  
      /// Unfortunately this isn't implemented in the in-proc provider, so we always throw an exception.  
      /// </summary>  
      /// <returns>Always throws an NYI exception.</returns>  
  
      public DataTable GetSchemaTable() {  
         throw new NotImplementedException("GetSchemaTable is not currently implemented.");  
      }  
  
      /// <summary>  
      /// Determines if there is any data to read  
      /// </summary>  
      /// <value>True if there is data to read, otherwise false</value>  
  
      public bool HasRows {  
         get {  
            throw new NotImplementedException("HasRows is not currently implemented.");  
         }  
      }  
  
      /// <summary>  
      /// Determines if the result set is available for use  
      /// </summary>  
      /// <value>Returns false if the result set is available for use, otherwise true</value>  
      public bool IsClosed {  
         get { return state == StateValues.closed; }  
      }  
  
      /// <summary>  
      /// Multiple result sets are not supported.  
      /// </summary>  
      /// <returns>Always throws an invalid operation exception</returns>  
  
      public bool NextResult() {  
         throw new InvalidOperationException("Multiple result sets are not support for result set class");  
      }  
  
      /// <summary>  
      /// Fetches the next row from the result set on the server and makes that data available to the caller.  
      /// </summary>  
      /// <returns>True if and only if there is data to read</returns>  
      public bool Read() {  
         return fetch(string.Format(CultureInfo.InvariantCulture, "FETCH NEXT FROM {0};", cursorName));  
      }  
  
      /// <summary>  
      /// Returns the number of records affected by processing this result set.  This may only be called when  
      /// the result set is closed.  Since this implementation supports only read-only access, there   
      /// can never be any rows affected.  
      /// </summary>  
      /// <value>Always -1</value>  
      public int RecordsAffected {  
         get {  
            EnsureState(StateValues.closed);  
            return -1;  
         }  
      }  
  
      #endregion  
  
      #region Navigation methods  
  
      /// <summary>  
      /// Move to a particular record number in the results returned by the query.  Requires  
      /// a scrollable result set.  
      /// </summary>  
      /// <param name="position"></param>  
      /// <returns>True if and only if there is data to read</returns>  
  
      public bool ReadAbsolute(int position) {  
         EnsureScroll();  
         return fetch(string.Format(CultureInfo.InvariantCulture, "FETCH ABSOLUTE {0} FROM {1};",  
             position, cursorName));  
      }  
  
      /// <summary>  
      /// Move to the first result in the results returned by the query and read it.  Requires  
      /// a scrollable result set.  
      /// </summary>  
      /// <returns>True if and only if there is data to read</returns>  
      public bool ReadFirst() {  
         EnsureScroll();  
         return fetch(string.Format(CultureInfo.InvariantCulture, "FETCH FIRST FROM {0};", cursorName));  
      }  
  
      /// <summary>  
      /// Move to the last result in the results returned by the query and read it.  Requires  
      /// a scrollable result set.  
      /// </summary>  
      /// <returns>True if and only if there is data to read</returns>  
      public bool ReadLast() {  
         EnsureScroll();  
         return fetch(string.Format(CultureInfo.InvariantCulture, "FETCH LAST FROM {0};", cursorName));  
      }  
  
      /// <summary>  
      /// Move to the immediate prior result in the results returned by the query, and read it.    
      /// Requires a scrollable result set.  
      /// </summary>  
      /// <returns>True if and only if there is data to read</returns>  
      public bool ReadPrevious() {  
         EnsureScroll();  
         return fetch(string.Format(CultureInfo.InvariantCulture, "FETCH PREVIOUS FROM {0};", cursorName));  
      }  
  
      /// <summary>  
      /// Move the specified number of records forward or backward from the current position  
      /// in the results returned by the query and read it.  Requires  
      /// a scrollable result set.  
      /// </summary>  
      /// <param name="position">How many records forward or backward</param>  
      /// <returns>True if and only if there is data to read</returns>  
      public bool ReadRelative(int position) {  
         EnsureScroll();  
         return fetch(string.Format(CultureInfo.InvariantCulture, "FETCH RELATIVE {0} FROM {1};", position, cursorName));  
      }  
  
      #endregion  
  
      #region IDisposable Members  
  
      /// <summary>  
      /// Dispose is idential to closing the result set.  
      /// </summary>  
      public void Dispose() {  
         Dispose(true);  
      }  
  
      protected virtual void Dispose(bool shouldClearManagedResources) {  
         Close();  
         if (shouldClearManagedResources)  
            connection = null;  
      }  
  
      #endregion  
  
      #region IDataRecord Members  
      /// <summary>  
      /// The number of columns available in the results just read  
      /// </summary>  
      /// <value>The number of columns</value>  
      public int FieldCount {  
         get {  
            EnsureState(StateValues.read);  
            return results.FieldCount;  
         }  
      }  
  
      public int VisibleFieldCount {  
         get {  
            EnsureState(StateValues.read);  
            return visibleFieldCount;  
         }  
      }  
  
      public IDataReader GetData(int i) {  
         throw new InvalidOperationException("GetData is no longer implemented");  
      }  
  
      /// <summary>  
      /// What kind of data is acceptable for the specified column  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>What kind of data is acceptable</returns>  
      public string GetDataTypeName(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetDataTypeName(i);  
      }  
  
      /// <summary>  
      /// Returns the object which represents the kind of data which is   
      /// acceptable for the specified column.  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The object which represents the kind of data is which acceptable</returns>  
      public Type GetFieldType(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetFieldType(i);  
      }  
  
      /// <summary>  
      /// Returns the symbolic id of the specified column  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The symbolic id (name) of the column</returns>  
      public string GetName(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetName(i);  
      }  
  
      /// <summary>  
      /// Given a column name, returns the integer id the column.  
      /// </summary>  
      /// <param name="name">Which column</param>  
      /// <returns>The integer id of the column</returns>  
      public int GetOrdinal(string name) {  
         EnsureState(StateValues.read);  
         //Note: this implementation does not deal with all collation and internationalization issues.  
         return results.GetOrdinal(name);  
      }  
  
      /// <summary>  
      /// Places all the values contained in the current row into the supplied object array.  The elements  
      /// of the array are based on the SQL type system.  
      /// </summary>  
      /// <param name="values">Where to put the row values</param>  
      /// <returns>How many columns of values were placed in the array</returns>  
      public int GetSqlValues(object[] values) {  
         EnsureState(StateValues.read);  
         return results.GetSqlValues(values);  
      }  
  
      /// <summary>  
      /// Places all the values contained in the current row into the supplied object array.  The   
      /// elements of the array are based on the CLR type system.  
      /// </summary>  
      /// <param name="values">Where to put the values</param>  
      /// <returns>How many columns of data were put in the array</returns>  
      public int GetValues(object[] values) {  
         EnsureState(StateValues.read);  
         return results.GetValues(values);  
      }  
  
      /// <summary>  
      /// Accesses the data in the specified column by name  
      /// </summary>  
      /// <param name="name">Which column to access</param>  
      /// <returns>What was contained in the specified column for the current row</returns>  
      public object this[string name] {  
         get {  
            EnsureState(StateValues.read);  
            return results[name];  
         }  
      }  
  
      /// <summary>  
      /// Accesses the data in the specified column by numerical id  
      /// </summary>  
      /// <param name="i">Which column to access</param>  
      /// <returns>The data in the specified column</returns>  
      public object this[int i] {  
         get {  
            EnsureState(StateValues.read);  
            return results[i];  
         }  
      }  
  
      //// Methods in this region provide access to column data for the current row as   
      //// objects using the Sql type system.  The advantage of using this type system is that  
      //// you can deal very precisely with the database's concept of null.  Also, some types   
      //// (such as SqlXml) provide more efficient methods of data access than the CLR type system.  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlBinary object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlBinary</returns>  
      public SqlBinary GetSqlBinary(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlBinary(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlBoolean object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlBoolean</returns>  
      public SqlBoolean GetSqlBoolean(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlBoolean(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlByte object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlByte</returns>  
      public SqlByte GetSqlByte(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlByte(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlBytes object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlBytes</returns>  
      public SqlBytes GetSqlBytes(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlBytes(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlChars object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlChars</returns>  
      public SqlChars GetSqlChars(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlChars(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlDateTime object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlDateTime</returns>  
      public SqlDateTime GetSqlDateTime(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlDateTime(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlDecimal object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlDecimal</returns>  
      public SqlDecimal GetSqlDecimal(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlDecimal(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlDouble object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlDouble</returns>  
      public SqlDouble GetSqlDouble(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlDouble(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlGuid object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlGuid</returns>  
      public SqlGuid GetSqlGuid(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlGuid(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlInt16 object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlInt16</returns>  
      public SqlInt16 GetSqlInt16(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlInt16(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlInt32 object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlInt32</returns>  
      public SqlInt32 GetSqlInt32(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlInt32(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlInt64 object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlInt64</returns>  
      public SqlInt64 GetSqlInt64(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlInt64(i);  
      }  
  
      /// <summary>  
      /// Return schema information for the specified column  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>Schema information for the column represented by a SqlMetaData object</returns>  
      public Microsoft.SqlServer.Server.SqlMetaData GetSqlMetaData(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlMetaData(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlMoney object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlMoney</returns>  
      public SqlMoney GetSqlMoney(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlMoney(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlSingle object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlSingle</returns>  
      public SqlSingle GetSqlSingle(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlSingle(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlString object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlString</returns>  
      public SqlString GetSqlString(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlString(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a Sql object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a Sql object</returns>  
      public object GetSqlValue(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlValue(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a SqlXml object  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as SqlXml</returns>  
      public SqlXml GetSqlXml(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetSqlXml(i);  
      }  
  
      //// Methods in this region provide access to column data for the current row  
      //// using the CLR type system.  The advantage of using these methods is seamless integration  
      //// with other CLR components.  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a Boolean  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as Boolean</returns>  
      public bool GetBoolean(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetBoolean(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a byte   
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a byte</returns>  
      public byte GetByte(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetByte(i);  
      }  
  
      /// <summary>  
      /// Copies bytes from the specified column of the current row into a buffer supplied by  
      /// the caller.  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <param name="fieldOffset">Where to start in the column's data</param>  
      /// <param name="buffer">Where to place the bytes</param>  
      /// <param name="bufferOffset">Where to start in the buffer</param>  
      /// <param name="length">How many bytes to copy</param>  
      /// <returns>How many bytes were actually copied</returns>  
      public long GetBytes(int i, long fieldOffset, byte[] buffer, int bufferoffset, int length) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetBytes(i, fieldOffset, buffer, bufferoffset, length);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a  char.  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a char</returns>  
      public char GetChar(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetChar(i);  
      }  
  
      /// <summary>  
      /// Copies characters from the specified column of the current row into a buffer supplied by  
      /// the caller.  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <param name="fieldoffset">Where to start in the column's data</param>  
      /// <param name="buffer">Where to place the characters</param>  
      /// <param name="bufferoffset">Where to start in the buffer</param>  
      /// <param name="length">How many characters to copy</param>  
      /// <returns>How many characters were actually copied</returns>  
      public long GetChars(int i, long fieldoffset, char[] buffer, int bufferoffset, int length) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetChars(i, fieldoffset, buffer, bufferoffset, length);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a DateTime   
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a DateTime</returns>  
      public DateTime GetDateTime(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetDateTime(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a decimal.   
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a decimal</returns>  
      public decimal GetDecimal(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetDecimal(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a double  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a double</returns>  
      public double GetDouble(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetDouble(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a float  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a float</returns>  
      public float GetFloat(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetFloat(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a Guid   
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a Guid</returns>  
      public Guid GetGuid(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetGuid(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a short   
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a short</returns>  
      public short GetInt16(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetInt16(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as an int  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as an int</returns>  
      public int GetInt32(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetInt32(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a long   
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a long</returns>  
      public long GetInt64(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetInt64(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as a string   
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as a string</returns>  
      public string GetString(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetString(i);  
      }  
  
      /// <summary>  
      /// Access the data in the specified column in the current row as an object    
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>The data as an object</returns>  
      public object GetValue(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.GetValue(i);  
      }  
  
      /// <summary>  
      /// Returns true if and only if the data in the specified column of the current row is null  
      /// as defined by the database.  Note this is different than the CLR concept of null.  
      /// </summary>  
      /// <param name="i">Which column</param>  
      /// <returns>A Boolean indicating whether the column value is null</returns>  
      public bool IsDBNull(int i) {  
         EnsureState(StateValues.read);  
         EnsureOrdinal(i);  
         return results.IsDBNull(i);  
      }  
  
      #endregion  
  
      #region Private implementation  
  
      /// <summary>  
      /// Utility method which executes a server side cursor motion command and reads the row  
      /// at the current location of the cursor after the motion is complete.  Note that reading one row  
      /// at a time can be pretty inefficient.  See comments at the start of this class.  
      /// </summary>  
      /// <param name="commandText">What SQL cursor motion command to execute</param>  
      /// <returns>True if and only if there is data to read</returns>  
  
      private bool fetch(string commandText) {  
         EnsureState(StateValues.read, StateValues.opened,  
              "The result set must be open to perform the read operation.");  
         SqlCommand cmd = connection.CreateCommand();  
         cmd.CommandText = commandText;  
  
         SqlDataReader reader = null;  
         try {  
            reader = cmd.ExecuteReader();  
            //If there is no data, return false;  
            if (!reader.Read()) {  
               state = StateValues.opened;  
               return false;  
            }  
            else {  
               //Otherwise, advance the state to 'read'  
               state = StateValues.read;  
               //If we haven't cached schema information get it now  
               if (results == null) FetchMetadata(reader);  
               //Cache data values  
               object[] values = new object[FieldCount];  
               reader.GetSqlValues(values);  
               results.SetValues(values);  
               //Indicate there is data read by returning true  
               return true;  
            }  
         }  
         finally {  
            if (reader != null) {  
               reader.Close();  
               reader = null;  
            }  
         }  
      }  
  
      /// <summary>  
      /// Helper method for the fetch method.  Initializes the data structures which cache   
      /// the schema information for the current query.  
      /// </summary>  
      /// <param name="reader">The reader from where schema information may be read</param>  
      private void FetchMetadata(SqlDataReader reader) {  
         int columnCount = reader.FieldCount;  
         visibleFieldCount = columnCount;  
         //Initialize the data cache  
         SqlMetaData[] metadata = new SqlMetaData[visibleFieldCount];  
         //Fill the schema cache for each column  
         DataTable schemaTable = reader.GetSchemaTable();  
         SqlString collationString = new SqlString(String.Empty);  
         for (int i = 0; i < metadata.Length; i++) {  
            DataRow columnSchema = schemaTable.Rows[i];  
            metadata[i] = new SqlMetaData(  
                    reader.GetName(i),  
                    (SqlDbType)columnSchema["ProviderType"],  
                    (int)columnSchema["ColumnSize"],  
                    (byte)((short)columnSchema["NumericPrecision"]),  
                    (byte)((short)columnSchema["NumericScale"]),  
                    collationString.LCID,  
                    collationString.SqlCompareOptions,  
                    (Type)columnSchema["DataType"]  
                );  
         }  
  
         results = new Microsoft.SqlServer.Server.SqlDataRecord(metadata);  
      }  
  
      /// <summary>  
      /// Helper method which validates the state of the result set before processing a   
      /// requested operation.  If the result set is in an invalid state an exception is thrown.  
      /// </summary>  
      /// <param name="desiredState"></param>  
      private void EnsureState(StateValues desiredState) {  
         if (state != desiredState) {  
            //Compute the default message  
            string message = string.Format(CultureInfo.InvariantCulture, "Expected result set state to be {0} but it was {1}",  
                desiredState.ToString(), state.ToString());  
            //If we have more specific information, provide a different message.  
            if (desiredState == StateValues.read)  
               message = "You must read a valid row of the result set "  
                   + "before performing the requested operation.";  
            throw new InvalidStateException(message);  
         }  
      }  
  
      /// <summary>  
      /// Similar to the single argument EnsureState method except that the two specified states   
      /// are both legal and a custom error message must be provided.  
      /// </summary>  
      /// <param name="desiredState1"></param>  
      /// <param name="desiredState2"></param>  
      /// <param name="message"></param>  
      private void EnsureState(StateValues desiredState1, StateValues desiredState2, string message) {  
         if (state != desiredState1 && state != desiredState2)  
            throw new InvalidStateException(message);  
      }  
  
      /// <summary>  
      /// Makes certain that the requested column is valid, and throws an exception with a   
      /// clear error message if it isn't.  This would be more useful than the error message  
      /// given when accessing beyond the valid boundary of an array, for example.  
      /// </summary>  
      /// <param name="i">The column trying to be accessed</param>  
      private void EnsureOrdinal(int i) {  
         if (i >= results.FieldCount)  
            throw new ArgumentException(  
                string.Format(CultureInfo.InvariantCulture, "Attempt to access column {0} when only {1} columns exist",  
                i, results.FieldCount));  
      }  
  
      /// <summary>  
      /// Makes certain that the result set is in a proper state to read data, and that  
      /// scrolling is allowed.  This method is called from methods which require scrolling capabilities.  
      /// </summary>  
  
      private void EnsureScroll() {  
         if (!isScrollable)  
            throw new InvalidOperationException("Attempting to execute a scroll operation on a non-scrollable result set.");  
      }  
  
      #endregion  
  
      #region IEnumerable Members  
  
      public IEnumerator GetEnumerator() {  
         return new ResultSetEnumerator(this);  
      }  
  
      #endregion  
   }  
  
   public class ResultSetEnumerator : IEnumerator {  
      private bool isRead;  
  
      private ResultSet resultSetToEnumerate;  
  
      public ResultSetEnumerator(ResultSet resultSetToEnumerate) {  
         this.resultSetToEnumerate = resultSetToEnumerate;  
      }  
  
      #region IEnumerator Members  
      /// <summary>  
      /// Returns the current element of a result set which is represented by a SqlDataRecord  
      /// </summary>  
      /// <value>A SqlDataRecord which represents the current record in a result set</value>  
      object IEnumerator.Current {  
         get {  
            if (!isRead)  
               throw new InvalidOperationException(  
                   "The enumerator is positioned before the first element "  
                   + "of the collection or after the last element");  
            return resultSetToEnumerate.CurrentRecord;  
  
         }  
      }  
  
      public SqlDataRecord Current {  
         get {  
            if (!isRead)  
               throw new InvalidOperationException(  
                   "The enumerator is positioned before the first element "  
                   + "of the collection or after the last element");  
            return resultSetToEnumerate.CurrentRecord;  
         }  
      }  
  
      /// <summary>  
      /// Advance to the next valid element of a result set  
      /// </summary>  
      /// <returns>True if there is a valid element available at this position</returns>  
      public bool MoveNext() {  
         if (!isRead) {  
            if (resultSetToEnumerate.ReadFirst()) {  
               isRead = true;  
               return true;  
            }  
            else  
               return false;  
         }  
         else {  
            return resultSetToEnumerate.Read();  
         }  
      }  
  
      /// <summary>  
      /// Reset the current position of the enumerator to just prior to the current element  
      /// </summary>  
      public void Reset() {  
         isRead = false;  
      }  
  
      #endregion  
   }  
```  
  
 此代码用于测试库 `ResultSet.`  
  
 C#  
  
```  
#region Using directives  
  
using System;  
using System.Collections.Generic;  
using System.Text;  
using System.Configuration;  
using System.Data;  
using System.Data.SqlClient;  
using System.Data.SqlTypes;  
using Microsoft.SqlServer.Server;  
using System.Reflection;  
using System.Runtime.CompilerServices;  
#endregion  
  
[assembly: AssemblyVersion("1.0.*")]  
[assembly: System.CLSCompliant(true)]  
[assembly: System.Runtime.InteropServices.ComVisible(false)]  
[assembly: System.Security.Permissions.EnvironmentPermission(System.Security.Permissions.SecurityAction.RequestMinimum)]  
  
    public sealed class TestResultSet  
    {  
        //The factor to mulitply by the current price to get the new price for each color  
        private const String priceIncreases = "Black, 1.02, Silver, 1.01, Red, 1.05, Yellow, 1.02, Green, 1.03";  
        //Given the name of a color, return the price increase factor as a SqlMoney object.  
        private static readonly Dictionary<String, SqlMoney> increaseDictionary  
            = new Dictionary<String, SqlMoney>(priceIncreases.Length);  
  
        //Don't allow construction of instances since the public portion of this class   
        //is only used to contain static methods.  
        private TestResultSet()  
        {  
        }  
  
        /// <summary>  
        ///     The Adventure Works Cycles corporation needs to increase the standard costs and list price  
        ///     for its most popular bikes due to price increases for the paint used on those bikes.  This   
        ///     program demonstrates browsing popular bikes and updating prices in parallel using   
        ///     the result set sample to implement that scenario.  Note that programmers   
        ///     should always consider whether using a JOIN in a server side query or   
        ///     update would be more efficient that using this style of programming.  
        /// </summary>  
        /// <param name="args">Not used</param>  
        public static void Test()  
        {  
            //Use the priceIncreases constant to initialize entries in the increaseDictionary collection.  
            InitializeIncreaseDictionary();  
  
            SqlConnection myConnection = null;  
            bool isRSOpened = false;  
  
            //Initialize the command to get most popular bikes  
myConnection = new SqlConnection("context connection=true");  
ResultSet rs = null;  
try  
{  
myConnection.Open();  
rs = new ResultSet(myConnection,  
"SELECT TOP 10 P.ProductID, P.Name, P.Color, P.StandardCost, P.ListPrice, "  
+ "sum(SOD.OrderQty) FROM Production.Product AS P "  
+ "JOIN Sales.SalesOrderDetail AS SOD ON P.ProductID = SOD.ProductID "  
+ "JOIN Production.ProductSubcategory AS PSC "  
+ "ON P.ProductSubcategoryID = PSC.ProductSubcategoryID "  
+ "JOIN Production.ProductCategory AS PC "  
+ "ON PSC.ProductCategoryID = PC.ProductCategoryID "  
+ "WHERE PC.Name = 'Bikes' "  
+ "GROUP BY P.ProductID, P.Name, P.Color, P.StandardCost, P.ListPrice "  
+ "ORDER BY sum(SOD.OrderQty) desc;");  
  
//Initialize the command to update the price of a product  
SqlCommand updateCommand = myConnection.CreateCommand();  
updateCommand.CommandText = "usp_UpdateProductPrice";  
updateCommand.CommandType = CommandType.StoredProcedure;  
  
SqlParameter productIDParameter = new SqlParameter("@ProductID", SqlDbType.Int);  
updateCommand.Parameters.Add(productIDParameter);  
SqlParameter standardCostParameter = new SqlParameter("@StandardCost", SqlDbType.Money);  
updateCommand.Parameters.Add(standardCostParameter);  
SqlParameter listPriceParameter = new SqlParameter("@ListPrice", SqlDbType.Money);  
updateCommand.Parameters.Add(listPriceParameter);  
  
rs.Open();  
isRSOpened = true;  
  
while (rs.Read())  
{  
//Column two in the result set contains the color of the bike  
SqlMoney rateOfIncrease = increaseDictionary[rs.GetString(2)];  
//Column zero in the result set contains the product ID  
productIDParameter.Value = rs.GetInt32(0);  
//Column three in the result set contains the current standard cost  
standardCostParameter.Value = rs.GetSqlMoney(3) * rateOfIncrease;  
//Column four in the result set contains the current list price  
listPriceParameter.Value = rs.GetSqlMoney(4) * rateOfIncrease;  
//Update the prices of one of the popular bikes based   
//on its color and current prices.    
updateCommand.ExecuteNonQuery();  
}  
}  
finally  
{  
if (myConnection != null)  
{  
  
if (isRSOpened)  
rs.Close();  
  
myConnection.Close();  
}  
}  
        }  
  
        /// <summary>  
        /// Helper function which fills the dictonary containing the price increases for each color  
        /// </summary>  
        static void InitializeIncreaseDictionary()  
        {  
            //Initialize a dictionary which contains the price increase factors for each color  
            String[] splitIncreases = priceIncreases.Split(new Char[] { ',' });  
            for (int i = 0; i < splitIncreases.Length - 1; i += 2)  
            {  
                increaseDictionary[splitIncreases[i].Trim()] = SqlMoney.Parse(splitIncreases[i + 1].Trim());  
            }  
        }  
    }  
  
```  
  
 这是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 安装脚本 (`Install.sql`)，该脚本部署程序集并创建存储过程。  
  
```  
USE AdventureWorks  
GO  
  
IF EXISTS (SELECT [name] from sys.procedures WHERE [name] = N'usp_RSTest')  
DROP PROCEDURE usp_RSTest;  
GO  
  
IF EXISTS (SELECT [name] from sys.procedures WHERE [name] = N'usp_UpdateProductPrice')  
DROP PROCEDURE usp_UpdateProductPrice;  
GO  
  
-- If the assembly we want to add already exists, drop it.  
  
IF EXISTS (SELECT [name] FROM sys.assemblies WHERE [name] = N'TestResultSet')  
DROP ASSEMBLY TestResultSet;  
GO  
  
IF EXISTS (SELECT [name] FROM sys.assemblies WHERE [name] = N'ResultSet')  
DROP ASSEMBLY ResultSet;  
GO  
  
-- Add the assembly which contains the CLR methods we want to invoke on the server.  
DECLARE @SamplesPath nvarchar(1024);  
  
set @SamplesPath = N'C:\MySample\'  
  
CREATE ASSEMBLY [ResultSet]  
FROM @SamplesPath + 'ResultSet.dll'  
WITH permission_set = safe;  
  
CREATE ASSEMBLY [TestResultSet]    
FROM @SamplesPath + 'TestResultSet.dll'  
WITH permission_set = safe;  
GO  
  
CREATE PROCEDURE [usp_RSTest]  
AS EXTERNAL NAME [TestResultSet].[TestResultSet].Test  
GO  
  
CREATE PROCEDURE usp_UpdateProductPrice  
(  
    @ProductID int,  
    @StandardCost money,  
    @ListPrice money  
)     
AS  
SET NOCOUNT ON;  
  
UPDATE Production.Product  
SET StandardCost = @StandardCost,  
    ListPrice = @ListPrice  
WHERE ProductID = @ProductID;  
GO  
```  
  
 这是用于测试该示例的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 测试脚本 (`test.sql`)。  
  
```  
USE AdventureWorks  
GO  
  
SELECT TOP 10 P.ProductID, P.Name, P.Color, P.StandardCost, P.ListPrice, sum(SOD.OrderQty) FROM Production.Product AS P  
JOIN Sales.SalesOrderDetail AS SOD ON P.ProductID = SOD.ProductID  
JOIN Production.ProductSubcategory AS PSC ON P.ProductSubcategoryID = PSC.ProductSubcategoryID  
JOIN Production.ProductCategory AS PC ON PSC.ProductCategoryID = PC.ProductCategoryID  
WHERE PC.Name = 'Bikes'  
GROUP BY P.ProductID, P.Name, P.Color, P.StandardCost, P.ListPrice   
ORDER BY sum(SOD.OrderQty) desc;  
GO  
  
EXEC usp_RSTest  
GO  
  
SELECT TOP 10 P.ProductID, P.Name, P.Color, P.StandardCost, P.ListPrice, sum(SOD.OrderQty) FROM Production.Product AS P  
JOIN Sales.SalesOrderDetail AS SOD ON P.ProductID = SOD.ProductID  
JOIN Production.ProductSubcategory AS PSC ON P.ProductSubcategoryID = PSC.ProductSubcategoryID  
JOIN Production.ProductCategory AS PC ON PSC.ProductCategoryID = PC.ProductCategoryID  
WHERE PC.Name = 'Bikes'  
GROUP BY P.ProductID, P.Name, P.Color, P.StandardCost, P.ListPrice   
ORDER BY sum(SOD.OrderQty) desc;  
GO  
```  
  
 下面的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 从数据库中删除程序集和存储过程。  
  
```  
USE AdventureWorks  
GO  
  
IF EXISTS (SELECT [name] from sys.procedures WHERE [name] = N'usp_RSTest')  
DROP PROCEDURE usp_RSTest;  
GO  
  
IF EXISTS (SELECT [name] from sys.procedures WHERE [name] = N'usp_UpdateProductPrice')  
DROP PROCEDURE usp_UpdateProductPrice;  
GO  
  
-- If the assembly we want to add already exists, drop it.  
  
IF EXISTS (SELECT [name] FROM sys.assemblies WHERE [name] = N'TestResultSet')  
DROP ASSEMBLY TestResultSet;  
GO  
  
IF EXISTS (SELECT [name] FROM sys.assemblies WHERE [name] = N'ResultSet')  
DROP ASSEMBLY ResultSet;  
GO  
```  
  
## <a name="see-also"></a>请参阅  
 [公共语言运行时 (CLR) 集成的使用方案和示例](../../../2014/database-engine/dev-guide/usage-scenarios-and-examples-for-common-language-runtime-clr-integration.md)  
  
  
