---
title: sp_audit_write (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_audit_write
- sp_audit_write_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_audit_write
ms.assetid: 4c523848-1ce6-49ad-92b3-e0e90f24f1c2
author: stevestein
ms.author: sstein
ms.openlocfilehash: 60dbabcadaf5108572eaba6361fab28eaf0f49b3
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68046138"
---
# <a name="spauditwrite-transact-sql"></a>sp_audit_write (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  添加到用户定义的审核事件**USER_DEFINED_AUDIT_GROUP**。 如果**USER_DEFINED_AUDIT_GROUP**未启用，则**sp_audit_write**将被忽略。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_audit_write [ @user_defined_event_id = ] user_defined_event_id
    [ , [ @succeeded = ] succeeded ]
    [ , [ @user_defined_information = ] 'user_defined_information' ]
    [ ; ]
```  
  
## <a name="arguments"></a>参数  
 `[ @user_defined_event_id = ] user_defined_event_id`  
 参数由用户定义并且记录在**user_defined_event_id**审核日志的列。 *@user_defined_event_id* 是类型**smallint**。  
  
 `[ @succeeded = ] succeeded`  
 用户为指示事件是否成功而传递的参数。 它将出现在审核日志的成功列中。 `@succeeded` 是**位**。  
  
 `[ @user_defined_information = ] 'user_defined_information'`  
 由用户定义的文本，并且该文本将记录在审核日志的新的 user_defined_event_id 列中。 `@user_defined_information` 是**nvarchar(4000)** 。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
 失败是由不正确的输入参数或者未能写入目标审核日志导致的。  
  
## <a name="remarks"></a>备注  
 当**USER_DEFINED_AUDIT_GROUP**添加到服务器审核规范或数据库审核规范，触发的事件**sp_audit_write**将包含在审核日志。  
  
## <a name="permissions"></a>权限  
 要求的成员身份**公共**数据库角色。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-user-defined-audit-event-with-informational-text"></a>A. 创建具有信息文本的用户定义的审核事件  
 下面的示例创建一个审核事件，该事件具有 ID 27、成功值 0，并且包括可选信息文本。  
  
```  
EXEC sp_audit_write @user_defined_event_id =  27 ,   
              @succeeded =  0   
            , @user_defined_information = N'Access to a monitored object.' ;  
```  
  
### <a name="b--creating-a-user-defined-audit-event-without-informational-text"></a>B.  创建没有信息文本的用户定义的审核事件  
 下面的示例创建一个审核事件，该事件具有 ID 27、成功值 0，并且不包括可选信息文本或可选参数名称。  
  
```  
EXEC sp_audit_write 27, 0;  
  
```  
  
## <a name="see-also"></a>请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_addrole (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)   
 [sp_dropuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_grantdbaccess (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
