---
title: sp_help_fulltext_system_components (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_help_fulltext_components_TSQL
- sp_help_fulltext_components
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_system_components
ms.assetid: ac1fc7a0-7f46-4a12-8c5c-8d378226a8ce
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 949d22b0acdd4cc6d1e9d865f4f65e847d87aa46
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68055047"
---
# <a name="sphelpfulltextsystemcomponents-transact-sql"></a>sp_help_fulltext_system_components (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  返回注册的断字符、筛选器和协议处理程序的信息。 **sp_help_fulltext_system_components**也会返回一组数据库和全文目录的使用过指定的组件的标识符。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_fulltext_system_components   
         { 'all'| [ @component_type = ] 'component_type' }  
    , [ @param = ] 'param'  
```  
  
## <a name="arguments"></a>参数  
 'all'  
 返回所有全文组件的信息。  
  
`[ @component_type = ] component_type` 指定的组件的类型。 *component_type*可以是以下之一：  
  
-   **wordbreaker**  
  
-   **filter**  
  
-   **协议处理程序**  
  
-   **fullpath**  
  
 如果指定完整路径，则*param*还必须使用组件 DLL 的完整路径来指定或返回错误消息。  
  
`[ @param = ] param` 根据组件类型，这是以下值之一： 区域设置标识符 (LCID)、 文件扩展名与"。"前缀的协议处理程序或组件 DLL 的完整路径的完整组件名称。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 为系统组件返回下列结果集。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**componenttype**|**sysname**|组件的类型。 可以是以下类型之一：<br /><br /> filter<br /><br /> 协议处理程序<br /><br /> 断字符|  
|**componentname**|**sysname**|组件的名称。|  
|**clsid**|**uniqueidentifier**|组件的类标识符。|  
|**fullpath**|**nvarchar(256)**|指向组件位置的路径。<br /><br /> NULL = 不是成员的调用方**serveradmin**固定的服务器角色。|  
|**version**|**nvarchar(30)**|组件的版本。|  
|**manufacturer**|**sysname**|组件制造商的名称。|  
  
 返回以下结果集仅当一个或多个全文目录存在，则使用*component_type*。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**dbid**|**int**|数据库 ID。|  
|**ftcatid**|**int**|全文目录的 ID。|  
  
## <a name="permissions"></a>权限  
 要求的成员身份**公共**角色; 但是，用户只能看到他们具有 VIEW DEFINITION 权限的全文目录有关的信息。 只有的成员**serveradmin**固定的服务器角色可以看到中的值**fullpath**列。  
  
## <a name="remarks"></a>备注  
 此方法在准备升级时尤为重要。 执行特定数据库中的存储过程，然后使用输出确定升级是否将影响特定目录。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-all-full-text-system-components"></a>A. 列出所有全文系统组件  
 下面的示例列出了已经在该服务器实例上注册的所有全文系统组件。  
  
```  
EXEC sp_help_fulltext_system_components 'all';  
GO  
```  
  
### <a name="b-listing-word-breakers"></a>B. 列出断字符  
 下面的示例列出了在服务实例上注册的所有断字符。  
  
```  
EXEC sp_help_fulltext_system_components 'wordbreaker';  
GO  
```  
  
### <a name="c-determining-whether-a-specific-word-breaker-is-registered"></a>C. 确定特定断字符是否已注册。  
 下面的示例列出了土耳其语 (LCID = 1055) 的断字符（如果已经在系统上安装该语言并已在服务实例上注册）。 此示例指定参数名称， **@component_type** 并 **@param** 。  
  
```  
EXEC sp_help_fulltext_system_components @component_type = 'wordbreaker', @param = 1055;  
GO  
```  
  
 默认情况下，未安装此断字符，所以结果集为空。  
  
### <a name="d-determining-whether-a-specific-filter-has-been-registered"></a>D. 确定某个特定筛选器是否已注册。  
 下面的示例列出了 .xdoc 组件的筛选器（如果已经在系统上手动安装该组件并已在服务器实例上注册）。  
  
```  
EXEC sp_help_fulltext_system_components 'filter', '.xdoc';  
GO  
```  
  
 默认情况下，未安装此筛选器，所以结果集为空。  
  
### <a name="e-listing-a-specific-dll-file"></a>E. 列出特定 .dll 文件  
 `nlhtml.dll`下面的示例列出了特定的 .ddl 文件 ，默认情况下会安装该文件。  
  
```  
EXEC sp_help_fulltext_system_components 'fullpath',   
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\nlhtml.dll';  
GO  
  
```  
  
## <a name="see-also"></a>请参阅  
 [查看或更改注册的筛选器和断字符](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md)   
 [配置和管理断字符和词干分析器以便搜索](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [配置和管理搜索筛选器](../../relational-databases/search/configure-and-manage-filters-for-search.md)   
 [全文搜索和语义搜索存储过程&#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
