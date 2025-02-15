---
title: 使用 INSERT-并行数据仓库加载数据 |Microsoft Docs
description: 使用 T-SQL INSERT 语句将数据加载到并行数据仓库 (PDW) 分布或复制表。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 51115070427d61e4e594035625afd1393f4e3c5f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67960680"
---
# <a name="load-data-with-insert-into-parallel-data-warehouse"></a>加载数据并将插入到并行数据仓库

可以使用 tsql INSERT 语句将数据加载到 SQL Server 并行数据仓库 (PDW) 分布或复制表的。 插入的详细信息，请参阅[插入](../t-sql/statements/insert-transact-sql.md)。 有关复制的表和分布式表中的所有非分布列，PDW 使用 SQL Server 将隐式转换为目标列的数据类型的语句中指定的数据值。 有关 SQL Server 数据转换规则的详细信息，请参阅[数据类型转换为 SQL](../t-sql/data-types/data-type-conversion-database-engine.md)。 但是，对于分布列 PDW 支持仅子网的 SQL Server 支持的隐式转换。 因此，当使用 INSERT 语句将数据加载到的分布列时，源数据必须指定下表中定义的格式之一。  
  
  
## <a name="InsertingLiteralsBinary"></a>将文本插入到二进制类型  
下表定义可接受文本的类型、 格式和类型的分布列中插入文本值的转换规则**二进制**(*n*) 或**varbinary**(*n*)。  
  
|文本类型|格式|转换规则|  
|----------------|----------|--------------------|  
|二进制文字|0x*hexidecimal_string*<br /><br />例如：0x12Ef|必须以 0x 前缀二进制文本。<br /><br />数据源长度不能超过指定的数据类型的字节数。<br /><br />如果数据源长度的大小小于**二进制**数据类型，数据被填充为零的权限访问的数据类型大小。|  
  
## <a name="InsertingLiteralsDateTime"></a>将文本插入到日期和时间类型  
使用特定的格式，括在单引号内字符值来表示日期和时间文本。 下表定义了允许的文本类型、 格式和转换规则的类型的 SQL Server PDW 分布列中插入日期或时间文字**datetime**， **smalldatetime**，**日期**，**时间**， **datetimeoffset**，或者**datetime2**。  
  
### <a name="datetime-data-type"></a>datetime 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**datetime**。 任何空字符串 （"） 转换的默认值为 1900年-01-01 12:00:00.000。 包含仅空格的字符串 () 生成错误。  
  
|文本类型|格式|转换规则|  
|----------------|----------|--------------------|  
|字符串文本用**datetime**格式|年-月-日 hh: mm: [.nnn]<br /><br />例如：'2007-05-08 12:35:29.123'|插入值时，缺少的小数位数将设置为 0。 例如，文本 2007年-05-08 12:35 作为插入 2007年-05-08 12:35:00.000。|  
|字符串文本用**smalldatetime**格式|年-月-日 hh: mm<br /><br />例如：2007年-05-08 12:35|秒数和剩余的小数位数设置为 0 时插入的值。|  
|字符串文本用**日期**格式|'YYYY-MM-DD'<br /><br />例如：'2007-05-08'|时间 （小时、 分钟、 秒和小数部分） 的值设置为 12:00:00.000 时插入的值。|  
|字符串文本用**datetime2**格式|年-月-日 hh:mm:ss.nnnnnnn<br /><br />例如：'2007-05-08 12:35:29.1234567'|源数据不能超过三个小数位数。 例如，文本 2007年-05-08 12:35:29.123 将插入，但值 2007年-05-08 12:35:29.1234567 将生成错误。|  
  
### <a name="smalldatetime-data-type"></a>smalldatetime 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**smalldatetime**。 任何空字符串 （"） 转换的默认值为 1900年-01-01 12:00。 包含仅空格的字符串 () 生成错误。  
  
|文本类型|格式|转换规则|  
|----------------|----------|--------------------|  
|字符串文本用**smalldatetime**格式|年-月-日 hh: mm 或者年-月-日 hh:mm:00<br /><br />例如：2007年-05-08 12:00 或 2007年-05-08 12:00:00|源数据必须具有对年、 月、 日期、 小时和分钟值。 秒是可选的并且如果存在，必须设置为 00 的值。 任何其他值将生成错误。|  
|字符串文本用**日期**格式|'YYYY-MM-DD'<br /><br />例如：'2007-05-08'|时间 （小时、 分钟、 秒和小数部分） 的值设置为 0 时插入的值。|  
  
