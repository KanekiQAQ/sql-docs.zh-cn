---
title: catalog.rename_environment（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: c73d7452-31c5-4f4e-afcc-e9eca760c826
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 2db5b171f0f853383c2bae99202c511151c38d8d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68007229"
---
# <a name="catalogrenameenvironment-ssisdb-database"></a>catalog.rename_environment（SSISDB 数据库）

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中重命名环境。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.rename_environment [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @new_environment_name= ] new_environment_name  
```  
  
## <a name="arguments"></a>参数  
 [ @folder_name = ] folder_name   
 包含环境的文件夹的名称。 *folder_name* 为 **nvarchar(128)** 。  
  
 [ @environment_name = ] environment_name   
 环境的原始名称。 environment_name 为 nvarchar(128)   。  
  
 [ @new_environment_name = ] *new_environment_name*  
 环境的新名称。 *new_environment_name* 为 **nvarchar(128)** 。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 None  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对环境的 MODIFY 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   原始环境名称无效  
  
-   新名称已用于现有环境  
  
## <a name="remarks"></a>Remarks  
 当您重命名环境时，项目中的环境引用不会自动更新。 必须相应地更新环境引用。 即使通过更改环境名称损坏了环境引用，此存储过程也会成功。 在此存储过程完成之后，必须更新环境引用。  
  
> [!NOTE]  
>  当一个环境引用无效时，对使用这些引用的包进行验证和执行将失败。  
  
  
