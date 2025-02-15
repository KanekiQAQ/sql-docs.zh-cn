---
title: 针对 OLE DB 日期和时间改进的数据类型支持 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- date/time [OLE DB], data type support
- OLE DB, date/time improvements
ms.assetid: d40e3fd6-9057-4371-8236-95cef300603e
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 655b6045e8178e74e994e61691ede28052d8fb5a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68106970"
---
# <a name="data-type-support-for-ole-db-date-and-time-improvements"></a>针对 OLE DB 日期和时间改进的数据类型支持
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  本主题提供有关 OLE DB 信息 ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client) 类型的支持[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]日期/时间数据类型。  
  
## <a name="data-type-mapping-in-rowsets-and-parameters"></a>行集和参数中的数据类型映射  
 OLE DB 提供了两个新的数据类型，以支持新的服务器类型：DBTYPE_DBTIME2 和 DBTYPE_DBTIMESTAMPOFFSET。 下表显示全部服务器类型映射：  
  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|OLE DB 数据类型|ReplTest1|  
|-----------------------------------------|----------------------|-----------|  
|DATETIME|DBTYPE_DBTIMESTAMP|135 (oledb.h)|  
|smalldatetime|DBTYPE_DBTIMESTAMP|135 (oledb.h)|  
|date|DBTYPE_DBDATE|133 (oledb.h)|  
|time|DBTYPE_DBTIME2|145 (sqlncli.h)|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|146 (sqlncli.h)|  
|datetime2|DBTYPE_DBTIMESTAMP|135 (oledb.h)|  
  
## <a name="data-formats-strings-and-literals"></a>数据格式：字符串和文字  
  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|OLE DB 数据类型|客户端转换的字符串格式|  
|-----------------------------------------|----------------------|------------------------------------------|  
|DATETIME|DBTYPE_DBTIMESTAMP|'yyyy-mm-dd hh:mm:ss[.999]'<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对于 Datetime 最多支持三位数字的秒小数部分。|  
|smalldatetime|DBTYPE_DBTIMESTAMP|'yyyy-mm-dd hh:mm:ss'<br /><br /> 此数据类型精确到 1 分钟。 秒部分在输出中将为零，在输入中由服务器进行四舍五入。|  
|date|DBTYPE_DBDATE|'yyyy-mm-dd'|  
|time|DBTYPE_DBTIME2|'hh:mm:ss[.9999999]'<br /><br /> 可以选择指定最多达到七位数字的秒小数部分。|  
|datetime2|DBTYPE_DBTIMESTAMP|'yyyy-mm-dd hh:mm:ss[.fffffff]'<br /><br /> 可以选择指定最多达到七位数字的秒小数部分。|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|'yyyy-mm-dd hh:mm:ss[.fffffff] +/-hh:mm'<br /><br /> 可以选择指定最多达到七位数字的秒小数部分。|  
  
 日期/时间文字的转义序列没有更改。  
  
 结果中秒的小数部分使用点 (.)，而不是冒号 (:)。  
  
 返回给应用程序的字符串值长度始终与给定列的长度相同。 将使用前导零将年、月、日、小时、分钟和秒部分填充到最大长度。 在日期和时间之间只有一个空格，时间和时区偏移量之间也只有一个空格。 时区偏移量始终带符号。 偏移量为零时，此符号将为正号 (+)。 符号和偏移量值之间没有空格。 必要时将使用尾随零将秒小数部分填充到列的定义精度，但是不会填充更多的零。 对于 datetime 列，秒小数部分有三位数字。 对于 smalldatetime 列，没有秒小数部分，秒始终为零。  
  
 从应用程序提供的字符串值转换将更为灵活，允许各个组成部分值小于最大长度。 年可以包含 1-4 位数字。 月、日、小时、分钟和秒可以包含 1 位或 2 位数字。 日期/时间和时间/时区偏移量之间可以有任意多的空格。 零小时零分钟的偏移量符号可以为正号，也可以为负号。 可以为秒小数部分填充尾随零，使之最多达到 9 位数字。 可以使用小数点结束时间部分，不带秒小数部分。  
  
 空字符串不是有效的日期/时间文字，它不表示 NULL 值。 尝试将空字符串转换为日期/时间值将导致具有 SQLState 22018 和消息“为转换指定的字符值无效”的错误。  
  
## <a name="data-formats-data-structures"></a>数据格式：数据结构  
 在以下所述的 OLE DB 特定结构中，OLE DB 遵守与 ODBC 相同的约束。 这些值取自公历：  
  
-   “月”范围为从 1 到 12。  
  
-   “日”字段范围为 1 到所在月包含的天数，必须与“年”和“月”字段一致，考虑闰年。  
  
-   “小时”范围为从 0 到 23。  
  
-   “分钟”范围为从 0 到 59。  
  
-   秒范围是 0 到 59。 这允许最多两个闰秒以便与恒星时间保持同步。  
  
 已对以下现有 OLE DB 结构的实现进行了修改，以支持新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日期和时间数据类型。 不过未更改定义。  
  
-   DBTYPE_DATE（这是自动化 DATE 类型。 它在内部表示为 double  。 完整的部件是自 1899 年 12 月 30 日以来的天数和小数部分是一天的小部分。 此类型的精确度为 1 秒，因此具有有效的 0 刻度。）  
  
