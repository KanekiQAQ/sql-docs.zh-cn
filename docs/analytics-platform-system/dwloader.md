---
title: dwloader 命令行加载器-并行数据仓库 |Microsoft Docs
description: dwloader 是将表行批量加载到现有表的并行数据仓库 (PDW) 命令行工具。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: dd3f005346c5faae9e02513a144d04d80857b770
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67961029"
---
# <a name="dwloader-command-line-loader-for-parallel-data-warehouse"></a>dwloader 命令行加载程序，用于并行数据仓库
**dwloader**是将表行批量加载到现有表的并行数据仓库 (PDW) 命令行工具。 当加载行，可以将所有行都添加到表的末尾 (*追加模式*或*fastappend 模式*)、 追加新行和更新现有行 (*upsert 模式*)，或删除所有现有的行之前加载，然后将所有行都插入到一个空表 (*重新加载模式*)。  
  
**用于将数据加载过程**  
  
1.  准备源数据。  
  
    使用您自己的 ETL 进程创建想要加载的源数据。 源数据必须格式化为与目标表的架构匹配。 将源数据存储到一个或多个文本文件并将文本文件复制到你正在加载服务器上的相同目录。 加载服务器的信息，请参阅[获取和配置加载服务器](acquire-and-configure-loading-server.md)  
  
2.  准备加载选项。  
  
    决定将使用哪些加载选项。 在配置文件中存储的加载选项。 将配置文件复制到加载服务器上的本地位置。 **Dwloader**本主题中描述了配置选项。  
  
3.  准备加载故障选项。  
  
    决定要如何**dwloader**以处理加载失败的行。 若要执行加载**dwloader**首先将数据加载到临时表，然后将数据传输到目标表。 加载程序将数据加载到临时表，它跟踪加载失败的行数。 例如，将无法加载格式不正确的行。 失败的行复制到拒绝文件。 默认情况下，负载将中止后首次拒绝除非您指定不同的拒绝阈值。  
  
4.  安装**dwloader**。  
  
    如果尚未安装，请安装 dwloader 加载服务器上。 

