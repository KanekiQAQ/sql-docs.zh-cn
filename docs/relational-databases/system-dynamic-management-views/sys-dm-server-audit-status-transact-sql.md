---
title: sys.dm_server_audit_status (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 04/19/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_server_audit_status_TSQL
- sys.dm_server_audit_status
- dm_server_audit_status
- sys.dm_server_audit_status_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_server_audit_status dynamic management view
ms.assetid: 4aa32d54-2ae1-437e-bbaa-7f1df1404b44
author: stevestein
ms.author: sstein
ms.openlocfilehash: 3291edb34087e46739cf984d2412821fa66b7a07
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68053219"
---
# <a name="sysdmserverauditstatus-transact-sql"></a>sys.dm_server_audit_status (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  对于每次服务器审核返回一行，以指示该审核的当前状态。 有关详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**audit_id**|**int**|审核的 ID。 映射到**audit_id**字段中**sys.audits**目录视图。|  
|**name**|**sysname**|审核的名称。 与相同**名称**字段中**sys.server_audits**目录视图。|  
|**status**|**smallint**|服务器审核的数值状态：<br /><br /> 0 = 未启动<br /><br /> 1 =<br />        Started<br /><br /> 2 =<br />      运行时失败<br /><br /> 3 = 目标创建失败<br /><br /> 4 = 正在关闭|  
|**status_desc**|**nvarchar(256)**|显示服务器审核状态的字符串：<br /><br /> NOT_STARTED<br /><br /> STARTED<br /><br /> RUNTIME_FAIL<br /><br /> TARGET_CREATION_FAILED<br /><br /> SHUTTING_DOWN|  
|**status_time**|**datetime2**|审核的最后状态更改的时间戳（UTC 格式）。|  
|**event_session_address**|**varbinary(8)**|与审核关联的扩展事件会话的地址。 与相关**sys.db_xe_sessions.address**目录视图。|  
|**audit_file_path**|**nvarchar(256)**|当前使用的审核文件目标的完整路径名和文件名。 仅对文件审核填充。|  
|**audit_file_size**|**bigint**|审核文件的近似大小（字节数）。 仅对文件审核填充。|  
  
## <a name="permissions"></a>权限  
 主体必须具有**VIEW SERVER STATE**并**选择**权限。  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>请参阅  
 [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT (Transact-SQL)](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [创建服务器审核和服务器审核规范](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
