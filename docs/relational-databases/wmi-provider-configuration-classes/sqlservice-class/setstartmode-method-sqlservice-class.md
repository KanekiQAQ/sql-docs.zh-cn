---
title: SetStartMode 方法 （SqlService 类） |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
apiname:
- SetStartMode Method (SqlService Class)
apilocation:
- sqlmgmproviderxpsp2up.mof
apitype: MOFDef
helpviewer_keywords:
- SetStartMode method
ms.assetid: f6f198b4-f9a4-468c-8977-76462ef06e61
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 08309cd2f53c39ec4a37dc1345a1788379dd7310
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68006868"
---
# <a name="setstartmode-method-sqlservice-class"></a>SetStartMode 方法（SqlService 类）
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  修改服务实例的启动模式。  
  
## <a name="syntax"></a>语法  
  
```  
  
object.SetStartMode(StartMode)  
```  
  
## <a name="parts"></a>部件  
 *object*  
 一个表示服务的 [SqlService 类](../../../relational-databases/wmi-provider-configuration-classes/sqlservice-class/sqlservice-class.md) 对象。  
  
#### <a name="parameters"></a>Parameters  
 *StartMode*  
 一个指定服务实例的启动模式的 **uint32** 值。  
  
 有效值如下：  
  
 值 = 0。 启动 - 设备驱动程序由操作系统加载程序启动。 此值只对驱动程序服务有效。  
  
 值 = 1。 系统 - 设备驱动程序由 **IoInitSystem** 方法启动。 此值只对驱动程序服务有效。  
  
 值 = 2。 自动 - 服务将在系统启动期间由服务控制管理器自动启动。  
  
 值 = 3。 手动 - 服务将在进程调用 **StartService** 方法时由计算机管理器启动。  
  
 值 = 4。 已禁用 - 无法再启动服务。  
  
## <a name="property-valuereturn-value"></a>属性值/返回值  
 一个 **uint32** 值，如果服务已成功修改，则为 0；如果不支持请求，则为 1。 其他任何数字表示出现错误。  
  
## <a name="remarks"></a>备注  
  
## <a name="see-also"></a>请参阅  
 [启动和停止服务](https://technet.microsoft.com/library/ms174886\(v=sql.105\).aspx)  
  
  
