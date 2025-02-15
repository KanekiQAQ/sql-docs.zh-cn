---
title: 下载并安装 sqlpackage |Microsoft Docs
description: 下载并安装适用于 Windows、macOS 或 Linux 的 sqlpackage
ms.custom: tools|sos
ms.date: 06/20/2018
ms.prod: sql
ms.reviewer: alayu; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: pensivebrian
ms.author: broneill
ms.openlocfilehash: 406fb50ceaba177d02bf8d79d0c37191dbe178f8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67986255"
---
# <a name="download-and-install-sqlpackage"></a>下载并安装 sqlpackage

sqlpackage 在 Windows、macOS 和 Linux 上运行。

下载并安装最新的 .NET Framework 版本以及 macOS 和 Linux 预览版：

|平台|下载|发布日期|版本|生成
|:---|:---|:---|:---|:---|
|Windows|[MSI 安装程序](https://go.microsoft.com/fwlink/?linkid=2087429)|2019 年 4 月 15 日|18.2|15.0.4384.2|
|macOS .NET Core（预览版）|[zip 文件](https://go.microsoft.com/fwlink/?linkid=2087247)|2019 年 4 月 15 日 | 18.2 |15.0.4384.2|
|Linux .NET Core（预览版）|[zip 文件](https://go.microsoft.com/fwlink/?linkid=2087431)|2019 年 4 月 15 日 | 18.2 |15.0.4384.2|

有关最新版本的详细信息，请参阅[发行说明](release-notes-sqlpackage.md)。

[!INCLUDE[Freshness](../includes/paragraph-content/fresh-note-steps-feedback.md)]

## <a name="get-sqlpackage-for-windows"></a>获取适用于 Windows 的 sqlpackage

此版本的 sqlpackage 包括标准 Windows 安装程序体验和 .zip： 

1. 下载并运行[适用于 Windows 的 DacFramework.msi 安装程序](https://go.microsoft.com/fwlink/?linkid=2087429)。
2. 打开一个新的命令提示符窗口，并运行 sqlpackage.exe
    - sqlpackage 会安装到 ```C:\Program Files\Microsoft SQL Server\150\DAC\bin``` 文件夹
    - 在 x64 计算机上安装 x86 版本时，sqlpackage 会安装到 ```C:\Program Files (x86)\Microsoft SQL Server\150\DAC\bin``` 文件夹

## <a name="get-sqlpackage-preview-for-macos"></a>获取适用于 macOS 的 sqlpackage（预览版）

1. 下载[适用于 macOS 的 sqlpackage](https://go.microsoft.com/fwlink/?linkid=2087247)。
2. 若要提取文件并启动 sqlpackage，请打开一个新的终端窗口并键入以下命令：

   **.zip 安装：**

   ```bash
   mkdir sqlpackage
   unzip ~/Downloads/sqlpackage-osx-<version string>.zip ~/sqlpackage 
   echo 'export PATH="$PATH:~/sqlpackage"' >> ~/.bash_profile
   source ~/.bash_profile
   sqlpackage
   ```

## <a name="get-sqlpackage-preview-for-linux"></a>获取适用于 Linux 的 sqlpackage（预览版）

1. 通过使用安装程序之一或 tar.gz 存档来下载[适用于 Linux 的 sqlpackage](https://go.microsoft.com/fwlink/?linkid=2087431)：
2. 若要提取文件并启动 sqlpackage，请打开一个新的终端窗口并键入以下命令：

   **.zip 安装：**

   ```bash
   cd ~
   mkdir sqlpackage
   unzip ~/Downloads/sqlpackage-linux-<version string>.zip -d ~/sqlpackage 
   echo "export PATH=\"\$PATH:$HOME/sqlpackage\"" >> ~/.bashrc
   chmod a+x ~/sqlpackage/sqlpackage
   source ~/.bashrc
   sqlpackage
   ```

   > [!NOTE]
   > 在 Debian、Redhat 和 Ubuntu 上，可能缺少依赖项。 使用以下命令安装这些依赖项，具体取决于你的 Linux 版本：

   **Debian：**

   ```bash
   sudo apt-get install libunwind8
   ```

   **Redhat：**

   ```bash
   yum install libunwind
   yum install libicu
   ```

   **Ubuntu：**

   ```bash
   sudo apt-get install libunwind8

   # install the libicu library based on the Ubuntu version
   sudo apt-get install libicu52      # for 14.x
   sudo apt-get install libicu55      # for 16.x
   sudo apt-get install libicu57      # for 17.x
   sudo apt-get install libicu60      # for 18.x
   ```

## <a name="uninstall-sqlpackage-preview"></a>安装 sqlpackage（预览版）

如果使用 Windows 安装程序安装了 sqlpackage，则按照删除任何 Windows 应用程序的方式进行卸载。

如果使用 .zip 或其他存档安装了 sqlpackage，则只需删除文件。

## <a name="supported-operating-systems"></a>支持的操作系统

sqlpackage 在 Windows、macOS 和 Linux 上运行，并在以下平台上受支持：

### <a name="windows"></a>Windows

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7 SP1
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

### <a name="macos"></a>macOS

- macOS 10.13 High Sierra
- macOS 10.12 Sierra

### <a name="linux-x64"></a>Linux (x64)

- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.3
- SUSE Linux Enterprise Server v12 SP2
- Ubuntu 16.04

## <a name="next-steps"></a>Next Steps

- 了解有关 [sqlpackage](sqlpackage.md) 的详细信息

[Microsoft 隐私声明](https://go.microsoft.com/fwlink/?LinkId=521839)
