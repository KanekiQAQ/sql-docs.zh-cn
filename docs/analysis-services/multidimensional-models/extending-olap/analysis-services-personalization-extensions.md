---
title: Analysis Services 个性化设置扩展 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 85d4761b228a8b79e6c90ca0aa7f0c9a32738a03
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68208901"
---
# <a name="analysis-services-personalization-extensions"></a>Analysis Services 个性化设置扩展
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 个性化设置扩展是概念的实现插件体系结构的基础。 在插件体系结构中，您可以动态地开发新的多维数据集对象和功能，并可以与其他开发人员方便地共享。 在这种情况下，[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]个性化设置扩展提供可实现以下功能：  
  
-   **动态设计和部署**立即后在设计和部署[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]个性化设置扩展，用户下一步的用户会话开始时有权访问的对象和功能。  
  
-   **接口独立**而不考虑用于创建界面[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]个性化设置扩展，用户可以使用任何接口访问的对象和功能。  
  
-   **会话上下文**[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]个性化设置扩展不是现有基础结构中的永久对象，不需要重新处理该多维数据集。 在用户连接到数据库时，它们将公开并为该用户创建，并在整个用户会话期间保持可用。  
  
-   **快速分布**共享[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]个性化设置扩展与其他软件开发人员而无需了解有关在何处的详细的规范或如何找到此扩展的功能。  
  
 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 个性化设置扩展具有多种用途。 例如，您的公司的销售涉及到多种货币。 您可创建一个计算成员，以本地货币返回访问此多维数据集的人员的总销售额。 首先要此成员创建为个性化设置扩展。 然后，将此计算成员与一组用户共享。 共享后，这些用户将拥有该计算成员的直接访问，只要它们连接到服务器。 即使用户使用的接口不是用于创建该计算成员的接口，也可以访问该计算成员。  
  
 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 个性化设置扩展是对现有托管程序集体系结构的既简单又优雅的修改，在整个公开[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]<xref:Microsoft.AnalysisServices.AdomdServer>对象模型、 多维表达式 (MDX) 语法和架构行集。  
  
## <a name="logical-architecture"></a>逻辑体系结构  
 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 个性化设置扩展的体系结构以托管程序集体系结构以及以下四个基本元素为基础：  
  
 [PlugInAttribute] 自定义属性  
 当启动服务时，[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]加载所需的程序集，并确定哪个类具有<xref:Microsoft.AnalysisServices.AdomdServer.PlugInAttribute>自定义属性。  
  
> [!NOTE]  
>  [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 将定义自定义属性作为描述代码及影响运行时行为的方式。 有关详细信息，请参阅本主题中，"[的特性概述](http://go.microsoft.com/fwlink/?LinkId=82929)，"在[!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)]MSDN 上的开发人员指南。  
  
 与所有类<xref:Microsoft.AnalysisServices.AdomdServer.PlugInAttribute>自定义特性，[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]调用其默认构造函数。 调用在启动时的所有构造函数提供一个用来创建新对象的公共位置和不受任何用户活动。  
  
 除了生成创作和管理个性化设置相关信息的少量缓存，类构造函数通常会订阅 <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionOpened> 和 <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionClosing> 事件。 未能订阅这些事件可能会导致将类错误地标记为由公共语言运行时 (CLR) 垃圾收集器进行清理。  
  
 会话上下文  
 对于基于个性化设置扩展的那些对象，[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 会在客户端会话期间创建执行环境，并在此环境中动态地生成大多数对象。 与其他任何 CLR 程序集一样，此执行环境还可以访问其他函数和存储过程。 用户会话结束时，[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]删除动态创建的对象，并关闭执行环境。  
  
 Events  
 由会话事件触发对象创建**多维数据集 OpenedCubeOpened**并**多维数据集 ClosingCubeClosing**。  
  
 客户端与服务器之间的通信是通过特定事件发生的。 这些事件使客户端了解会导致生成客户端对象的情况。 客户端环境是使用两种事件动态创建的：会话事件和多维数据集事件。  
  
 会话事件与服务器对象关联。 当客户端登录到服务器，[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]创建会话，并触发<xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionOpened>事件。 客户端结束服务器上的会话时[!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]触发器<xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionClosing>事件。  
  
 多维数据集事件与连接对象关联。 连接到多维数据集会触发 <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.CubeOpened> 事件。 关闭到多维数据集的连接（通过关闭多维数据集或更改到其他多维数据集）将触发 <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.CubeClosing> 事件。  
  
 可跟踪性和错误处理  
 使用 [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)] 可跟踪所有活动。 未处理的错误会报告给 Windows 事件日志。  
  
 所有对象的创作和管理都是独立于此体系结构，是对象的开发人员的唯一责任。  
  
