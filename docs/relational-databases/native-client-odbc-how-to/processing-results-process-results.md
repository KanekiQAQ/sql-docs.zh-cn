---
title: 处理结果 (ODBC) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- processing results [ODBC]
ms.assetid: 4810fe3f-78ee-4f0d-8bcc-a4659fbcf46f
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dfd7e36ca2bad2e067d82fa5ad0751f2ef7aef34
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68133454"
---
# <a name="processing-results---process-results"></a>处理结果 - 处理结果
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

ODBC 应用程序中的处理结果包括首先确定结果集的特征，然后通过使用将数据检索至程序变量[SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md)或[SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md).  
  
### <a name="to-process-results"></a>处理结果  
  
1.  检索结果集信息。  
  
2.  如果使用了绑定列，则对每个要绑定的列，调用 [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md)，以将程序缓冲区绑定到该列。  
  
3.  对于结果集中的每一行：  
  
    -   调用 [SQLFetch](https://go.microsoft.com/fwlink/?LinkId=58401) 以获取下一行。  
  
    -   如果使用绑定列，则在绑定列缓冲区使用现在可用的数据。  
  
    -   如果使用绑定列，在最后一个绑定列后一次或多次调用 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) 以获取未绑定列的数据。 应以列号的递增顺序调用 **SQLGetData**。  
  
    -   多次调用 **SQLGetData** 以从 text 或 image 列获取数据。  
  
4.  当 [SQLFetch](https://go.microsoft.com/fwlink/?LinkId=58401) 通过返回 SQL_NO_DATA 指示到达结果集末尾时，调用 [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) 确定是否有其他可用结果集。  
  
    -   如果返回 SQL_SUCCESS，则其他结果集可用。  
  
    -   如果返回 SQL_NO_DATA，则没有其他结果集可用。  
  
    -   如果返回 SQL_SUCCESS_WITH_INFO 或 SQL_ERROR，则调用 [SQLGetDiagRec](https://go.microsoft.com/fwlink/?LinkId=58402) 以确定来自 PRINT 或 RAISERROR 语句的输出是否可用。  
  
         如果将绑定语句参数用于某一存储过程的输出参数或返回值，则使用在绑定参数缓冲区中当前提供的数据。 此外，在使用绑定参数时，对 [SQLExecute](https://go.microsoft.com/fwlink/?LinkId=58400) 或 [SQLExecDirect](https://go.microsoft.com/fwlink/?LinkId=58399) 的每个调用都将执行 *S* 次 SQL 语句，其中，*S* 是绑定参数数组中元素的数目。 这意味着，将存在 *S* 组要处理的结果，而其中每组结果都由所有结果集、输出参数以及 SQL 语句的单次执行通常返回的返回代码构成。  
  
    > [!NOTE]  
    >  在某一结果集包含计算行时，每个计算行都可作为单独的结果集提供。 这些计算结果集混杂在普通行内，并且将普通行分为多个结果集。  
  
5.  或者，使用 SQL_UNBIND 调用 [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) 以释放任何绑定列缓冲区。  
  
6.  如果其他结果集可用，则转到步骤 1。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

> [!NOTE]  
>  若要在 [SQLFetch](https://go.microsoft.com/fwlink/?LinkId=58401) 返回 SQL_NO_DATA 之前取消处理结果集，请调用 [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md)。  
  
## <a name="see-also"></a>请参阅  
[检索结果集信息&#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/processing-results-retrieve-result-set-information.md)   
  
  
