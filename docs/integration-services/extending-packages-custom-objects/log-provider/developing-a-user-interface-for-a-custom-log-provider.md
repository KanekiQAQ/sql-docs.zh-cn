---
title: 为自定义日志提供程序开发用户界面 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom user interface [Integration Services], custom log providers
- custom log providers [Integration Services], developing custom user interface
ms.assetid: 6fd2d269-d87a-4134-82a1-40a09b3b5453
author: janinezhang
ms.author: janinez
ms.openlocfilehash: f3dccd3641592ebaadb7df3dd467cb28cf9a617f
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68062938"
---
# <a name="developing-a-user-interface-for-a-custom-log-provider"></a>为自定义日志提供程序开发用户界面

[!INCLUDE[ssis-appliesto](../../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  许多 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 日志提供程序都有一个自定义用户界面，该界面实现 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsLogProviderUI>，并用已筛选的可用连接管理器下拉列表替换“配置 SSIS 日志”对话框中的“配置”文本框   。 但是，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中不实现自定义日志提供程序的自定义用户界面。  
  
## <a name="see-also"></a>另请参阅  
 [创建自定义日志提供程序](../../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md)   
 [编写自定义日志提供程序代码](../../../integration-services/extending-packages-custom-objects/log-provider/coding-a-custom-log-provider.md)  
  
  