### <a name="date-data-type"></a>Date 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**日期**。 任何空字符串 （"） 转换的默认值为 1900年-01-01。 包含仅空格的字符串 () 生成错误。  
  
|文本类型|格式|转换规则|  
|----------------|----------|--------------------|  
|字符串文本用**日期**格式|'YYYY-MM-DD'<br /><br />例如：'2007-05-08'|这是唯一接受的格式。|  
  
### <a name="time-data-type"></a>time 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**时间**。 任何空字符串 （"） 转换为默认值"00:00:00.0000"。 包含仅空格的字符串 () 生成错误。  
  
|文本类型|格式|转换规则|  
|----------------|----------|--------------------|  
|字符串文本用**时间**格式|'hh:mm:ss.nnnnnnn'<br /><br />例如：'12:35:29.1234567'|如果数据源有小于或等于精度 （数字的小数位数） 的精度大于**时间**数据类型，数据填充到右侧的零。 例如，作为"12:35:29.1230000"插入文本值"12:35:29.123"。<br /><br />具有更大的精度比目标数据类型的值将被拒绝。|  
  
### <a name="datetimeoffset-data-type"></a>datetimeoffset 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**datetimeoffset** (*n*)。 默认格式是年-月-日 hh:mm:ss.nnnnnnn {+ |-} hh: mm。 空字符串 （"） 转换的默认值为 1900年-01-01 12:00:00.0000000 + 00:00。 包含仅空格的字符串 () 生成错误。 小数位数取决于列定义。 例如，定义为的列**datetimeoffset** (2) 将具有两个小数位。  
  
