---
title: sp_check_subset_filter (TRANSACT-SQL) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_check_TSQL
- sp_check_subset_filter
- filter
- subset
- subset_TSQL
- sp_check
- sp_check_subset_filter_TSQL
helpviewer_keywords:
- sp_check_subset_filter
ms.assetid: 525cfcfc-f317-478d-ba84-72e62285f160
author: stevestein
ms.author: sstein
ms.openlocfilehash: fa956275619286c059dacf25a5b9b2b83ed732e6
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68070517"
---
# <a name="spchecksubsetfilter-transact-sql"></a>sp_check_subset_filter (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  用于对任何表检查筛选子句，以确定筛选子句对该表是否有效。 此存储过程返回所提供的筛选器的相关信息，包括筛选器是否适合用于预计算分区。 此存储过程在发布服务器上包含该发布的数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "主题链接图标") [TRANSACT-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_check_subset_filter [ @filtered_table = ] 'filtered_table'  
        , [ @subset_filterclause = ] 'subset_filterclause'  
    [ , [ @has_dynamic_filters = ] has_dynamic_filters OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @filtered_table = ] 'filtered_table'` 是筛选的名称。 *filtered_table*是**nvarchar(400)** ，无默认值。  
  
`[ @subset_filterclause = ] 'subset_filterclause'` 是要测试的筛选器子句。 *subset_filterclause*是**nvarchar(1000)** ，无默认值。  
  
`[ @has_dynamic_filters = ] has_dynamic_filters` 是，如果筛选器子句的参数化的行筛选器。 *has_dynamic_filters*是**位**，默认值为 NULL 并且是输出参数。 返回的值**1**时筛选器子句是参数化的行筛选器。  
  
## <a name="result-sets"></a>结果集  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|**can_use_partition_groups**|**bit**|是发布是否限定使用预计算的分区;其中**1**意味着，可以使用分区，预计算并**0**意味着不能使用它们。|  
|**has_dynamic_filters**|**bit**|如果提供的筛选器子句包括至少一个参数化的行筛选器;其中**1**表示使用参数化的行筛选器，并**0**表示不使用此类函数。|  
|**dynamic_filters_function_list**|**nvarchar(500)**|筛选子句中动态筛选项目的函数的列表，其中，以分号分隔每个函数。|  
|**uses_host_name**|**bit**|如果[host_name （)](../../t-sql/functions/host-name-transact-sql.md)函数用在筛选器子句中，其中**1**表示此函数。|  
|**uses_suser_sname**|**bit**|如果[suser_sname （)](../../t-sql/functions/suser-sname-transact-sql.md)函数用在筛选器子句中，其中**1**表示此函数。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** （成功） 或**1** （失败）  
  
## <a name="remarks"></a>备注  
 **sp_check_subset_filter**合并复制中使用。  
  
 **sp_check_subset_filter**可以执行对任何表中，即使未发布表。 在定义筛选项目之前，此存储过程可以用来验证筛选子句。  
  
## <a name="permissions"></a>权限  
 只有的成员**sysadmin**固定的服务器角色或**db_owner**固定的数据库角色可以执行**sp_check_subset_filter**。  
  
## <a name="see-also"></a>请参阅  
 [使用预计算分区优化参数化筛选器性能](../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)  
  
  
