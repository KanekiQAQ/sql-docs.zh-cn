---
title: 排序规则和 Unicode 支持 | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- binary collations [SQL Server]
- expression-level collations [SQL Server]
- Windows collations [SQL Server]
- globalization [SQL Server], about globalization
- supplementary character support
- code pages [SQL Server], about code pages
- collations [SQL Server], about collations
- Unicode [SQL Server], collations
- database-level collations [SQL Server]
- reading order
- column-level collations
- locales [SQL Server], about locales
- locales [SQL Server]
- code pages [SQL Server]
- SQL Server collations
- UTF-8
- UTF-16
- server-level collations [SQL Server]
ms.assetid: 92d34f48-fa2b-47c5-89d3-a4c39b0f39eb
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bcff15423fb1ab3f1f05347bddba6eab09fae713
ms.sourcegitcommit: ab867100949e932f29d25a3c41171f01156e923d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67419200"
---
# <a name="collation-and-unicode-support"></a>Collation and Unicode Support
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的排序规则可为您的数据提供排序规则、区分大小写属性和区分重音属性。 与诸如 **char** 和 **varchar** 等字符数据类型一起使用的排序规则规定可表示该数据类型的代码页和对应字符。 无论你是要安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的新实例，还原数据库备份，还是将服务器连接到客户端数据库，都必须了解正在处理的数据的区域设置要求、排序顺序以及是否区分大小写和重音。 若要列出在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例上可用的排序规则，请参阅 [sys。fn_helpcollations &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)。    
    
在为你的服务器、数据库、列或表达式选择排序规则时，也在向你的数据分配某些特征，这些特征会影响数据库中许多操作的结果。 例如，使用 `ORDER BY` 构造查询时，结果集的排序顺序可能取决于应用于该数据库的排序规则或 `COLLATE` 子句中在查询的表达式级别规定的排序规则。    
    
为了充分利用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中的排序规则支持，您必须了解本主题中所定义的术语以及这些术语与您的数据的特征之间的关系。    
    
##  <a name="Terms"></a> 排序规则术语    
    
