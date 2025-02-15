---
title: sp_server_info (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_server_info
- sp_server_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_server_info
ms.assetid: 2dc2c262-3cfa-4a84-8127-3632ba583543
author: stevestein
ms.author: sstein
ms.openlocfilehash: 6aae34fb03322a40f1b970df6271bb89d18b3293
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68104456"
---
# <a name="spserverinfo-transact-sql"></a>sp_server_info (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、数据库网关或基础数据源的属性名称和匹配值的列表。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_server_info [[@attribute_id = ] 'attribute_id']  
```  
  
## <a name="arguments"></a>参数  
`[ @attribute_id = ] 'attribute_id'` 是该属性的整数 ID。 *attribute_id*是**int**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 None  
  
## <a name="result-sets"></a>结果集  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**ATTRIBUTE_ID**|**int**|属性的 ID 号。|  
|**ATTRIBUTE_NAME**|**varchar(** 60 **)**|属性名称。|  
|**ATTRIBUTE_VALUE**|**varchar(** 255 **)**|属性的当前设置。|  
  
 下表列出了各个属性。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] ODBC 客户端库当前使用的属性**1**， **2**， **18**， **22**，并**500**连接时间。  
  
|ATTRIBUTE_ID|ATTRIBUTE_NAME 说明|ATTRIBUTE_VALUE|  
|-------------------|---------------------------------|----------------------|  
|**1**|DBMS_NAME|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**2**|DBMS_VER|[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] - *x.xx.xxxx*|  
|**10**|OWNER_TERM|owner|  
|**11**|TABLE_TERM|table|  
|**12**|MAX_OWNER_NAME_LENGTH|128|  
|**13**|TABLE_LENGTH<br /><br /> 指定表名的最大字符数。|128|  
|**14**|MAX_QUAL_LENGTH<br /><br /> 指定表限定符（由三部分组成的表名的第一部分）名称的最大长度。|128|  
|**15**|COLUMN_LENGTH<br /><br /> 指定列名的最大字符数。|128|  
|**16**|IDENTIFIER_CASE<br /><br /> 在数据库（系统目录中对象的事例）中指定用户定义的名称（表名、列名、存储过程名）。|SENSITIVE|  
|**17**|TX_ISOLATION<br /><br /> 指定服务器所采用的初始事务隔离级别，此级别与 SQL-92 中定义的隔离级别相对应。|2|  
|**18**|COLLATION_SEQ<br /><br /> 指定该服务器的字符集排序。|charset=iso_1 sort_order=dictionary_iso charset_num=1 sort_order_num=51|  
|**19**|SAVEPOINT_SUPPORT<br /><br /> 指定基础 DBMS 是否支持命名保存点。|Y|  
|**20**|MULTI_RESULT_SETS<br /><br /> 指定基础数据库或网关本身是否支持多个结果集（通过网关可以将多个语句与返回给客户端的多个结果集一起发送）。|Y|  
|**22**|ACCESSIBLE_TABLES<br /><br /> 指定是否在**sp_tables**，网关返回唯一表、 视图和等等，可由当前用户 （即，具有至少选择为表的权限的用户） 访问。|Y|  
|**100**|USERID_LENGTH<br /><br /> 指定用户名的最大字符数。|128|  
|**101**|QUALIFIER_TERM<br /><br /> 指定表限定符（由三部分组成的名称的第一部分）的 DBMS 供应商术语。|“数据库”|  
|**102**|NAMED_TRANSACTIONS<br /><br /> 指定基础 DBMS 是否支持命名事务。|Y|  
|**103**|SPROC_AS_LANGUAGE<br /><br /> 指定能否将存储过程作为语言事件执行。|Y|  
|**104**|ACCESSIBLE_SPROC<br /><br /> 指定是否在**sp_stored_procedures**，网关返回仅由当前用户可执行存储的过程。|Y|  
|**105**|MAX_INDEX_COLS<br /><br /> 指定 DBMS 索引中的最大列数。|16|  
|**106**|RENAME_TABLE<br /><br /> 指定是否可以重命名表。|Y|  
|**107**|RENAME_COLUMN<br /><br /> 指定是否可以重命名列。|Y|  
|**108**|DROP_COLUMN<br /><br /> 指定是否可以删除列。|Y|  
|**109**|INCREASE_COLUMN_LENGTH<br /><br /> 指定是否可以增大列的大小。|Y|  
|**110**|DDL_IN_TRANSACTION<br /><br /> 指定 DDL 语句是否可以出现在事务中。|Y|  
|**111**|DESCENDING_INDEXES<br /><br /> 指定是否支持降序索引。|Y|  
|**112**|SP_RENAME<br /><br /> 指定是否可以重命名存储过程。|Y|  
|**113**|REMOTE_SPROC<br /><br /> 指定能否通过 DB-Library 中的远程存储过程函数执行存储过程。|Y|  
|**500**|SYS_SPROC_VERSION<br /><br /> 指定当前实现的目录存储过程的版本。|当前的版本号|  
  
## <a name="remarks"></a>备注  
 **sp_server_info**返回提供的信息的子集**SQLGetInfo** ODBC 中。  
  
## <a name="permissions"></a>权限  
 需要对架构的 SELECT 权限。  
  
## <a name="see-also"></a>请参阅  
 [目录存储的过程&#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
