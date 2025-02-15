---
title: PowerShell 扩展
titleSuffix: Azure Data Studio
description: 安装并使用 PowerShell for Azure Data Studio
ms.custom: seodec18
ms.date: 04/19/2019
ms.reviewer: alayu; sstein
ms.prod: sql
ms.technology: azure-data-studio
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
manager: matthend
ms.openlocfilehash: c7a2dbdccf92a52d5733a04915acc3f76dc3f033
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "65105951"
---
# <a name="powershell-editor-support-for-azure-data-studio"></a>对 Azure Data Studio 的 PowerShell 编辑器支持

此扩展提供了丰富的 PowerShell 编辑器支持在[Azure Data Studio](https://github.com/Microsoft/azuredatastudio)。
现在您可以编写和调试 PowerShell 脚本使用 Azure Data Studio 提供的出色类似于 IDE 的接口。

![PowerShell 扩展](media/powershell-extension/powershell-extension.png)


## <a name="features"></a>功能

- 语法突出显示
- 代码片段
- IntelliSense 为 cmdlet 和的详细信息
- 提供的基于规则的分析[PowerShell 脚本分析器](http://github.com/PowerShell/PSScriptAnalyzer)
- 转到定义的 cmdlet 和变量
- 查找 cmdlet 和变量的引用
- 文档和工作区符号发现
- 运行选定的 PowerShell 代码中使用的所选的内容<kbd>F8</kbd>
- 启动下面游标使用的符号的联机帮助<kbd>Ctrl</kbd>+<kbd>F1</kbd>
- 基本的交互式控制台支持 ！


## <a name="installing-the-extension"></a>安装扩展

可以按照中的步骤安装 PowerShell 扩展的正式版本[Azure Data Studio 文档](https://docs.microsoft.com/sql/azure-data-studio/extensions)。
在扩展窗格中，搜索"PowerShell"扩展，并安装那里。  你将收到自动有关的任何将来的扩展更新 ！

此外可以安装 VSIX 包从我们[版本页](https://github.com/PowerShell/vscode-powershell/releases)并通过命令行进行安装：

```powershell
azuredatastudio --install-extension PowerShell-<version>.vsix
```

## <a name="platform-support"></a>平台支持

- **Windows 7 至 10**与 Windows PowerShell v3 及更高版本，和 PowerShell Core
- **Linux** PowerShell core （所有的 PowerShell 支持分发）
- **macOS** PowerShell core

读取[常见问题解答](https://github.com/PowerShell/vscode-powershell/wiki/FAQ)的常见问题的解答。

## <a name="installing-powershell-core"></a>安装 PowerShell Core

如果运行的 MacOS 或 Linux 上的 Azure Data Studio，您可能还需要安装 PowerShell Core。

PowerShell Core 是一个开放源代码项目上[GitHub](https://github.com/powershell/powershell)。
在 MacOS 或 Linux 平台上安装 PowerShell Core 的详细信息，请参阅以下文章：

- [Linux 上安装 PowerShell Core](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6)
- [在 macOS 上安装 PowerShell Core](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6)

## <a name="example-scripts"></a>示例脚本

在扩展中有一些示例脚本`examples`文件夹可以用来发现 PowerShell 编辑和调试功能。  查看包含[README.md](https://github.com/PowerShell/vscode-powershell/blob/master/examples/README.md)文件若要详细了解如何使用它们。

此文件夹位于以下路径：

```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.PowerShell-<version>/examples
```

或如果您使用的预览版本的扩展

 ```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.powershell-preview-<version>/examples
```

若要打开/查看 Azure Data Studio 扩展的示例，请在 PowerShell 命令提示符下运行下面的代码：

```powershell
azuredatastudio (Get-ChildItem $Home\.azuredatastudio\extensions\ms-vscode.PowerShell-*\examples)[-1]
```

### <a name="creating-and-opening-files"></a>创建和打开文件

若要创建并打开新的文件在编辑器内，使用新建-EditorFile 从 PowerShell 集成终端中。

```powershell
PS C:\temp> New-EditorFile ExportData.ps1
```

此命令适用于任何文件类型，而不仅仅是 PowerShell 文件。

```powershell
PS C:\temp> New-EditorFile ImportData.py
```

若要在 Azure 数据工作室中打开一个或多个文件，请使用`Open-EditorFile`命令。

```powershell
Open-EditorFile ExportData.ps1, ImportData.py
```

### <a name="no-focus-on-console-when-executing"></a>在控制台上执行时没有焦点

对于那些习惯于使用 SSMS 的用户，您所习惯能够执行查询，以及然后能够重新在无需切换回查询窗格中再次执行它。  在这种情况下，代码编辑器的默认行为可能会感到很陌生。  若要在使用执行时，在编辑器中保持焦点<kbd>F8</kbd>更改以下设置：

```json
"powershell.integratedConsole.focusConsoleOnExecute": false
```

默认值是`true`用于可访问性目的。

请注意此设置将阻止焦点更改到控制台中，即使您使用显式之类的输入，调用命令`Get-Credential`。

## <a name="sql-powershell-examples"></a>SQL PowerShell 示例
为了使用这些示例 （下面），您需要安装中的 SqlServer 模块[PowerShell 库](https://www.powershellgallery.com/packages/SqlServer)。

```powershell
Install-Module -Name SqlServer
```

> [!NOTE]
> 与版本`21.1.18102`及更高版本，`SqlServer`模块支持[PowerShell Core](https://github.com/PowerShell/PowerShell) 6.2 最多支持，除了 Windows PowerShell。

在此示例中，我们使用`Get-SqlInstance`cmdlet 来获取 ServerA 和 ServerB Server SMO 对象。  此命令将包括实例名称的输出的默认版本，Service Pack 和更新级别 CU 实例。

```powershell
Get-SqlInstance -ServerInstance ServerA, ServerB
```

下面是一个示例的输出将如下所示：

```
Instance Name             Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------             -------    ------------ -----------  ------------ ----------------
ServerA                   13.0.5233  SP2          CU4          Windows      Windows Server 2016 Datacenter
ServerB                   14.0.3045  RTM          CU12         Linux        Ubuntu
```
`SqlServer`模块包含提供程序调用`SQLRegistration`可用于以编程方式访问已保存 SQL Server 连接的以下类型：

+ 数据库引擎服务器 （已注册的服务器）
+ 中央管理服务器 (CMS)
+ Analysis Services
+ Integration Services
+ Reporting Services

 在以下示例中，我们将执行`dir`(别名为`Get-ChildItem`) 若要获取已注册的服务器文件中列出的所有 SQL Server 实例的列表。

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse 
```

下面是一个示例的输出可能如下所示：

```powershell
Mode Name
---- ----
-    ServerA
-    ServerB
-    localhost\SQL2017
-    localhost\SQL2016Happy
-    localhost\SQL2017
```

对于涉及到一个数据库或在数据库中，对象的许多操作`Get-SqlDatabase`，可以使用 cmdlet。  如果您提供两个值`-ServerInstance`和`-Database`将检索参数，仅该一个数据库对象。  但是，如果仅指定`-ServerInstance`参数，将返回该实例上的所有数据库的完整列表。

下面是一个示例的输出将如下所示：

```
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa
master               Normal        6.00 MB  368.00 KB Simple       140 sa
model                Normal       16.00 MB    5.53 MB Full         140 sa
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa
```

此下一个示例使用`Get-SqlDatabase`cmdlet 来检索 ServerB 实例上的所有数据库的列表然后显示的网格/表 (使用`Out-GridView`cmdlet) 来选择应备份哪些数据库。  一旦用户单击"确定"按钮，将备份仅突出显示的数据库。

```powershell
Get-SqlDatabase -ServerInstance ServerB |
Out-GridView -PassThru |
Backup-SqlDatabase -CompressionOption On
```

此示例中，同样，获取列表的所有 SQL Server 实例文件中列出你已注册的服务器，然后调用`Get-SqlAgentJobHistory`自午夜，列出每个 SQL Server 实例报告每个失败的 SQL 代理作业。

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE {$_.Mode -ne 'd' } |
FOREACH {
    Get-SqlAgentJobHistory -ServerInstance  $_.Name -Since Midnight -OutcomesType Failed
}
```

在此示例中，我们将执行`dir`(别名以供`Get-ChildItem`) 来获取文件中列出你已注册的服务器，所有 SQL Server 实例的列表，然后使用`Get-SqlDatabase`cmdlet 来获取这些实例的每个数据库的列表。

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE { $_.Mode -ne 'd' } |
FOREACH {
    Get-SqlDatabase -ServerInstance $_.Name
}
```

下面是一个示例的输出将如下所示：

```
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level      
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa   
master               Normal        6.00 MB  368.00 KB Simple       140 sa   
model                Normal       16.00 MB    5.53 MB Full         140 sa   
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa   
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa   
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa   
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa   
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa   
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa   
```

## <a name="reporting-problems"></a>报告问题

如果 PowerShell 扩展中的任何问题，请参阅[故障排除文档](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md)有关诊断和报表问题的信息。

#### <a name="security-note"></a>安全说明

任何安全问题，请参阅[此处](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md#note-on-security)。

## <a name="contributing-to-the-code"></a>参与代码

请查看[开发文档](https://github.com/PowerShell/vscode-powershell/blob/master/docs/development.md)有关如何参与此扩展的详细信息 ！

## <a name="maintainers"></a>维护人员

- [Keith Hill](https://github.com/rkeithhill) - [@r_keith_hill](http://twitter.com/r_keith_hill)
- [Tyler Leonhardt](https://github.com/tylerl0706) - [@TylerLeonhardt](http://twitter.com/tylerleonhardt)
- [Rob Holt](https://github.com/rjmholt)

## <a name="license"></a>许可证

此扩展[MIT 许可证下获得许可](https://github.com/PowerShell/vscode-powershell/blob/master/LICENSE.txt)。 我们包含与此项目的版本的第三方二进制文件的详细信息，请参阅[第三方通知](https://github.com/PowerShell/vscode-powershell/blob/master/Third%20Party%20Notices.txt)文件。

## <a name="code-of-conductconduct-md"></a>[行为准则][conduct-md]

此项目已采用[Microsoft 开放源代码行为准则][conduct-code]。
有关详细信息，请参阅[代码的行为准则常见问题解答][ conduct-FAQ]或联系[ opencode@microsoft.com ] [ conduct-email]与任何其他问题或意见。

[conduct-code]: http://opensource.microsoft.com/codeofconduct/
[conduct-FAQ]: http://opensource.microsoft.com/codeofconduct/faq/
[conduct-email]: mailto:opencode@microsoft.com
[conduct-md]: https://github.com/PowerShell/vscode-powershell/blob/master/CODE_OF_CONDUCT.md
