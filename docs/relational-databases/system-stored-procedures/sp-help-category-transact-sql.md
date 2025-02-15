---
title: sp_help_category (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_help_category
- sp_help_category_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_category
ms.assetid: 8cad1dcc-b43e-43bd-bea0-cb0055c84169
author: stevestein
ms.author: sstein
ms.openlocfilehash: c297578fabca3c20781c6227307f25dbece1bbfd
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68055230"
---
# <a name="sphelpcategory-transact-sql"></a>sp_help_category (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  提供有关作业、警报或操作员的指定类的信息。  
   
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_category [ [ @class = ] 'class' ]   
     [ , [ @type = ] 'type' ]   
     [ , [ @name = ] 'name' ]   
     [ , [ @suffix = ] suffix ]   
```  
  
## <a name="arguments"></a>参数  
`[ @class = ] 'class'` 有关哪些请求信息的类。 *类*是**varchar(8)** ，默认值为**作业**。 *类*可以是下列值之一。  
  
|ReplTest1|描述|  
|-----------|-----------------|  
|**JOB**|提供有关作业类别的信息。|  
|**发出警报**|提供有关警报类别的信息。|  
|**运算符**|提供有关操作员类别的信息。|  
  
`[ @type = ] 'type'` 为其请求信息的类别的类型。 *类型*是**varchar(12)** ，默认值为 NULL，并且可以是下列值之一。  
  
|值|描述|  
|-----------|-----------------|  
|**LOCAL**|本地作业类别。|  
|**MULTI -SERVER**|多服务器作业类别。|  
|**NONE**|以外的其他类的类别**作业**。|  
  
`[ @name = ] 'name'` 为其请求信息的类别的名称。 *名称*是**sysname**，默认值为 NULL。  
  
`[ @suffix = ] suffix` 指定是否**category_type**结果集中的列是 ID 还是名称。 *后缀*是**位**，默认值为**0**。 **1**显示了**category_type**作为名称，并**0**显示为一个 id。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="result-sets"></a>结果集  
 当 **@suffix** 是**0**， **sp_help_category**返回以下结果集：  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**category_id**|**int**|类别 ID|  
|**category_type**|**tinyint**|类别的类型：<br /><br /> **1** = 本地<br /><br /> **2** = 多服务器<br /><br /> **3** = 无|  
|**name**|**sysname**|类别名称|  
  
 当 **@suffix** 是**1**， **sp_help_category**返回以下结果集：  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**category_id**|**int**|类别 ID|  
|**category_type**|**sysname**|类别的类型。 之一**本地**，**多服务器**，或**NONE**|  
|**name**|**sysname**|类别名称|  
  
## <a name="remarks"></a>备注  
 **sp_help_category**必须从运行**msdb**数据库。  
  
 如果未指定参数，则结果集将提供有关所有作业类别的信息。  
  
## <a name="permissions"></a>权限  
 默认情况下，只有 **sysadmin** 固定服务器角色的成员才可以执行此存储过程。 其他用户必须被授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] msdb **数据库中下列** 代理固定数据库角色的权限之一：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 有关这些角色的权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-local-job-information"></a>A. 返回本地作业信息  
 以下示例将返回有关在本地管理的作业的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_category  
    @type = N'LOCAL' ;  
GO  
```  
  
### <a name="b-returning-alert-information"></a>B. 返回警报信息  
 以下示例将返回有关 Replication 警报类别的信息。  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_category  
    @class = N'ALERT',  
    @name = N'Replication' ;  
GO  
```  
  
## <a name="see-also"></a>请参阅  
 [sp_add_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md)   
 [sp_delete_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-category-transact-sql.md)   
 [sp_update_category &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-category-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
