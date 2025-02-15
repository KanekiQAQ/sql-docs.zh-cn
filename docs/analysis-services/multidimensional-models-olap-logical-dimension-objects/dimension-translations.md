---
title: 维度翻译 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b58303abfd94710452143e014fee0019826a2a97
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68180768"
---
# <a name="dimension-translations"></a>维度翻译
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  翻译是将显示的标签和标题从一种语言更改为另一种语言的简单机制。 每个翻译都被定义为一对值：带已翻译文本的字符串和带语言 ID 的数字。 翻译可用于 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 中的所有对象。 还可以翻译维度的属性值。 客户端应用程序不但要负责查找用户定义的语言设置，还要将所有标题和标签都切换为以该语言显示。 根据您的需要，一个对象可有多种翻译。  
  
 一个简单的 <xref:Microsoft.AnalysisServices.Translation> 对象由语言 ID 号和翻译后的标题组成。 语言 ID 号**整数**语言 id。 翻译后的标题是已翻译的文本。  
  
 在中[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]，维度翻译是维度的名称的名称的特定于语言的表示形式[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]对象或其成员，如标题、 成员或层次结构级别之一。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 此外支持多维数据集对象的翻译。  
  
 翻译为可支持多种语言的客户端应用程序提供了服务器支持。 通常，来自不同国家/地区的用户都会查看多维数据集及其维度。 将多维数据集及其维度的各种元素翻译为其他语言很有用，因为这样这些用户才能查看并了解多维数据集。 例如，法国的业务用户可以从使用法语区域设置，工作站访问多维数据集并查看以法语的对象属性值。 但是，德国的业务用户如果通过采用了德语区域设置的工作站来访问相同的多维数据集，则会看到以德语显示的同一对象属性值。  
  
 客户端计算机的排序规则和语言信息以区域设置标识符 (LCID) 的形式进行存储。 客户端通过连接将 LCID 传递给 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例。 实例使用 LCID 来确定在为 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 对象提供元数据时所用的翻译集。 如果 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 对象未包含指定的翻译，则使用默认语言将内容返回给客户端。  
  
## <a name="see-also"></a>请参阅  
 [多维数据集翻译](../../analysis-services/multidimensional-models-olap-logical-cube-objects/cube-translations.md)   
 [Analysis Services 中的翻译支持](../../analysis-services/translation-support-in-analysis-services.md)   
 [全球化提示和最佳实践 (Analysis Services)](../../analysis-services/globalization-tips-and-best-practices-analysis-services.md)  
  
  
