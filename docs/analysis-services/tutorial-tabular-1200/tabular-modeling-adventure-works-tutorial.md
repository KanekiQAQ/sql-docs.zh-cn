---
title: Analysis Services Internet Sales 教程 （1200年兼容级别） |Microsoft Docs
ms.date: 05/08/2019
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: tutorial
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 06b3aace2320882d209e6a7ab0f67a16a6f8a6df
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "65503623"
---
# <a name="adventure-works-internet-sales-tutorial-1200"></a>Adventure Works Internet Sales 教程 (1200)
[!INCLUDE[ssas-appliesto-sql2016-later-aas](../../includes/ssas-appliesto-sql2016-later-aas.md)]

本教程提供的课程介绍如何创建在 Analysis Services 表格模型[1200年兼容级别](../tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md)通过使用[SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)，并将您的模型部署到 Analysis Services在本地服务器或在 Azure 中。  
 
如果使用的 SQL Server 2017 或 Azure Analysis Services，并且想要在 1400年兼容级别创建您的模型，请使用[表格建模 （1400年兼容级别）](../tutorial-tabular-1400/as-adventure-works-tutorial.md)。 此更新的版本使用新式获取数据功能连接并导入源数据，使用的 M 语言来配置分区，以及包含其他补充课程。

> [!IMPORTANT]
> 应在你的服务器支持的最新兼容性级别创建表格模型。 更高版本的兼容性级别模型提供改进的性能，其他功能，并将升级到将来的兼容性级别更加无缝地。
 
  
## <a name="what-you-learn"></a>学习内容   
  
-   如何在 SSDT 中创建新的表格模型项目。
  
-   如何将数据从 SQL Server 关系数据库导入表格模型项目。  
  
-   如何创建和管理模型中表之间的关系。  
  
-   如何创建和管理可帮助用户分析模型数据的计算、度量值和关键绩效指标。  
  
-   如何创建和管理透视和层次结构可帮助用户更轻松地通过提供业务和特定于应用程序的视点中浏览模型数据。  
  
-   如何创建分区将表数据分割成更小逻辑部分，可以处理独立于其他分区。  
  
-   如何通过创建角色以及用户成员来保护模型对象和数据的安全。  
  
-   如何将表格模型部署到 Analysis Services 服务器的本地或 Azure 中。  
  
## <a name="scenario"></a>应用场景  
本教程基于 Adventure Works Cycles，虚构公司。 Adventure Works 是大型的跨国制造公司生产的自行车，产品、 部件和附件的北美、 欧洲和亚洲的商业市场的公司。 总部设在华盛顿州的尔公司雇佣了 500 名工人。 此外，Adventure Works 还雇用遍布其市场群的多个区域销售团队。  
  
为了更好地支持销售和营销团队以及高级管理人员的数据分析需要，您需要创建一个用户表格模型，以便分析 AdventureWorksDW 示例数据库中的互联网销售数据。  
  
为了完成本教程和 Adventure Works 互联网销售表格模型，您必须完成一系列课程。 在每个课程是一系列任务;完成每个任务的顺序是必需的课程。 虽然在特定的课程中可能有多个任务的实现类似的结果，但如何完成每个任务的方式略有不同。 这是为了说明是通常以完成特定任务，并使用已在前面的任务中掌握的技能的多个方法。  
  
各个课程的目的是引导您完成创作基本的表格模型使用 SSDT 中包括的功能的许多在内存中模式下运行。 因为每一课都以上一课为基础，所以，您应该按顺序完成课程。 完成所有课程操作后，你已编写并部署 Adventure Works Internet 销售示例表格模型的 Analysis Services 服务器上。  
  
本教程并未提供有关以下内容的课程或信息：通过使用 SQL Server Management Studio 管理已部署的表格模型数据库，或者使用报表客户端应用程序连接到已部署的模型以浏览模型数据。  
  
## <a name="prerequisites"></a>先决条件  
若要完成本教程，需要以下先决条件：  
  
-   最新版[SSDT](../../ssdt/download-sql-server-data-tools-ssdt.md)。

-   SQL Server Management Studio 最新版本。 [获取最新版本](../../ssms/download-sql-server-management-studio-ssms.md)。 
  
-   客户端应用程序，如[Power BI Desktop](https://powerbi.microsoft.com/desktop/)或 Excel。    
  
-   包含 Adventure Works DW 示例数据库的 SQL Server 实例。 此示例数据库包括完成本教程所需的数据。 [获取最新版本](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)。  
  

-   Azure Analysis Services 或 SQL Server 2016 或更高版本的 Analysis Services 实例部署到您的模型。 [注册免费的 Azure Analysis Services 试用版](https://azure.microsoft.com/services/analysis-services/)。
  
## <a name="lessons"></a>课程  
本教程包括以下几课：  
  
|课程|学完本课的估计时间|  
|----------|------------------------------|  
|[第 1 课：创建新的表格模型项目](lesson-1-create-a-new-tabular-model-project.md)|10 分钟。|  
|[第 2 课：添加数据](lesson-2-add-data.md)|20 分钟|  
|[第 3 课：标记为日期表](lesson-3-mark-as-date-table.md)|3 分钟|  
|[第 4 课：创建关系](lesson-4-create-relationships.md)|10 分钟。|  
|[第 5 课：创建计算的列](lesson-5-create-calculated-columns.md)|15 分钟|
|[第 6 课：创建度量值](lesson-6-create-measures.md)|30 分钟|  
|[第 7 课：创建关键绩效指标](lesson-7-create-key-performance-indicators.md)|15 分钟|  
|[第 8 课：创建透视](lesson-8-create-perspectives.md)|5 分钟|  
|[第 9 课：创建层次结构](lesson-9-create-hierarchies.md)|20 分钟|  
|[第 10 课：创建分区](lesson-10-create-partitions.md)|15 分钟|  
|[第 11 课：创建角色](lesson-11-create-roles.md)|15 分钟|  
|[第 12 课：在 Excel 中分析](lesson-12-analyze-in-excel.md)|20 分钟| 
|[第 13 课：部署](lesson-13-deploy.md)|5 分钟|  
  
## <a name="supplemental-lessons"></a>补充课程  
本教程还包括补充课程。 这一节中的主题不是完成本教程所必需的，但对于更好地了解高级表格模型创作功能会很有帮助。  
  
|课程|学完本课的估计时间|  
|----------|------------------------------|  
|[通过使用行筛选器实现动态安全性](supplemental-lesson-implement-dynamic-security-by-using-row-filters.md)|30 分钟|  
|[为 Power View 报表配置报表属性](supplemental-lesson-configure-reporting-properties-for-power-view-reports.md)|30 分钟| 

  
## <a name="next-steps"></a>后续步骤  
若要开始本教程，请继续第一课：[第 1 课：创建新的表格模型项目](lesson-1-create-a-new-tabular-model-project.md)。  
  
  
  

