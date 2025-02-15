---
title: T-SQL 语言元素-并行数据仓库 |Microsoft Docs
description: T-SQL 语言元素的分析平台系统 (APS) SQL Server 并行数据仓库 (PDW)。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.openlocfilehash: 0f019599e5dde2759da9f1e8f94e71b2fe2b9c9a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67959944"
---
# <a name="t-sql-language-elements-for-parallel-data-warehouse"></a>并行数据仓库 T-SQL 语言元素
T-SQL 语言元素的分析平台系统 (APS) SQL Server 并行数据仓库 (PDW)。

## <a name="core-elements"></a>核心元素
* [语法约定](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
* [对象命名规则](../relational-databases/databases/database-identifiers.md)
* [保留的关键字](../t-sql/language-elements/reserved-keywords-transact-sql.md)
* [排序规则](https://msdn.microsoft.com/library/ff848763.aspx)
* [注释](../t-sql/language-elements/comment-transact-sql.md)
* [常量](../t-sql/data-types/constants-transact-sql.md)
* [数据类型](../t-sql/data-types/data-types-transact-sql.md)
* [EXECUTE](../t-sql/language-elements/execute-transact-sql.md)
* [expressions](../t-sql/language-elements/expressions-transact-sql.md)
* [KILL](../t-sql/language-elements/kill-transact-sql.md)
* [IDENTITY 属性解决方法](../t-sql/statements/create-table-transact-sql-identity-property.md)
* [PRINT](../t-sql/language-elements/print-transact-sql.md)
* [USE](../t-sql/language-elements/use-transact-sql.md)

## <a name="batches-control-of-flow-and-variables"></a>批、 流控制和变量
* [BEGIN...END](../t-sql/language-elements/begin-end-transact-sql.md)
* [BREAK](../t-sql/language-elements/break-transact-sql.md)
* [DECLARE @local_variable](../t-sql/language-elements/declare-local-variable-transact-sql.md)
* [IF...ELSE](../t-sql/language-elements/if-else-transact-sql.md)
* [RAISERROR](../t-sql/language-elements/raiserror-transact-sql.md)
* [SET@local_variable](../t-sql/language-elements/set-local-variable-transact-sql.md)
* [THROW](../t-sql/language-elements/throw-transact-sql.md)
* [TRY...CATCH](../t-sql/language-elements/try-catch-transact-sql.md)
* [WHILE](../t-sql/language-elements/while-transact-sql.md)

## <a name="operators"></a>运算符
* [+（加）](../t-sql/language-elements/add-transact-sql.md)
* [+（字符串串联）](../t-sql/language-elements/string-concatenation-transact-sql.md)
* [-（负）](../t-sql/language-elements/unary-operators-negative.md)
* [-（减）](../t-sql/language-elements/subtract-transact-sql.md)
* [*（乘）](../t-sql/language-elements/multiply-transact-sql.md)
* [/ (Divide)](../t-sql/language-elements/divide-transact-sql.md)
* [取模](../t-sql/language-elements/modulo-transact-sql.md)

## <a name="wildcard-characters-to-match"></a>若要匹配的通配符
* [=（等于）](../t-sql/language-elements/equals-transact-sql.md)
* [> （大于）](../t-sql/language-elements/greater-than-transact-sql.md)
* [< （小于）](../t-sql/language-elements/less-than-transact-sql.md)
* [> = （大于或等于）](../t-sql/language-elements/greater-than-or-equal-to-transact-sql.md)
* [< = （小于或等于）](../t-sql/language-elements/less-than-or-equal-to-transact-sql.md)
* [<> （不等于）](../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)
* [\!= （不等于）](../t-sql/language-elements/not-equal-to-transact-sql-exclamation.md)
* [AND](../t-sql/language-elements/and-transact-sql.md)
* [BETWEEN](../t-sql/language-elements/between-transact-sql.md)
* [EXISTS](../t-sql/language-elements/exists-transact-sql.md)
* [IN](../t-sql/language-elements/in-transact-sql.md)
* [不是](../t-sql/queries/is-null-transact-sql.md)
* [LIKE](../t-sql/language-elements/like-transact-sql.md)
* [NOT](../t-sql/language-elements/not-transact-sql.md)
* [OR](../t-sql/language-elements/or-transact-sql.md)

### <a name="bitwise-operators"></a>位运算符
* [&（位与）](../t-sql/language-elements/bitwise-and-transact-sql.md)
* [|（位或）](../t-sql/language-elements/bitwise-or-transact-sql.md)
* [^ (位异或)](../t-sql/language-elements/bitwise-exclusive-or-transact-sql.md)
* [~（位非）](../t-sql/language-elements/bitwise-not-transact-sql.md)
* [^=（位异或等于）](../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)
* [|=（位或等于）](../t-sql/language-elements/bitwise-or-equals-transact-sql.md)
* [&=（位与等于）](../t-sql/language-elements/bitwise-and-equals-transact-sql.md)

## <a name="functions"></a>函数
* [@@DATEFIRST](../t-sql/functions/datefirst-transact-sql.md)
* [@@ERROR](../t-sql/functions/error-transact-sql.md)
* [@@LANGUAGE](../t-sql/functions/language-transact-sql.md)
* [@@SPID](../t-sql/functions/spid-transact-sql.md)
* [@@TRANCOUNT](../t-sql/functions/trancount-transact-sql.md)
* [@@VERSION](../t-sql/functions/version-transact-sql-configuration-functions.md)
* [ABS](../t-sql/functions/abs-transact-sql.md)
* [ACOS](../t-sql/functions/acos-transact-sql.md)
* [ASCII](../t-sql/functions/ascii-transact-sql.md)
* [ASIN](../t-sql/functions/asin-transact-sql.md)
* [ATAN](../t-sql/functions/atan-transact-sql.md)
* [ATN2](../t-sql/functions/atn2-transact-sql.md)
* [BINARY_CHECKSUM](../t-sql/functions/binary-checksum-transact-sql.md)
* [CASE](../t-sql/language-elements/case-transact-sql.md)
* [CAST 和 CONVERT](../t-sql/functions/cast-and-convert-transact-sql.md)
* [CEILING](../t-sql/functions/ceiling-transact-sql.md)
* [CHAR](../t-sql/functions/char-transact-sql.md)
* [CHARINDEX](../t-sql/functions/charindex-transact-sql.md)
* [CHECKSUM](../t-sql/functions/checksum-transact-sql.md)
* [COALESCE](../t-sql/language-elements/coalesce-transact-sql.md)
* [COL_NAME](../t-sql/functions/col-name-transact-sql.md)
* [COLLATIONPROPERTY](../t-sql/functions/collation-functions-collationproperty-transact-sql.md)
* [CONCAT](../t-sql/functions/concat-transact-sql.md)
* [COS](../t-sql/functions/cos-transact-sql.md)
* [COT](../t-sql/functions/cot-transact-sql.md)
* [COUNT](../t-sql/functions/count-transact-sql.md)
* [COUNT_BIG](../t-sql/functions/count-big-transact-sql.md)
* [CUME_DIST](../t-sql/functions/cume-dist-transact-sql.md)
* [CURRENT_TIMESTAMP](../t-sql/functions/current-timestamp-transact-sql.md)
* [CURRENT_USER](../t-sql/functions/current-user-transact-sql.md)
* [DATABASEPROPERTYEX](../t-sql/functions/databasepropertyex-transact-sql.md)
* [DATALENGTH](../t-sql/functions/datalength-transact-sql.md)
* [DATEADD](../t-sql/functions/dateadd-transact-sql.md)
* [DATEDIFF](../t-sql/functions/datediff-transact-sql.md)
* [DATEFROMPARTS](../t-sql/functions/datefromparts-transact-sql.md)
* [DATENAME](../t-sql/functions/datename-transact-sql.md)
* [DATEPART](../t-sql/functions/datepart-transact-sql.md)
* [DATETIME2FROMPARTS](../t-sql/functions/datetime2fromparts-transact-sql.md)
* [DATETIMEFROMPARTS](../t-sql/functions/datetimefromparts-transact-sql.md)
* [DATETIMEOFFSETFROMPARTS](../t-sql/functions/datetimeoffsetfromparts-transact-sql.md)
* [DAY](../t-sql/functions/day-transact-sql.md)
* [DB_ID](../t-sql/functions/db-id-transact-sql.md)
* [DB_NAME](../t-sql/functions/db-name-transact-sql.md)
* [DEGREES](../t-sql/functions/degrees-transact-sql.md)
* [DENSE_RANK](../t-sql/functions/dense-rank-transact-sql.md)
* [DIFFERENCE](../t-sql/functions/difference-transact-sql.md)
* [EOMONTH](../t-sql/functions/eomonth-transact-sql.md)
* [ERROR_MESSAGE](../t-sql/functions/error-message-transact-sql.md)
* [ERROR_NUMBER](../t-sql/functions/error-number-transact-sql.md)
* [ERROR_PROCEDURE](../t-sql/functions/error-procedure-transact-sql.md)
* [ERROR_SEVERITY](../t-sql/functions/error-severity-transact-sql.md)
* [ERROR_STATE](../t-sql/functions/error-state-transact-sql.md)
* [EXP](../t-sql/functions/exp-transact-sql.md)
* [FIRST_VALUE](../t-sql/functions/first-value-transact-sql.md)
* [FLOOR](../t-sql/functions/floor-transact-sql.md)
* [GETDATE](../t-sql/functions/getdate-transact-sql.md)
* [GETUTCDATE](../t-sql/functions/getutcdate-transact-sql.md)
* [HAS_DBACCESS](../t-sql/functions/has-dbaccess-transact-sql.md)
* [HASHBYTES](../t-sql/functions/hashbytes-transact-sql.md)
* [INDEXPROPERTY](../t-sql/functions/indexproperty-transact-sql.md)
* [ISDATE](../t-sql/functions/isdate-transact-sql.md)
* [ISNULL](../t-sql/functions/isnull-transact-sql.md)
* [ISNUMERIC](../t-sql/functions/isnumeric-transact-sql.md)
* [LAG](../t-sql/functions/lag-transact-sql.md)
* [LAST_VALUE](../t-sql/functions/last-value-transact-sql.md)
* [LEAD](../t-sql/functions/lead-transact-sql.md)
* [LEFT](../t-sql/functions/left-transact-sql.md)
* [LEN](../t-sql/functions/len-transact-sql.md)
* [LOG](../t-sql/functions/log-transact-sql.md)
* [LOG10](../t-sql/functions/log10-transact-sql.md)
* [LOWER](../t-sql/functions/lower-transact-sql.md)
* [LTRIM](../t-sql/functions/ltrim-transact-sql.md)
* [MAX](../t-sql/functions/max-transact-sql.md)
* [MIN](../t-sql/functions/min-transact-sql.md)
* [MONTH](../t-sql/functions/month-transact-sql.md)
* [NCHAR](../t-sql/functions/nchar-transact-sql.md)
* [NTILE](../t-sql/functions/ntile-transact-sql.md)
* [NULLIF](../t-sql/language-elements/nullif-transact-sql.md)
* [OBJECT_ID](../t-sql/functions/object-id-transact-sql.md)
* [OBJECT_NAME](../t-sql/functions/object-name-transact-sql.md)
* [OBJECTPROPERTY](../t-sql/functions/objectproperty-transact-sql.md)
* [OIBJECTPROPERTYEX](../t-sql/functions/objectpropertyex-transact-sql.md)
* [ODBCS 标量函数](../t-sql/functions/odbc-scalar-functions-transact-sql.md)
* [OVER 子句](../t-sql/queries/select-over-clause-transact-sql.md)
* [PARSENAME](../t-sql/functions/parsename-transact-sql.md)
* [PATINDEX](../t-sql/functions/patindex-transact-sql.md)
* [PERCENTILE_CONT](../t-sql/functions/percentile-cont-transact-sql.md)
* [PERCENTILE_DISC](../t-sql/functions/percentile-disc-transact-sql.md)
* [PERCENT_RANK](../t-sql/functions/percent-rank-transact-sql.md)
* [PI](../t-sql/functions/pi-transact-sql.md)
* [POWER](../t-sql/functions/power-transact-sql.md)
* [QUOTENAME](../t-sql/functions/quotename-transact-sql.md)
* [RADIANS](../t-sql/functions/radians-transact-sql.md)
* [RAND](../t-sql/functions/rand-transact-sql.md)
* [RANK](../t-sql/functions/rank-transact-sql.md)
* [REPLACE](../t-sql/functions/replace-transact-sql.md)
* [REPLICATE](../t-sql/functions/replicate-transact-sql.md)
* [REVERSE](../t-sql/functions/reverse-transact-sql.md)
* [RIGHT](../t-sql/functions/right-transact-sql.md)
* [ROUND](../t-sql/functions/round-transact-sql.md)
* [ROW_NUMBER](../t-sql/functions/row-number-transact-sql.md)
* [RTRIM](../t-sql/functions/rtrim-transact-sql.md)
* [SCHEMA_ID](../t-sql/functions/schema-id-transact-sql.md)
* [SCHEMA_NAME](../t-sql/functions/schema-name-transact-sql.md)
* [SERVERPROPERTY](../t-sql/functions/serverproperty-transact-sql.md)
* [SESSION_USER](../t-sql/functions/session-user-transact-sql.md)
* [SIGN](../t-sql/functions/sign-transact-sql.md)
* [SIN](../t-sql/functions/sin-transact-sql.md)
* [SMALLDATETIMEFROMPARTS](../t-sql/functions/smalldatetimefromparts-transact-sql.md)
* [SOUNDEX](../t-sql/functions/soundex-transact-sql.md)
* [SPACE](../t-sql/functions/space-transact-sql.md)
* [SQL_VARIANT_PROPERTY](../t-sql/functions/sql-variant-property-transact-sql.md)
* [SQRT](../t-sql/functions/sqrt-transact-sql.md)
* [SQUARE](../t-sql/functions/square-transact-sql.md)
* [STATS_DATE](../t-sql/functions/stats-date-transact-sql.md)
* [STDEV](../t-sql/functions/stdev-transact-sql.md)
* [STDEVP](../t-sql/functions/stdevp-transact-sql.md)
* [STR](../t-sql/functions/str-transact-sql.md)
* [STUFF](../t-sql/functions/stuff-transact-sql.md)
* [SUBSTRING](../t-sql/functions/substring-transact-sql.md)
* [SUM](../t-sql/functions/sum-transact-sql.md)
* [SUSER_SNAME](../t-sql/functions/suser-sname-transact-sql.md)
* [SWITCHOFFSET](../t-sql/functions/switchoffset-transact-sql.md)
* [SYSDATETIME](../t-sql/functions/sysdatetime-transact-sql.md)
* [SYSDATETIMEOFFSET](../t-sql/functions/sysdatetimeoffset-transact-sql.md)
* [SYSTEM_USER](../t-sql/functions/system-user-transact-sql.md)
* [SYSUTCDATETIME](../t-sql/functions/sysutcdatetime-transact-sql.md)
* [TAN](../t-sql/functions/tan-transact-sql.md)
* [TERTIARY_WEIGHTS](../t-sql/functions/collation-functions-tertiary-weights-transact-sql.md)
* [TIMEFROMPARTS](../t-sql/functions/timefromparts-transact-sql.md)
* [TODATETIMEOFFSET](../t-sql/functions/todatetimeoffset-transact-sql.md)
* [TYPE_ID](../t-sql/functions/type-id-transact-sql.md)
* [TYPE_NAME](../t-sql/functions/type-name-transact-sql.md)
* [TYPEPROPERTY](../t-sql/functions/typeproperty-transact-sql.md)
* [UNICODE](../t-sql/functions/unicode-transact-sql.md)
* [UPPER](../t-sql/functions/upper-transact-sql.md)
* [USER](../t-sql/functions/user-transact-sql.md)
* [USER_NAME](../t-sql/functions/user-name-transact-sql.md)
* [VAR](../t-sql/functions/var-transact-sql.md)
* [VARP](../t-sql/functions/varp-transact-sql.md)
* [YEAR](../t-sql/functions/year-transact-sql.md)
* [XACT_STATE](../t-sql/functions/xact-state-transact-sql.md)

## <a name="transactions"></a>事务
* [事务](../t-sql/language-elements/transactions-sql-data-warehouse.md)

## <a name="diagnostic-sessions"></a>诊断会话
* [CREATE DIAGNOSTICS SESSION](../t-sql/language-elements/create-diagnostics-session-transact-sql.md)

## <a name="procedures"></a>过程
* [sp_addrolemember](../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)
* [sp_columns](../relational-databases/system-stored-procedures/sp-columns-transact-sql.md)
* [sp_configure](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
* [sp_datatype_info_90](../relational-databases/system-stored-procedures/sp-datatype-info-90-sql-data-warehouse.md)
* [sp_droprolemember](../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)
* [sp_execute](../relational-databases/system-stored-procedures/sp-execute-transact-sql.md)
* [sp_executesql](../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)
* [sp_fkeys](../relational-databases/system-stored-procedures/sp-fkeys-transact-sql.md)
* [sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md)
* [sp_pdw_database_encryption](../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)
* [sp_pdw_database_encryption_regenerate_system_keys](../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)
* [sp_pdw_log_user_data_masking](../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)
* [sp_pdw_remove_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)
* [sp_pkeys](../relational-databases/system-stored-procedures/sp-pkeys-transact-sql.md)
* [sp_prepare](../relational-databases/system-stored-procedures/sp-prepare-transact-sql.md)
* [sp_spaceused](../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)
* [sp_special_columns_100](../relational-databases/system-stored-procedures/sp-special-columns-100-sql-data-warehouse.md)
* [sp_sproc_columns](../relational-databases/system-stored-procedures/sp-sproc-columns-transact-sql.md)
* [sp_statistics](../relational-databases/system-stored-procedures/sp-statistics-transact-sql.md)
* [sp_tables](../relational-databases/system-stored-procedures/sp-tables-transact-sql.md)
* [sp_unprepare](../relational-databases/system-stored-procedures/sp-unprepare-transact-sql.md)

## <a name="set-statements"></a>SET 语句
* [SET ANSI_DEFAULTS](../t-sql/statements/set-ansi-defaults-transact-sql.md)
* [SET ANSI_NULL_DFLT_OFF](../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)
* [SET ANSI_NULL_DFLT_ON](../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)
* [SET ANSI_NULLS](../t-sql/statements/set-ansi-nulls-transact-sql.md)
* [SET ANSI_PADDING](../t-sql/statements/set-ansi-padding-transact-sql.md)
* [SET ANSI_WARNINGS](../t-sql/statements/set-ansi-warnings-transact-sql.md)
* [SET ARITHABORT](../t-sql/statements/set-arithabort-transact-sql.md)
* [SET ARITHIGNORE](../t-sql/statements/set-arithignore-transact-sql.md)
* [SET CONCAT_NULL_YIELDS_NULL](../t-sql/statements/set-concat-null-yields-null-transact-sql.md)
* [SET DATEFIRST](../t-sql/statements/set-datefirst-transact-sql.md)
* [SET DATEFORMAT](../t-sql/statements/set-dateformat-transact-sql.md)
* [SET FMTONLY](../t-sql/statements/set-fmtonly-transact-sql.md)
* [集 IMPLICIT_TRANSACITONS](../t-sql/statements/set-implicit-transactions-transact-sql.md)
* [SET LOCK_TIMEOUT](../t-sql/statements/set-lock-timeout-transact-sql.md)
* [集 NUMBERIC_ROUNDABORT](../t-sql/statements/set-numeric-roundabort-transact-sql.md)
* [SET QUOTED_IDENTIFIER](../t-sql/statements/set-quoted-identifier-transact-sql.md)
* [SET ROWCOUNT](../t-sql/statements/set-rowcount-transact-sql.md)
* [SET TEXTSIZE](../t-sql/statements/set-textsize-transact-sql.md)
* [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
* [SET XACT_ABORT](../t-sql/statements/set-xact-abort-transact-sql.md)

## <a name="next-steps"></a>后续步骤
有关更多参考信息，请参阅[T-SQL 语句](tsql-statements.md)并[T-SQL 系统视图](tsql-system-views.md)。

<!--Image references-->

<!--Article references-->

<!--MSDN references-->
