---
title: 直接执行语句 (ODBC) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- statement execution
ms.assetid: b690f9de-66e1-4ee5-ab6a-121346fb5f85
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c2342e24ec6763be32fce8d4fa5ade96b25c9bc6
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67898561"
---
# <a name="execute-a-statement-directly-odbc"></a>直接执行语句 (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

    
### <a name="to-execute-a-statement-directly-and-one-time-only"></a>直接执行语句并且只执行一次  
  
1.  如果语句有参数标记，使用[SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md)要绑定到程序变量的每个参数。 使用数据值填充程序变量，然后设置任何执行时数据参数。  
  
2.  调用[SQLExecDirect](https://go.microsoft.com/fwlink/?LinkId=58399)执行语句。  
  
3.  如果使用执行时数据输入的参数， [SQLExecDirect](https://go.microsoft.com/fwlink/?LinkId=58399)返回 SQL_NEED_DATA。 通过使用分块区发送数据[SQLParamData](https://go.microsoft.com/fwlink/?LinkId=58405)并[SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md)。  

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

### <a name="to-execute-a-statement-multiple-times-by-using-column-wise-parameter-binding"></a>通过使用按列参数绑定多次执行语句  
  
1.  调用[SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)以设置以下属性：  
  
     将 SQL_ATTR_PARAMSET_SIZE 设置为参数集 (S) 的数目。  
  
     将 SQL_ATTR_PARAM_BIND_TYPE 设置为 SQL_PARAMETER_BIND_BY_COLUMN。  
  
     将 SQL_ATTR_PARAMS_PROCESSED_PTR 属性设置为指向 SQLUINTEGER 变量，以包含所处理的参数个数。  
  
     将 SQL_ATTR_PARAMS_STATUS_PTR 设置为指向 SQLUSSMALLINT 变量的数组 [S]，以包含参数状态指示器。  
  
2.  对于每个参数标记：  
  
     分配 S 参数缓冲区的数组以存储数据值。  
  
     分配 S 参数缓冲区的数组以存储数据长度。  
  
     调用[SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md)若要将参数数据值和数据长度数组绑定到语句参数。  
  
     设置任意执行时数据 text 或 image 参数。  
  
     将 S 数据值和 S 数据长度放到绑定参数数组中。  
  
3.  调用[SQLExecDirect](https://go.microsoft.com/fwlink/?LinkId=58399)执行语句。 驱动程序将有效地执行该语句 S 次，每组参数一次。  
  
4.  如果使用执行时数据输入的参数， [SQLExecDirect](https://go.microsoft.com/fwlink/?LinkId=58399)返回 SQL_NEED_DATA。 通过使用分块区发送数据[SQLParamData](https://go.microsoft.com/fwlink/?LinkId=58405)并[SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md)。  
  
### <a name="to-execute-a-statement-multiple-times-by-using-row-wise-parameter-binding"></a>通过使用按行参数绑定多次执行语句  
  
1.  分配结构数组 [S]，其中，S 是参数的集合数。 该结构对于每个参数有一个元素，并且每个元素有两部分：  
  
     第一部分是合适的数据类型的变量，以包含参数数据。  
  
     第二部分是 SQLINTEGER 变量，以包含状态指示器。  
  
2.  调用[SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)以设置以下属性：  
  
     将 SQL_ATTR_PARAMSET_SIZE 设置为参数集 (S) 的数目。  
  
     将 SQL_ATTR_PARAM_BIND_TYPE 设置为在步骤 1 中分配的结构的大小。  
  
     将 SQL_ATTR_PARAMS_PROCESSED_PTR 属性设置为指向 SQLUINTEGER 变量，以包含所处理的参数个数。  
  
     将 SQL_ATTR_PARAMS_STATUS_PTR 设置为指向 SQLUSSMALLINT 变量的数组 [S]，以包含参数状态指示器。  
  
3.  对于每个参数标记，调用[SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md)以参数的数据值和数据长度指针指向其在步骤 1 中分配的结构数组的第一个元素中的变量。 如果参数是执行时数据参数，则设置它。  
  
4.  用数据值填充绑定参数缓冲区数组。  
  
5.  调用[SQLExecDirect](https://go.microsoft.com/fwlink/?LinkId=58399)执行语句。 驱动程序将有效地执行该语句 S 次，每组参数一次。  
  
6.  如果使用执行时数据输入的参数， [SQLExecDirect](https://go.microsoft.com/fwlink/?LinkId=58399)返回 SQL_NEED_DATA。 通过使用分块区发送数据[SQLParamData](https://go.microsoft.com/fwlink/?LinkId=58405)并[SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md)。  
  
 **请注意**按列和按行绑定更常见的做法与结合[SQLPrepare 函数](https://go.microsoft.com/fwlink/?LinkId=59360)并[SQLExecute](https://go.microsoft.com/fwlink/?LinkId=58400)比与[SQLExecDirect](https://go.microsoft.com/fwlink/?LinkId=58399).  
  
## <a name="see-also"></a>请参阅  
 [执行查询操作指南主题&#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
  
