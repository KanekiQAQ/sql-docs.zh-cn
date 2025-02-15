---
title: 使用 XMLA 部署模型解决方案 |Microsoft Docs
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5ce0f50d158d715ddbbc689c47a012a4e92ba7c4
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68208959"
---
# <a name="deploy-model-solutions-using-xmla"></a>使用 XMLA 部署模型解决方案
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中， **“编写数据库脚本为”** 命令的 **“CREATE 到”** 选项用于创建整个 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库或其中一个构成对象的 XML 脚本。 然后，生成的脚本可以在另一台计算机上运行来重新创建 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 数据库的架构（元数据）。 脚本生成整个数据库，而且当使用脚本时，没有用于增量更新已部署对象的机制。 运行脚本并部署数据库之后，必须对新创建的数据库进行处理，然后用户才能浏览该数据库。  
  
 有关“编写数据库脚本为”  命令的详细信息，请参阅[记录和编写 Analysis Services 数据库脚本](../../analysis-services/multidimensional-models/document-and-script-an-analysis-services-database.md)。  
  
## <a name="modifying-object-properties-in-the-xml-script"></a>在 XML 脚本中修改对象属性  
 当使用“编写数据库脚本为”  命令时，不能修改数据库对象的特定属性（如数据库名称、数据源连接字符串以及安全设置）。 这些属性必须当脚本生成后在脚本中手动修改，或者当运行脚本后在已部署的数据库中修改。  
  
> [!IMPORTANT]  
>  如果在用于数据源的连接字符串中指定密码或出于模拟目的而指定密码，则此密码不包含在 XML 脚本中。 在这种情况下，由于处理时需要密码，因此必须在执行 XML 脚本之前将该密码手动添加到此脚本中，或者在执行 XML 脚本后添加该密码。  
  
## <a name="see-also"></a>请参阅  
 [使用部署向导部署模型解决方案](../../analysis-services/multidimensional-models/deploy-model-solutions-using-the-deployment-wizard.md)   
 [同步 Analysis Services 数据库](../../analysis-services/multidimensional-models/synchronize-analysis-services-databases.md)  
  
  
