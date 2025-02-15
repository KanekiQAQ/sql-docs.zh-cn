---
title: 定义赋值和其他脚本命令 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4a96f96319c76c9c4d5a9b22a6e613ed8bf90cee
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68178828"
---
# <a name="define-assignments-and-other-script-commands"></a>定义赋值和其他脚本命令
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  在多维数据集设计器的“计算”  选项卡上，单击工具栏中的“新建脚本命令”  图标以创建空脚本。 创建新脚本时，该脚本最初以空白标题显示在“计算”选项卡的 **“脚本组织程序”** 窗格中。您在“计算表达式”窗格中键入的字符将在 **“脚本组织程序”** 中显示为项的名称。 因此，您可能要在第一行中键入被注释的名称，以更方便地标识 **“脚本组织程序”** 窗格中的脚本。 有关详细信息，请参阅 [对 Microsoft SQL Server 2005 中的 MDX 脚本编写的介绍](http://go.microsoft.com/fwlink/?LinkId=81892)。  
  
> [!IMPORTANT]  
>  当您最初切换到多维数据集设计器的 **“计算”** 选项卡时， **“脚本组织程序”** 窗格将包含具有 CALCULATE 命令的单个脚本。 CALCULATE 命令控制多维数据集中单元的聚合，仅当您要手动指定多维数据集的聚合方式时，才应编辑该命令。  
  
 可以使用“计算表达式”窗格以多维表达式 (MDX) 语法生成表达式。 生成表达式时，可以将多维数据集组件、函数和模板从 **“计算工具”** 窗格拖动或复制到“计算表达式”窗格。 这种做法可以将项的脚本添加到“计算表达式”窗格中您放置或粘贴的位置。 用适当的值替换参数及其分隔符（« 和 »）。  
  
> [!IMPORTANT]  
>  当使用“计算表达式”窗格编写包含多个语句的表达式时，请确保 MDX 脚本中除最后一行外的所有行都以分号 (;) 结尾。 将计算串联为单个 MDX 脚本并且每个脚本后都追加一个分号，以确保 MDX 脚本可以正确编译。 如果您在“计算表达式”窗格中为脚本的最后一行添加分号，多维数据集将会正确生成和部署，但不能对其运行查询。  
  
## <a name="see-also"></a>请参阅  
 [多维模型中的计算](../../analysis-services/multidimensional-models/calculations-in-multidimensional-models.md)  
  
  
