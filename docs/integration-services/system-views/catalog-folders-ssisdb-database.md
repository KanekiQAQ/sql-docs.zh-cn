---
title: catalog.folders（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 21a37c16-60aa-4b3f-8bca-ac90ad1697ac
author: janinezhang
ms.author: janinez
ms.openlocfilehash: a1f0a4c2964e61b7eaa0a3064615b36629bc464e
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68047967"
---
# <a name="catalogfolders-ssisdb-database"></a>catalog.folders（SSISDB 数据库）

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  显示 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的文件夹。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|id|**bigint**|文件夹的唯一标识符。|  
|NAME|**sysname(nvarchar(128)**|文件夹的名称，此名称在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中是唯一的。|  
|description|**nvarchar(1024)**|文件夹的说明。|  
|created_by_sid|**varbinary(85)**|创建了文件夹的用户的安全标识符 (SID)。|  
|created_by_name|**nvarchar(128)**|创建文件夹的用户名。|  
|created_time|**datetimeoffset(7)**|创建文件夹的日期和时间。|  
  
## <a name="remarks"></a>Remarks  
 此视图对于目录中的每个文件夹显示一行。  
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   针对文件夹的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
> [!NOTE]  
>  当您具有在服务器上执行操作的权限时，您还具有查看有关此操作的信息的权限。 将实施行级安全性；只显示您有权查看的行。  
  
  