-   DBTYPE_DBDATE  
  
-   DBTYPE_DBTIME  
  
-   DBTYPE_DBTIMESTAMP（小数字段由 OLE DB 定义为秒的十亿分之一（纳秒），范围为 0-999,999,999）  
  
-   DBTYPE_FILETIME  
  
### <a name="dbtypedbtime2"></a>DBTYPE_DBTIME2  
 此结构在 32 位和 64 位操作系统中都填充到 12 个字节。  
  
```  
typedef struct tagDBTIME2 {  
    USHORT hour;  
    USHORT minute;  
    USHORT second;  
    ULONG fraction;  
    } DBTIME2;  
```  
  
### <a name="dbtype-dbtimestampoffset"></a>DBTYPE_ DBTIMESTAMPOFFSET  
  
```  
typedef struct tagDBTIMESTAMPOFFSET {  
    SHORT year;  
    USHORT month;  
    USHORT day;  
    USHORT hour;  
    USHORT minute;  
    USHORT second;  
    ULONG fraction;  
    SHORT timezone_hour;  
    SHORT timezone_minute;  
    } DBTIMESTAMPOFFSET;  
```  
  
 如果 `timezone_hour` 为负数，`timezone_minute` 必须为负数或零。 如果 `timezone_hour` 为正数，`timezone minute` 必须为正数或零。 如果 `timezone_hour` 是零，`timezone minute` 可以取 -59 到 +59 之间的值。  
  
### <a name="ssvariant"></a>SSVARIANT  
 此结构现在包括新结构 DBTYPE_DBTIME2 和 DBTYPE_DBTIMESTAMPOFFSET，并为相应类型添加了秒小数部分。  
  
```  
struct SSVARIANT {  
   SSVARTYPE vt;  
   DWORD dwReserved1;  
   DWORD dwReserved2;  
   union {  
// ...  
      DBTIMESTAMP tsDateTimeVal;  
      DBDATE dDateVal;  
      struct _Time2Val {  
         DBTIME2 tTime2Val;  
         BYTE bScale;  
      } Time2Val;  
      struct _DateTimeVal {  
         DBTIMESTAMP tsDateTimeVal;  
         BYTE bScale;  
      } DateTimeVal;  
      struct _DateTimeOffsetVal {   
         DBTIMESTAMPOFFSET tsoDateTimeOffsetVal;  
         BYTE bScale;  
      } DateTimeOffsetVal;  
// ...  
   };  
};  
```  
  
 此外，将按以下方式对与 SSVARIANT 类型编码（该编码确定枚举的类型）关联的枚举进行了扩展：  
  
```  
enum SQLVARENUM {  
// ...  
   // Datetime  
   VT_SS_DATETIME      = DBTYPE_DBTIMESTAMP,  
   VT_SS_SMALLDATETIME = 206,  
  
   VT_SS_DATE = DBTYPE_DBDATE,  
   VT_SS_TIME2 = DBTYPE_DBTIME2,  
   VT_SS_DATETIME2 = 212  
   VT_SS_DATETIMEOFFSET = DBTYPE_DBTIMESTAMPOFFSET  
};  
```  
  
 应用程序迁移到[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用的本机客户端**sql_variant**并且依赖于受限制精度的**datetime**将必须进行更新，如果基础架构更新为使用**datetime2**而非**datetime**。  
  
 还通过添加以下内容，对 SSVARIANT 的访问宏进行了扩展：  
  
```  
#define V_SS_DATETIME2(X)       V_SS_UNION(X, DateTimeVal)  
#define V_SS_TIME2(X)           V_SS_UNION(X, Time2Val)  
#define V_SS_DATE(X)            V_SS_UNION(X, dDateVal)  
#define V_SS_DATETIMEOFFSET(X)  V_SS_UNION(X, DateTimeOffsetVal)  
```  
  
## <a name="data-type-mapping-in-itabledefinitioncreatetable"></a>ITableDefinition::CreateTable 中的数据类型映射  
 以下类型映射用于 itabledefinition:: Createtable 所使用的 DBCOLUMNDESC 结构：  
  
|OLE DB 数据类型 (*wType*)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|说明|  
|----------------------------------|-----------------------------------------|-----------|  
|DBTYPE_DBDATE|date||  
|DBTYPE_DBTIMESTAMP|**datetime2**(p)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口将检查 DBCOLUMDESC *bScale*成员以确定秒的小数部分精度。|  
|DBTYPE_DBTIME2|**time**(p)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口将检查 DBCOLUMDESC *bScale*成员以确定秒的小数部分精度。|  
|DBTYPE_DBTIMESTAMPOFFSET|**datetimeoffset**(p)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口将检查 DBCOLUMDESC *bScale*成员以确定秒的小数部分精度。|  
  
 当应用程序指定了在 DBTYPE_DBTIMESTAMP *wType*，它会重写到映射**datetime2**通过提供中的类型名称*pwszTypeName*。 如果**datetime**指定，则*bScale*必须为 3。 如果**smalldatetime**指定，则*bScale*必须为 0。 如果*bScale*与不一致*wType*并*pwszTypeName*，则返回 DB_E_BADSCALE。  
  
## <a name="see-also"></a>请参阅  
 [日期和时间改进 (OLE DB)](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
