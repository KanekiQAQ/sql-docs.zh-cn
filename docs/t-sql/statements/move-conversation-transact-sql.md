---
title: MOVE CONVERSATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- MOVE_CONVERSATION_TSQL
- MOVE CONVERSATION
- MOVE_TSQL
- MOVE
dev_langs:
- TSQL
helpviewer_keywords:
- moving conversations
- MOVE CONVERSATION statement
- relocating conversations
- conversations [Service Broker], groups
- conversations [Service Broker], moving
ms.assetid: 1da4d2c9-e767-434e-b49b-615711a7f626
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: d76dd0507336a110337f01509440d4ad64afc085
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68129440"
---
# <a name="move-conversation-transact-sql"></a>MOVE CONVERSATION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  将会话移动到不同的会话组。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
MOVE CONVERSATION conversation_handle  
   TO conversation_group_id  
[ ; ]  
```  
  
## <a name="arguments"></a>参数  
 *conversation_handle*  
 一个变量或常量，包含要移动的会话的会话句柄。 conversation_handle 的类型必须为 uniqueidentifier   。  
  
 TO conversation_group_id   
 一个变量或常量，包含会话将要移至的会话组的标识符。 conversation_group_id 的类型必须为 uniqueidentifier   。  
  
## <a name="remarks"></a>Remarks  
 MOVE CONVERSATION 语句将由 conversation_handle 指定的会话移动到由 conversation_group_id 标识的会话组   。 只能在与相同队列关联的会话组之间重定向对话框。  
  
> [!IMPORTANT]  
>  如果 MOVE CONVERSATION 语句不是批处理或存储过程中的第一个语句，前面的语句必须以分号 (;)（[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句终止符）结尾  。  
  
 MOVE CONVERSATION 语句将锁定与 conversation_handle 关联的会话组和由 conversation_group_id 指定的会话组，直到包含该语句的事务提交或回滚   。  
  
 MOVE CONVERSATION 在用户定义函数中无效。  
  
## <a name="permissions"></a>权限  
 若要移动会话，当前用户必须是会话和会话组的所有者，或者是 sysadmin 固定服务器角色的成员或 db_owner 固定数据库角色的成员。  
  
## <a name="examples"></a>示例  
 下面的示例将会话移动到不同的会话组。  
  
```  
DECLARE @conversation_handle UNIQUEIDENTIFIER,  
        @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_handle =  
    <retrieve conversation handle from database> ;  
SET @conversation_group_id =  
    <retrieve conversation group ID from database> ;  
  
MOVE CONVERSATION @conversation_handle TO @conversation_group_id ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [BEGIN DIALOG CONVERSATION (Transact-SQL)](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [GET CONVERSATION GROUP (Transact-SQL)](../../t-sql/statements/get-conversation-group-transact-sql.md)   
 [END CONVERSATION (Transact-SQL)](../../t-sql/statements/end-conversation-transact-sql.md)   
 [sys.conversation_groups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-groups-transact-sql.md)   
 [sys.conversation_endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
