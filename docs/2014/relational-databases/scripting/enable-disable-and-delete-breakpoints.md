---
title: 启用、禁用和删除断点 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: 357b5874-273f-43a9-8e30-83872bdea5dc
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: edc19f948689fafea8cde0fb4ae2fd5f79de3242
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66064062"
---
# <a name="enable-disable-and-delete-breakpoints"></a>启用、禁用和删除断点
  若要查看和管理所有打开的断点，可以使用 **“断点”** 窗口。 使用此窗口可以查看断点信息，并且执行一些诸如删除、禁用或启用断点之类的操作。  
  
## <a name="the-breakpoints-window"></a>“断点”窗口  
 **“断点”** 窗口列出了一些信息，例如断点处于哪个代码行。 在 **“断点”** 窗口中，还可以删除、禁用和启用断点。 有关 **“断点”** 窗口的详细信息，请参阅 [“断点” Window](transact-sql-debugger-breakpoints-window.md)。  
  
 禁用断点可以暂停断点执行，但会保留其定义，以便以后启用该断点。 删除断点将会永久删除此断点。 您必须切换一个新断点，以对该语句暂停执行。  
  
## <a name="to-open-the-breakpoints-window"></a>打开“断点”窗口  
 **To open the Breakpoints window**  
  
 可以使用以下方式之一打开 **“断点”** 窗口：  
  
-   在 **“调试”** 菜单上单击 **“窗口”** ，然后单击 **“断点”** 。  
  
-   在 **“调试”** 工具栏上，单击 **“断点”** 按钮。  
  
-   按 Ctrl+Alt+B。  
  
## <a name="to-disable-a-single-breakpoint"></a>禁用单个断点  
 **To disable a single breakpoint**  
  
 可以使用以下方式之一禁用单个断点：  
  
-   在查询编辑器窗口中，右键单击所需断点，然后单击“禁用断点”  。  
  
-   在“断点”窗口中，清除断点左侧的复选框。  
  
## <a name="to-disable-all-breakpoints"></a>禁用所有断点  
 **To disable all breakpoints**  
  
 可以使用以下方式之一禁用所有断点：  
  
-   在 **“调试”** 菜单上单击 **“禁用所有断点”** 。  
  
-   在 **“断点”** 窗口的工具栏上，单击 **“禁用所有断点”** 按钮。  
  
## <a name="to-enable-a-single-breakpoint"></a>启用单个断点  
 **To enable a single breakpoint**  
  
 可以使用以下方式之一启用单个断点：  
  
-   在查询编辑器窗口中，右键单击所需断点，然后单击“启用断点”  。  
  
-   在“断点”窗口中，单击断点左侧的框。  
  
## <a name="to-enable-all-breakpoints"></a>启用所有断点  
 **To enable all breakpoints**  
  
 可以使用以下方式之一启用所有断点：  
  
-   在 **“调试”** 菜单上单击 **“启用所有断点”** 。  
  
-   在 **“断点”** 窗口的工具栏上，单击 **“启用所有断点”** 按钮。  
  
## <a name="to-delete-a-single-breakpoint"></a>删除单个断点  
 **To delete a single breakpoint**  
  
 可以使用以下方式之一删除单个断点：  
  
-   在查询编辑器窗口中，右键单击所需断点，然后单击“删除断点”  。  
  
-   在断点窗口中，右键单击所需断点，然后在快捷菜单上单击“删除”  。  
  
-   在“断点”窗口中，选择所需断点，然后按 Delete。  
  
## <a name="to-delete-all-breakpoints"></a>删除所有断点  
 **To delete all breakpoints**  
  
 可以使用以下方式之一删除所有断点：  
  
-   在 **“调试”** 菜单上单击 **“删除所有断点”** 。  
  
-   在 **“断点”** 窗口的工具栏上，单击 **“删除所有断点”** 按钮。  
  
## <a name="see-also"></a>请参阅  
 [切换断点](../spatial/point.md)  
  
  
