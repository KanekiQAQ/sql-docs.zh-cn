---
title: “设置参数值”对话框 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: ce9c2201-4e9a-4495-948f-b68deeaa7955
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 83022e612afa862ae657f7f93dbe3a80ed8d0bb8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68070537"
---
# <a name="set-parameter-value-dialog-box"></a>设置参数值对话框

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  使用 **“设置参数值”** 对话框可为项目和包设置参数和连接管理器属性的值。  
  
 **您希望做什么？**  
  
-   [打开“设置参数值”对话框](#open_dialog)  
  
-   [配置选项](#option)  
  
##  <a name="open_dialog"></a> 打开“设置参数值”对话框  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，连接到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。  
  
     正在连接到承载 SSISDB 数据库的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例。  
  
2.  在对象资源管理器中，展开树以便显示 **“Integration Services 目录”** 节点。  
  
3.  展开 **“SSISDB”** 节点。  
  
4.  右键单击包或项目，单击“配置”  ，然后单击“参数”  选项卡或“连接管理器”  选项卡中的省略号按钮。  
  
##  <a name="option"></a> 配置选项  
 **参数**  
 列出参数名称。  
  
 **类型**  
 列出参数值的数据类型。  
  
 **Description**  
 显示参数的可选说明。  
  
 **编辑值**  
 选择此选项可编辑参数值。  
  
 **使用包中的默认值**  
 选择此选项可以使用存储在包中的默认参数值。  
  
 **使用环境变量**  
 选择此选项可使用存储在环境中的由项目或包引用的变量值。 若要向项目或包添加环境引用，请使用 **“配置”** 对话框。 有关详细信息，请参阅 [Configure Dialog Box](../../integration-services/catalog/configure-dialog-box.md)。  
  
  
