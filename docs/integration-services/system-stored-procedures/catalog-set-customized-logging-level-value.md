---
title: catalog.set_customized_logging_level_value | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: d83fb763-c7c6-4e20-bd10-0f995598b198
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 0146d58a1495ad5c17625edbb9b9c6f2d295cfb8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67985302"
---
# <a name="catalogsetcustomizedlogginglevelvalue"></a>catalog.set_customized_logging_level_value 

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  更改现有自定义日志记录级别记录的统计信息或事件。 有关自定义日志记录级别的详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
## <a name="syntax"></a>语法  
  
```sql  
catalog.set_customized_logging_level_value [ @level_name = ] level_name  
    , [ @property_name = ] property_name  
    , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>参数  
 [ @level_name = ] level_name  
 现有自定义日志记录级别的名称。  
  
 level_name 为 nvarchar(128)。  
  
 [ @property_name = ] property_name  
 要更改的属性的名称。 有效值为 PROFILE 和 EVENTS。  
  
 property_name 为 nvarchar(128)。  
  
 [ @property_value = ] property_value  
 指定自定义日志记录级别的指定属性的新值。  
  
 有关配置文件和事件的有效值列表，请参阅 [catalog.create_customized_logging_level](../../integration-services/system-stored-procedures/catalog-create-customized-logging-level.md)。  
  
 property_value 为 bigint。  
  
## <a name="remarks"></a>Remarks  
  
## <a name="return-codes"></a>返回代码  
 0（成功）  
  
 存储过程失败时引发错误。  
  
## <a name="result-set"></a>结果集  
 None  
  
## <a name="permissions"></a>权限  
 此存储过程需要下列权限之一：  
  
-   **ssis_admin** 数据库角色中的成员资格  
  
-   **sysadmin** 服务器角色中的成员资格  
  
## <a name="errors-and-warnings"></a>错误和警告  
 下表说明了导致存储过程失败的情况。  
  
-   用户不具有所需的权限。  
  
  
