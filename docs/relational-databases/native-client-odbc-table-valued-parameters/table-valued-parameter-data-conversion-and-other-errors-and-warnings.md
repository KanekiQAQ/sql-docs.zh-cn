---
title: 表值参数数据转换及其他错误和警告 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), data conversion
- table-valued parameters (ODBC), error messages
ms.assetid: edd45234-59dc-4338-94fc-330e820cc248
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a4abc28204df04ec77ee6b22e3c7a0c08c832bcd
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68129154"
---
# <a name="table-valued-parameter-data-conversion-and-other-errors-and-warnings"></a>表值参数数据转换及其他错误和警告
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  表值参数列值可按照与其他列和参数值相同的方式在客户端和服务器数据类型之间转换。 但是由于表值参数可以包含多个列和多个行，所以必须能够标识出现错误的实际值，这一点很重要。  
  
 当在表值参数列中检测到错误或警告时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 将生成诊断记录。 错误消息将包含表值参数的参数编号，以及列序号和行号。 应用程序还可以使用诊断记录中的诊断字段 SQL_DIAG_SS_TABLE_COLUMN_NUMBER 和 SQL_DIAG_SS_TABLE_ROW_NUMBER 确定与错误和警告关联的值。 这些诊断字段在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本中提供。  
  
 诊断记录的 SQLSTATE 和消息部分将在所有其他方面符合现有的 ODBC 行为。 即，除参数、 行和列的标识信息，错误消息具有相同的值为表值参数与非表值参数。  
  
## <a name="see-also"></a>请参阅  
 [表值参数&#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
