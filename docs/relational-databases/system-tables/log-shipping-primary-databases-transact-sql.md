---
title: log_shipping_primary_databases (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_primary_databases
- log_shipping_primary_databases_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_primary_databases system table
ms.assetid: 56888756-a798-42be-9b5e-0f9aa05a2cc6
author: stevestein
ms.author: sstein
ms.openlocfilehash: 893f016fba45d18947af376425012d21964709b0
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68004710"
---
# <a name="logshippingprimarydatabases-transact-sql"></a>log_shipping_primary_databases (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  在日志传送配置中为主数据库存储一条记录。 此表存储中**msdb**数据库。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|日志传送配置的主数据库 ID。|  
|**primary_database**|**sysname**|日志传送配置中主数据库的名称。|  
|**backup_directory**|**nvarchar(500)**|存储主服务器的事务日志备份文件的目录。|  
|**backup_share**|**nvarchar(500)**|备份目录的网络或 UNC 路径。|  
|**backup_retention_period**|**int**|日志备份文件在删除之前保留在备份目录中的时间长度（分钟）。|  
|**backup_job_id**|**uniqueidentifier**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]与主服务器上的备份作业相关联的代理作业 ID。|  
|**monitor_server**|**sysname**|实例的名称[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]正用作监视服务器，日志传送配置中。|  
|**monitor_server_security_mode**|**bit**|用于连接到监视服务器的安全模式。<br /><br /> 1 = Windows 身份验证。<br /><br /> 0 =[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]身份验证。|  
|**last_backup_file**|**nvarchar(500)**|最近一次事务日志备份的绝对路径。|  
|**last_backup_date**|**datetime**|上一次日志备份操作的时间和日期。|  
|**user_specified_monitor**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> **sp_help_log_shipping_primary_database**并**sp_help_log_shipping_secondary_primary**使用此列以控制中监视器设置显示[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。<br /><br /> 0 = 当调用任何一种两个存储过程时，用户未指定显式值 **@monitor_server** 参数。<br /><br /> 1 = 用户指定一个显式值。|  
|**backup_compression**|**tinyint**|指示日志传送配置是否覆盖服务器级的备份压缩行为。<br /><br /> 0 = 禁用。 永远不压缩日志备份，不管服务器配置的备份压缩设置是什么。<br /><br /> 1 = 已启用。 始终压缩日志备份，不管服务器配置的备份压缩设置是什么。<br /><br /> 2 = 使用的服务器配置[查看或配置 backup compression default 服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)服务器配置选项。 这是默认值。<br /><br /> 仅在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 Enterprise 版本中支持备份压缩。|  
  
## <a name="see-also"></a>请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_primary_database (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
