---
title: sysmail_delete_mailitems_sp (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysmail_delete_mailitems_sp_TSQL
- sysmail_delete_mailitems_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_mailitems_sp
ms.assetid: f87c9f4a-bda1-4bce-84b2-a055a3229ecd
author: stevestein
ms.author: sstein
ms.openlocfilehash: 23ecda5fd8d91f20133eb2295d38dc9d9ace66f6
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68069105"
---
# <a name="sysmaildeletemailitemssp-transact-sql"></a>sysmail_delete_mailitems_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  从数据库邮件内部表中永久删除电子邮件。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_delete_mailitems_sp  [ [ @sent_before = ] 'sent_before' ]  
    [ , [ @sent_status = ] 'sent_status' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @sent_before = ] 'sent_before'` 删除之前的日期和时间作为提供的电子邮件*sent_before*参数。 *sent_before*是**datetime**默认值为 NULL。 NULL 指示所有日期。  
  
`[ @sent_status = ] 'sent_status'` 删除指定的类型的电子邮件*sent_status*。 *sent_status*是**varchar(8)** ，无默认值。 有效输入包括**发送**，**未发送**，**重试**，以及**失败**。 NULL 指示所有状态。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 数据库邮件及其附件存储在**msdb**数据库。 应该定期删除邮件以免**msdb**从增长超过预期大小并符合你组织的文档保持期计划。 使用**sysmail_delete_mailitems_sp**存储过程来从数据库邮件表中永久删除电子邮件。 某个可选参数通过提供日期和时间，允许您仅删除较早的电子邮件。 早于该参数的电子邮件将被删除。 另一个可选参数允许您删除某一类型的指定为仅电子邮件**sent_status**参数。 你必须提供一个参数为 **@sent_before** 或 **@sent_status** 。 若要删除的所有消息，请使用 **@sent_before = getdate （） 函数**。  
  
 删除电子邮件也会删除与这些邮件相关的附件。 删除电子邮件不会删除中的相应条目**sysmail_event_log**。 使用[sysmail_delete_log_sp](../../relational-databases/system-stored-procedures/sysmail-delete-log-sp-transact-sql.md)从日志中删除项。  
  
## <a name="permissions"></a>权限  
 默认情况下，此存储的过程授予执行成员关闭**sysadmin**固定的服务器角色和**DatabaseMailUserRole**。 成员**sysadmin**固定的服务器角色才能执行此过程来删除所有用户发送电子邮件。 成员**DatabaseMailUserRole**可以仅删除由该用户发送电子邮件。  
  
## <a name="examples"></a>示例  
  
### <a name="a-deleting-all-e-mails"></a>A. 删除所有电子邮件  
 以下示例删除数据库邮件系统中的所有电子邮件。  
  
```  
DECLARE @GETDATE datetime  
SET @GETDATE = GETDATE();  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp @sent_before = @GETDATE;  
GO  
```  
  
### <a name="b-deleting-the-oldest-e-mails"></a>B. 删除最早的电子邮件  
 以下示例将删除数据库邮件日志中 `October 9, 2005` 之前的电子邮件。  
  
```  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp   
    @sent_before = 'October 9, 2005' ;  
GO  
```  
  
### <a name="c-deleting-all-e-mails-of-a-certain-type"></a>C. 删除特定类型的所有电子邮件  
 以下示例删除数据库邮件日志中所有失败的电子邮件。  
  
```  
EXECUTE msdb.dbo.sysmail_delete_mailitems_sp   
    @sent_status = 'failed' ;  
GO  
```  
  
## <a name="see-also"></a>请参阅  
 [sysmail_allitems &#40;TRANSACT-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-allitems-transact-sql.md)   
 [sysmail_event_log &#40;TRANSACT-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-event-log-transact-sql.md)   
 [sysmail_mailattachments &#40;TRANSACT-SQL&#41;](../../relational-databases/system-catalog-views/sysmail-mailattachments-transact-sql.md)   
 [创建 SQL Server 代理作业以存档数据库邮件和事件日志](../../relational-databases/database-mail/create-a-sql-server-agent-job-to-archive-database-mail-messages-and-event-logs.md)  
  
  