-   [排序规则](#Collation_Defn)    
    
-   [区域设置](#Locale_Defn)    
    
-   [代码页](#Code_Page_Defn)    
    
-   [排序顺序](#Sort_Order_Defn)    
    
###  <a name="Collation_Defn"></a> 排序规则    
排序规则指定表示数据集中每个字符的位模式。 排序规则还确定数据的排序和比较规则。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持在单个数据库中存储具有不同排序规则的对象。 对于非 Unicode 列，排序规则设置指定数据的代码页以及可以表示哪些字符。 必须将在非 Unicode 列间移动的数据从源代码页转换到目标代码页。    
    
当[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在具有不同排序规则设置的不同数据库上下文中运行时，其运行结果可能会不同。 如果可能，请为您的组织使用标准化排序规则。 这样就不必显式指定每个字符或 Unicode 表达式中的排序规则。 如果必须使用具有不同排序规则和代码页设置的对象，请对查询进行编码，以考虑排序规则的优先顺序规则。 有关详细信息，请参阅 [排序规则优先顺序 (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md)。    
    
与排序规则关联的选项区分大小写、区分重音、区分假名、区分全半角以及区分变体选择符。 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 为 [UTF-8](https://www.wikipedia.org/wiki/UTF-8) 编码引入了其他选项。 指定这些选项的方法是将它们追加到排序规则名称。 例如，排序规则 `Japanese_Bushu_Kakusu_100_CS_AS_KS_WS_UTF8` 区分大小写、区分重音、区分假名、区分全半角以及进行 UTF-8 编码。 再举一例，此排序规则 `Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS` 就不区分大小写、不区分重音、区分假名、区分全半角、区分变体选择符以及使用非 Unicode 编码。 下表描述了与这些不同选项关联的行为。    
    
|选项|描述|    
|------------|-----------------|    
|区分大小写 (\_CS)|区分大写字母和小写字母。 如果选择此项，排序时小写字母将在其对应的大写字母之前。 如果未选择此选项，则排序规则将不区分大小写。 即 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在排序时将大写字母和小写字母视为相同。 通过指定 \_CI，可以显式选择不区分大小写。|    
|区分重音 (\_AS)|区分重音字符和非重音字符。 例如，“a”和“ấ”视为不同字符。 如果未选择此选项，则排序规则将不区分重音。 即 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在排序时将字母的重音形式和非重音形式视为相同。 通过指定 \_AI，可以显式选择不区分重音。|    
|区分假名 (\_KS)|区分日语中的两种假名字符类型：平假名和片假名。 如果未选择此选项，则排序规则将不区分假名。 即 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在排序时将平假名字符和片假名字符视为相同。 省略此选项是指定不区分假名的唯一方法。|    
|区分全半角 (\_WS)|区分全角字符和半角字符。 如果未选择此选项，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会在排序时将同一字符的全角和半角形式视为相同。 省略此选项是指定不区分全半角的唯一方法。|    
|区分变体选择符 (\_VSS) | 区分 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]中首次引入的日语排序规则 Japanese_Bushu_Kakusu_140 和 Japanese_XJIS_140 中不同的象形变体选择符。 变体序列包含基本字符加上其他变体选择符。 如果未选择 \_VSS 选项，排序规则不区分变体选择符，并且在比较中不考虑变体选择符。 也就是说，出于排序的考虑，SQL Server 会将具有不同变体选择符但基于相同基本字符的字符视为相同。 另请参阅  [Unicode Ideographic Variation Database](https://www.unicode.org/reports/tr37/)（Unicode 象形变体数据库） <br/><br/> 全文搜索索引中不支持区分变体选择符 (\_VSS) 排序规则。 全文搜索索引仅支持区分重音 (\_AS)、区分假名 (\_KS) 和区分全半角 (\_WS) 选项。 SQL Server XML 和 CLR 引擎不支持 (\_VSS) 变体选择符。
|UTF-8 (\_UTF8)|启用要在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中存储的 UTF-8 编码数据。 如果未选择此选项，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会对适用的数据类型使用默认的非 Unicode 编码格式。| 
    
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持以下排序规则集：    
    
#### <a name="windows-collations"></a>Windows 排序规则    
Windows 排序规则根据关联的 Windows 系统区域设置来定义字符数据的存储规则。 在 Windows 排序规则中，使用与 Unicode 数据相同的算法实现非 Unicode 数据的比较。 Windows 基本排序规则指定应用字典排序时所用的字母表或语言，以及用于存储非 Unicode 字符数据的代码页。 Unicode 排序和非 Unicode 排序都与特定 Windows 版本中的字符串比较相兼容。 这保证了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中所有数据类型的一致性，还使开发人员能够使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所使用的相同规则对应用程序中的字符串排序。 有关详细信息，请参阅 [Windows 排序规则名称 (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)。    
    
#### <a name="binary-collations"></a>二进制排序规则    
二进制排序规则基于区域设置和数据类型定义的编码值顺序来对数据进行排序。 它们是区分大小写的。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的二进制排序规则定义了所使用的区域设置和 ANSI 代码页。 这将强制使用二进制排序顺序。 由于它们相对简单，因此二进制排序规则有助于提高应用程序性能。 对于非 Unicode 数据类型，数据比较将基于 ANSI 代码页中定义的码位。 对于 Unicode 数据类型，数据比较将基于 Unicode 码位。 对于 Unicode 数据类型的二进制排序规则，数据排序将不考虑区域设置。 例如，对 Unicode 数据应用 Latin_1_General_BIN 和 Japanese_BIN，会得到完全相同的排序结果。    
    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中有两种二进制排序规则：旧的 **BIN** 排序规则和新的 **BIN2** 排序规则。 在 **BIN2** 排序规则中，所有字符根据其码位排序。 在 **BIN** 排序规则中，仅首字符按照码位排序，其余字符根据其字节值排序。 （由于 Intel 平台是一个 little endian 体系结构，因此 Unicode 码字符始终以字节对调的形式存储。）    
    
#### <a name="sql-server-collations"></a>SQL Server 排序规则    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则 (SQL_\*) 提供与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本兼容的排序顺序。 非 Unicode 数据的字典排序规则与 Windows 操作系统提供的任何排序例程都不兼容。 但是，Unicode 数据的排序与特定版本的 Windows 排序规则兼容。 由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则对非 Unicode 数据和 Unicode 数据使用不同的比较规则，因此对于相同数据的比较会看到不同的结果，具体取决于基本数据类型。 有关详细信息，请参阅 [SQL Server 排序规则名称 (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)。    
    
> [!NOTE]    
> 升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的英文实例时可以指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则 (SQL_\*)，以便与现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例兼容。 由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的默认排序规则是在安装过程中定义的，因此在以下情况下确保慎重指定排序规则设置：    
>     
> -   应用程序代码依赖早期 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则的行为。    
> -   必须存储反映多种语言的字符数据。    
    
 支持在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的下列级别设置排序规则：    
    
#### <a name="server-level-collations"></a>服务器级排序规则   
默认服务器排序规则是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中设置的，还将成为系统数据库和所有用户数据库的默认排序规则。 请注意，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中不能选择仅 Unicode 排序规则，因为不支持将它们用作服务器级排序规则。    
    
除了导出所有数据库对象和数据、重新生成 **master** 数据库和导入所有数据库对象和数据以外，一旦为服务器分配了某一排序规则后，就无法更改该排序规则。 与更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的默认排序规则相反，您可在创建新数据库或数据库列时指定所需的排序规则。    
    
#### <a name="database-level-collations"></a>数据库级排序规则    
创建或修改数据库时，您可使用 CREATE DATABASE 或 ALTER DATABASE 语句的 COLLATE 子句指定默认数据库排序规则。 如果未指定排序规则，将为该数据库分配服务器排序规则。    
    
除了更改服务器的排序规则外，无法更改系统数据库的排序规则。    
    
数据库排序规则将应用于数据库中的所有元数据，并且是所有字符串列、临时对象、变量名称和数据库中使用的任何其他字符串的默认排序规则。 当更改用户数据库的排序规则时，如果在数据库访问临时表中进行查询，则可能出现排序规则冲突。 临时表始终存储在 tempdb 系统数据库中，该数据库使用实例的排序规则  。 如果排序规则导致计算字符数据时出现冲突，则比较用户数据库和 **tempdb** 之间的字符数据的查询可能会失败。 您可以通过在查询中指定 COLLATE 子句来解决此问题。 有关详细信息，请参阅[排序规则 (Transact-SQL)](~/t-sql/statements/collations.md)。    
    
#### <a name="column-level-collations"></a>列级排序规则    
当您创建或更改表时，可使用 COLLATE 子句指定每个字符串列的排序规则。 如果未指定排序规则，则为该列分配数据库的默认排序规则。    
    
#### <a name="expression-level-collations"></a>表达式级排序规则    
表达式级排序规则在语句运行时设置，并且影响结果集的返回方式。 这可以使 ORDER BY 排序结果特定于区域设置。 使用如下的 COLLATE 子句可以实现表达式级排序规则：    
    
```sql    
SELECT name FROM customer ORDER BY name COLLATE Latin1_General_CS_AI;    
```    
    
###  <a name="Locale_Defn"></a> 区域设置    
区域设置是与位置或区域性相关联的一组信息。 它可以包括所用语言的名称和标识符、用于书写该语言的文字以及文化习俗。 排序规则可以与一个或多个区域设置相关联。 有关详细信息，请参阅 [Microsoft 分配的区域设置 ID](https://msdn.microsoft.com/goglobal/bb964664.aspx)。    
    
###  <a name="Code_Page_Defn"></a> Code Page    
 代码页是给定脚本的有序字符集，其中数值索引（即码位值）与每个字符相关联。 Windows 代码页通常被称为“字符集”   。 代码页用于支持不同的 Windows 系统区域设置所使用的字符集和键盘布局。     
 
###  <a name="Sort_Order_Defn"></a> Sort Order    
 排序顺序指定数据值的排序方式。 它影响数据比较的结果。 数据的排序通过使用排序规则而实现，且可使用索引对排序进行优化。    
    
##  <a name="Unicode_Defn"></a> Unicode 支持    
Unicode 是一种将码位映射到字符的标准。 由于它旨在涵盖全球所有语言的所有字符，因此，无需使用不同代码页来处理不同字符集。 
   
客户端使用的代码页由操作系统设置确定。 若要在 Windows 操作系统上设置客户端代码页，请使用“控制面板”中的 **“区域设置”** 。    

非 Unicode 数据类型有明显的局限性， 这是因为非 Unicode 计算机只能使用单个代码页。 通过使用 Unicode 您可能会体验到性能提升，因为需要较少的代码页转换。 必须在数据库级、列级或表达式级单独选择 Unicode 排序规则，因为在服务器级不支持 Unicode 排序规则。    
   
在将数据从服务器移到客户端时，服务器排序规则可能无法由旧版本的客户端驱动程序识别。 将数据从 Unicode 服务器移到非 Unicode 客户端时，可能会出现此情况。 最好升级客户端操作系统，以使基础系统排序规则得以升级。 如果客户端上安装了数据库客户端软件，则可以考虑对数据库客户端软件应用服务更新。    
    
> [!TIP]
> 还可以尝试针对服务器上的数据使用另一个排序规则。 选择一个映射到客户端上的代码页的排序规则。    

如果在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]）中存储反映多种语言的字符数据，请使用 Unicode 数据类型（nchar  、nvarchar  和 ntext  ），而不是非 Unicode 数据类型（char  、varchar  和 text  ）。 

> [!NOTE]
> 对于 Unicode 数据类型，[!INCLUDE[ssde_md](../../includes/ssde_md.md)]最多可以使用 UCS-2 表示 65,535 个字符；或者，如果使用了附属字符，可表示整个 Unicode 范围（‭1,114,111 个字符）。 若要详细了解如何启用附属字符，请参阅[附属字符](#Supplementary_Characters)。

或者，从 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 开始，如果使用已启用 UTF-8 的排序规则 (\_UTF8)，则以前的非 Unicode 数据类型（**char** 和 **varchar**）将变为 Unicode (UTF-8) 数据类型。 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 不会更改以前存在的 Unicode (UTF-16) 数据类型（**nchar**、**nvarchar** 和 **ntext**）的行为。 如需了解更多详情，请参阅 [UTF-8 与 UTF-16 的存储差异](#storage_differences)。
       
若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]）中提供的 UTF-16 排序规则来改进对一些 Unicode 字符的搜索和排序（仅 Windows 排序规则），可以选择附属字符 (\_SC) 排序规则之一，或版本 140 排序规则之一。    
 
若要使用 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 中提供的 UTF-8 排序规则来改进对某些 Unicode 字符的搜索和排序（仅 Windows 排序规则），必须选择已启用 UTF-8 编码的排序规则 (\_UTF8)。
 
-   UTF8 标志可应用于：    
    -   90 版本的排序规则 
        > [!NOTE]
        > 仅当此版本中已存在补充字符 (\_SC) 或区分变体选择符 (\_VSS) 识别排序规则时。
    -   100 版本的排序规则    
    -   140 版本的排序规则   
    -   BIN2<sup>1</sup> 二进制排序规则
    
-   UTF8 标志不能应用于：    
    -   不支持补充字符 (\_SC) 或区分变体选择符 (\_VSS) 的 90 版本的排序规则    
    -   BIN 或 BIN2<sup>2</sup> 二进制排序规则    
    -   SQL\* 排序规则  
    
<sup>1</sup>自 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CTP 2.3 起。 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CTP 3.0 已将排序规则 UTF8_BIN2 替换为 Latin1_General_100_BIN2_UTF8。     
<sup>2</sup>截至 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] CTP 2.3。 
    
若要评估与使用 Unicode 或非 Unicode 数据类型相关的问题，请测试您的具体方案以确定您所在环境下的性能差异大小。 最好对整个组织中的系统所使用的排序规则进行标准化，并尽可能部署 Unicode 服务器和客户端。    
    
在许多情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与其他服务器或客户端交互，你的组织可能会使用应用程序和服务器实例之间的多种数据访问标准。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客户端是两种主要类型之一：    
    
-   使用 OLE DB 和开放式数据库连接 (ODBC) 3.7 版或更高版本的**Unicode 客户端** 。    
-   使用 DB-Library 和 ODBC 3.6 版或更低版本的**非 Unicode 客户端** 。    
    
下表提供有关以 Unicode 和非 Unicode 服务器的各种组合使用多语言数据的信息。    
    
|“服务器”|客户端|优点或局限性|    
|------------|------------|-----------------------------|    
|Unicode|Unicode|因为 Unicode 数据在整个系统中使用，所以此方案可提供最佳的性能并可保护检索到的数据免受破坏。 ActiveX 数据对象 (ADO)、OLE DB 和 ODBC 3.7 版或更高版本都采用这样的配置。|    
|Unicode|非 Unicode|在这种情况下，尤其对于正在运行新操作系统的服务器与正在运行旧版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]或基于旧操作系统的客户端之间的连接，当您向客户端计算机移动数据时，会受到限制或出现错误。 服务器上的 Unicode 数据尝试映射到非 Unicode 客户端的对应代码页，以转换数据。|    
|非 Unicode|Unicode|该方案对使用多语言数据不是理想配置。 在此方案下不能向非 Unicode 服务器写入 Unicode 数据。 在向服务器的代码页之外的服务器发送数据时，很可能会发生问题。|    
|非 Unicode|非 Unicode|这是对多语言数据有极大局限性的方案。 您只可使用一个代码页。|    
    
##  <a name="Supplementary_Characters"></a> 增补字符    
Unicode 联盟为每个字符都分配一个唯一码位（介于 000000 和 10FFFF 之间的值）。 最常用字符的码位值介于范围 000000 和 00FFFF（65,535 个字符）之间，可以装入内存中和磁盘上的 8 位字或 16 位字中。 通常将此范围指定为基本多文种平面 (BMP)。 

但 Unicode 联盟额外建立了 16 个字符“平面”，每个平面的大小都与 BMP 相同。 此定义允许 Unicode 表示介于码位范围 000000 和 10FFFF 之间的 1,114,112 个字符（即 2<sup>16</sup>* 17 个字符）。 码位值大于 00FFFF 的字符需要 2 到 4 个连续 8 位字 (UTF-8)，或 2 个连续 16 位字 (UTF-16)。 超出 BMP 的字符称为“附属字符”  ，其他连续 8 位字或 16 位字称为“代理项对”  。 如需了解附属字符、代理项、代理项对的更多详情，请参阅 [Unicode 标准](http://www.unicode.org/standard/standard.html)。    

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供用于存储介于 BMP 范围（000000 和 00FFFF）内的 Unicode 数据的数据类型（如 nchar  和 nvarchar  ），而[!INCLUDE[ssde_md](../../includes/ssde_md.md)]使用 UCS-2 编码它们。 

[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 引入了新附属字符 (\_SC) 排序规则系列，可以与下面的数据类型结合使用来表示整个 Unicode 字符范围（000000 和 10FFFF）：nchar  、nvarchar  和 sql_variant  。 例如： `Latin1_General_100_CI_AS_SC`或 `Japanese_Bushu_Kakusu_100_CI_AS_SC`（如果使用日语排序规则）。 
 
[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 将附属字符支持扩展到，与已启用 UTF-8 的新排序规则 ([\_UTF8](#utf8)) 结合使用的数据类型 char  和 varchar  。 这些也能表示整个 Unicode 字符范围。   

> [!NOTE]
> 自 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 起，所有新 \_140  排序规则自动支持附属字符。

如果使用增补字符：    
    
-   只有在 90 版本或更高版本的排序规则中才可以将增补字符用于排序和比较操作。    
-   所有 100 版本的排序规则均支持使用补充字符进行语言排序。    
-   不支持将增补字符用在元数据（如数据库对象的名称）中。    
-   使用含补充字符 (\_SC) 的排序规则的数据库无法在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 复制时启用。 这是因为，创建了一些用于复制的系统表和存储过程，它们使用不支持附属字符的旧 ntext  数据类型。  

-   SC 标志可应用于：    
    -   90 版本的排序规则    
    -   100 版本的排序规则    
    
-   SC 标志不能应用于：    
    -   80 版本非版本化 Windows 排序规则    
    -   BIN 或 BIN2 二进制排序规则    
    -   SQL\* 排序规则    
    -   140 版本的排序规则（这些排序规则无需 SC 标志，因为它们已支持补充字符）    
    
下表比较了某些字符串函数和字符串运算符在使用具有和没有补充字符识别 (SCA) 排序规则的补充字符时的行为：    
    
|字符串函数或运算符|具有补充字符识别 (SCA) 排序规则|没有 SCA 排序规则|    
|---------------------------------|--------------------------|-----------------------------|    
|[CHARINDEX](../../t-sql/functions/charindex-transact-sql.md)<br /><br /> [LEN](../../t-sql/functions/len-transact-sql.md)<br /><br /> [PATINDEX](../../t-sql/functions/patindex-transact-sql.md)|UTF-16 代理项对作为单个码位计数。|UTF-16 代理项对作为两个码位计数。|    
|[LEFT](../../t-sql/functions/left-transact-sql.md)<br /><br /> [REPLACE](../../t-sql/functions/replace-transact-sql.md)<br /><br /> [REVERSE](../../t-sql/functions/reverse-transact-sql.md)<br /><br /> [RIGHT](../../t-sql/functions/right-transact-sql.md)<br /><br /> [SUBSTRING](../../t-sql/functions/substring-transact-sql.md)<br /><br /> [STUFF](../../t-sql/functions/stuff-transact-sql.md)|这些函数会将每个代理项对作为单个码位处理并按预期方式工作。|这些函数可能拆分任意代理项对并导致意外的结果。|    
|[NCHAR](../../t-sql/functions/nchar-transact-sql.md)|返回对应于 0 到 0x10FFFF 范围内指定的 Unicode 码位值的字符。 如果指定的值位于 0 到 0xFFFF 范围内，则返回一个字符。 对于较高的值，则返回相应的代理项。|对于高于 0xFFFF 的值，将返回 NULL 而非相应的代理项。|    
|[UNICODE](../../t-sql/functions/unicode-transact-sql.md)|返回 0 到 0x10FFFF 范围内的一个 UTF-16 码位。|返回 0 到 0xFFFF 范围内的一个 UCS-2 码位。|    
|[匹配一个通配符](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)<br /><br /> [通配符 - 无需匹配的字符](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)|增补字符支持所有通配符操作。|增补字符不支持这些通配符操作。 支持其他通配符运算符。|    
    
## <a name="GB18030"></a> GB18030 支持    
GB18030 是在中华人民共和国用于对中文字符进行编码的一个单独标准。 在 GB18030 中，字符长度可以是 1 个字节、2 个字节或 4 个字节。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过对从客户端应用程序进入服务器的 GB18030 编码字符进行确认，然后在本机将其转换并存储为 Unicode 字符，来对这些字符提供支持。 这些字符存储在服务器中后，在所有后续操作中均视为 Unicode 字符。 可以使用任何中文排序规则，最好使用最新的 100 版本。 所有 _100 级排序规则均支持使用 GB18030 字符进行语言排序。 如果数据中包含增补字符（代理项对），您可以使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中提供的 SC 排序规则来改进搜索和排序操作。    
    
## <a name="Complex_script"></a> 复杂文种支持    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可支持输入、存储、更改和显示复杂文种。 复杂文种包括下列几种类型：    
    
-   脚本包括从右到左和从左到右两种文字组合，如阿拉伯语和英语文字的组合。    
-   脚本的字符根据其位置或在与其他字符组合时改变形状，如阿拉伯语、印度语和泰语字符。    
-   像泰语这样的语言需要内部词典识别单词，因为这些单词之间不断字。    
    
与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 交互的数据库应用程序必须使用支持复杂文种的控件。 在托管代码中创建的标准 Windows 窗体控件支持复杂文种。    

## <a name="Japanese_Collations"></a> 在  [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]
 
从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始，支持新的日语排序规则系列，并具有各种排列选项（\_CS、\_AS、\_KS、\_WS、\_VSS）。 

若要列出这些排序规则，可以查询 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]：      

```sql 
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name LIKE 'Japanese_Bushu_Kakusu_140%' OR Name LIKE 'Japanese_XJIS_140%'
``` 

由于所有新排序规则都内置有对附属字符的支持，因此新 \_140  排序规则都没有（或不需要）SC 标志。

[!INCLUDE[ssde_md](../../includes/ssde_md.md)]索引、内存优化表、列存储索引和本机编译模块支持这些排序规则。

<a name="ctp23"></a>

## <a name="utf8"></a> UTF-8 支持
[!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 完全支持广泛使用的 UTF-8 字符编码作为导入或导出编码，以及作为字符串数据的数据库级别或列级别排序规则。 UTF-8 受 char  和 varchar  数据类型支持，并在创建对象的排序规则或将其更改为带有 `UTF8` 后缀的排序规则时启用。 例如：将 `LATIN1_GENERAL_100_CI_AS_SC` 更改为 `LATIN1_GENERAL_100_CI_AS_SC_UTF8`。 

UTF-8 仅适用于支持增补字符的 Windows 排序规则，如 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中所引入的。 nchar  和 nvarchar  仅支持 UCS-2 或 UTF-16 编码，并保持不变。

### <a name="storage_differences"></a> UTF-8 与 UTF-16 的存储差异
Unicode 联盟为每个字符都分配一个唯一码位（介于 000000 和 10FFFF 之间的值）。 使用 [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] 时，UTF-8 和 UTF-16 编码都可用来表示整个范围：    
-  如果使用 UTF-8 编码，ASCII 范围（000000 和 00007F）内的字符需要 1 个字节，介于 000080 和 0007FF、000800 和 00FFFF 以及 0010000 和 0010FFFF 之间的码位分别需要 2、3 和 4 个字节。 
-  如果使用 UTF-16 编码，介于 000000 和 00FFFF 以及 0010000 和 0010FFFF 之间的码位分别需要 2 和 4 个字节。 

下表概述了各个字符范围和编码类型的编码存储字节：

|代码范围（十六进制）|代码范围（十进制）|使用 UTF-8 时的存储字节<sup>1</sup>|使用 UTF-16 时的存储字节<sup>1</sup>|    
|---------------------------------|---------------------------------|--------------------------|-----------------------------|   
|000000 - 00007F|0 - 127|1|2|
|000080 - 00009F<br />0000A0 - 0003FF<br />000400 - 0007FF|128 - 159<br />160 - 1,023<br />1,024 - 2,047|2|2|
|000800 - 003FFF<br />004000 - 00FFFF|2,048 - 16,383<br />16,384 - 65,535|3|2|
|010000 - 03FFFF<sup>2</sup><br /><br />040000 - 10FFFF<sup>2</sup>|65,536 - 262,143<sup>2</sup><br /><br />262,144 - 1,114,111<sup>2</sup>|4|4|

<sup>1</sup>存储字节是指编码字节长度，而不是数据类型各自在磁盘上的存储大小。 若要详细了解磁盘上的存储大小，请参阅 [nchar 和 nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)，以及 [char 和 varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)。

<sup>2</sup>[附属字符](#Supplementary_Characters)的码位范围。

如上所示，选择适当的 Unicode 编码和数据类型可以节省大量存储，具体视使用的字符集而定。 例如，如果使用已启用 UTF-8 的排序规则将包含 ASCII 字符的现有列数据类型从 `NCHAR(10)` 更改为 `CHAR(10)`，意味着减少 50% 的存储需求。 之所以会有这种减少是因为，`NCHAR(10)` 需要 20 个字节进行存储，而 `CHAR(10)` 则需要 10 个字节用于相同的 Unicode 字符串表示形式。

选择是要将 UTF-8 编码还是 UTF-16 编码用于数据库或列前，请先考虑要存储的字符串数据的分布情况：
-  如果它主要在 ASCII 范围内（如英语），使用 UTF-8 和 UTF-16 时每个字符分别需要 1 和 2 个字节。 UTF-8 具有存储优势。 
-  如果超出 ASCII 范围（几乎所有拉丁字母语言以及希腊语、西里尔文、科普特语、亚美尼亚语、希伯来语、阿拉伯语、叙利亚语、它拿语和西非书面文），使用 UTF-8 和 UTF-16 时每个字符都需要 2 个字节。 在这种情况下，可比较的数据类型（例如，char  与 nchar  之间）没有显著的存储差异。
-  如果它主要是东亚语言（如韩语、中文和日语），使用 UTF-8 和 UTF-16 时每个字符分别需要 3 和 2 个字节。 UTF-16 具有存储优势。 
-  使用 UTF-8 和 UTF-16 时，介于 010000 和 10FFFF 范围内的字符都需要 4 个字节。 在这种情况下，可比较的数据类型（例如，char  与 nchar  之间）没有存储差异。

有关其他注意事项，请参阅[编写国际化 Transact-SQL 语句](../../relational-databases/collations/write-international-transact-sql-statements.md)。

##  <a name="Related_Tasks"></a> 相关任务    
    
|任务|主题|    
|----------|-----------|    
|介绍如何设置或更改 SQL Server 实例的排序规则。|[设置或更改服务器排序规则](../../relational-databases/collations/set-or-change-the-server-collation.md)|    
|介绍如何设置或更改用户数据库的排序规则。|[设置或更改数据库排序规则](../../relational-databases/collations/set-or-change-the-database-collation.md)|    
|介绍如何设置或更改数据库中的列的排序规则。|[设置或更改列排序规则](../../relational-databases/collations/set-or-change-the-column-collation.md)|    
|介绍如何返回服务器级、数据库级或列级的排序规则信息。|[查看排序规则信息](../../relational-databases/collations/view-collation-information.md)|    
|介绍如何编写更易于在不同语言之间移植或更轻松地支持多种语言的 Transact-SQL 语句。|[编写国际化 Transact-SQL 语句](../../relational-databases/collations/write-international-transact-sql-statements.md)|    
|介绍如何更改有关日期、时间和货币数据的使用和显示方式的错误消息和首选项的语言。|[设置会话语言](../../relational-databases/collations/set-a-session-language.md)|    
    
##  <a name="Related_Content"></a> 相关内容    
[SQL Server 最佳实践 - 排序规则更改](https://go.microsoft.com/fwlink/?LinkId=113891)    
[使用 Unicode 字符格式导入或导出数据 &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)        
[编写国际化 Transact-SQL 语句](../../relational-databases/collations/write-international-transact-sql-statements.md)     
[“SQL Server 最佳实践 - 迁移到 Unicode”](https://go.microsoft.com/fwlink/?LinkId=113890)- 不再保留   
[Unicode 联盟网站](https://go.microsoft.com/fwlink/?LinkId=48619)   
[Unicode 标准](http://www.unicode.org/standard/standard.html)      
    
## <a name="see-also"></a>另请参阅    
[包含数据库的排序规则](../../relational-databases/databases/contained-database-collations.md)     
[创建全文索引时选择语言](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)     
[sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)    
    
 
