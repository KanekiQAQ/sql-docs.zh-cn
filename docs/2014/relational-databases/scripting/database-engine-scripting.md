---
title: 数据库引擎脚本 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], PowerShell
- scripts [SQL Server]
- scripting [SQL Server Database Engine]
- scripting [SQL Server Database Engine], PowerShell
ms.assetid: 9978a884-59a2-4e7f-a82a-335149f3a261
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 10e7b4c7e2972ed797048dbcaedcaaeec4d682d4
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "66090519"
---
# <a name="database-engine-scripting"></a>数据库引擎脚本
  [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 支持 [!INCLUDE[msCoName](../../includes/msconame-md.md)] PowerShell 脚本环境，以管理 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例和这些实例中的对象。 还可以在与脚本环境非常类似的环境中生成并运行包含 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 和 Xquery 的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询。  
  
## <a name="sql-server-powershell"></a>SQL Server PowerShell  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包含两个可用来实现以下内容的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 管理单元：  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 提供程序，它将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理对象模型层次结构公开为类似于文件系统路径的 PowerShell 路径。 可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理对象模型类来管理路径的每个节点处表示的对象。  
  
-   一组执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 命令的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cmdlet。 其中一个 cmdlet 是 **Invoke-Sqlcmd**。 这用于运行[!INCLUDE[ssDE](../../includes/ssde-md.md)]查询与运行的脚本`sqlcmd`实用程序。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了用于运行 PowerShell 的以下功能：  
  
-   可导入到 PowerShell 会话中的 **sqlps** PowerShell 模块，该模块之后将加载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理单元。可以交互方式运行即席 PowerShell 命令。 可以使用诸如 .\MyFolder\MyScript.ps1 这样的命令来运行脚本文件。  
  
-   PowerShell 脚本文件可用作 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理 PowerShell 作业步骤的输入，这些步骤按预订的时间间隔或者作为对系统事件的响应来运行脚本。  
  
-   用于启动 PowerShell 和导入 **模块的** sqlps [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实用工具。 然后，您可以执行该模块支持的所有操作。 可以在命令提示符中启动 **sqlps** 实用工具，也可以通过在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio 对象资源管理器树中右键单击节点并选择“启动 PowerShell”  来启动 sqlps 实用工具。  
  
## <a name="database-engine-queries"></a>数据库引擎查询  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询脚本包含三种类型的元素：  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 语言语句。  
  
-   Xquery 语言语句。  
  
-   命令和变量从`sqlcmd`实用程序。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供了三种用于生成和运行 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询的环境：  
  
-   可以在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器中以交互方式运行和调试 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]查询。 可以在一个会话中编写并调试多个语句，然后将所有这些语句保存在一个脚本文件中。  
  
-   `sqlcmd`命令提示实用工具，可以以交互方式运行[!INCLUDE[ssDE](../../includes/ssde-md.md)]查询和运行现有[!INCLUDE[ssDE](../../includes/ssde-md.md)]查询脚本文件。  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询脚本文件通常是使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 查询编辑器在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中以交互方式进行编码的。 之后，可在下面的某个环境中打开此文件：  
  
-   使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]“文件”  /“打开”  菜单在新的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口中打开此文件。  
  
-   使用 **-i**_input_file_参数来运行此文件`sqlcmd`实用程序。  
  
-   通过 **PowerShell 脚本中的** Invoke-Sqlcmd **cmdlet 使用** -QueryFromFile [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 参数运行此文件。  
  
-   使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理 [!INCLUDE[tsql](../../includes/tsql-md.md)] 作业步骤按计划的时间间隔或作为对系统事件的响应来运行脚本。  
  
 此外，还可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 生成脚本向导来生成 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 对象资源管理器中右键单击对象，然后选择“生成脚本”  菜单项。 **“生成脚本”** 会启动向导，指导您完成创建脚本的过程。  
  
## <a name="database-engine-scripting-tasks"></a>数据库引擎脚本任务  
  
|任务说明|主题|  
|----------------------|-----------|  
|介绍如何使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中的代码和文本编辑器来以交互方式开发、调试和运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本|[查询和文本编辑器 (SQL Server Management Studio)](../scripting/query-and-text-editors-sql-server-management-studio.md)|  
|介绍如何使用 `sqlcmd` 实用工具从命令提示符运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本，包括以交互方式开发脚本的能力。|[sqlcmd 操作指南主题](../../database-engine/sqlcmd-how-to-topics.md)|  
|介绍如何将 SQL Server 组件集成到 Windows PowerShell 2.0 环境中，然后生成 PowerShell 脚本以便管理 SQL Server 实例和对象。|[SQL Server PowerShell](../../powershell/sql-server-powershell.md)|  
|介绍如何使用 **“生成和发布脚本”** 向导创建从数据库重新创建一个或多个对象的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。|[生成脚本 (SQL Server Management Studio)](generate-scripts-sql-server-management-studio.md)|  
  
## <a name="see-also"></a>请参阅  
 [sqlcmd 实用工具](../../tools/sqlcmd-utility.md)   
 [教程：编写 Transact-SQL 语句](../../t-sql/tutorial-writing-transact-sql-statements.md)  
  
  
