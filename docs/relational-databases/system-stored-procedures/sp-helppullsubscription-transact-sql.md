---
title: sp_helppullsubscription (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_helppullsubscription_TSQL
- sp_helppullsubscription
helpviewer_keywords:
- sp_helppullsubscription
ms.assetid: a0d9c3f1-1fe9-497c-8e2f-5b74f47a7346
author: stevestein
ms.author: sstein
ms.openlocfilehash: 777e9b2afceb3e9a58030a225a0cbff34375165b
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67914966"
---
# <a name="sphelppullsubscription-transact-sql"></a>sp_helppullsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  显示订阅服务器上的一个或多个订阅的有关信息。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helppullsubscription [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @publication = ] 'publication' ]  
    [ , [ @show_push = ] 'show_push' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 是的远程服务器的名称。 *发布服务器*是**sysname**，默认值为 **%** ，表示返回所有发布服务器的信息。  
  
`[ @publisher_db = ] 'publisher_db'` 是发布服务器数据库的名称。 *publisher_db*是**sysname**，默认值为 **%** ，这会返回所有发布服务器数据库。  
  
`[ @publication = ] 'publication'` 是发布的名称。 *发布*是**sysname**，默认值为 **%** ，这会返回所有发布。 如果此参数等于 ALL，唯一的请求订阅 independent_agent = **0**返回。  
  
`[ @show_push = ] 'show_push'` 是否要返回所有推送订阅。 *show_push*是**nvarchar(5)** ，默认值为 FALSE，表示不返回推送订阅。  
  
## <a name="result-sets"></a>结果集  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|发布服务器的名称。|  
|**发布服务器数据库**|**sysname**|发布服务器数据库名。|  
|**publication**|**sysname**|发布的名称。|  
|**independent_agent**|**bit**|表明该发布是否有独立的分发代理。|  
|**订阅类型**|**int**|发布的订阅类型。|  
|**分发代理**|**nvarchar(100)**|处理订阅的分发代理。|  
|**发布说明**|**nvarchar(255)**|对发布的说明。|  
|**上次更新时间**|**date**|订阅信息上次更新的时间。 这是由 ISO 日期 (114) 和 ODBC 时间 (121) 组成的 UNICODE 字符串。 格式为 yyyymmdd hh:mi:sss.mmm，其中“yyyy”表示年，“mm”表示月，“dd”表示日，“hh”表示小时，“mi”表示分钟，“sss”表示秒，“mmm”表示毫秒。|  
|**订阅名称**|**varchar(386)**|订阅的名称。|  
|**最后一个事务时间戳**|**varbinary(16)**|上一个复制的事务的时间戳。|  
|**更新模式**|**tinyint**|允许的更新类型。|  
|**分发代理 job_id**|**int**|分发代理的作业 ID。|  
|**enabled_for_synmgr**|**int**|指示是否可以通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 同步管理器同步订阅。|  
|**订阅 guid**|**binary(16)**|订阅服务器上的发布的版本的全局标识符。|  
|**subid**|**binary(16)**|匿名订阅的全局标识符。|  
|**immediate_sync**|**bit**|表示是否在每次快照代理运行时创建或重新创建同步文件。|  
|**发布服务器登录名**|**sysname**|在发布服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录 ID。|  
|**发布服务器密码**|**nvarchar(524)**|在发布服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的密码（加密）。|  
|**发布服务器 security_mode**|**int**|在发布服务器上实现的安全模式：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]身份验证<br /><br /> **1** = Windows 身份验证<br /><br /> **2** = 同步触发器使用静态**sysservers**项执行远程过程调用 (RPC) 和*发布者*必须在定义**sysservers**为远程服务器或链接的服务器的表。|  
|**分发服务器**|**sysname**|分发服务器的名称。|  
|**distributor_login**|**sysname**|在分发服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的登录 ID。|  
|**distributor_password**|**nvarchar(524)**|在分发服务器上用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证的密码（加密）。|  
|**distributor_security_mode**|**int**|在分发服务器上实施的安全模式：<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]身份验证<br /><br /> **1** = Windows 身份验证|  
|**ftp_address**|**sysname**|仅为保持向后兼容。|  
|**ftp_port**|**int**|仅为保持向后兼容。|  
|**ftp_login**|**sysname**|仅为保持向后兼容。|  
|**ftp_password**|**nvarchar(524)**|仅为保持向后兼容。|  
|**alt_snapshot_folder**|**nvarchar(255)**|存储快照文件夹的位置（如果该位置是默认位置以外的位置）。|  
|**working_directory**|**nvarchar(255)**|使用文件传输协议 (FTP) 传输快照文件（指定了该选项时）时将文件传输到的目录的完全限定路径。|  
|**use_ftp**|**bit**|订阅通过 Internet 订阅发布，并配置 FTP 寻址属性。 如果**0**，订阅不使用 FTP。 如果**1**，订阅使用 FTP。|  
|**publication_type**|**int**|指定发布的复制类型：<br /><br /> **0** = 事务复制<br /><br /> **1** = 快照复制<br /><br /> **2** = 合并复制|  
|**dts_package_name**|**sysname**|指定 Data Transformation Services (DTS) 包的名称。|  
|**dts_package_location**|**int**|存储 DTS 包的位置：<br /><br /> **0** = 分发服务器<br /><br /> **1** = 订阅服务器|  
|**offload_agent**|**bit**|指定是否可以远程激活代理。 如果**0**，不能远程激活代理。|  
|**offload_server**|**sysname**|指定用于远程激活的服务器所在的网络的名称。|  
|**last_sync_status**|**int**|订阅状态：<br /><br /> **0** = 所有作业都在等待开始<br /><br /> **1** = 一个或多个作业正在启动<br /><br /> **2** = 所有作业都已成功执行<br /><br /> **3** = 至少一个作业正在执行<br /><br /> **4** = 所有作业计划和空闲<br /><br /> **5** = 至少一个作业正在尝试在上一次失败后执行<br /><br /> **6** = 至少一个作业无法成功执行|  
|**last_sync_summary**|**sysname**|对上一次同步结果的说明。|  
|**last_sync_time**|**datetime**|订阅信息上次更新的时间。 这是由 ISO 日期 (114) 和 ODBC 时间 (121) 组成的 UNICODE 字符串。 格式为 yyyymmdd hh:mi:sss.mmm，其中“yyyy”表示年，“mm”表示月，“dd”表示日，“hh”表示小时，“mi”表示分钟，“sss”表示秒，“mmm”表示毫秒。|  
|**job_login**|**nvarchar(512)**|是运行分发代理的 Windows 帐户的格式返回*域*\\*用户名*。|  
|**job_password**|**sysname**|出于安全原因，值为" **\*\*\*\*\*\*\*\*\*\*** "是始终返回。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_helppullsubscription**快照和事务复制中使用。  
  
## <a name="permissions"></a>权限  
 只有的成员**sysadmin**固定的服务器角色或**db_owner**固定的数据库角色可以执行**sp_helppullsubscription** 。  
  
## <a name="see-also"></a>请参阅  
 [sp_addpullsubscription &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_droppullsubscription &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
