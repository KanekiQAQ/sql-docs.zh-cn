---
title: CREATE EXTERNAL LANGUAGE (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: dphansen
ms.technology: t-sql
ms.topic: language-reference
author: nelgson
ms.author: negust
manager: cgronlun
monikerRange: '>=sql-server-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 5a3d66d95907c8ddbc4efd33fe58ee4ddbbb9423
ms.sourcegitcommit: db9bed6214f9dca82dccb4ccd4a2417c62e4f1bd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476004"
---
# <a name="create-external-language-transact-sql"></a>CREATE EXTERNAL LANGUAGE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

从指定的文件路径或字节流中注册数据库中的外部语言扩展。 此语句充当一种通用机制，数据库管理员使用它可在 SQL Server 支持的任何 OS 平台上注册新外部语言扩展。 有关详细信息，请参阅[语言扩展](https://docs.microsoft.com/sql/language-extensions/language-extensions-overview)。

> [!NOTE]
> 目前仅支持将 Java  用作外部语言。 “R”和“Python”是保留名称，不能使用这些特定名称创建外部语言   。 若要详细了解如何使用 R  和 Python  ，请参阅 [SQL Server 机器学习服务](https://docs.microsoft.com/sql/advanced-analytics/)。

## <a name="syntax"></a>语法

```text
CREATE EXTERNAL LANGUAGE language_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH (<option_spec>)
[ ; ]  

<file_spec> ::=  
{
    ( CONTENT = { <external_lang_specifier> | <content_bits>,
    FILE_NAME = <external_lang_file_name>
    [ , PLATFORM = <platform> ]
    [ , PARAMETERS = <external_lang_parameters> ]
    [ , ENVIRONMENT_VARIABLES = <external_lang_env_variables> )
}

<external_lang_specifier> :: =  
{
    '[file_path\]os_file_name'  
}

<content_bits> :: =  
{
    varbinary_literal
    | varbinary_expression
}

<external_lang_file_name> :: =  
'extension_file_name'


<platform> :: =
{
    WINDOWS
  | LINUX
}

<external_lang_parameters> :: =  
'extension_specific_parameters'
```

### <a name="arguments"></a>参数

**language_name**

语言是数据库范围的对象。 语言名称在数据库中必须唯一。

**owner_name**

指定拥有外部语言的用户或角色的名称。 如果未指定，则所有权授予当前用户。 可能需要为其他用户授予使用特定语言执行脚本的显式权限，具体取决于用户权限。

**file_spec**

指定语言扩展的内容。 每个平台每种特定语言只允许使用一个 filespec。

**external_lang_specifier**

包含扩展代码的 .zip 或 tar.gz 文件的完整文件路径。 此内容可以是 .zip 文件（在 Windows 上）或 tar.gz（在 Linux 上）的路径。

**content_bits**

将语言的内容指定为十六进制文字，类似于程序集。

如果需要创建语言或更改现有语言（并具有执行此操作的所需权限），但服务器上的文件系统受限，无法将库文件复制到服务器可以访问的位置，此选项会非常有用。

**external_lang_file_name**

扩展 .dll 或 .so 文件的名称。 如果 <external_lang_specifier> .zip 或 tar.gz 中有多个 .dll 或 .so 文件，需要使用名称来识别正确的文件。

**external_lang_parameters**

这提供了向外部语言运行时提供一组参数的可能性。 外部进程启动后，会将参数值提供给外部运行时。 但是，在外部进程启动之前，语言扩展可以访问环境变量。

**external_lang_env_variables**

这提供了在外部进程启动之前向外部语言运行时提供一组环境变量的可能性。 环境变量的一个例子是运行时本身的主目录。 例如：JRE_HOME。

**平台**

混合 OS 方案需要此参数。 在混合体系结构中，语言需要按平台进行一次注册。 平台和语言名称对每种外部语言是唯一键。 如果未指定平台，则假定为当前 OS。

## <a name="remarks"></a>Remarks

在 CTP 3.0 中，不支持“PARAMETERS”和“ENVIRONMENT_VARIABLES”   。

## <a name="permissions"></a>权限

需要 `CREATE EXTERNAL LANGUAGE` 权限。 默认情况下，dbo 用户或担任 db_owner 角色的任何成员都有权创建外部语言   。 对于其他所有用户，必须使用 [GRANT](https://docs.microsoft.com/sql/t-sql/statements/grant-database-permissions-transact-sql) 语句显式授予他们权限，同时将 CREATE EXTERNAL LANGUAGE 指定为特权。

更改库需要单独的权限，`ALTER ANY EXTERNAL LANGUAGE`。

### <a name="execute-external-script-permission"></a>EXECUTE EXTERNAL SCRIPT 权限

SQL Server 2019 中引入了 EXECUTE EXTERNAL SCRIPT 权限，以便可以授予对特定语言的外部脚本执行权限。 以前只有 EXECUTE ANY EXTERNAL SCRIPT 数据库权限，无法授予对特定语言的执行权限。

这意味着需要向非 dbo 用户授予执行特定语言的权限  ：

```sql
GRANT EXECUTE EXTERNAL SCRIPT ON EXTERNAL LANGUAGE ::language_name 
TO database_principal_name;
```

### <a name="reference-permissions-to-external-libraries"></a>外部库的引用权限

与程序集类似，外部语言需要引用权限，以便外部库和外部语言之间存在关联。 例如，如果要删除外部语言，用户首先需要确保删除引用该语言的所有外部库。 可在层次结构中将外部语言视为比外部库更高级别的对象。

## <a name="examples"></a>示例

### <a name="a-create-an-external-language-in-a-database"></a>A. 在数据库中创建外部语言  

以下示例将一种名为 Java 的外部语言添加到 Windows 上的 SQL Server 的数据库中。

```sql
CREATE EXTERNAL LANGUAGE Java 
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

### <a name="b-create-an-external-language-for-both-windows-and-linux"></a>B. 同时为 Windows 和 Linux 创建外部语言

最多可以指定两个 `<file_spec>`，一个用于 Windows，另一个用于 Linux。

```sql
CREATE EXTERNAL LANGUAGE Java
FROM
(CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll', PLATFORM = WINDOWS),
(CONTENT = N'<path-to-tar.gz>', FILE_NAME = 'javaextension.so', PLATFORM = LINUX);
GO
```

## <a name="see-also"></a>另请参阅

[ALTER EXTERNAL LANGUAGE (Transact-SQL)](alter-external-language-transact-sql.md)  
[DROP EXTERNAL LANGUAGE (Transact-SQL)](drop-external-language-transact-sql.md)  
[sys.external_languages](../../relational-databases/system-catalog-views/sys-external-languages-transact-sql.md)  
[sys.external_language_files](../../relational-databases/system-catalog-views/sys-external-language-files-transact-sql.md)  
