---
title: XML 任务 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql12.dts.designer.xmltask.f1
helpviewer_keywords:
- XML [Integration Services]
- XML task [Integration Services]
ms.assetid: 9f761846-390e-46d5-9db7-858943d40849
author: janinezhang
ms.author: janinez
manager: craigg
ms.openlocfilehash: a878a61678fcad2fe15ac71d8ed7d29f24057852
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62829362"
---
# <a name="xml-task"></a>XML 任务
  XML 任务用于与 XML 数据配合使用。 使用此任务，包可以检索 XML 文档，使用可扩展样式表语言转换 (XSLT) 样式表和 XPath 表达式对文档应用运算，合并多个文档，还可以验证、比较更新的文档并将其保存到文件和变量。  
  
 此任务使 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包能够在运行时动态修改 XML 文档。 可以将 XML 任务用于下列目的：  
  
-   重新设置 XML 文档格式。 例如，XML 任务可以访问 XML 文件中的报表并动态应用 XSLT 样式表来自定义文档表示形式。  
  
-   选择 XML 文档部分。 例如，XML 任务可以访问 XML 文件中的报表并动态应用 XPath 表达式来选择 XML 文档的一部分。 此运算还可以获得并处理 XML 文档中的值。  
  
-   合并来自多个源的文档。 例如，XML 任务可从多个源下载报表并动态地将它们合并为一个综合的 XML 文档。  
  
-   验证 XML 文档并选择性地获取详细错误输出。 有关详细信息，请参阅 [Validate XML with the XML Task](xml-task.md)。  
  
 通过使用 XML 源从 XML 文档中提取值的方法，可以将 XML 数据包含在数据流中。 有关详细信息，请参阅 [XML Source](../data-flow/xml-source.md)。  
  
## <a name="xml-operations"></a>XML 操作  
 XML 任务首先执行的运算是检索特定的 XML 文档。 此运算内置于 XML 任务中并可自动执行。 检索后的 XML 文档可用作 XML 任务执行运算的数据源。  
  
 XML 的 Diff、Merge 和 Patch 运算需要两个操作数。 第一个操作数指定源 XML 文档。 第二个操作数也指定一个 XML 文档，其内容取决于运算的要求。 例如，Diff 运算要比较两个文档；因此第二个操作数指定另一个相似的 XML 文档，供源 XML 文档与之比较。  
  
 XML 任务可使用变量或文件连接管理器作为源，或将 XML 数据包含在任务属性中。  
  
 如果源是变量，则该指定的变量包含 XML 文档的路径。  
  
 如果源是文件连接管理器，则该指定的文件连接管理器提供源信息。 文件连接管理器与 XML 任务分开配置，并在 XML 任务中被引用。 文件连接管理器的连接字符串可指定 XML 文件的路径。 有关详细信息，请参阅 [File Connection Manager](../connection-manager/file-connection-manager.md)。  
  
 XML 任务可以设置为将运算结果保存到变量或文件。 如果要保存到文件中，XML 任务将使用文件连接管理器访问该文件。 您也可以将 Diff 运算生成的 Diffgram 结果保存到文件和变量中。  
  
## <a name="predefined-xml-operations"></a>预定义的 XML 运算  
 XML 任务包含一组用来处理 XML 文档的预定义运算。 下表介绍了这些运算。  
  
|操作|Description|  
|---------------|-----------------|  
|差异|比较两个 XML 文档。 Diff 运算把源 XML 文档作为基准文档，将其与另一个 XML 文档进行比较，检测二者间的差异，并将差异写入 XML Diffgram 文档。 此运算包含用来自定义比较的属性。|  
|合并|合并两个 XML 文档。 Merge 运算使用源 XML 文档作为基文档，将第二个文档的内容添加到其中。 此运算可以在基文档内指定合并位置。|  
|Patch|将 Diff 运算的输出（称为 Diffgram 文档）应用到 XML 文档，以创建包含该 Diffgram 文档内容的新的父文档。|  
|验证|根据文档类型定义 (DTD) 或 XML 架构定义 (XSD) 架构验证 XML 文档。|  
|XPath|执行 XPath 查询和计算。|  
|XSLT|对 XML 文档执行 XSL 转换。|  
  
### <a name="diff-operation"></a>Diff 运算  
 根据比较要求必须是快速还是精确，可以将 Diff 运算配置为使用不同的比较算法。 还可通过对 Diff 运算配置，使之可以根据当前比较文档的大小，自动选择使用快速比较或精确比较。  
  
 Diff 运算中包含一组用于自定义 XML 比较的选项。 下表对这些选项进行说明：  
  
