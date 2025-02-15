---
title: catalog.set_execution_property_override_value | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 37cb3c01-f4c0-4978-8e40-a975456def5a
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 3796b56d66ec2a4862d71454422bb2b248469977
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67897820"
---
# <a name="catalogsetexecutionpropertyoverridevalue"></a>catalog.set_execution_property_override_value 

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  为 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中的执行实例设置属性的值。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.set_execution_property_override_value [ @execution_id = execution_id  
    , [ @property_path = ] property_path  
    , [ @property_value = ] property_value  
    , [ @sensitive = ] sensitive  
```  
  
## <a name="arguments"></a>参数  
 [ @execution_id = ] execution_id   
 执行实例的唯一标识符。 execution_id 为 bigint   。  
  
 [ @property_path = ] *property_path*  
 指向包中属性的路径。 *property_path* 为 **nvarchar(4000)** 。  
  
 [ @property_value = ] property_value   
 要赋给该属性的覆盖值。 *property_value* 为 **nvarchar(max)** 。  
  
 [ @sensitive = ] sensitive   
 当值为 1 时，属性是敏感的并在存储时加密。 当值为 0 时，属性是不敏感的并以纯文本形式存储值。 *sensitive* 参数为 **bit**。  
  
## <a name="remarks"></a>Remarks  
 此过程执行的功能与“执行包”  对话框上“高级”  选项卡中的“属性重写”  部分执行的功能相同。 该属性的路径从包任务的“包路径”  属性派生。  
  
## <a name="return-code-value"></a>返回代码值  
 0（成功）  
  
## <a name="result-sets"></a>结果集  
 None  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下面的列表描述了一些可能引发错误或警告的情况：  
  
-   用户没有相应的权限  
  
-   执行标识符无效  
  
-   属性路径无效  
  
-   属性值的数据类型与属性的数据类型不匹配  
  
## <a name="see-also"></a>另请参阅  
 [catalog.set_execution_parameter_value（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
  
  
