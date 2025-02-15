---
title: Transact-SQL 调试器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, introduction
ms.assetid: 6e914699-0d85-46c2-aa2d-3e339ac2c4ce
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 86db1bbcf975b68b5091975453f163e97a5ed036
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253312"
---
# <a name="transact-sql-debugger"></a>Transact-SQL 调试器
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器通过调查代码的运行时行为可以帮助您查找 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码中的错误。 将 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口设置为调试模式后，可在特定的代码行上暂停执行，并检查那些 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句使用和返回的信息和数据。  
  
## <a name="stepping-through-transact-sql-code"></a>单步执行 Transact-SQL 代码  
 当 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询编辑器窗口处于调试模式时，可以使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器提供的以下选项逐个导航 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 代码：  
  
-   在各个 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句上设置断点。  
  
     断点指定一个点，您希望在到该点时暂停执行以便您检查数据。 在启动调试器时，调试器会在查询编辑器窗口中的第一个代码行上暂停。 若要运行到您设置的第一个断点，可以使用 **“继续”** 功能。 也可以使用 **“继续”** 功能从窗口当前暂停的任何位置运行到下一个断点。 您可以编辑断点，以便指定一些操作（诸如在哪些条件下断点应暂停执行），指定要输出到 **“输出”** 窗口中的信息，以及更改断点的位置。  
  
-   单步执行下一个语句。  
  
     使用此选项可以在一组语句中逐个导航，并在导航时观察它们的行为。  
  
-   单步或逐过程对存储过程或函数执行调用。  
  
     如果确认存储过程中没有错误，可以逐过程执行。 这样可以完整执行该过程，并将结果返回到代码。  
  
     如果要调试一个存储过程或函数，则可单步执行模块。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 会打开一个由模块源代码填充的新 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口，将该窗口置于调试模式，并暂停对模块中第一个语句的执行。 然后，您就可以在模块代码中导航，例如通过设置断点或逐句通过代码。  
  
 有关如何通过调试器浏览代码的详细信息，请参阅 [Transact-SQL 代码](../../relational-databases/scripting/step-through-transact-sql-code.md)。  
  
## <a name="viewing-debugger-information"></a>查看调试器信息  
 每当调试器对特定 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句暂停执行时，您可以都使用以下调试器窗口来查看当前执行状态：  
  
-   **局部变量** 和 **监视** 这些窗口显示当前分配的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式。 表达式是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 子句，其计算结果为单个标量表达式。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器支持查看引用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 变量、参数或名称以 @@ 开头的内置函数的表达式。 这些窗口还显示当前分配给表达式的数据值。  
  
-   **快速监视。** 此窗口显示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式的值，还可将该表达式保存到 **监视** 窗口。  
  
-   **断点。** 此窗口显示当前设置的断点，通过此窗口可对断点进行管理。  
  
-   **调用堆栈。** 此窗口显示当前执行位置。 它还提供关于执行如何从原始查询编辑器窗口开始，通过所有函数、存储过程或触发器，最后到达当前执行位置的信息。  
  
-   **输出。** 此窗口显示各种消息和程序数据，如来自调试器的系统消息。  
  
-   **结果** 和 **消息** “查询编辑器”窗口上的这些选项卡显示以前执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的结果。  
  
## <a name="transact-sql-debugger-tasks"></a>Transact-SQL 调试器任务  
  
|任务说明|主题|  
|----------------------|-----------|  
|介绍如何配置 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器以用于远程调试。|[运行 TSQL 调试器之前配置防火墙规则](../../relational-databases/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md)|  
|介绍如何启动、停止和控制调试器的操作。|[运行 Transact-SQL 调试器](../../relational-databases/scripting/run-the-transact-sql-debugger.md)|  
|介绍如何使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器逐句通过代码。|[Transact-SQL 代码](../../relational-databases/scripting/step-through-transact-sql-code.md)|  
|介绍如何使用调试器查看 [!INCLUDE[tsql](../../includes/tsql-md.md)] 数据，例如参数、变量和系统信息。|[Transact-SQL 调试器信息](../../relational-databases/scripting/transact-sql-debugger-information.md)|  
  
## <a name="see-also"></a>另请参阅  
 [查询和文本编辑器 (SQL Server Management Studio)](../../relational-databases/scripting/query-and-text-editors-sql-server-management-studio.md)  
  
  
