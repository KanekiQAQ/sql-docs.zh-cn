---
title: 管理 Azure Data Studio 中的笔记本
titleSuffix: SQL Server big data clusters
description: 了解如何管理 Azure Data Studio 中的笔记本。 这包括打开笔记本，保存它们，并更改你的大数据群集连接。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 12/06/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 5417166ea69abe726f47b6bf2adede4b937d5b00
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67958280"
---
# <a name="how-to-manage-notebooks-in-azure-data-studio"></a>如何管理 Azure Data Studio 中的笔记本

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

本文介绍如何打开和保存在 Azure 数据工作室中具有 SQL Server 2019 预览 notebook 文件。 它还演示了如何更改你的连接到 SQL Server 大数据群集。

## <a name="prerequisites"></a>系统必备

本文假定你已有想要在 Azure Data Studio 中使用的笔记本。 如果你想要创建的 notebook，请参阅[如何在 SQL Server 2019 预览版中使用笔记本](notebooks-guidance.md)。 若要在 Azure Data Studio 中使用 notebook，必须满足以下先决条件：

- [部署大数据群集](quickstart-big-data-cluster-deploy.md)。
- [SQL Server 2019 大数据工具](deploy-big-data-tools.md):
   - **Azure Data Studio**
   - **SQL Server 2019 扩展**
   - **kubectl**

## <a name="open-a-notebook"></a>打开笔记本

有几种方法打开**打开 Notebook**对话框。 可以使用文件菜单、 面板和命令面板。 以下各节介绍每种方法。

### <a name="file-menu"></a>文件菜单

选择**文件打开**从文件菜单 （在 Windows) Ctrl + O 和 Cmd + O （在 Mac)。

![选择文件打开打开打开文件对话框](./media/notebooks-how-to-manage/open-file-1.png) 

### <a name="dashboard"></a>面板

单击**打开 Notebook**在仪表板中打开文件打开对话框。

![通过选择打开 Notebook 仪表板中打开打开文件对话框](./media/notebooks-how-to-manage/open-file-2.png) 

### <a name="command-palette"></a>命令面板

使用命令**文件：打开**从命令面板中通过键入 Ctrl + Shift + P （在 Windows) 和 Cmd + Shift + P （在 Mac)。

![通过在命令面板中输入 File:Open 打开打开文件对话框](./media/notebooks-how-to-manage/open-file-3.png)

## <a name="save-a-notebook"></a>保存笔记本

当前没有保存笔记本的一种方法。 必须选择**保存**从 notebook 工具栏。

![通过笔记本工具栏中单击保存保存文件](./media/notebooks-how-to-manage/save-file-1.png)

> [!NOTE]
> 以下方法当前不保存更改为笔记本:
>
> - **文件另存**，**文件另存为...** 并**文件将保存所有**从文件菜单命令。
> - **文件：保存**在命令面板中输入的命令。

## <a name="change-the-big-data-cluster"></a>更改大数据群集

若要更改 SQL Server 大数据群集的 notebook:

1. 单击**将附加到**从 notebook 工具栏的菜单。

   ![单击附加到 notebook 工具栏中的菜单](./media/notebooks-how-to-manage/select-attach-to-1.png)

2. 单击从服务器**将附加到**菜单。

   ![选择一个服务器从附加到菜单](./media/notebooks-how-to-manage/select-attach-to-2.png)

## <a name="next-steps"></a>后续步骤

有关 Azure Data Studio 中的笔记本的详细信息，请参阅[如何在 SQL Server 2019 预览版中使用笔记本](notebooks-guidance.md)。