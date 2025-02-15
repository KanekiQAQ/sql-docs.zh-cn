---
title: FORMAT_STRING 内容 (MDX) |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: da10e8ab533c2fbfec76c89527fca0fd67082a6d
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62802447"
---
# <a name="mdx-cell-properties---formatstring-contents"></a>MDX 单元属性 - FORMAT_STRING 内容
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  **FORMAT_STRING** 单元属性格式化 **VALUE** 单元属性以生成 **FORMATTED_VALUE** 单元属性的值。 **FORMAT_STRING** 单元属性可以处理字符串和数字原始值，它对该值应用格式表达式以返回 **FORMATTED_VALUE** 单元属性的格式化值。 下列各表详述了用于处理字符串值和数值的语法和格式字符。  
  
## <a name="string-values"></a>字符串值  
 字符串的格式表达式可以是一部分，也可以是由分号 (;) 分隔开的两部分。  
  
|用法|结果|  
|-----------|------------|  
|一部分|格式应用于所有字符串值。|  
|两部分|第一部分应用于字符串数据，而第二部分应用于 Null 值和零长度字符串 ("")。|  
  
 下表所描述的字符可出现在字符串的格式字符串中。  
  
|字符|Description|  
|---------------|-----------------|  
|@|表示显示一个字符或一个空格的字符占位符。 如果字符串在格式字符串中出现 at 符号 (@) 的位置有一个字符，格式化字符串将显示该字符。 否则，格式化字符串在该位置显示一个空格。 除非格式字符串中有惊叹号 (!)，否则将从右向左填充占位符。|  
|&|表示显示一个字符或不显示任何内容的字符占位符。 如果字符串在出现“与”符号 (&) 的位置有一个字符，格式化字符串将显示该字符。 否则，格式化字符串什么也不显示。 除非格式字符串中有惊叹号 (!)，否则将从右向左填充占位符。|  
|\<|强制小写。 格式化字符串以小写格式显示所有字符。|  
|>|强制大写。 格式化字符串以大写格式显示所有字符。|  
|!|强制从左到右填充占位符。 （默认方式是从右到左填充占位符。）|  
  
## <a name="numeric-values"></a>数值  
 数字的用户定义格式表达式可以有一到四部分（各部分之间用分号分隔）。 如果格式参数包含一个命名数字格式，则只允许有一部分。  
  
|用法|结果|  
|-----------|------------|  
|一部分|格式表达式应用于所有值。|  
|两部分|第一部分应用于正值和零，第二部分应用于负值。|  
|三部分|第一部分应用于正值，第二部分应用于负值，第三部分应用于零。|  
|四部分|第一部分应用于正值，第二部分应用于负值，第三部分应用于零，第四部分应用于 Null 值。|  
  
 下面的示例有两部分。 第一部分定义正值和零的格式，第二部分定义负值的格式。  
  
```  
"$#,##0;($#,##0)"  
```  
  
 如果包含两个连续的分号，则缺少的部分用正值的格式显示。 例如，下面的格式用第一部分的格式显示正值和负值，如果值为零，则显示“Zero”：  
  
```  
"$#,##0;;\Z\e\r\o"  
```  
  
 下表标识了可出现在数字格式的格式字符串中的字符。  
  