|文本类型|格式|转换规则|  
|----------------|----------|--------------------|  
|字符串文本用**datetime**格式|年-月-日 hh: mm: [.nnn]<br /><br />例如：'2007-05-08 12:35:29.123'|缺少的小数位数和偏移量的值设置为 0 时插入的值。 例如，文本 2007年-05-08 12:35:29.123 作为插入 2007年-05-08 12:35:29.1230000 + 00:00。|  
|字符串文本用**smalldatetime**格式|年-月-日 hh: mm<br /><br />例如：2007年-05-08 12:35|插入值时，秒、 剩余的小数位数和偏移量的值都设置为 0。|  
|字符串文本用**日期**格式|'YYYY-MM-DD'<br /><br />例如：'2007-05-08'|时间 （小时、 分钟、 秒和小数部分） 的值设置为 0 时插入的值。 例如，文字"2007年-05-08 作为插入 2007年-05-08 00:00:00.0000000 + 00:00。|  
|字符串文本用**datetime2**格式|年-月-日 hh:mm:ss.nnnnnnn<br /><br />例如：'2007-05-08 12:35:29.1234567'|源数据不能超过指定的 datetimeoffset 列中的小数秒数。 如果数据源具有小于或等于数秒的小数部分，右侧用零填充数据。 例如，如果数据类型为 datetimeoffset (5)，文本值 2007年-05-08 12:35:29.123 + 12:15 作为插入 12:35:29.12300 + 12:15。|  
|字符串文本用**datetimeoffset**格式|年-月-日 hh:mm:ss.nnnnnnn {+&#124;-} hh: mm<br /><br />例如：2007年-05-08 12:35:29.1234567 + 12:15|源数据不能超过指定的 datetimeoffset 列中的小数秒数。 如果数据源具有小于或等于数秒的小数部分，右侧用零填充数据。 例如，如果数据类型为 datetimeoffset (5)，文本值 2007年-05-08 12:35:29.123 + 12:15 作为插入 12:35:29.12300 + 12:15。|  
  
### <a name="datetime2-data-type"></a>datetime2 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**datetime2** (*n*)。 默认格式为年-月-日 hh:mm:ss.nnnnnnn。 空字符串 （"） 转换的默认值为 1900年-01-01 12:00:00。 包含仅空格的字符串 () 生成错误。 小数位数取决于列定义。 例如，定义为的列**datetime2** (2) 将具有两个小数位。  
  
|文本类型|格式|转换规则|  
|----------------|----------|--------------------|  
|字符串文本用**datetime**格式|年-月-日 hh: mm: [.nnn]<br /><br />例如：'2007-05-08 12:35:29.123'|秒的小数部分是可选的当插入的值设置为 0。<br /><br />具有小数部分位数多于目标数据类型的值将被拒绝。|  
|字符串文本用**smalldatetime**格式|年-月-日 hh: mm<br /><br />例如：'2007-05-08 12'|可选秒数和剩余的小数位数设置为 0 时插入的值。|  
|字符串文本用**日期**格式|'YYYY-MM-DD'<br /><br />例如：'2007-05-08'|时间 （小时、 分钟、 秒和小数部分） 的值设置为 0 时插入的值。 例如，文字"2007年-05-08 作为插入 2007年-05-08 12:00:00.0000000。|  
|字符串文本用**datetime2**格式|年-月-日 hh:mm:ss:nnnnnnn<br /><br />例如：'2007-05-08 12:35:29.1234567'|如果数据源包含数据和时间是小于或等于中指定的值的组件**datetime2**(*n*)，则数据将插入; 否则将生成错误。|  
  
## <a name="InsertLiteralsNumeric"></a>将文本插入到数值类型  
下表定义的接受的格式和将文本值插入到使用数值类型的 SQL Server PDW 分布列的转换规则。  
  
### <a name="bit-data-type"></a>bit 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**位**。 空字符串 （"） 或一个字符串，包含仅空格 () 转换为 0。  
  
|文本类型|format|转换规则|  
|----------------|----------|--------------------|  
|字符串文本用**整数**格式|nnnnnnnnnn<br /><br />例如："1"或者"321"|一个整数值格式化为字符串文字不能包含负值。 例如，"-123"的值将生成错误。<br /><br />大于 1 的值转换为 1。 例如，"123"的值转换为 1。|  
|字符串文本|'TRUE' 或者 'FALSE'<br /><br />示例: ' true'|值 'TRUE' 转换为 1;值 'FALSE' 转换为 0。|  
|整数文本|nnnnnnnn<br /><br />例如：1 或 321|大于 1 或小于 0 的值转换为 1。 例如，值 123 和-123 都转换为 1。|  
|十进制文本|nnnnn.nnnn<br /><br />例如：1234.5678|大于 1 或小于 0 的值转换为 1。 例如，将 123.45 和-123.45 的值转换为 1。|  
  
### <a name="decimal-data-type"></a>decimal 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**十进制**(*p，s*)。 与 SQL Server 相同的数据转换规则。 有关详细信息，请参阅[数据类型转换](../t-sql/data-types/data-type-conversion-database-engine.md)MSDN 上。  
  
|文本类型|格式|  
|----------------|----------|  
|字符串文本用**整数**格式|nnnnnnnnnnnn<br /><br />例如："321312313123"|  
|字符串文本用**十进制**格式|'nnnnnn.nnnnn'<br /><br />例如："123344.34455"|  
|整数文本|nnnnnnnnnnnn<br /><br />例如：321312313123|  
|十进制文本|nnnnnn.nnnnn<br /><br />例如："123344.34455"|  
  
### <a name="float-and-real-data-types"></a>float 和 real 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**float**或**实际**。 数据转换规则都与 SQL Server 相同。 有关详细信息，请参阅[数据类型转换](../t-sql/data-types/data-type-conversion-database-engine.md)MSDN 上。  
  
|文本类型|格式|  
|----------------|----------|  
|字符串文本用**整数**格式|nnnnnnnnnnnn<br /><br />例如："321312313123"|  
|字符串文本用**十进制**格式|'nnnnnn.nnnnn'<br /><br />例如："123344.34455"|  
|字符串文本用**浮点数**格式|n.nnnnnE+nn<br /><br />例如：3.12323E + 14|  
|整数文本|nnnnnnnnnnnn<br /><br />例如：321312313123|  
|十进制文本|nnnnnn.nnnnn<br /><br />例如：123344.34455|  
|浮点型|n.nnnnnE+nn<br /><br />例如：3.12323E+14|  
  
### <a name="int-bigint-tinyint-smallint-data-types"></a>int、 bigint、 tinyint、 smallint 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**int**， **bigint**， **tinyint**，或者**smallint**。 数据源不能超过给定的数据类型允许的范围。 例如，对于范围**tinyint**是 0 到 255 和范围**int**为-2,147,483,648 到 2,147,483,647。  
  
|文本类型|格式|转换规则|  
|------------|------|----------------|
|字符串文本用**整数**格式|nnnnnnnnnnnnnn<br /><br />例如："321312313123"| 无 |  
|整数文本|nnnnnnnnnnnnnn<br /><br />例如：321312313123| 无|  
|十进制文本|nnnnnn.nnnnn<br /><br />例如：123344.34455|小数点右侧的值将被截断。|  
  
### <a name="money-and-smallmoney-data-types"></a>money 和 smallmoney 数据类型  
Money 文字值表示为带有可选的小数点和货币符号作为前缀的数字。 数据源不能超过给定的数据类型允许的范围。 例如，对于范围**smallmoney**是-214，748.3648 214，748.3647 和范围**资金**是-922,337,203,685,477.5808 到 922,337,203,685,477.5807。 下表定义的接受的格式和类型的分布列中插入文本值的规则**资金**或**smallmoney**。  
  
|文本类型|格式|转换规则|  
|----------------|----------|--------------------|  
|字符串文本用**整数**格式|nnnnnnnn<br /><br />例如："123433"|缺少数字后的小数位数将设置为 0 时插入的值。 例如，将作为 12345.0000 插入文本"12345"。|  
|字符串文本用**十进制**格式|'nnnnnn.nnnnn'<br /><br />例如："123344.34455"|如果小数点后的位数超过 4，值舍入为最接近的值。 例如，作为 123344.3446 插入值"123344.34455"。|  
|字符串文本用**资金**格式|$nnnnnn.nnnn<br /><br />示例:"$ 123456.7890"|可选的货币符号不插入的值。<br /><br />如果小数点后的位数超过 4，值舍入为最接近的值。|  
|整数文本|nnnnnnnn<br /><br />例如：123433|缺少数字后的小数位数将设置为 0 时插入的值。 例如，将作为 12345.0000 插入文字 12345。|  
|十进制文本|nnnnnn.nnnnn<br /><br />例如：123344.34455|如果小数点后的位数超过 4，值舍入为最接近的值。 例如，作为 123344.3446 插入值 123344.34455。|  
|Money 文字|$nnnnnn.nnnn<br /><br />示例: $ 123456.7890|可选的货币符号不插入的值。<br /><br />如果小数点后的位数超过 4，值舍入为最接近的值。|  
  
## <a name="InsertLiteralsString"></a>将文本插入到字符串类型  
下表定义的接受的格式和使用字符串类型的 SQL Server PDW 列中插入文本值的转换规则。  
  
### <a name="char-varchar-nchar-and-nvarchar-data-types"></a>char、 varchar、 nchar 和 nvarchar 数据类型  
下表定义的接受的格式和类型的分布列中插入文本值的规则**char**， **varchar**， **nchar**和**nvarchar**。 数据源长度不能超过指定的数据类型的大小。 如果数据源长度的大小小于**char**或**nchar**数据类型，数据填充到右侧的空白区域以达到数据类型大小。  
  
|文本类型|格式|转换规则|  
|----------------|----------|--------------------|  
|字符串文本|格式: 字符串<br /><br />示例: ' abc'| 无|  
|Unicode 字符串文本|格式：N'character 字符串<br /><br />例如：N'abc'|  无 |  
|整数文本|格式： nnnnnnnnnnn<br /><br />例如：321312313123| 无 |  
|十进制文本|格式： nnnnnn.nnnnnnn<br /><br />例如：12344.34455| 无 |  
|Money 文字|格式： $nnnnnn.nnnnn<br /><br />示例: $ 123456.99|货币符号不插入的值。 若要插入的货币符号，请为字符串文字插入的值。 这将匹配的格式**dwloader**工具，它将每个文本视为文字字符串。<br /><br />不允许使用逗号。<br /><br />如果小数点后的位数超过 2，值舍入为最接近的值。 例如，作为 123.95 插入值 123.946789。<br /><br />使用 CONVERT 函数将 money 的文本，则允许仅默认样式 0 （不以逗号分隔，小数点后的 2 位数）。|  

  
## <a name="see-also"></a>请参阅  
 
[分布式的数据](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-distributed-data/)  
[INSERT](../t-sql/statements/insert-transact-sql.md)  
  
<!-- MISSING LINKS
[Grant permissions to load data](grant-permissions-to-load-data.md)  
[Metadata query examples](metadata-query-examples.md) 
-->
