---
title: 开发使用 Analysis Services 脚本语言 (ASSL) |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c113e07099ed96abdb0eb5f62c8517ee422d3cc7
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68208477"
---
# <a name="developing-with-analysis-services-scripting-language-assl"></a>使用 Analysis Services 脚本语言 (ASSL) 开发
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Analysis Services 脚本语言 (ASSL) 是对 XMLA 的扩展，该语言添加了对象定义语言和命令语言，用来在服务器上直接创建和管理 Analysis Services 结构。 您可以在自定义应用程序中使用 ASSL 通过 XMLA 协议与 Analysis Services 通信。 ASSL 由两个部分组成：  
  
-   数据定义语言 (DDL)（也称为对象定义语言）定义并说明 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 实例以及该实例所包含的数据库和数据库对象。  
  
-   例如发送操作命令的命令语言**创建**， **Alter**，或**过程**，Analysis Services 的实例。 此命令语言述[XML for Analysis &#40;XMLA&#41;引用](https://docs.microsoft.com/bi-reference/xmla/xml-for-analysis-xmla-reference)。  
  
 若要查看描述 [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)] 中的多维解决方案的 ASSL，可以在项目级别使用 View Code 命令。 您还可以在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 中使用 XMLA 查询编辑器创建或编辑 ASSL 脚本。 生成的脚本可用于管理服务器上的对象或在服务器上运行命令。  
  
## <a name="see-also"></a>请参阅  
 [ASSL 对象和对象特征](../../../analysis-services/multidimensional-models/scripting-language-assl/assl-objects-and-object-characteristics.md)   
 [ASSL XML 约定](../../../analysis-services/multidimensional-models/scripting-language-assl/assl-xml-conventions.md)   
 [数据源和绑定（SSAS 多维）](../../../analysis-services/multidimensional-models/data-sources-and-bindings-ssas-multidimensional.md)  
  
  
