---
title: catalog.create_environment（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 66367092-9f6e-40e6-90bd-81efb078ab70
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 7871b8225e6574067befb3ab8a90af1875410def
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68110384"
---
# <a name="catalogcreateenvironment-ssisdb-database"></a>catalog.create_environment（SSISDB 数据库）

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中创建环境。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.create_environment [@folder_name =] folder_name  
     , [@environment_name =] environment_name  
  [  , [@environment_description =] environment_description ]  
```  
  
## <a name="arguments"></a>参数  
 [@folder_name =] folder_name   
 将包含环境的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [@environment_name =] environment_name   
 环境的名称。 environment_name 为 nvarchar(128)   。  
  
 [@environment_description=] *environment_description*  
 环境的可选说明。 *environment_description* 为 **nvarchar(1024)** 。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 None  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对文件夹的 READ 和 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   数据库角色  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   找不到文件夹名称  
  
-   指定的文件夹中已存在同名的环境  
  
## <a name="remarks"></a>Remarks  
 环境名称在文件夹中必须是唯一的。  
  
  
