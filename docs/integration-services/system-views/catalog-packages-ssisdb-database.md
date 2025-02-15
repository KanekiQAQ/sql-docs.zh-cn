---
title: catalog.packages（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- packages view [Integration Services]
- catalog.packages view [Integration Services]
ms.assetid: a634e94d-f492-4dfd-9611-a35f545106a1
author: janinezhang
ms.author: janinez
ms.openlocfilehash: a921aaeebc57ab72f75c980b0877a9fa0723fdca
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67997846"
---
# <a name="catalogpackages-ssisdb-database"></a>catalog.packages（SSISDB 数据库）

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  显示 SSISDB  目录中显示的所有包的详细信息。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|package_id|**bigint**|包的唯一标识符 (ID)。|  
|NAME|**nvarchar(256)**|包的唯一名称。|  
|package_guid|**uniqueidentifier**|标识包的全局唯一标识符 (GUID)。|  
|description|**nvarchar(1024)**|包的可选说明。|  
|package_format_version|**int**|用于开发包的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。|  
|version_major|**int**|包的主版本。|  
|version_minor|**int**|包的次版本。|  
|version_build|**int**|包的内部版本号。|  
|version_comments|**nvarchar(1024)**|包版本的可选注释。|  
|version_guid|**uniqueidentifier**|唯一标识包版本的 GUID。|  
|project_id|**bigint**|项目的唯一 ID。|  
|entry_point|**bit**|值为 `1` 表示直接启动包。 值为 `0` 表示该包由另一个包通过“执行包”任务启动。 默认值是 `1`。|  
|validation_status|**char(1)**|验证的状态。|  
|last_validation_time|**datetimeoffset(7)**|最后验证的时间。|  
  
## <a name="remarks"></a>Remarks  
 此视图对于目录中的每个包显示一行。  
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   针对相应项目的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格。  
  
> [!NOTE]  
>  如果您对项目具有 READ 权限，则也对与该项目关联的所有包和环境引用具有 READ 权限。 将实施行级安全性；只显示您有权查看的行。  
  
  