|Option|Description|  
|------------|-----------------|  
|**IgnoreComments**|该值用于指定是否比较注释节点。|  
|**IgnoreNamespaces**|该值用于指定是否将元素的命名空间统一资源标识符 (URI) 与其属性名称相比较。 如果将此选项设置为 `true`，则名称部分相同但命名空间不同的两个元素将被视为相同元素。|  
|**IgnorePrefixes**|该值用于指定是否比较元素前缀和属性名称。 如果将此选项设置为 `true,`，则本地名称相同但命名空间 URI 和前缀不同的两个元素将被视为相同元素。|  
|**IgnoreXMLDeclaration**|该值用于指定是否比较 XML 声明。|  
|**IgnoreOrderOfChildElements**|该值用于指定是否比较子元素顺序。 如果将此选项设置为 `true`，则在同级组成的列表中仅位置不同的子元素将被视为相同元素。|  
|**IgnoreWhiteSpaces**|该值用于指定是否比较空格。|  
|**IgnoreProcessingInstructions**|该值用于指定是否比较处理指令。|  
|**IgnoreDTD**|该值用于指定是否忽略 DTD。|  
  
### <a name="merge-operation"></a>Merge 运算  
 使用 XPath 语句标识源文档中的合并位置时，此语句应返回一个节点。 如果该语句返回多个节点，则仅使用第一个节点。 第二个文档的内容在 XPath 查询返回的第一个节点下合并。  
  
### <a name="xpath-operation"></a>XPath 运算  
 可将 XPath 运算配置为使用不同类型的 XPath 功能。  
  
-   选择“计算”  选项，以实现各种 XPath 函数，如 sum()。  
  
-   选择 **“结点列表”** 选项，将选定节点以 XML 片段方式返回。  
  
-   选择“值”  选项，以返回所有选定节点的内部文本值，这些值串联为一个字符串。  
  
### <a name="validation-operation"></a>Validation 运算  
 可以将 Validation 运算配置为使用文档类型定义 (DTD) 架构或 XML 架构定义 (XSD) 架构。  
  
 启用 `ValidationDetails` 以获取详细错误输出。 有关详细信息，请参阅 [Validate XML with the XML Task](xml-task.md)。  
  
## <a name="xml-document-encoding"></a>XML 文档编码  
 XML 任务仅支持合并 Unicode 文档。 这表示 XML 任务只能对采用 Unicode 编码的文档应用 Merge 运算。 使用其他编码将导致 XML 任务失败。  
  
> [!NOTE]  
>  Diff 和 Patch 运算中包含一个选项，可以忽略第二操作数 XML 数据中的 XML 声明，这就使得在这些运算中能够使用具有其他编码的文档。  
  
 若要验证是否可以使用 XML 文档，请查看 XML 声明。 该声明必须显式指定 UTF-8（即 8 位 Unicode 编码）。  
  
 下面的标记显示 Unicode 8 位编码。  
  
 `<?xml version="1.0" encoding="UTF-8"?>`  
  
## <a name="custom-logging-messages-available-on-the-xml-task"></a>XML 任务可用的自定义日志记录消息  
 下表介绍了 XML 任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../performance/integration-services-ssis-logging.md)和[日志记录的自定义消息](../custom-messages-for-logging.md)。  
  
|日志项|Description|  
|---------------|-----------------|  
|`XMLOperation`|提供任务所执行的操作的相关信息|  
  
## <a name="configuration-of-the-xml-task"></a>XML 任务的配置  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可以在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请单击下列主题之一：  
  
-   [XML 任务编辑器（“常规”页）](../general-page-of-integration-services-designers-options.md)  
  
-   [Validate XML with the XML Task](xml-task.md)  
  
-   [“表达式”页](../expressions/expressions-page.md)  
  
 有关在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中如何设置属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](../set-the-properties-of-a-task-or-container.md)  
  
## <a name="programmatic-configuration-of-the-xml-task"></a>XML 任务的编程配置  
 有关以编程方式设置这些属性的详细信息，请单击以下主题：  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.XMLTask.XMLTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 [设置任务或容器的属性](../set-the-properties-of-a-task-or-container.md)  
  
## <a name="related-content"></a>相关内容  
  
-   agilebi.com 上的博客文章 [XML 目标脚本组件](http://agilebi.com/jwelch/2007/06/02/xml-destination-script-component/)  
  
-   www.codeplex.com 上的 CodePlex 示例 [处理 XML 数据包示例](http://msftisprodsamples.codeplex.com/wikipage?title=SS2008!Process%20XML%20Data%20Package%20Sample&version=10&ProjectName=msftisprodsamples)  
  
  
