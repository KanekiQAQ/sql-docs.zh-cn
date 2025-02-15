---
title: sp_adddistpublisher (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_adddistpublisher
- sp_adddistpublisher_TSQL
helpviewer_keywords:
- sp_adddistpublisher
ms.assetid: 04e15011-a902-4074-b38c-3ec2fc73b838
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 5c9cab9530a67b01274bf5d1bafa579199f0a4d8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68072784"
---
# <a name="spadddistpublisher-transact-sql"></a>sp_adddistpublisher (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]

  配置发布服务器以使用指定的分发数据库。 此存储过程在分发服务器上的任何数据库中执行。 请注意，存储的过程[sp_adddistributor &#40;TRANSACT-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md)并[sp_adddistributiondb &#40;-&#41; ](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)必须在使用此存储之前运行过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_adddistpublisher [ @publisher= ] 'publisher'   
        , [ @distribution_db= ] 'distribution_db'   
    [ , [ @security_mode= ] security_mode ]   
    [ , [ @login= ] 'login' ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @working_directory= ] 'working_directory' ]   
    [ , [ @storage_connection_string= ] 'storage_connection_string']
    [ , [ @trusted= ] 'trusted' ]   
    [ , [ @encrypted_password= ] encrypted_password ]   
    [ , [ @thirdparty_flag = ] thirdparty_flag ]  
    [ , [ @publisher_type = ] 'publisher_type' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 是发布服务器名称。 *发布服务器*是**sysname**，无默认值。  
  
`[ @distribution_db = ] 'distribution_db'` 是分发数据库的名称。 *distributor_db*是**sysname**，无默认值。 复制代理使用该参数连接到发布服务器。  
  
`[ @security_mode = ] security_mode` 是实现的安全模式。 此参数仅由复制代理连接到发布服务器为排队更新订阅或与非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]发布服务器。 *security_mode*是**int**，可以是下列值之一。  
  
|ReplTest1|Description|  
|-----------|-----------------|  
|**0**|分发服务器中的复制代理使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证连接到发布服务器。|  
|**1** （默认值）|分发服务器中的复制代理使用 Windows 身份验证连接到发布服务器。|  
  
`[ @login = ] 'login'` 是的登录名。 此参数是必需的如果*security_mode*是**0**。 login 的数据类型为 sysname，默认值为 NULL   。 复制代理使用该参数连接到发布服务器。  
  
`[ @password = ] 'password']` 是的密码。 *密码*是**sysname**，默认值为 NULL。 复制代理使用该参数连接到发布服务器。  
  
> [!IMPORTANT]  
>  不要使用空密码。 请使用强密码。  
  
`[ @working_directory = ] 'working_directory'` 是用于存储发布的数据和架构文件的工作目录的名称。 *working_directory*是**nvarchar(255)** ，默认值为此实例的 ReplData 文件夹[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，例如`C:\Program Files\Microsoft SQL Server\MSSQL\MSSQ.1\ReplData`。 名称应按 UNC 格式指定。  

 对于 Azure SQL 数据库，请使用`\\<storage_account>.file.core.windows.net\<share>`。

`[ @storage_connection_string = ] 'storage_connection_string'` 是必需的 SQL 数据库。 使用下存储访问密钥从 Azure 门户 > 设置。

 > [!INCLUDE[Azure SQL Database link](../../includes/azure-sql-db-repl-for-more-information.md)]

`[ @trusted = ] 'trusted'` 此参数已弃用，提供是为了向后兼容性。 *受信任*是**nvarchar(5)** ，并将其设置为任何内容，但是**false**将导致错误。  
  
`[ @encrypted_password = ] encrypted_password` 设置*encrypted_password*不再受支持。 尝试将此项设置**位**参数**1**将导致错误。  
  
`[ @thirdparty_flag = ] thirdparty_flag` 发布服务器时，是[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 *thirdparty_flag*是**位**，可以是下列值之一。  
  
|ReplTest1|描述|  
|-----------|-----------------|  
|**0** （默认值）|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
|**1**|非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。|  
  
`[ @publisher_type = ] 'publisher_type'` 当发布服务器不是指定的发布服务器类型[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 *publisher_type*数据类型为 sysname，并可以是下列值之一。  
  
|ReplTest1|描述|  
|-----------|-----------------|  
|**MSSQLSERVER**<br /><br /> （默认值）|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。|  
|**ORACLE**|指定标准的 Oracle 发布服务器。|  
|**ORACLE 网关**|指定 Oracle 网关发布服务器。|  
  
 有关 Oracle 发布服务器和 Oracle 网关发布服务器之间的差异的详细信息，请参阅[配置 Oracle 发布服务器](../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md)。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_adddistpublisher**使用快照复制、 事务复制和合并复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistpublisher-tran_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有的成员**sysadmin**固定的服务器角色可以执行**sp_adddistpublisher**。  
  
## <a name="see-also"></a>请参阅  
 [配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpdistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [配置分发](../../relational-databases/replication/configure-distribution.md)  
  
  
