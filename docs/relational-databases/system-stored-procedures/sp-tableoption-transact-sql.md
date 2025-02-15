---
title: sp_tableoption (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_tableoption_TSQL
- sp_tableoption
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tableoption
ms.assetid: 0a57462c-1057-4c7d-bce3-852cc898341d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 15c3c9716adefbb95d24c9528dce8607678998c8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68096062"
---
# <a name="sptableoption-transact-sql"></a>sp_tableoption (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  设置用户定义表的选项值。 sp_tableoption 可用于控制包含的表的行内行为**varchar （max)** ， **nvarchar （max)** ， **varbinary （max)** ， **xml**，**文本**， **ntext**，**映像**，或大型用户定义类型列。  
  
> [!IMPORTANT]  
>  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，将删除 text in row 功能。 若要存储大值数据，我们建议你使用的**varchar （max)** ， **nvarchar （max)** 并**varbinary （max)** 数据类型。  
  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_tableoption [ @TableNamePattern = ] 'table'   
     , [ @OptionName = ] 'option_name'   
     ,[ @OptionValue =] 'value'  
```  
  
## <a name="arguments"></a>参数  
 [ @TableNamePattern =] '*表*  
 用户定义数据库表的限定名称或非限定名称。 如果提供了包含数据库名称的完全限定表名，则数据库名称必须为当前数据库的名称。 不能同时设置多个表的表选项。 *表*是**nvarchar(776)** ，无默认值。  
  
 [ @OptionName =] '*option_name*  
 表选项名称。 *option_name*是**varchar(35)** ，无默认值为 NULL。 *option_name*可以是下列值之一。  
  
|ReplTest1|描述|  
|-----------|-----------------|  
|table lock on bulk load|禁用时（默认值），使用户定义表的大容量处理获得行锁。 启用时，使用户定义表的大容量处理获得大容量更新锁。|  
|insert row lock|不再支持。<br /><br /> 此选项对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的锁定行为没有影响，保留此选项只是为了与现有脚本和过程兼容。|  
|text in row|如果为 OFF 或 0（禁用，默认值），则不更改当前行为，且在行中不存在 BLOB。<br /><br /> 当指定为和@OptionValue为 ON （启用） 或介于 24 到 7000，新的整数值**文本**， **ntext**，或**映像**字符串直接存储在数据行。 现有的所有 BLOB (二进制大型对象：**文本**， **ntext**，或**图像**数据) 更新了 BLOB 值将更改为 text in row 格式。 有关详细信息，请参阅“备注”。|  
|large value types out of row|1 = **varchar （max)** ， **nvarchar （max)** ， **varbinary （max)** ， **xml**和表中的大型用户定义类型 (UDT) 列存储在行外，用一个 16 字节指针指向根目录。<br /><br /> 0 = **varchar （max)** ， **nvarchar （max)** ， **varbinary （max)** ， **xml**和大型 UDT 值直接存储在数据行，最大限制8000 个字节，只要记录中可以容纳该值。 如果记录中容纳不下该值，则指针存储在行内，其余内容存储在 LOB 存储空间内的行外。 默认值为 0。<br /><br /> 大型用户定义类型 (UDT) 适用范围：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 至 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。 <br /><br /> 使用的 TEXTIMAGE_ON 选项[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)指定较大的数据类型存储的位置。 |  
|Vardecimal 存储格式|**适用范围**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。<br /><br /> 为 TRUE、ON 或 1 时，将为 vardecimal 存储格式启用指定的表。 为 FALSE、OFF 或 0 时，将不为 vardecimal 存储格式启用此表。 仅当该数据库已通过使用启用了 vardecimal 存储格式时，可以启用 Vardecimal 存储格式[sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md)。 在中[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本， **vardecimal**存储格式已弃用。 请改用 ROW 压缩。 有关详细信息，请参阅 [Data Compression](../../relational-databases/data-compression/data-compression.md)。 默认值为 0。|  
  
 [ @OptionValue =] '*值*  
 是是否*option_name*启用 （TRUE、 ON 或 1） 还是禁用 (FALSE、 OFF 或 0)。 *值*是**varchar(12)** ，无默认值。 *值*是不区分大小写。  
  
 对于 text in row 选项，有效选项值是 0、ON、OFF，或从 24 到 7000 的整数。 当*值*为 ON 时，默认为 256 个字节的限制值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或错误号（失败）  
  
## <a name="remarks"></a>备注  
 sp_tableoption 仅可用于设置用户定义表的选项值。 若要显示表属性，请使用 OBJECTPROPERTY。  
  
 sp_tableoption 中的 text in row 选项只能对包含文本列的表启用或禁用。 如果表不含文本列，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将引发错误。  
  
 当启用了 text in row 选项时，@OptionValue参数允许用户指定要为 BLOB 存储在行中的最大大小。 默认值为 256 字节，但是值可以介于 24 到 7000 个字节之间。  
  
 **文本**， **ntext**，或**图像**字符串存储在数据行中，如果以下条件适用：  
  
-   text in row 已启用。  
  
-   字符串的长度小于中指定的限制 @OptionValue  
  
-   数据行中有足够的可用空间。  
  
 当在数据行中存储 BLOB 字符串时，读取和写入**文本**， **ntext**，或**图像**字符串可以与读取或写入字符和二进制字符串一样快。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不必访问不同的页就可读取或写入 BLOB 字符串。  
  
 如果**文本**， **ntext**，或**图像**字符串大于指定的限制或行中的可用空间，而是指针存储在行。 在行中存储 BLOB 字符串的条件依然适用：在数据行来存放指针中必须有足够的空间。  
  
 存储在表行中的 BLOB 字符串和指针被视为类似于可变长度字符串。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 仅使用存储字符串或指针所需的字节数。  
  
 如果先启用了 text in row，则不会立即转换现有的 BLOB 字符串。 仅当字符串被更新时才将其转换。 同样，text in row 选项限制已增加，**文本**， **ntext**，或**图像**字符串数据行中已将不会转换以遵从新限制直到它们进行更新的时间。  
  
> [!NOTE]  
>  禁用 text in row 选项或减少该选项的限制值需要转换所有的 BLOB；因此，此过程可能需要较长的时间，具体时间则取决于必须转换的 BLOB 字符串数。 在转换过程中，表将被锁定。  
  
 表变量（包括返回表变量的函数）的 text in row 选项会自动启用，并将内联限制值默认为 256 个字节。 此选项不可更改。  
  
 Text in row 选项支持 TEXTPTR、 WRITETEXT、 UPDATETEXT 和 READTEXT 函数。 用户可以使用 SUBSTRING() 函数读取部分 BLOB，但是必须记住，各个行内文本指针之间具有不同的持续时间和数量限制。  
  
 若要将表从 vardecimal 存储格式改回为正常的十进制存储格式，数据库必须处于 SIMPLE 恢复模式。 更改恢复模式将断开用于备份目的的日志链，因此在从表中删除 vardecimal 存储格式后，应创建完整数据库备份。  
  
 如果要将现有 LOB 数据类型列 （text、 ntext 或 image） 转换为小型到中型大值类型 （varchar （max）、 nvarchar （max），或 varbinary(max)) 和大多数语句是否不引用你的环境中的大值类型列，请考虑更改**large_value_types_out_of_row**到**1**以获得最佳性能。 当**large_value_types_out_of_row**选项值已更改，现有 varchar （max）、 nvarchar （max）、 varbinary （max），并不会立即转换 xml 值。 字符串的存储在随后更新时更改。 插入表的新值根据生效的表选项存储。 对于直接结果，可以制作一份数据和更改后再重新填充表**large_value_types_out_of_row**设置或更新到其自身的每个小中大型值类型列，以便的存储字符串是有效更改的表选项。 考虑在更新或重新填充后重新生成表的索引，以压缩表格。 
    
  
## <a name="permissions"></a>权限  
 执行 sp_tableoption 要求对表拥有 ALTER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-storing-xml-data-out-of-the-row"></a>A. 将 xml 数据存储在行外  
 下面的示例指定**xml**中的数据`HumanResources.JobCandidate`表将存储在行外。  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'HumanResources.JobCandidate', 'large value types out of row', 1;  
```  
  
### <a name="b-enabling-vardecimal-storage-format-on-a-table"></a>B. 在表中启用 vardecimal 存储格式  
 下面的示例修改`Production.WorkOrderRouting`表以存储`decimal`中的数据类型`vardecimal`存储格式。  

```sql  
USE master;  
GO  
-- The database must be enabled for vardecimal storage format  
-- before a table can be enabled for vardecimal storage format  
EXEC sp_db_vardecimal_storage_format 'AdventureWorks2012', 'ON';  
GO  
USE AdventureWorks2012;  
GO  
EXEC sp_tableoption 'Production.WorkOrderRouting',   
   'vardecimal storage format', 'ON';  
```  
  
## <a name="see-also"></a>请参阅  
 [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据库引擎存储过程&#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  