## <a name="infrastructure-foundations"></a>基础结构基础  
 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 个性化设置扩展基于现有组件。 下面简要列出了提供个性化设置扩展功能的增强功能和改进功能。  
  
### <a name="assemblies"></a>程序集  
 可以将自定义属性 <xref:Microsoft.AnalysisServices.AdomdServer.PlugInAttribute> 添加到自定义程序集，用以标识 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 个性化设置扩展类。  
  
### <a name="changes-to-the-adomdserver-object-model"></a>对 AdomdServer 对象模型的更改  
 <xref:Microsoft.AnalysisServices.AdomdServer> 对象模型中的下列对象已得到增强或已添加到模型。  
  
#### <a name="new-adomdconnection-class"></a>新增 AdomdConnection 类  
 <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection> 类是新增的，并通过属性和事件公开多个性化设置扩展。  
  
 **属性**  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.SessionID%2A>，只读字符串值，表示当前连接的会话 ID。  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.ClientCulture%2A>，对与当前会话关联的客户端区域性的只读引用。  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.User%2A>，对表示当前用户的标识接口的只读引用。  
  
 **事件**  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.CubeOpened>  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.CubeClosing>  
  
#### <a name="new-properties-in-the-context-class"></a>上下文类中的新增属性  
 <xref:Microsoft.AnalysisServices.AdomdServer.Context> 类具有两个新增属性：  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Context.Server%2A>，对新增服务器对象的只读引用。  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Context.CurrentConnection%2A>，对新增 <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection> 对象的只读引用。  
  
#### <a name="new-server-class"></a>新增服务器类  
 <xref:Microsoft.AnalysisServices.AdomdServer.Server> 类是新增的，并通过类属性和事件公开多个性化设置扩展。  
  
 **属性**  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Server.Name%2A>，表示服务器名称的只读字符串值。  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Server.Culture%2A>，对与服务器关联的全局区域性的只读引用。  
  
 **事件**  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionOpened>  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionClosing>  
  
#### <a name="adomdcommand-class"></a>AdomdCommand 类  
 <xref:Microsoft.AnalysisServices.AdomdServer.AdomdCommand> 类现在支持以下 MDX 命令：  
  
-   [CREATE MEMBER 语句 (MDX)](../../../mdx/mdx-data-definition-create-member.md)  
  
-   [UPDATE MEMBER 语句&#40;MDX&#41;](../../../mdx/mdx-data-definition-update-member.md)  
  
-   [DROP MEMBER 语句&#40;MDX&#41;](../../../mdx/mdx-data-definition-drop-member.md)  
  
-   [CREATE SET 语句 (MDX)](../../../mdx/mdx-data-definition-create-set.md)  
  
-   [DROP SET 语句&#40;MDX&#41;](../../../mdx/mdx-data-definition-drop-set.md)  
  
-   [CREATE KPI 语句&#40;MDX&#41;](../../../mdx/mdx-data-definition-create-kpi.md)  
  
-   [DROP KPI 语句&#40;MDX&#41;](../../../mdx/mdx-data-definition-drop-kpi.md)  
  
### <a name="mdx-extensions-and-enhancements"></a>MDX 扩展和增强功能  
 CREATE MEMBER 命令进行了增强与**标题**属性， **display_folder**属性，并且**associated_measure_group**属性。  
  
 添加 UPDATE MEMBER 命令是为了在求解计算丢失优先顺序时需要更新的情况下避免重新创建成员。 更新不能更改计算成员的作用域将计算的成员移动到不同的父级，也无法定义其他**solveorder**。  
  
 CREATE SET 命令增强**标题**属性， **display_folder**属性，并且新**静态 |动态**关键字。 *静态*意味着仅在创建时计算集。 *动态*意味着每次在查询中使用一组计算集。 默认值是**静态**如果省略了关键字。  
  
 在 MDX 语法中增加了 CREATE KPI 和 DROP KPI 命令。 可以从任意 MDX 脚本动态创建 KPI。  
  
### <a name="schema-rowsets-extensions"></a>架构行集扩展  
 在 MDSCHEMA_MEMBERS 上*作用域*添加列。 作用域值如下所示：MDMEMBER_SCOPE_GLOBAL = 1、 MDMEMBER_SCOPE_SESSION = 2。  
  
 在 MDSCHEMA_SETS 上*set_evaluation_context*添加列。 将计算上下文值如下所示：MDSET_RESOLUTION_STATIC = 1, MDSET_RESOLUTION_DYNAMIC = 2.  
  
 在 MDSCHEMA_KPIS 上，增加了作用域列。 作用域值如下所示：MDKPI_SCOPE_GLOBAL = 1、 MDKPI_SCOPE_SESSION = 2。  
  
  
