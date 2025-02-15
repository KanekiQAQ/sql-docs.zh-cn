---
title: sp_attachsubscription (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_attachsubscription
- sp_attachsubscription_TSQL
helpviewer_keywords:
- sp_attachsubscription
ms.assetid: b9bbda36-a46a-4327-a01e-9cd632e4791b
author: stevestein
ms.author: sstein
ms.openlocfilehash: d9f144d9d896fb75af5f59850c249b9044d1b781
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68046149"
---
# <a name="spattachsubscription-transact-sql"></a>sp_attachsubscription (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  将现有的订阅数据库附加到任何订阅服务器。 此存储过程在新订阅服务器的主数据库中执行。  
  
> [!IMPORTANT]  
>  不推荐使用此功能，该功能将在未来版本中删除。 在新的开发工作中不要使用此功能。 对于使用参数化筛选器分区的合并发布，建议您使用分区快照的新功能，这些功能可简化大量订阅的初始化。 有关详细信息，请参阅 [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)。 对于未分区的发布，可以使用备份来初始化订阅。 有关详细信息，请参阅 [初始化事务订阅（不使用快照）](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)中手动初始化订阅。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_attachsubscription [ @dbname = ] 'dbname'  
        , [ @filename = ] 'filename'  
    [ , [ @subscriber_security_mode = ] 'subscriber_security_mode' ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]   
    [ , [ @distributor_login = ] 'distributor_login' ]   
    [ , [ @distributor_password = ] 'distributor_password' ]   
    [ , [ @publisher_security_mode = ] publisher_security_mode ]   
    [ , [ @publisher_login = ] 'publisher_login' ]   
    [ , [ @publisher_password = ] 'publisher_password' ]   
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @db_master_key_password = ] 'db_master_key_password' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @dbname = ] 'dbname'` 是按名称指定目标订阅数据库的字符串。 *dbname*是**sysname**，无默认值。  
  
`[ @filename = ] 'filename'` 是的名称和物理位置的主 MDF (**主**数据文件)。 *文件名*是**nvarchar(260)** ，无默认值。  
  
`[ @subscriber_security_mode = ] 'subscriber_security_mode'` 是订阅服务器同步时连接到订阅服务器时要使用的安全模式。 *subscriber_security_mode*是**int**，默认值为 NULL。  
  
> [!NOTE]  
>  必须使用 Windows 身份验证。 如果*subscriber_security_mode*不是**1** （Windows 身份验证），则返回错误。  
  
`[ @subscriber_login = ] 'subscriber_login'` 是同步时连接到订阅服务器时要使用的订阅服务器登录名。 *subscriber_login*是**sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  已不推荐使用此参数，保留它只是为了让脚本能够向后兼容。 如果*subscriber_security_mode*不是**1**并*subscriber_login*是指定，则返回错误。  
  
`[ @subscriber_password = ] 'subscriber_password'` 为订阅服务器密码。 *subscriber_password*是**sysname**，默认值为 NULL。  
  
> [!NOTE]  
>  已不推荐使用此参数，保留它只是为了让脚本能够向后兼容。 如果*subscriber_security_mode*不是**1**并*subscriber_password*是指定，则返回错误。  
  
`[ @distributor_security_mode = ] distributor_security_mode` 是同步时连接到分发服务器时要使用的安全模式。 *distributor_security_mode*是**int**，默认值为**0**。 **0**指定[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]身份验证。 **1**指定 Windows 身份验证。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` 是使用同步时连接到分发服务器的分发服务器登录名。 *distributor_login*如果，则需要*distributor_security_mode*设置为**0**。 *distributor_login*是**sysname**，默认值为 NULL。  
  
`[ @distributor_password = ] 'distributor_password'` 是分发服务器密码。 *distributor_password*如果，则需要*distributor_security_mode*设置为**0**。 *distributor_password*是**sysname**，默认值为 NULL。 值*distributor_password*必须是少于 120 个 Unicode 字符。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。 如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @publisher_security_mode = ] publisher_security_mode` 是同步时连接到发布服务器时要使用的安全模式。 *publisher_security_mode*是**int**，默认值为**1**。 如果**0**，指定[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]身份验证。 如果**1**，指定 Windows 身份验证。 [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @publisher_login = ] 'publisher_login'` 是要进行同步时连接到发布服务器时使用的登录名。 *publisher_login*是**sysname**，默认值为 NULL。  
  
`[ @publisher_password = ] 'publisher_password'` 连接到发布服务器时使用的密码。 *publisher_password*是**sysname**，默认值为 NULL。 值*publisher_password*必须是少于 120 个 Unicode 字符。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。 如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @job_login = ] 'job_login'` 是用于运行代理的 Windows 帐户的登录名。 *job_login*是**nvarchar(257)** ，无默认值。 此 Windows 帐户总是用于与分发服务器建立代理连接。  
  
`[ @job_password = ] 'job_password'` 是用于运行代理的 Windows 帐户的密码。 *job_password*是**sysname**，无默认值。 值*job_password*必须是少于 120 个 Unicode 字符。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @db_master_key_password = ] 'db_master_key_password'` 是的用户定义数据库主密钥的密码。 *db_master_key_password*是**nvarchar(524)** ，默认值为 NULL。 如果*db_master_key_password*未指定，则将删除并重新创建现有数据库主密钥。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_attachsubscription**快照复制、 事务复制和合并复制中使用。  
  
 如果发布保持期已满，则不能将订阅附加到发布中。 如果指定一个保持期已满的订阅，则附加该订阅或先对其进行同步时将发生错误。 发布的发布保持期**0** （永不过期） 将被忽略。  
  
## <a name="permissions"></a>权限  
 只有的成员**sysadmin**固定的服务器角色可以执行**sp_attachsubscription**。  
  
## <a name="see-also"></a>请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