|字符|Description|  
|---------------|-----------------|  
|None|显示不带任何格式的数字。|  
|**0**|表示显示一个数字或零 (0) 的数字占位符。<br /><br /> 如果数字在格式字符串中出现零的位置有一个数字，格式化值将显示该数字。 否则，格式化值在该位置显示零。<br /><br /> 如果数字的位数少于格式表达式中零的个数（不管在小数点的哪一侧），格式化值将显示前导零或尾随零。<br /><br /> 如果小数点分隔符右侧的数字位数多于格式表达式中小数点分隔符右侧的零的个数，格式化值将对数字四舍五入，使其小数位数与零的个数一样多。<br /><br /> 如果小数点分隔符左侧的数字位数多于格式表达式中小数点分隔符左侧的零的个数，格式化值将显示多出的位数而不作任何修改。|  
|**#**|表示显示一个数字或不显示任何内容的数字占位符。<br /><br /> 如果表达式在格式字符串中出现数字符号 ( **#** ) 的位置有一个数字，格式化值将显示该数字。 否则，格式化值在该位置什么也不显示。<br /><br /> 除了当数字的位数等于或少于格式表达式中小数点分隔符任意一侧的 **#** 字符数时不显示前导零或尾随零外，数字符号 ( **) 占位符的作用类似于零 (** 0 **#** ) 数字占位符。|  
|**。**|表示决定小数点分隔符左侧和右侧显示几位数的小数点占位符。<br /><br /> 如果格式表达式中句点 ( **#** . **) 左侧只包含数字符号 (** ) 字符，则小于 1 的数字以小数点分隔符开头。 若要显示用小数显示的前导零，请使用零 (0) 作为小数点分隔符左侧的第一个数字占位符。<br /><br /> 在格式化输出中用作小数点占位符的实际字符取决于计算机系统所识别的数字格式。<br /><br /> 注意：在某些区域设置中，逗号用作十进制分隔符。|  
|**%**|表示百分比占位符。 将表达式乘以 100， 并在格式字符串中出现百分比符号的位置插入百分比字符 ( **%** )。|  
|**、**|表示将小数点分隔符左侧具有四位或更多位数的数字中的千位和百位分隔开的千位分隔符。<br /><br /> 如果格式包含一个由数字占位符（**0** 或 **#** ）包围的千位分隔符，则指定千位分隔符的标准用法。<br /><br /> 两个相邻的千位分隔符或一个千位分隔符紧挨小数点分隔符的左侧（无论是否指定小数）表示“通过除以 1000 来将数字按比例减小，并按所需的方式四舍五入”。 例如，可以使用格式字符串“ **##0**,,”将 100,000,000 表示为 100。 小于 1,000,000 的数字显示为 0。 对于两个相邻的千位分隔符，只要不是出现在紧挨小数点分隔符的左侧，就被视为指定使用千位分隔符。<br /><br /> 在格式化输出中用作千位分隔符的实际字符取决于计算机系统所识别的数字格式。<br /><br /> 注意：在某些区域设置中，句点用作千位分隔符。|  
|**设置用户帐户 ：**|表示时间分隔符，在设置时间值的格式时，时间分隔符用于分隔小时、分钟和秒。<br /><br /> 注意：在某些区域设置，可能用其他字符作为时间分隔符。<br /><br /> 在格式化输出中用作时间分隔符的实际字符取决于计算机的系统设置。|  
|**/**|表示日期分隔符，在设置日期值的格式时，日期分隔符用于分隔年、月和日。<br /><br /> 在格式化输出中用作日期分隔符的实际字符取决于计算机的系统设置。<br /><br /> 注意：在某些区域设置中，可能用其他字符作为日期分隔符。|  
|**E- E+ e- e+**|表示科学记数法格式。<br /><br /> 如果格式表达式在**E-** 、 **#** E+ **、** e- **或**e+ **的右侧至少包含一个数字占位符（** 0 **或**），则以科学记数法格式显示格式化值，并在数字和其指数之间插入 E 或 e。 右侧的数字占位符的个数决定了指数中的数字个数。 使用 **E-** 或 **e-** 在负指数的后面包括一个减号。 使用 **E+** 或 **e+** 在负指数的后面包括一个减号，在正指数的后面包括一个加号。|  
|**- + $ ( )**|显示文字字符。<br /><br /> 若要显示所列字符以外的其他字符，请在该字符前加上一个反斜杠 ( **\\** ) 或将该字符放在双引号 ( **" "** ) 中。|  
|**\\**|显示格式字符串中的下一个字符。<br /><br /> 若要将具有特殊含义的字符显示为文字字符，请在该字符前加上一个反斜杠 ( **\\** )。 反斜杠本身不显示。 使用反斜杠与将下一个字符放在双引号中的作用是相同的。 若要显示反斜杠，请使用两个反斜杠 ( **\\\\** )。 不能显示为文字字符的字符示例包括下列字符：<br /><br /> <br /><br /> 日期格式设置和时间格式设置字符-  ， **c**， **d**， **h**， **m**， **n**， **p**， **q**， **s**， **t**， **w**， **y**， **/** ，和 **:**<br /><br /> 数值格式字符- **#** ， **0**， **%** ， **E**， **e**，**逗号**，并**段**<br /><br /> 字符串格式设置字符- **@** ， **&** ， **\<** ， **>** ，和 **！**|  
|**"ABC"**|显示双引号 ( **" "** ) 里面的字符串。<br /><br /> 若要将字符串包含在代码内的格式中，请将文本放在 Chr(**34**) 之间。 （双引号的字符代码为 **34**。）|  
  
### <a name="named-numeric-formats"></a>命名数字格式  
 下表标识预定义数字格式的名称：  
  
|格式名|Description|  
|-----------------|-----------------|  
|`General Number`|显示不带千位分隔符的数字。|  
|`Currency`|显示带千位分隔符（如果适合）的数字。 显示小数分隔符右边的两位数字。 输出基于系统区域设置。|  
|`Fixed`|小数点分隔符左侧至少显示一个数字，右侧至少显示两个数字。|  
|`Standard`|显示带千位分隔符的数字，其中小数点分隔符左侧至少有一个数字，右侧至少有两个数字。|  
|`Percent`|将数字乘以 100 后显示，并在右侧追加百分号 (%)。 小数点分隔符右侧总是显示两个数字。|  
|`Scientific`|使用标准的科学记数法。|  
|`Yes/No`|如果数字为 0，则显示 No；否则显示 Yes。|  
|`True/False`|如果数字为 0，则显示 False；否则显示 True。|  
|`On/Off`|如果数字为 0，则显示 Off；否则显示 On。|  
  
## <a name="date-values"></a>日期值  
 下表标识了可以出现在日期/时间格式的格式字符串中的字符。  
  
|字符|Description|  
|---------------|-----------------|  
|**设置用户帐户 ：**|表示时间分隔符，在设置时间值的格式时，时间分隔符用于分隔小时、分钟和秒。<br /><br /> 在格式化输出中用作时间分隔符的实际字符取决于计算机的系统设置。<br /><br /> 注意：在某些区域设置中，可能用其他字符作为时间分隔符。|  
|**/**|表示日期分隔符，在设置日期值的格式时，日期分隔符用于分隔年、月和日。<br /><br /> 在格式化输出中用作日期分隔符的实际字符取决于计算机的系统设置。<br /><br /> 注意：在某些区域设置，可能用其他字符是表示日期分隔符|  
|**C**|按顺序将日期显示为 **ddddd** ，将时间显示为 **ttttt**。<br /><br /> 如果日期序列号没有小数部分，则只显示日期信息。 如果没有整数部分，则只显示时间信息。|  
|**d**|将天显示为不带前导零的数字 (1-31)。|  
|**dd**|将天显示为带前导零的数字 (01-31)。|  
|**ddd**|将天显示为缩写 (Sun Sat)。|  
|**dddd**|将天显示为全名 （星期日至星期六）。|  
|**ddddd**|将日期显示为完整日期（包括日、月和年），根据用户系统的短日期格式设置进行格式化。<br /><br /> 对于 Microsoft Windows，默认的短日期格式为 **m/d/yy**。|  
|**dddddd**|将日期序列号显示为完整日期（包括日、月和年），根据计算机系统所识别的长日期设置进行格式化。<br /><br /> 对于 Windows，默认的长日期格式为 **mmmm dd, yyyy**。|  
|**w**|将一周中的天显示为数字（1 代表星期天，依次排列到 7，7 代表星期六）。|  
|**ww**|一年中的周显示为数字 (1-54)。|  
|**m**|将月显示为不带前导零的数字 (1-12)。<br /><br /> 如果 **m** 紧跟 **h** 或 **hh**，则显示分钟而不显示月份。|  
|**mm**|将月显示为带前导零的数字 （01 到 12 个）。<br /><br /> 如果 **m** 紧跟 **h** 或 **hh**，则显示分钟而不显示月份。|  
|**mmm**|将月显示为缩写 （Jan 12 月）。|  
|**mmmm**|将月显示为完整的月份名称 （年 1 月-12 月）。|  
|**q**|显示为数字 (1-4) 的一年中的每个季度。|  
|**y**|一年中的天显示为数字 (1-366)。|  
|**yy**|将年份显示为两位数字 (00-99)。|  
|**yyyy**|将年份显示为四位数字 (100-9999)。|  
|h |不带前导零 (0-23) 将小时显示为数字。|  
|**hh**|将小时显示为带前导零 (00-23) 的数字。|  
|**n**|将分钟显示为数字，不带前导零 (0-59)。|  
|**nn**|将分钟显示为带前导零 (00-59) 的数字。|  
|**s**|将秒显示为数字，不带前导零 (0-59)。|  
|**ss**|将秒显示为带前导零 (00-59) 的数字。|  
|**t t t t t**|将时间显示为完整的时间（包括小时、分钟和秒），使用计算机系统所识别的时间格式定义的时间分隔符进行格式化。<br /><br /> 如果选择了前导零选项，并且时间早于 10:00，无论是在上午 (A.M.) 还是在下午 (P.M.)， 都显示前导零。 例如，09:59。<br /><br /> 对于 Windows，默认的时间格式为 **h:mm:ss**。|  
|**AM/PM**|将从午夜到正午的任何一小时与大写的 **AM** 一起显示，将从正午到午夜的任何一小时与大写的 **PM** 一起显示。<br /><br /> 注意：使用 12 小时时钟。|  
|**am/pm**|将从午夜到正午的任何一小时与小写的 **am** 一起显示，将从正午到午夜的任何一小时与小写的 **pm** 一起显示。<br /><br /> 注意：使用 12 小时时钟。|  
|**A/P**|将从午夜到正午的任何一小时与大写的 **A** 一起显示，将从正午到午夜的任何一小时与大写的 **P** 一起显示。<br /><br /> 注意：使用 12 小时时钟。|  
|**a/p**|将从午夜到正午的任何一小时与小写的 **a** 一起显示，将从正午到午夜的任何一小时与小写的 **p** 一起显示。<br /><br /> 注意：使用 12 小时时钟。|  
|**AMPM**|将 AM 字符串文字以计算机系统定义的形式与从午夜到正午的任何一小时一起显示，将 PM 字符串文字以计算机系统定义的形式与从正午到午夜的任何一小时一起显示。<br /><br /> **AMPM** 可以是大写或小写形式，但显示的字符串的大小写应与计算机系统设置定义的字符串的大小写一致。<br /><br /> 对于 Windows，默认的格式为 **AM/PM**。<br /><br /> 注意：使用 12 小时时钟。|  
  
### <a name="named-date-formats"></a>命名日期格式  
 下表标识预定义的日期格式和时间格式的名称：  
  
|格式名|Description|  
|-----------------|-----------------|  
|`General Date`|显示日期和/或时间。 对于实数，显示日期和时间，例如，4/3/93 05:34 PM。 如果没有小数部分，则只显示日期，例如 4/3/93。 如果没有整数部分，则只显示时间，例如 05:34 PM。 日期显示的格式由您的系统设置决定。|  
|`Long Date`|根据您的系统的长日期格式显示日期。|  
|`Medium Date`|使用适合于宿主应用程序语言版本的中日期格式显示日期。|  
|`Short Date`|使用您的系统的短日期格式显示日期。|  
|`Long Time`|使用您的系统的长时间格式显示时间；包括小时、分钟和秒。|  
|`Medium Time`|使用小时、分钟和 AM/PM 指示符以 12 小时格式显示时间。|  
|`Short Time`|使用 24 小时格式显示时间，例如 17:45。|  
  
## <a name="see-also"></a>请参阅  
 [基于 LANGUAGE 和 FORMAT_STRING 上 FORMATTED_VALUE](../../../analysis-services/multidimensional-models/mdx/mdx-cell-properties-formatted-value-property.md)   
 [使用单元属性 (MDX)](../../../analysis-services/multidimensional-models/mdx/mdx-cell-properties-using-cell-properties.md)   
 [创建和使用属性值 (MDX)](http://msdn.microsoft.com/library/0cafb269-03c8-4183-b6e9-220f071e4ef2)   
 [MDX 查询基础知识 (Analysis Services)](../../../analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services.md)  
  
  
