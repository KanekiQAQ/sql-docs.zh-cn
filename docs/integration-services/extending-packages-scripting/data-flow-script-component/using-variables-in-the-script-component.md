---
title: 在脚本组件中使用变量 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], using variables
ms.assetid: 92d1881a-1ef1-43ae-b1ca-48d0536bdbc2
author: janinezhang
ms.author: janinez
ms.openlocfilehash: f8c55cc39659c4fac68bf21dfb2a1dc1eb101b09
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68103936"
---
# <a name="using-variables-in-the-script-component"></a>在脚本组件中使用变量

[!INCLUDE[ssis-appliesto](../../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  变量存储包及其容器、任务和事件处理程序在运行时可以使用的值。 有关详细信息，请参阅 [Integration Services (SSIS) 变量](../../../integration-services/integration-services-ssis-variables.md)。  
  
 在“脚本转换编辑器”的“脚本”页上的 ReadOnlyVariables 和 ReadWriteVariables 字段中输入逗号分隔的变量列表，可使现有变量能够由自定义脚本进行只读或读/写访问     。 请注意，变量名称区分大小写。 使用 Value 属性可以从单个变量读取数据或向其中写入数据  。 当脚本在运行时操作变量时，脚本组件将在后台处理任何所需的锁定。  
  
> [!IMPORTANT]  
>  ReadWriteVariables 集合仅供在 PostExecute 方法中尽可能优化性能并降低锁定冲突的风险   。 因此不能在处理每行数据时直接递增包变量的值， 而应递增本地变量的值，并在所有数据处理完毕后，在 PostExecute 方法中将包变量的值设置为本地变量的值  。 还可以使用 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> 属性解决此限制问题，本主题稍后会进行介绍。 但是，在处理每一行时直接写包变量将会对性能产生负面影响，并增加锁定冲突的风险。  
  
 有关“脚本转换编辑器”的“脚本”页的详细信息，请参阅[在脚本组件编辑器中配置脚本组件](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md)和[脚本转换编辑器（“脚本”页）](../../../integration-services/data-flow/transformations/script-transformation-editor-script-page.md)   。  
  
 脚本组件在 ComponentWrapper 项目项中创建一个 Variables 集合类，该集合类为每个预配置变量值包含一个强类型的取值函数属性，该属性与变量本身同名   。 此集合通过 ScriptMain 类的 Variables 属性公开   。 取值函数属性根据需要提供对变量值的只读或读/写权限。 例如，如果将名为 `MyIntegerVariable` 的整数变量添加到 ReadOnlyVariables 列表中，可在脚本中使用以下代码检索该变量的值  ：  
  
 `Dim myIntegerVariableValue As Integer = Me.Variables.MyIntegerVariable`  
  
 还可以使用 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> 属性（通过调用 `Me.VariableDispenser` 进行访问）在脚本组件中使用变量。 在这种情况下，使用的不是变量的已命名类型化取值函数属性，而是直接访问这些变量。 使用 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> 时，您必须在您自己的代码中处理锁定语义和变量值的数据类型转换。 如果要使用在设计时不可用，而是以编程方式在运行时创建的变量，则必须使用 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> 属性，而不是已命名的类型化取值函数属性。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services (SSIS) 变量](../../../integration-services/integration-services-ssis-variables.md)   
 [在包中使用变量](https://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787)  
  
  
