---
title: catalog.revoke_permission（SSISDB 数据库）| Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- revoke_permission stored procedure [Integration Services]
- catalog.revoke_permission stored procedure [Integration Services]
ms.assetid: 850b9c26-5c7c-47b9-a61c-5cf9bb5948cf
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 81209c7e194f70536f9da27ac7358da5df265fcd
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67897872"
---
# <a name="catalogrevokepermission-ssisdb-database"></a>catalog.revoke_permission（SSISDB 数据库）

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中撤消对某个安全对象的权限。  
  
## <a name="syntax"></a>语法  
  
```sql
catalog.revoke_permission [ @object_type = ] object_type  
    , [ @object_id = ] object_id  
    , [ @principal_id = ] principal_id  
    , [ @permission_type = ] permission_type  
```  
  
## <a name="arguments"></a>参数  
 [ @object_type = ] *object_type*  
 安全对象的类型。 安全对象类型包括文件夹 (`1`）、项目 (`2`)、环境 (`3`) 和操作 (`4`)。*object_type* 为 **smallint** _。_  
  
 [ @object_id = ] *object_id*  
 安全对象的唯一标识符 (ID)。 *object_id* 为 **bigint**。  
  
 [ @principal_id = ] *principal_id*  
 被撤消权限的主体的 ID。 *principal_id* 为 **int**。  
  
 [ @permission_type = ] *permission_type*  
 权限的类型。 *permission_type* 为 **smallint**。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）  
  
 1（object_class 无效）  
  
 2（object_id 不存在）  
  
 3（主体不存在）  
  
 4（权限无效）  
  
 5（其他错误）  
  
## <a name="result-sets"></a>结果集  
 None  
  
## <a name="remarks"></a>Remarks  
 None  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   针对对象的 ASSIGN_PERMISSIONS 权限  
  
-   **ssis_admin** 数据库角色的成员资格  
  
-   **sysadmin** 服务器角色的成员资格  
  
## <a name="remarks"></a>Remarks  
 如果指定 permission_type，存储过程会删除显式分配给主体的针对对象的权限。 即使不发生此类情况，该过程也将返回成功代码值 (`0`)。 如果忽略 permission_type，存储过程会删除主体针对对象的所有权限。  
  
> [!NOTE]  
>  如果主体是具有指定权限的角色的成员，则该主体可能仍对该对象具有指定的权限。  
  
 通过此存储过程可以撤消下表中所示的权限类型：  
  
|permission_type 值|权限名称|权限说明|适用对象类型|  
|----------------------------|---------------------|----------------------------|-----------------------------|  
|`1`|READ|允许主体读取被视为对象一部分的信息（如属性）。 它不允许主体枚举或读取该对象中包含的其他对象的内容。|文件夹、项目、环境、操作|  
|`2`|MODIFY|允许主体修改被视为对象一部分的信息（如属性）。 它不允许主体修改该对象中包含的其他对象。|文件夹、项目、环境、操作|  
|`3`|在运行 CREATE 语句前执行|允许主体执行项目中的所有包。|项目|  
|`4`|MANAGE_PERMISSIONS|允许主体向对象分配权限。|文件夹、项目、环境、操作|  
|`100`|CREATE_OBJECTS|允许主体在文件夹中创建对象。|文件夹|  
|`101`|READ_OBJECTS|允许主体读取文件夹中的所有对象。|文件夹|  
|`102`|MODIFY_OBJECTS|允许主体修改文件夹中的所有对象。|文件夹|  
|`103`|EXECUTE_OBJECTS|允许主体执行文件夹中所有项目的所有包。|文件夹|  
|`104`|MANAGE_OBJECT_PERMISSIONS|允许主体管理文件夹中所有对象的权限。|文件夹|  
  
  
