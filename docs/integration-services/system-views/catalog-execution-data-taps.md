---
title: catalog.execution_data_taps | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 54226c01-5b8f-4730-8a5f-1da2613f9689
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 7db0723ed403340b0e24136ac6bac7217943d401
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68065246"
---
# <a name="catalogexecutiondatataps"></a>catalog.execution_data_taps 

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  显示在执行中定义的每个数据分流点的信息。  
  
|列名|数据类型|描述|  
|-----------------|---------------|-----------------|  
|data_tap_id|**bigint**|数据分流点的唯一标识符 (ID)。|  
|execution_id|**bigint**|执行实例的唯一标识符 (ID)。|  
|package_path|**nvarchar(max)**|分流数据的数据流任务的包路径。|  
|dataflow_path_id_string|**nvarchar(4000)**|数据流路径的标识字符串。|  
|dataflow_task_guid|**uniqueidentifier**|数据流任务的唯一 ID。|  
|max_rows|**int**|要捕获的行数。 如果未指定此值，则将捕获所有行。|  
|filename|**nvarchar(4000)**|数据转储文件的名称。 有关详细信息，请参阅 [生成包执行的转储文件](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)。|  
  
## <a name="permissions"></a>权限  
 此视图需要下列权限之一：  
  
-   针对执行实例的 READ 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
> [!NOTE]  
>  当您具有在服务器上执行操作的权限时，您还具有查看有关此操作的信息的权限。 将实施行级安全性；只显示您有权查看的行。  
  
## <a name="see-also"></a>另请参阅  
 [生成包执行的转储文件](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)  
  
  
