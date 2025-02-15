---
title: 部署 Analysis Services 表格模型从 SQL Server Data Tools |Microsoft Docs
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 09d859cf8b5c372b9588266b9210837012396ea6
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68162860"
---
# <a name="deploy-from-sql-server-data-tools"></a>从 SQL Server Data Tools 进行部署
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  使用本主题中的任务在 SSDT 中使用部署命令部署表格模型解决方案。  
  
##  <a name="bkmk_deploy"></a> 配置“部署选项”和“部署服务器”属性  
 在您部署表格模型解决方案之前，必须首先指定“部署选项”属性和“部署服务器”属性。 有关部署属性和设置的详细信息，请参阅[表格模型解决方案部署](../../analysis-services/tabular-models/tabular-model-solution-deployment-ssas-tabular.md)。  
  
#### <a name="to-configure-options-and-properties"></a>若要配置选项和属性  
  
1.  在 SSDT 中，在**解决方案资源管理器**，右键单击项目名称，然后单击**属性**。  
  
2.  在中 **\<项目名称 > 属性**对话框中**部署选项**，如果不同于默认设置指定属性设置。  
  
    > [!NOTE]  
    >  对于缓存模式下的模型，“查询模式”  始终为“内存中”  。  
  
    > [!NOTE]  
    >  不能为 DirectQuery 模式下的模型指定“模拟设置”  。  
  
3.  在“部署服务器”  中，指定“服务器”  （名称）、“版本”  、“数据库”（名称）  和“多维数据集名称”  属性设置（如果这些设置不同于默认设置），然后单击“确定”  。  
  
> [!NOTE]  
>  还可以指定“默认部署服务器”属性设置，以便您创建的任何新项目将自动部署到指定的服务器。 有关详细信息，请参阅[配置默认数据建模和部署属性](../../analysis-services/tabular-models/configure-default-data-modeling-and-deployment-properties-ssas-tabular.md)。  
  
##  <a name="bkmk_deploy_proc"></a> 部署表格模型  
  
#### <a name="to-deploy-a-tabular-model"></a>部署表格模型
  
-   在 SSDT 中，在**构建**菜单上，单击**部署\<项目名称 >** 。  
  
     “部署”  对话框将出现，并且指示在模型中包括的每个表的元数据部署和处理的状态（除非将“处理选项”属性设置为“不处理”）。 部署过程完成后，使用 SSMS 连接到 Analysis Services 实例，并验证已创建新的模型数据库对象，或使用客户端报告应用程序连接到已部署的模型。  
  
##  <a name="bkmk_deploy_status"></a> 部署状态  
 通过 **“部署”** 对话框，您可以监视“部署”操作的进度。 也可以停止部署操作。  
  
 **“状态”**  
 指示部署操作成功与否。  
  
 **详细信息**  
 列出已部署的元数据项、每个元数据项的状态并提供针对任何问题的一条消息。  
  
 **停止部署**  
 单击此选项可以暂停部署操作。 如果部署操作用时过长或出现太多错误，则此选项很有用。  
  
## <a name="see-also"></a>请参阅  
 [表格模型解决方案部署](../../analysis-services/tabular-models/tabular-model-solution-deployment-ssas-tabular.md)   
 [配置默认数据建模和部署属性](../../analysis-services/tabular-models/configure-default-data-modeling-and-deployment-properties-ssas-tabular.md)  
  
  
