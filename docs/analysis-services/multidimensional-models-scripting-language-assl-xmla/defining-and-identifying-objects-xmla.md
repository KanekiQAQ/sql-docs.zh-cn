---
title: 定义和标识对象 (XMLA) |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 48223f9718ae4eb87b0880c2f266c886ec7abbb0
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62634582"
---
# <a name="defining-and-identifying-objects-xmla"></a>定义和标识对象 (XMLA)
  在 XML for Analysis (XMLA) 命令中，将使用对象标识符和对象引用标识对象，并使用 Analysis Services 脚本语言 (ASSL) 元素 XMLA 命令定义对象。  
  
## <a name="object-identifiers"></a>对象标识符  
 对象标识的实例上定义通过对象的唯一标识符[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]。 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 创建对象时，可以显式指定对象标识符，也可以由 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例确定。 可以使用[Discover](https://docs.microsoft.com/bi-reference/xmla/xml-elements-methods-discover)方法来检索对象标识符的后续**Discover**或[Execute](https://docs.microsoft.com/bi-reference/xmla/xml-elements-methods-execute)方法调用。  
  
## <a name="object-references"></a>对象引用  
 多个 XMLA 命令，如[删除](https://docs.microsoft.com/bi-reference/xmla/xml-elements-commands/delete-element-xmla)或[进程](https://docs.microsoft.com/bi-reference/xmla/xml-elements-commands/process-element-xmla)，使用对象引用以明确的方式引用的对象。 对象引用包含要对其执行命令的对象的对象标识符以及该对象的祖先的对象标识符。 例如，分区的对象引用包含分区的对象标识符，以及该分区的父度量值组、多维数据集和数据库的对象标识符。  
  
## <a name="object-definitions"></a>对象定义  
 [创建](https://docs.microsoft.com/bi-reference/xmla/xml-elements-commands/create-element-xmla)并[Alter](https://docs.microsoft.com/bi-reference/xmla/xml-elements-commands/alter-element-xmla) XMLA 中的命令创建或更改，分别对象上[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例。 这些对象的定义由[ObjectDefinition](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/objectdefinition-element-xmla)包含 ASSL 中的元素的元素。 对象标识符可以显式指定的所有主要对象和诸多次要对象通过[ID](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/id-element-xmla)元素。 如果**ID**不使用元素，[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例提供唯一标识符，其取决于要标识的对象的命名约定。 有关如何使用详细信息**创建**并**Alter**命令定义对象，请参阅[创建和更改对象&#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/creating-and-altering-objects-xmla.md)。  
  
## <a name="see-also"></a>请参阅  
 [对象元素&#40;XMLA&#41;](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/object-element-xmla)   
 [ParentObject 元素&#40;XMLA&#41;](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/object-element-xmla)   
 [Source 元素&#40;XMLA&#41;](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/source-element-xmla)   
 [Target 元素&#40;XMLA&#41;](https://docs.microsoft.com/bi-reference/xmla/xml-elements-properties/target-element-xmla)   
 [在 Analysis Services 中使用 XMLA 开发](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md)  
  
  
