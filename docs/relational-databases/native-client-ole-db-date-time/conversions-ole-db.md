---
title: 绑定和转换 (OLE DB) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- conversions [OLE DB]
- bindings [OLE DB]
- OLE DB, bindings and conversions
ms.assetid: c187df58-a8c8-4c74-a76f-663abbc5f0c1
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bfefde49bb86833025fc14c2ea84782e8caa84a6
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68107091"
---
# <a name="conversions-ole-db"></a>转换 (OLE DB)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  本部分讨论如何之间进行转换**datetime**并**datetimeoffset**值。 本节中描述的这些转换或者已由 OLE DB 提供，或者是 OLE DB 的一致扩展。  
  
 OLE DB 中时间和日期的文字和字符串的格式通常遵循 ISO，并且不依赖于客户端区域性。 但 DBTYPE_DATE 是个例外，它遵循的标准是 OLE 自动化。 但是，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 只在数据从客户端传输或传输到客户端时在两个类型之间转换，所以，应用程序无法强制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 在 DBTYPE_DATE 和字符串格式之间转换。 否则，字符串使用以下格式（括号中的文本指示某一可选元素）：  
  
-   格式**datetime**并**datetimeoffset**字符串是：  
  
     yyyy  -mm  -dd  [ hh  :mm  :ss  [.9999999  ][ ± hh  :mm  ]]  
  
-   时间字符串的格式为  ：  
  
     hh:mm:ss[.9999999]      
  
-   格式**日期**字符串是：  
  
     *yyyy*-*mm*-*dd*  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 和 SQLOLEDB 的早期版本实现了 OLE 转换，以防标准转换失败。 因此，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 10.0 和更高版本执行的某些转换不同于 OLE DB 规范。  
  
 从字符串转换允许更灵活处理空格和字段宽度。 有关详细信息，请参阅"数据格式：在字符串和文字"一节[OLE DB 日期和时间改进的数据类型支持](../../relational-databases/native-client-ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md)。  
  
 下面是一般的转换规则：  
  
-   在某一字符串转换为日期/时间类型时，该字符串首先作为某一 ISO 文字进行分析。 如果此分析失败，则该字符串将作为某一 OLE 日期文字进行分析，该日期文字具有时间部分。  
  
-   如果未提供时间但接收方可存储时间，则将时间设置为零。 如果未提供日期但接收方可存储日期，则在使用 ISO 转换时将该日期设置为当前日期，在使用 OLE 转换时将该日期设置为 1899-12-30。  
  
-   如果在客户端正使用的数据类型中未提供时区，但服务器可以存储时区，则假定客户端上的数据处于客户端时区中。  
  
-   如果在服务器上未提供时区，但客户端具有时区信息，则假定采用 UTC 时区。 此行为与服务器行为不同。  
  
-   如果提供了时间但接收方无法存储时间，则忽略时间部分。  
  
-   如果提供了日期但接收方无法存储日期，则忽略日期部分。  
  
-   如果在从客户端转换为服务器时截断了秒或秒的小数部分，则返回 DB_E_ERRORSOCCURRED 并且设置状态 DBSTATUS_E_DATAOVERFLOW。  
  
-   如果在从服务器转换为客户端时截断了秒或秒的小数部分，则设置 DBSTATUS_S_TRUNCATED。  
  
## <a name="in-this-section"></a>本节内容  
 [在客户端和服务器之间执行的转换](../../relational-databases/native-client-ole-db-date-time/conversions-performed-from-client-to-server.md)  
 说明在使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 编写的客户端应用程序与 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]（或更高版本）之间执行的日期/时间转换。  
  
 [在服务器和客户端之间执行的转换](../../relational-databases/native-client-ole-db-date-time/conversions-performed-from-server-to-client.md)  
 说明在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]（或更高版本）与使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 编写的客户端应用程序之间执行的日期/时间转换。  
  
## <a name="see-also"></a>请参阅  
 [日期和时间改进 (OLE DB)](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
