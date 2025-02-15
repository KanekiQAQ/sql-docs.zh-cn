---
title: “断点”窗口 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Breakpoints Window [Transact-SQL]
ms.assetid: bad88d10-fdd5-4d3d-b5ea-a4f063847485
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4795f3567f1e9b0067d7811c1631c73bac2b0ac8
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68259015"
---
# <a name="transact-sql-debugger---breakpoints-window"></a>Transact-SQL 调试器 -“断点”窗口
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  **“断点”** 窗口列出在当前 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器中设置的所有断点。 若要管理断点，请使用“断点”窗口中的工具栏。  断点是代码中的某些位置，在调试模式下执行在这些位置暂停，以便您可以查看调试数据。  
  
## <a name="task-list"></a>任务列表  
 **访问“断点”窗口**  
  
-   在 **“调试”** 菜单上单击 **“窗口”**，然后单击 **“断点”**。  
  
## <a name="breakpoints-window-columns"></a>“断点”窗口中包含的列  
 默认情况下， **“断点”** 窗口列出以下列。  
  
 **名称**  
 显示断点名称。 断点名称是由调试器提供的。 该名称含有包含此断点的数据库引擎查询编辑器窗口的名称和查询编辑器中设有此断点的行的行号。  
  
 **条件**  
 显示“(无条件)”。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器不支持设置断点条件。  
  
 **命中计数**  
 显示“始终中断”。  
  
 在 **“列”** 列表中选择以下列后，可以添加或删除这些列。  
  
 **Filter**  
 显示“(无)”。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器不支持设置断点筛选器。  
  
 **命中条件**  
 显示“中断”。  
  
 **语言**  
 如果是 **，则显示** Transact-SQL [!INCLUDE[tsql](../../includes/tsql-md.md)]。  
  
 **函数**  
 显示设有断点的行的行号。  
  
 **File**  
 显示包含断点的源文件的名称和设有此断点的行的行号。  
  
 **Address**  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器不支持此功能。  
  
 **处理**  
 显示“[SQL]”，则表明这是一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 进程。 后面跟随代码在其中执行的[!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例的名称。  
  
## <a name="breakpoints-window-toolbar"></a>“断点”窗口工具栏  
 如果当前 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口具有活动断点，则 **“断点”** 窗口将显示一个可用于管理这些断点的工具栏。  
  
 **删除**  
 删除所选断点。  
  
 **删除全部断点**  
 删除“断点”窗口中显示的全部断点。  
  
 **禁用所有断点**  
 禁用所有断点，以使其不再中断代码执行，但保留断点。 禁用所有断点后，该按钮即变为 **“启用所有断点”**。  
  
 **“启用所有断点”**  
 启用所有断点，以使它们中断代码执行。 启用所有断点后，该按钮即变为 **“禁用所有断点”**。  
  
 **转到源代码**  
 将光标定位在查询编辑器中包含所选断点的行。  
  
 **列**  
 列出所有可以在“断点”窗口中显示的列。 复选框指示所显示的列。 若要在 **“断点”** 窗口中添加或删除某一列，请在此列表中选择该列。  
  
## <a name="see-also"></a>另请参阅  
 [Transact-SQL 调试器](../../relational-databases/scripting/transact-sql-debugger.md)  