<!-- MISSING LINK
    See [Install dwloader Command-Line Loader &#40;SQL Server PDW&#41;](install-dwloader-command-line-loader-sql-server-pdw.md). 
--> 
  
5.  运行**dwloader**。  
  
    登录到正在加载服务器和运行可执行文件**dwloader.exe**和相应的命令行选项。  
  
6.  验证结果。  
  
    你可以检查失败的行 （使用-R 指定） 的文件以查看任何行未能加载。 如果此文件为空，已成功加载所有行。 **dwloader**是事务性的因此如果任何步骤失败 （不是拒绝的行），则所有步骤将都回滚到其初始状态。  
  
<!-- 
![Topic link icon](media/topic-link.gif "Topic_Link")[Syntax Conventions](syntax-conventions-sql-server-pdw.md)  
-->  


## <a name="syntax"></a>语法  
  
```  
dwloader.exe { -h }  
  
dwloader.exe   
    {  
        { -U login_name  -P password  }  
        | -W  
    }  
    [ -f parameter_file ]  
    [ -S target_appliance ]  
    { -T target_database_name . [ schema ] . table_name }   
    { -i source_data_location } [ <source_data_options> ]  
    { -R load_failure_file_name } [ <load_failure_options> ]  
    [ <loading_options> ]  
}  
  
<source_data_options> ::=  
{  
    [ -fh number_header_rows ]  
    [ < variable_length_column_options > | < fixed_width_column_options > ]  
    [ -D { mdy | myd | ymd | ydm | dmy | dym | custom_date_format } ]  
    [ -dt datetime_format_file ]  
}  
  
<variable_length_column_options> ::=  
{  
    [ -e character_encoding ]  
    -r row_delimiter   
    [ -s string_delimiter ]  
    -t field_delimiter   
}  
  
<fixed_width_column_options> ::=  
{  
    -w fixed_width_config_file   
    [ -e character_encoding ]   
    -r row_delimiter   
}  
  
<load_failure_options> ::=  
{  
    [ -rt { value | percentage } ]  
    [ -rv reject_value ]  
    [ -rs reject_sample_value ]  
}  
  
<loading_options> ::=  
{  
    [ -d staging_database_name ]  
    [ -M { append | fastappend | upsert -K merge_column [ ,...n ] | reload } ]  
    [ -b batchsize ]   
    [ -c ]  
    [ -E ]  
    [ -m ]  
    [ -N ]  
    [ -se ]
    [ -l ]   
}  
```  
  
## <a name="arguments"></a>参数  
**-h**  
显示简单的帮助信息使用加载程序。 如果未不指定任何其他命令行参数，仅显示帮助。  
  
**-U** *login_name*  
具有适当的权限执行加载的有效 SQL Server 身份验证登录名。  
  
**-P** *password*  
SQL Server 身份验证密码*login_name*。  
  
**-W**  
使用 Windows 身份验证。 (无*login_name*或*密码*必需。) 

<!-- MISSING LINK
For information about configuring Windows Authentication, see [Security - Configure Domain Trusts](security-configure-domain-trusts.md).  
-->
  
**-f** *parameter_file_name*  
使用参数文件， *parameter_file_name*，代替命令行参数。 *parameter_file_name*可以包含任何命令行参数，除非*user_name*并*密码*。 如果在命令行和参数文件中指定参数，则命令行重写文件的参数。  
  
参数文件包含一个参数，不带 **-** 前缀，每行。  
  
示例：  
  
`rt=percentage`  
  
`rv=25`  
  
* *-S***target_appliance*  
指定 SQL Server PDW 设备，将收到加载的数据。  
  
*有关 Infiniband 连接*， *target_appliance*指定为 < 设备名称 >-SQLCTL01。 若要配置此命名连接，请参阅[配置无线带宽技术网络适配器](configure-infiniband-network-adapters.md)。  
  
为以太网连接*target_appliance*是控制节点群集的 IP 地址。  
  
如果省略，dwloader 默认为 dwloader 已安装时指定的值。 

<!-- MISSING LINK
For more information about this install option, see [Install dwloader Command-Line Loader](install-dwloader.md).  
-->
  
**-T** *target_database_name。* [*架构*]。*table_name*  
目标表的三部分组成的名称。  
  
* *-I***source_data_location*  
若要加载的一个或多个源文件的位置。 每个源文件必须是一个文本文件或使用 gzip 压缩的文本文件。 只有一个源文件可以压缩到每个 gzip 文件。  
  
若要设置源文件的格式：  
  
-   源代码文件的格式必须根据负载选项。  
  
-   在源文件中的每个行包含一个表行的数据。 源数据必须与目标表的架构匹配。 列顺序和数据类型也必须匹配。 行中的每个字段表示目标表中的列。  
  
-   默认情况下的字段为可变长度，并由分隔符分隔。 若要指定的分隔符类型，请使用 < variable_length_column_options > 命令行选项。 若要指定固定的长度的字段，请使用 < fixed_width_column_options > 命令行选项。  
  
若要指定源数据位置：  
  
-   源数据位置可以是网络路径或加载服务器上的目录的本地路径。  
  
-   若要指定所有文件的目录中，输入目录路径后, 跟 * 通配符字符。  加载程序不会从在源数据位置的任何子目录中加载文件。 加载程序错误时的目录中的 gzip 文件存在。  
  
-   若要指定的目录中的某些文件，请使用字符的组合和 * 通配符。  
  
若要使用一个命令将多个文件加载：  
  
-   所有文件必须都位于相同的目录。  
  
-   文件必须是文本的所有文件、 所有 gzip 文件或组合文本和 gzip 文件。  
  
-   没有任何文件可以包含标头信息。  
  
-   所有文件必须都使用相同的字符编码类型。 请参阅-e 选项。  
  
-   所有文件必须都加载到同一个表。  
  
-   将连接在一起的所有文件，并将其加载，就好像它们是一个文件，并已拒绝的行将转到单个拒绝文件。  
  
示例：  
  
-   -i \\\loadserver\loads\daily\\*.gz  
  
-   -i \\\loadserver\loads\daily\\*.txt  
  
-   -i \\\loadserver\loads\daily\monday.*  
  
-   -i \\\loadserver\loads\daily\monday.txt  
  
-   -i \\\loadserver\loads\daily\\*  
  
**-R** *load_failure_file_name*  
如果加载失败**dwloader**将存储在名为的文件中的失败信息到负载测试和失败描述失败的行*load_failure_file_name*。 如果此文件已存在，dwloader 将覆盖现有文件。 *load_failure_file_name*第一次失败的时候创建。 如果成功，加载的所有行*load_failure_file_name*未创建。  
  
**-fh** *number_header_rows*  
若要忽略的开始处的行 （行） 数*source_data_file_name*。 默认值为 0。  
  
<variable_length_column_options>  
选项*source_data_file_name*具有分隔字符的可变长度列。 默认情况下*source_data_file_name*包含可变长度列中的 ASCII 字符。  
  
对于 ASCII 文件，由连续放置分隔符表示 null 值。 例如，在以管道分隔的文件 ("|")，null 值将由"| |"。 在以逗号分隔文件中，null 值将由"（、、"。 此外， **-E** (-emptyStringAsNull) 必须指定选项。 有关详细信息-E，请参阅下面。  
  
**-e** *character_encoding*  
指定的字符编码类型的数据要从数据文件加载。 选项为 ASCII （默认值）、 UTF8、 UTF16，或 UTF16BE，其中 UTF16 是稍有 endian，UTF16BE 是大字节序。 这些选项是不区分大小写。  
  
**-t** *field_delimiter*  
行中每个字段 （列） 为分隔符。 字段分隔符为一个或多个这些 ASCII 转义字符或 ASCII 十六进制值。  
  
|名称|转义符|十六进制字符|  
|--------|--------------------|-----------------|  
|Tab|\t|0x09|  
|回车符 (CR)|\r|0x0d|  
|换行符 (LF)|\n|0x0a|  
|CRLF|\r\n|0x0d0x0a|  
|逗号|','|0x2c|  
|双引号|\\"|0x22|  
|单引号|\\'|0x27|  
  
若要在命令行上指定的管道字符，将用双引号引起来，"|"。 这会由命令行分析器来避免错误解释。 其他字符都用单引号括起来。  
  
示例：  
  
-t"|"  
  
-t ' '  
  
-t 0x0a  
  
-t \t  
  
-t '~|~'  
  
**-r** *row_delimiter*  
源数据文件的每个行分隔符。 行分隔符是一个或多个 ASCII 值。  
  
若要指定回车符 (CR)、 换行符 (LF) 或制表符字符作为分隔符，可以使用转义字符 （\r、 \n、 \t） 或其十六进制值 （0 x，0 d，09）。 若要指定任何其他特殊字符作为分隔符，请使用其十六进制值。  
  
回车 + 换行的示例：  
  
-r \r\n  
  
-r 0x0d0x0a  
  
CR 的示例：  
  
-r \r  
  
-r 0x0d  
  
LF 的示例：  
  
-r \n  
  
-r 0x0a  
  
需要 Unix LF。 需要 Windows CR。  
  
**-s** *string_delimiter*  
字符串数据的分隔符类型字段的带分隔符的文本输入文件。 字符串分隔符是一个或多个 ASCII 值。  它可以被指定为一个字符 (例如，-s *) 或十六进制值 (例如，-s 0x22 为双引号)。  
  
示例：  
  
-s *  
  
-s 0x22  
  
< fixed_width_column_options>  
有关具有固定长度列的源数据文件的选项。 默认情况下*source_data_file_name*包含可变长度列中的 ASCII 字符。  
  
UTF8-e 时，不支持固定的宽度的列。  
  
**-w** *fixed_width_config_file*  
路径和指定的字符数，每个列中的配置文件的名称。 必须指定每个字段。  
  
此文件必须驻留在加载服务器。 路径可以是 UNC，相对或绝对路径。 中的每一行*fixed_width_config_file*包含一列和字符数为该列的名称。 没有每列的一个行，如下所示，并在文件中的顺序必须与目标表中的顺序匹配：  
  
*column_name*=*num_chars*  
  
*column_name*=*num_chars*  
  
固定宽度的配置文件的示例：  
  
SalesCode=3  
  
SalesID=10  
  
示例中的行*source_data_file_name*:  
  
230Shirts0056  
  
320Towels1356  
  
在上一示例中，加载的第一行将具有 SalesCode ="230"和 SalesID = Shirts0056。 第二次加载的一行都将有 SalesCode ="320"和 SaleID = Towels1356。  
  
有关如何处理前导空格和尾随空格或数据类型转换在固定的宽度模式下的信息，请参阅[数据类型转换规则适用于 dwloader](dwloader-data-type-conversion-rules.md)。  
  
**-e** *character_encoding*  
指定的字符编码类型的数据要从数据文件加载。 选项为 ASCII （默认值）、 UTF8、 UTF16，或 UTF16BE，其中 UTF16 是稍有 endian，UTF16BE 是大字节序。 这些选项是不区分大小写。  
  
UTF8-e 时，不支持固定的宽度的列。  
  
**-r** *row_delimiter*  
源数据文件的每个行分隔符。 行分隔符是一个或多个 ASCII 值。  
  
若要指定回车符 (CR)、 换行符 (LF) 或制表符字符作为分隔符，可以使用转义字符 （\r、 \n、 \t） 或其十六进制值 （0 x，0 d，09）。 若要指定任何其他特殊字符作为分隔符，请使用其十六进制值。  
  
回车 + 换行的示例：  
  
-r \r\n  
  
-r 0x0d0x0a  
  
CR 的示例：  
  
-r \r  
  
-r 0x0d  
  
LF 的示例：  
  
-r \n  
  
-r 0x0a  
  
需要 Unix LF。 需要 Windows CR。  
  
**-D** { **ymd** | ydm | mdy | myd | dmy |dym |*custom_date_format* }  
在输入文件中指定的月 (m) 和日 (d)，年 (y) 的所有日期时间字段的顺序。 默认顺序是要求使用 ymd。 若要指定的同一源文件中的多个订单格式，请使用-dt 选项。  
  
要求使用 ymd |dmy  
ydm 和 dmy 允许相同的输入的格式。 这两允许要需要在开始或结束日期的年份。 例如，对于这两**ydm**并**dmy**日期格式，您可以输入文件中拥有 2013年-02-03 或者 02-03-2013年。  
  
ydm  
仅可以加载到列数据类型 datetime 和 smalldatetime 数据类型的格式为 ydm 的输入。 无法将 ydm 值加载到 datetime2、 日期或 datetimeoffset 数据类型的列。  
  
mdy  
允许 mdy <month> <space> <day> <comma> <year>。  
  
1975 年 1 月 1，mdy 输入数据的示例：  
  
-   1975 年 1 月 1 日  
  
-   75 年 1 月 1 日，  
  
-   年 1 月/1/75  
  
-   01011975  
  
myd  
年 3 月的输入文件示例 04,2010:03-2010年-04，3/2010年/4  
  
dym  
2010 年 3 月 4 日的输入的文件示例：04-2010年-03、 4/2010/3  
  
*custom_date_format*  
*custom_date_format*是一种自定义日期格式 (例如，MM/dd/yyyy) 进行同步后向兼容性。 dwloader 不强制实施自定义日期格式。 相反，当指定自定义日期格式**dwloader**会将它转换为相应的设置的要求使用 ymd、 ydm、 mdy、 myd、 dym 或 dmy。  
  
例如，如果指定-D MM/dd/yyyy，dwloader 需要所有日期输入进行排序的月份第一次，然后年和日然后 (mdy)。 它不会强制实施 2 个字符几个月、 2 位数字表示日和 4 位数年份解释为指定的自定义日期格式。 以下是可从-D MM/dd/yyyy 的日期格式时在输入文件格式设置日期的方式的示例：01/02/2013，Jan.02.2013，2013 年 1 月 2 日  
  
有关更全面的格式设置信息，请参阅[数据类型转换规则适用于 dwloader](dwloader-data-type-conversion-rules.md)。  
  
**-dt** *datetime_format_file*  
在名为的文件中指定每个日期时间格式*datetime_format_file*。 与不同的命令行参数包含空格的文件参数不必须括在双引号内。 您不能更改日期时间格式，如加载数据。 源数据文件和目标表中的相应列必须具有相同的格式。  
  
每个行包含在目标表和其日期时间格式中的列的名称。  
  
示例：  
  
`LastReceiptDate=ymd`  
  
`ModifiedDate=dym`  
  
**-d** *staging_database_name*  
将包含临时表的数据库名称。 默认值为-T 选项，这是目标表的数据库使用指定的数据库。 有关使用临时数据库的详细信息，请参阅[创建临时数据库](staging-database.md)。  
  
**-M** *load_mode_option*  
指定是否要追加的 upsert，或重新加载数据。 追加的默认模式。  
  
追加  
加载程序将行插入目标表中的现有行的末尾。  
  
fastappend  
加载程序将行插入直接，而无需使用临时表，到目标表中的现有行的末尾。 fastappend 需要多事务 (-m) 选项。 使用 fastappend 时，不能指定临时数据库。 没有与 fastappend，这意味着，必须由您自己的加载进程处理失败或已中止负载进行恢复，会回滚。  
  
upsert **-K**  *merge_column* [ ,...*n* ]  
加载程序使用 SQL Server 合并语句更新现有行和插入新行。  
  
-K 选项指定要合并的基础的列。 这些列构成的合并密钥，应表示唯一行。 如果目标表中存在合并项，更新的行。 如果合并键不存在目标表中，追加行。  
  
对于哈希分布式表，合并密钥必须是或者包含分布列。  
  
复制表的合并密钥是一个或多个列的组合。 根据应用程序需要指定了这些列。  
  
多个列必须是以逗号分隔 （不含空格） 或空格以逗号分隔并括在单引号。  
  
如果源表中的两个行具有匹配合并密钥值，其各自的行必须相同。  
  
重新加载  
加载程序将截断目标表之前它将插入的源数据。  
  
**-b** *batchsize*  
建议仅使用 Microsoft 支持部门*batchsize*是 DMS 到计算节点上的 SQL Server 实例执行大容量复制的 SQL Server 批处理大小。  当*batchsize*指定，则 SQL Server PDW 将替代用于每个负载的动态计算的批处理负载大小。  
  
从 SQL Server 2012 PDW 开始，控制节点动态计算每个负载的批处理大小默认情况下。 此自动计算基于多个参数，例如内存大小、 目标表类型、 目标表架构、 负载类型、 文件大小和用户的资源类。  
  
例如，如果负载模式是 FASTAPPEND 和表具有聚集列存储索引，SQL Server PDW 情况将默认尝试使用的批处理大小为 1,048,576，以便将变得关闭行组，并直接加载到列存储而无需通过增量存储。 如果内存不允许 1048576 的批大小，dwloader 将选择较小的批大小。  
  
如果负载类型为 FASTAPPEND， *batchsize*适用于数据加载到表，否则*batchsize*适用于将数据加载到临时表。  
  
<reject_options>  
指定用于确定加载程序将允许加载失败的数目的选项。 如果加载失败次数超过了阈值，则加载程序将暂停并提交任何行。  
  
**-rt** {**值**| 百分比}  
指定是否-*reject_value*中 **-rv** *reject_value*选项是大量文本的行 （值） 或失败 （百分比） 的速率。 默认值为。  
  
百分比选项是-rs 选项根据时间间隔发生的实时计算。  
  
例如，如果加载程序尝试加载 100 行和 25 失败，75 成功，失败率为 25%。  
  
**-rv** *reject_value*  
指定要停止负载前允许的数量或百分比的行的拒绝。 **-Rt**选项用于确定如果*reject_value*引用的行数或行的百分比。  
  
默认值*reject_value*为 0。  
  
与-rt 值结合使用，加载程序将被拒绝的行数超过 reject_value 时停止负载。  
  
当与-rt 百分比配合使用，则加载程序按间隔计算百分比 (-rs 选项)。 因此，失败行的百分比可能会超过*reject_value*。  
  
**-rs** *reject_sample_size*  
用于`-rt percentage`选项以指定的增量百分比检查。 例如，如果 reject_sample_size 为 1000年，则加载程序将计算失败行的百分比，以尝试加载 1000年行后。 尝试加载每个其他 1000年行后，它重新计算失败行的百分比。  
  
**-c**  
从左侧和右侧的 char、 nchar、 varchar 和 nvarchar 字段中删除空白字符。 将为每个字段的只包含空白字符为空字符串。  
  
示例：  
  
' 获取截断为 '  
  
'abc' 获取截断为 'abc'  
  
当使用-E，使用-c-E 操作先发生。 字段只能包含空白字符将转换为空字符串，并不为 NULL。  
  
**-E**  
将空字符串转换为 NULL。 默认值是不执行这些转换。  
  
**-m**  
使用的第二个阶段的加载; 多事务模式当数据从临时表加载到分布式表。  
  
与 **-m**，SQL Server PDW 执行并提交并行的负载。 这比默认的加载模式下，更快地执行，但不是事务安全。  
  
无需 **-m**，SQL Server PDW 执行，并按顺序承诺加载，在分布区中的每个计算节点，并同时跨计算节点。 此方法比多事务模式下，慢但事务安全。  
  
**-m**是可选的*追加*，*重新加载*，并且*upsert*。  
  
**-m** fastappend 需要。  
  
**-m**不能用于复制的表。  
  
**-m**仅适用于第二个加载阶段。 它不适用于第一的加载阶段;将数据加载到临时表。  
  
没有多事务模式下，这意味着，必须由您自己的加载进程处理失败或已中止负载进行恢复，会回滚。  
  
我们建议使用 **-m**仅时加载到空表，以便能够恢复而不丢失数据。 若要从负载故障中恢复： 除去目标表、 解决加载问题、 重新创建目标表中，并再次运行该负载。  
  
**-N**  
验证目标设备具有有效的 SQL Server PDW 证书从受信任的颁发机构。 用于帮助确保你的数据未被的攻击者劫持并发送到未经授权的位置。 该证书必须已安装在设备上。 安装证书的唯一受支持的方法是，让设备管理员若要使用 Configuration Manager 工具安装。 如果您不确定设备是否已安装受信任的证书要求你设备的管理员。  
  
**-se**  
跳过加载空文件。 这还将跳过正在解压缩空 gzip 文件。

**-l**  
可用 CU7.4 更新后，指定可以加载的最大行长度 （以字节为单位）。 有效值为 32768 和 33554432 之间的整数。 只能使用在需要时加载大型行 （大于 32 KB），因为这将会分配更多客户端和服务器上的内存。
  
## <a name="return-code-values"></a>返回代码值  
0 （成功） 或其他整数值 （失败）  
  
在命令窗口或批处理文件中，使用`errorlevel`要显示的返回代码。 例如：  
  
```  
dwloader  
echo ReturnCode=%errorlevel%  
if not %errorlevel%==0 echo Fail  
if %errorlevel%==0 echo Success  
```  
  
使用 PowerShell 时，使用`$LastExitCode`。  
  
## <a name="permissions"></a>权限  
需要负载的权限和目标表适用的权限 (INSERT、 UPDATE、 DELETE)。 将在临时数据库要求 （用于创建临时表） 创建权限。 如果未使用临时数据库，然后在目标数据库上需要创建具有权限。 

<!-- MISSING LINK
For more information, see [Grant permissions to load data](grant-permissions-to-load-data.md).  
-->
  
## <a name="general-remarks"></a>一般备注  
数据类型转换时使用 dwloader 加载的信息，请参阅[数据类型转换规则适用于 dwloader](dwloader-data-type-conversion-rules.md)。  
  
如果参数包含一个或多个空格，则将用双引号引起来的参数。  
  
应从其安装位置运行加载程序。 可执行文件 dwloader 预装了设备和位于 C:\Program Files\Microsoft SQL Server 数据 Warehouse\DWLoader 目录中。  
  
您可以重写参数文件中指定的参数 (-f 选项) 通过将指定为命令行参数。  
  
可以同时运行多个实例的加载程序。 加载程序实例的最大数目预配置，并且不能更改。 

<!-- MISSING LINK
For the maximum number of loads per appliance, see [Minimum and Maximum Values](minimum-maximum-values.md)  
-->
  
加载的数据可能需要更多或更少的空间比在源位置中的设备上。 您可以执行测试导入和数据来估计磁盘使用情况的子集。  
  
尽管**dwloader**是一个事务的过程并将回滚正常失败时，它不能回滚完成大容量加载已成功完成后返回。 若要取消某个活动**dwloader**处理中，键入 CTRL + C。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
必须小于 LOG_SIZE 的数据库，我们建议所有并发加载的总大小同时发生的所有加载的总大小为小于 50%的 LOG_SIZE。 若要实现此大小限制，你可以拆分为多个批次较大负载。 有关 LOG_SIZE 的详细信息，请参阅[创建数据库](../t-sql/statements/create-database-parallel-data-warehouse.md)  
  
在加载时使用一种负载命令的多个文件，所有拒绝的行写入相同的拒绝文件。 拒绝文件不显示哪些输入的文件包含每个已拒绝的行。  
  
空字符串不应作为分隔符。 当空字符串用作行分隔符时，加载将失败。 作为列分隔符使用时，负载会忽略分隔符，并继续使用默认"|"作为列分隔符。 当用作字符串分隔符，则为空字符串将被忽略并应用默认行为。  
  
## <a name="locking-behavior"></a>锁定行为  
**dwloader**锁定行为会有所不同，具体取决于*load_mode_option*。  
  
-   **追加**-追加是建议的最常见的选项。 追加到一个临时表加载数据。 下面详细介绍的锁定。  
  
-   **快速追加**-快速追加加载直接到最终表采用使用 ExclusiveUpdate 表锁，并且是不使用临时表的唯一模式。  
  
-   **重新加载**-重新加载到临时表加载数据，并需要临时表和最终的表的排他锁。 重新加载不建议用于并发操作。  
  
-   **更新插入**-更新插入将数据加载到临时表，然后执行合并操作从临时表到最终表。 更新插入不需要最终的表的排他锁。 在使用 upsert，性能可能会有所不同。 在您的环境中测试行为。  
  
### <a name="locking-behavior"></a>锁定行为  
**追加模式锁定**  
  
追加可以在多事务 （使用-m 参数） 的模式下运行，但它不是安全的事务。 因此追加 （不使用-m 参数） 用作事务性操作。 遗憾的是，在最后一个 INSERT SELECT 操作时，事务模式低于当前六倍多事务模式。  
  
追加模式在两个阶段中加载数据。 第一阶段将数据从原始文件加载到临时表同时 （碎片可以发生）。 第二个阶段将数据从临时表加载到最终表。 第二个阶段执行**INSERT INTO...选择 WITH (TABLOCK)** 操作。 下表显示的最终表上的锁定行为和日志记录行为时使用追加模式：  
  
|表类型|多事务<br />模式 (-m)|表为空|支持的并发|日志记录|  
|--------------|-----------------------------------|------------------|-------------------------|-----------|  
|堆|是|是|是|最小|  
|堆|是|否|是|最小|  
|堆|否|是|否|最小|  
|堆|否|否|否|最小|  
|cl|是|是|否|最小|  
|cl|是|否|是|完全|  
|cl|否|是|否|最小|  
|cl|否|否|是|完全|  
  
上面的表所示**dwloader**使用追加模式加载到堆或聚集的索引 (CI) 表，或如果没有多事务标志，以及将加载到空表或非空表。 锁定和日志记录行为的负载的每个此类组合的表中显示。 例如，加载使用追加模式 （第 2 个） 阶段到聚集索引而无需多事务模式和一个空表将具有创建表的排他锁的 PDW 和日志记录是最小。 这意味着客户将无法再到空表同时加载 （第 2 个） 阶段和查询。 但是，在加载时使用相同的配置到非空表，PDW 不会发出对表的排他锁和并发性可能。 遗憾的是，完整的日志记录发生时，减慢了处理过程。  
  
## <a name="examples"></a>示例  
  
### <a name="a-simple-dwloader-example"></a>A. 简单 dwloader 示例  
下面的示例演示的用于启动**加载程序**与仅选择所需的选项。 其他选项会从全局配置文件中，获取*loadparamfile.txt*。  
  
使用 SQL Server 身份验证的示例。  
  
```  
--Load over Ethernet  
dwloader.exe -S 10.192.63.148 -U mylogin -P 123jkl -f /configfiles/loadparamfile.txt  
  
--Load over InfiniBand to appliance named MyPDW  
dwloader.exe -S MyPDW-SQLCTL01 -U mylogin -P 123jkl -f /configfiles/loadparamfile.txt  
```  
  
使用 Windows 身份验证的同一个示例。  
  
```  
--Load over Ethernet  
dwloader.exe -S 10.192.63.148 -W -f /configfiles/loadparamfile.txt  
  
--Load over InfiniBand to appliance named MyPDW  
dwloader.exe -S MyPDW-SQLCTL01 -W -f /configfiles/loadparamfile.txt  
```  
  
使用源代码文件和错误文件的参数的示例。  
  
```  
--Load over Ethernet  
dwloader.exe -U mylogin -P 123jkl -S 10.192.63.148  -i C:\SQLData\AWDimEmployees.csv -T AdventureWorksPDW2012.dbo.DimEmployees -R C:\SQLData\LoadErrors  
```  
  
### <a name="b-load-data-into-an-adventureworks-table"></a>B. 将数据加载到 AdventureWorks 表  
下面的示例是将数据加载到的批处理脚本的一部分**AdventureWorksPDW2012**。  若要查看完整脚本，请打开附带的 aw_create.bat 文件**AdventureWorksPDW2012**安装包。 

<!-- Missing link
For more information, see [Install AdventureWorksPDW2012](install-adventureworkspdw2012.md).  
-->

以下脚本代码片段使用 dwloader 将数据加载到 DimAccount 和 DimCurrency 表。 此脚本使用以太网地址。 如果它已使用 InfiniBand，服务器将是 *< appliance_name >* `-SQLCTL01`。  
  
```  
set server=10.193.63.134  
set user=<MyUser>  
set password=<MyPassword>  
  
set schema=AdventureWorksPDW2012.dbo  
set load="C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\dwloader.exe"  
set mode=reload  
  
--Loads data into the AdventureWorksPDW2012.dbo.DimAccount table  
--Source data is stored in the file DimAccount.txt,   
--which is in the current directory.  
  
set t1=DimAccount  
%load% -S %server% -E -M %mode% -e Utf16 -i .\%t1%.txt -T %schema%.%t1% -R %t1%.bad -t "|" -r \r\n -U %user% -P %password%   
  
--Loads data from the DimCurrency.txt file into  
--AdventureWorksPDW2012.dbo.DimCurrency  
set t1=DimCurrency  
%load% -S %server% -E -M %mode% -e Utf16 -i .\%t1%.txt -T %schema%.%t1% -R %t1%.bad -t "|" -r \r\n -U %user% -P %password%  
```  
  
下面是用于 DimAccount 表的 DDL。  
  
```  
CREATE TABLE DimAccount(  
AccountKey int NOT NULL,  
ParentAccountKey int,  
AccountCodeAlternateKey int,  
ParentAccountCodeAlternateKey int,  
AccountDescription nvarchar(50),  
AccountType nvarchar(50),  
Operator nvarchar(50),  
CustomMembers nvarchar(300),  
ValueType nvarchar(50),  
CustomMemberOptions nvarchar(200))  
with (CLUSTERED INDEX(AccountKey),  
DISTRIBUTION = REPLICATE);  
```  
  
下面是数据文件，DimAccount.txt，其中包含要加载到表 DimAccount 数据的示例。  
  
```  
--Sample of data in the DimAccount.txt load file.  
  
1||1||Balance Sheet||~||Currency|  
2|1|10|1|Assets|Assets|+||Currency|  
3|2|110|10|Current Assets|Assets|+||Currency|  
4|3|1110|110|Cash|Assets|+||Currency|  
5|3|1120|110|Receivables|Assets|+||Currency|  
6|5|1130|1120|Trade Receivables|Assets|+||Currency|  
7|5|1140|1120|Other Receivables|Assets|+||Currency|  
8|3|1150|110|Allowance for Bad Debt|Assets|+||Currency|  
9|3|1160|110|Inventory|Assets|+||Currency|  
10|9|1162|1160|Raw Materials|Assets|+||Currency|  
11|9|1164|1160|Work in Process|Assets|+||Currency|  
12|9|1166|1160|Finished Goods|Assets|+||Currency|  
13|3|1170|110|Deferred Taxes|Assets|+||Currency|  
```  
  
### <a name="c-load-data-from-the-command-line"></a>C. 从命令行中加载数据  
可以通过在命令行中，输入所有参数替换的示例 B 中的脚本，如下面的示例中所示。  
  
```  
C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\dwloader.exe -S <Control node IP> -E -M reload -e UTF16 -i .\DimAccount.txt -T AdventureWorksPDW2012.dbo.DimAccount -R DimAccount.bad -t "|" -r \r\n -U <login> -P <password>  
```  
  
命令行参数的说明：  
  
-   *C:\Program Files\Microsoft SQL Server 并行数据 Warehouse\100\dwloader.exe*是 dwloader.exe 的安装的位置。  
  
-   *-S*跟控制节点的 IP 地址。  
  
-   *-E*指定加载空字符串为 NULL。  
  
-   *-M 重新加载*指定要插入的源数据之前截断目标表。  
  
-   *-e UTF16*指示源文件使用小 endian 字符编码类型。  
  
-   *-i.\DimAccount.txt*指定该数据位于名为 DimAccount.txt 当前目录中存在的文件。  
  
-   *-T AdventureWorksPDW2012.dbo.DimAccount*指定要接收的数据的表的 3 个部分名称。  
  
-   *-R DimAccount.bad*指定加载失败的行都将写入一个名为 DimAccount.bad 文件。  
  
-   *-t"|"* 指示用竖线分隔 DimAccount.txt，对输入文件中的字段。  
  
-   *-r \r\n*指定 DimAccount.txt 中的每一行结尾回车和换行字符。  
  
-   *-U < login_name >-P <password>* 指定登录名和密码的登录名有权执行加载。  
  

<!-- MISSING LINK

## See Also  
[Common Metadata Query Examples](metadata-query-examples.md)  

-->
  
