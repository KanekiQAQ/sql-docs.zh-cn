---
title: SQLSetDescRec | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLSetDescRec function
ms.assetid: 203d02a2-aa09-462b-a489-a2cdd6f6023b
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bf490626d54bb041854497ae1bbad1487c669430
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68131080"
---
# <a name="sqlsetdescrec"></a>SQLSetDescRec
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  本主题讨论特定于 SQLSetDescRec 功能[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]本机客户端。  
  
## <a name="sqlsetdescrec-and-table-valued-parameters"></a>SQLSetDescRec 和表值参数  
 SQLSetDescRec 可以用于设置表值参数和表值参数列的描述符字段。 表值参数列仅在将描述符标头字段 SQL_SOPT_SS_PARAM_FOCUS 设置为特定记录（其 SQL_DESC_TYPE 设置为 SQL_SS_TABLE）的序数时可用。 有关 SQL_SOPT_SS_PARAM_FOCUS 的详细信息，请参阅[SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)。  
  
 下表介绍了参数和描述符字段之间的映射。  
  
|参数|对于非表值参数类型，包括表值参数列的相关的属性|表值参数的相关属性|  
|---------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------|  
|*类型*|SQL_DESC_TYPE|SQL_SS_TABLE|  
|*SubType*|忽略|对于 SQL_DATETIME 或 SQL_INTERVAL 类型的记录，请将它设置为 SQL_DESC_DATETIME_INTERVAL_CODE。|  
|*长度*|SQL_DESC_OCTET_LENGTH|表值参数类型名称的长度。 如果类型名称是以 null 结束，则它可为 SQL_NTS；如果不需要表值参数类型名称，则为零。|  
|*精度*|SQL_DESC_PRECISION|SQL_DESC_ARRAY_SIZE|  
|*小数位数*|SQL_DESC_SCALE|未使用。 此参数应为零。|  
|*DataPtr*|APD 中的 SQL_DESC_DATA_PTR|SQL_CA_SS_TYPE_NAME<br /><br /> 对于存储过程调用，此参数是可选的。如果不需要此参数，则可以指定为 NULL。 对于非过程调用的 SQL 语句，必须指定此参数。<br /><br /> *DataPtr*也可作为应用程序可用于在使用可变行绑定时标识此表值参数的唯一值。|  
|*StringLengthPtr*|SQL_DESC_OCTET_LENGTH_PTR|SQL_DESC_OCTET_LENGTH_PTR<br /><br /> 对于表值参数，它是要传输的行数或 SQL_DATA_AT_EXEC。 这是包含要使用 SQLExecDirect 传输的行数的值的指针。|  
|*IndicatorPtr*|SQL_DESC_INDICATOR_PTR|SQL_DESC_INDICATOR_PTR|  
  
 有关表值参数的详细信息，请参阅[表值参数&#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="sqlsetdescrec-support-for-enhanced-date-and-time-features"></a>SQLSetDescRec 对日期和时间增强功能的支持  
 日期/时间类型所允许的值如下所示：  
  
||*类型*|*SubType*|*长度*|*精度*|*小数位数*|  
|-|------------|---------------|--------------|-----------------|-------------|  
|DATETIME|SQL_DATETIME|SQL_CODE_TIMESTAMP|4|3|3|  
|smalldatetime|SQL_SQL_DATETIME|SQL_CODE_TIMESTAMP|8|0|0|  
|date|SQL_DATETIME|SQL_CODE_DATE|6|0|0|  
|time|SQL_SS_TIME2|0|10|0..7|0..7|  
|datetime2|SQL_DATETIME|SQL_CODE_TIMESTAMP|16|0..7|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|0|20|0..7|0..7|  
  
 有关详细信息，请参阅[日期和时间改进&#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlsetdescrec-support-for-large-clr-udts"></a>SQLSetDescRec 对大型 CLR UDT 的支持  
 **SQLSetDescRec**支持大型 CLR 用户定义类型 (Udt)。 有关详细信息，请参阅[Large CLR User-Defined 类型&#40;ODBC&#41;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>请参阅  
 [SQLSetDescRec](https://go.microsoft.com/fwlink/?LinkId=80704)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
