---
title: 调试脚本 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Script task [Integration Services], debugging
- debugging [Integration Services], scripts
- scripts [Integration Services], debugging
ms.assetid: fddf57d8-8607-4f88-85a0-1b683087b491
author: janinezhang
ms.author: janinez
ms.openlocfilehash: a8dee34e6d2bb7cfb858ee1f10378d5ac4916f9b
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67945571"
---
# <a name="debugging-script"></a>调试脚本

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) 中编写脚本任务和脚本组件所使用的脚本。  
  
 可在 VSTA 中设置断点并为断点编写脚本。 可以在 VSTA 中管理断点，但也可以使用 **设计器提供的** “设置断点” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 对话框来管理断点。 有关详细信息，请参阅 [Debugging Control Flow](../../integration-services/troubleshooting/debugging-control-flow.md)。  
  
 **“设置断点”** 对话框包含脚本断点。 这些断点出现在断点列表的底部，并且显示出现断点的行号和函数名。 可以从 **“设置断点”** 对话框中删除脚本断点。  
  
 在运行时，在脚本中的代码行上设置的断点与对包或对包中的任务和容器设置的断点集成在一起。 调试器可以从脚本中的断点运行到包、任务或容器上设置的断点，亦可反之。 例如，一个包可能有对中断条件设置的断点（当包接收到 **OnPreExecute** 和 **OnPostExecute** 事件时发生中断），同时又有对脚本行设置断点的脚本任务。 在此方案中，该包可以在与 **OnPreExecute** 事件相关联的中断条件处挂起执行，运行到脚本中的断点，并最终运行到与 **OnPostExecute** 事件相关联的中断条件。  
  
 有关调试脚本任务和脚本组件的详细信息，请参阅 [Coding and Debugging the Script Task](../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md) 和 [Coding and Debugging the Script Component](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)。  
  
### <a name="to-set-a-breakpoint-in-visual-studio-for-applications"></a>在 Visual Studio for Applications 中设置断点  
  
-   [通过在脚本任务和脚本组件中设置断点来调试脚本](../../integration-services/extending-packages-scripting/debug-a-script-by-setting-breakpoints-in-a-script-task-and-script-component.md)  
  
## <a name="see-also"></a>另请参阅  
 [包开发的故障排除工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)  
  
  
