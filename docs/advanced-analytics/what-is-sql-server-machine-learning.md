---
title: SQL Server 机器学习服务 (R、Python) 概述
description: SQL Server 中的机器学习服务功能的概述, 你可以在其中将 Python 和 R 与关系数据集成, 以便进行数据科学和统计建模、机器学习模型、预测分析、数据可视化等。
ms.prod: sql
ms.technology: machine-learning
ms.date: 06/13/2019
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: ead0dd3d9ba69a4bf0079fe8065a2d5aa7a11d3e
ms.sourcegitcommit: 63c6f3758aaacb8b72462c2002282d3582460e0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/25/2019
ms.locfileid: "68495401"
---
# <a name="sql-server-machine-learning-services-r-python"></a>SQL Server 机器学习服务 (R、Python)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

机器学习服务是 SQL Server 的一项功能, 用于执行数据库中 R 和 Python 脚本。 此功能包括用于高性能预测分析和机器学习的[Microsoft R 和 Python 包](#components)。 关系数据可以在 R 和 Python 脚本中通过包含 R 和 Python 语句的存储过程、T-sql 脚本或包含 T-sql 的 R 和 Python 代码来使用。

如果以前使用[SQL Server 2016 R 服务](r/sql-server-r-services.md), SQL Server 2017 和更高版本中的机器学习服务是下一代 r 支持, 其中包含以2016提供的基本 r、RevoScaleR、MicrosoftML 和其他库的更新版本。

在 Azure SQL 数据库中，[机器学习服务（使用 R)](https://docs.microsoft.com/azure/sql-database/sql-database-machine-learning-services-overview)目前处于公共预览状态。

## <a name="bring-compute-power-to-the-data"></a>将计算能力引入数据

机器学习服务的关键价值主张是其企业R和Python软件包的强大功能，可以大规模提供高级分析，并能够将计算和处理带到数据所在的位置，从而消除了在网络上提取数据的需求。 这有多个优点:

+ 数据安全性。 使 R 和 Python 的执行更接近数据源可避免浪费或不安全的数据移动。
+ 速度。 数据库针对基于集的操作进行了优化。 数据库中的最新创新 (如内存中表) 使汇总和聚合成为闪电, 是数据科学的完美补充。
+ 易于部署和集成。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]是对许多其他数据管理任务和应用程序执行的中心操作。 通过使用数据库或报表仓库中的数据, 您可以确保机器学习解决方案使用的数据是一致的, 并且是最新的。 
+ 跨云和本地的效率。 可以依靠企业数据管道 (包括[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]和 Azure 数据工厂), 而不是在 R 或 Python 会话中处理数据。 可以通过 Power BI 或 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 轻松执行结果报告或分析。

通过恰当地组合 SQL 和 R 来执行不同的数据处理和分析任务，数据科学家和开发人员变得更加高效。

<a name="components"></a>

## <a name="components"></a>组件

SQL Server 2017 支持 R 和 Python。 下表描述了这些组件。

| 组件 | Description |
|-----------|-------------|
| SQL Server Launchpad服务 | 管理外部R和Python运行时与数据库引擎实例之间通信的服务。 |
| R 包 | [**RevoScaleR**](r/ref-r-revoscaler.md)是主库的此库中的可缩放。此库中的函数是使用最广泛的函数。 在这些库中可以找到数据转换和操作、统计摘要、可视化以及许多形式的建模和分析。 此外，这些库中的函数可自动在可用内核之间分配工作负荷以进行并行处理，并且能够处理由计算引擎协调和管理的数据块。  <br/>[**MicrosoftML (R)** ](r/ref-r-microsoftml.md)添加了机器学习算法，用于创建用于文本分析、图像分析和情绪分析的自定义模型。 <br/>[**sqlRUtils**](r/ref-r-sqlrutils.md)提供了帮助函数，用于将R脚本放入T-SQL存储过程，向数据库注册存储过程，以及从R开发环境运行存储过程。<br/>[**olapR**](r/ref-r-olapr.md)用于在 R 脚本中生成或执行 MDX 查询。|
| Microsoft R Open (MRO) | [**MRO**](https://mran.microsoft.com/open) 是 Microsoft 提供的 R 的开源分发版。其中包括包和解释器。 请始终使用安装程序安装的 MRO 版本。 |
| R 工具 | R 控制台窗口和命令提示符是 R 分发版中的标准工具。  |
| R 示例和脚本 |  开源 R 和 RevoScaleR 包中包含内置数据集，以便你可以使用预安装的数据创建和运行脚本。 |
| Python 包 | [**revoscalepy** ](python/ref-py-revoscalepy.md) 是可缩放 Python 的主要库，具有用于数据操作、转换、可视化和分析的函数。 <br/>[**microsoftml (Python)** ](python/ref-py-microsoftml.md) 添加了机器学习算法，用于为文本分析、图像分析和情绪分析创建自定义模型。  |
| Python 工具 | 内置的 Python 命令行工具，可用于临时测试和多种任务。  |
| Anaconda | Anaconda 是 Python 和必备包的开源分发版。 |
| Python 示例和脚本 | 与  R一样，Python 包含内置的数据集和脚本。  |
| R 和 Python 中的预先训练的模型 | 预先训练的模型针对特定用例创建，并由 Microsoft 的数据科学工程团队维护。 可以按原样使用预先训练的模型对文本中的积极/消极情绪进行评分，或者使用你提供的新数据输入检测图像中的特征。 这些模型在机器学习服务中运行，但不能通过 SQL Server 安装程序安装。 有关详细信息，请参阅[在 SQL Server 上安装预先训练的机器学习模型](install/sql-pretrained-models-install.md)。 |

## <a name="using-sql-mls"></a>使用 SQL MLS

开发人员和分析人员通常在本地SQL服务器实例上运行代码。 通过添加机器学习服务并启用外部脚本执行，您可以在SQL Server模式中运行R和Python代码:将脚本包装在存储过程中，将模型存储在SQL Server表中，或者在查询中组合T-SQL和R或Python函数。

脚本执行位于数据安全模型的边界内：关系数据库的权限是脚本中数据访问的基础。 运行R或Python脚本的用户不应该能够使用SQL查询中该用户无法访问的任何数据。 您需要标准数据库读取和写入权限，以及运行外部脚本的其他权限。 为关系数据编写的模型和代码包装在存储过程中，或者序列化为二进制格式并存储在表中，或者如果将原始字节流序列化为文件，则从磁盘加载。

数据库内分析最常用的方法是使用[sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)，将R或Python脚本作为输入参数传递。

经典的客户端-服务器交互是另一种方法。 在具有 IDE 的任何客户端工作站上, 你可以安装[Microsoft R Client](https://docs.microsoft.com/machine-learning-server/r-client/what-is-microsoft-r-client)或[Python 库](https://docs.microsoft.com/machine-learning-server/install/python-libraries-interpreter), 然后编写将执行 (称为*远程计算上下文*) 的代码推送到远程 SQL Server 的数据和操作。 

最后，如果您使用的是[独立服务器](r/r-server-standalone.md)和Developer Edition，则可以使用相同的库和解释器在客户端工作站上构建解决方案，然后在SQL Server机器学习服务（数据库中）上部署生产代码。 

## <a name="how-to-get-started"></a>如何开始

### <a name="step-1-install-the-software"></a>步骤 1：安装软件

+ [SQL Server 机器学习服务 (数据库内)](install/sql-machine-learning-services-windows-install.md)
 
### <a name="step-2-configure-a-development-tool"></a>步骤 2：配置开发工具

数据科学家通常在他们自己的笔记本电脑或开发工作站上使用 R 或 Python 来探索数据，并构建和调整预测模型，直到获得效果良好的预测模型。 使用 SQL Server 中的数据库内分析则不需要更改此过程。 安装完成后，可以在本地和远程 SQL Server 上运行 R 或 Python 代码。

![rsql_keyscenario2](r/media/rsql-keyscenario2.png) 

+ **使用你喜欢的 IDE**。 可以将 R 和 Python 库链接到所选的开发工具。 有关详细信息, 请参阅[设置 R 工具](r/set-up-a-data-science-client.md)和[设置 Python 工具](python/setup-python-client-tools-sql.md)。  

+ **远程或本地工作**。 数据科研人员可以像平时一样连接到 SQL Server，然后将数据取回到客户端进行本地分析。 但是, 更好的解决方案是使用**RevoScaleR**或**revoscalepy** api 将计算推送到 SQL Server 计算机, 以避免成本高昂且不安全的数据移动。

+ **SQL Server 存储过程中嵌入 R 或 Python 脚本**。 代码充分优化后，将其封装在一个存储过程中，以避免不必要的数据移动和优化数据处理任务。

### <a name="step-3-write-your-first-script"></a>步骤 3：编写您的第一个脚本

从 T-SQL 脚本中调用 R 或 Python 函数：

+ [迅驰使用 R 了解数据库内分析](tutorials/sqldev-in-database-r-for-sql-developers.md)
+ [迅驰带有 R 的端到端演练](tutorials/walkthrough-data-science-end-to-end-walkthrough.md)
+ [Python使用 T-sql 运行 Python](tutorials/run-python-using-t-sql.md)
+ [Python使用 Python 了解数据库内分析](tutorials/sqldev-in-database-python-for-sql-developers.md)

选择最适合任务的语言。 R 最适合使用 SQL 难以实现的统计计算。 对于基于集合的数据操作，可以利用 SQL Server 的强大功能来实现性能最大化。 对列进行快速计算时，可使用内存数据库引擎。

### <a name="step-4-optimize-your-solution"></a>步骤 4：优化解决方案

当模型可基于企业数据进行扩展时，数据科学家往往会与 DBA 或 SQL 开发人员合作以优化以下流程：

+ 特征工程
+ 数据引入和数据转换
+ 评分

传统上，使用 R 的数据科学家会遇到性能和规模方面的问题，使用大型数据集时尤其如此。 这是因为公共运行时实现是单线程的，并且只能容纳适合本地计算机上可用内存的那些数据集。 与 SQL Server 机器学习服务集成可提供多种功能，有助于提升性能，同时还可提供更多数据：

+ **RevoScaleR**:此 R 包包含一些最常用 R 函数的实现, 经过重新设计后可提供并行度和缩放。 此包还包括一些功能，可通过将计算推送到 SQL Server 计算机来进一步提高性能和扩展性，这些计算机通常具有更大内存和更强的计算能力。

+ **revoscalepy**。 该 Python 库实现了 RevoScaleR 中最常用的函数（例如远程计算上下文）以及许多支持分布式处理的算法。

有关性能的详细信息, 请参阅此[性能案例研究](r/performance-case-study-r-services.md)和[R 和数据优化](r/r-and-data-optimization-r-services.md)。

### <a name="step-5-deploy-and-consume"></a>步骤 5：部署和使用

在脚本或模型准备好用于生产后，数据库开发人员可以将代码或模型嵌入到存储过程中，以便可以从应用程序中调用已保存的 R 或 Python 代码。 通过 SQL Server 存储和运行 R 代码有许多好处：可以使用便捷的 SQL Server 接口，并且所有计算都在数据库中进行，从而避免不必要的数据移动。

![rsql_keyscenario1](r/media/rsql-keyscenario1.png)

+ **安全且可扩展**。 SQL Server 使用新的可扩展性体系结构，可以保护数据库引擎的安全，并隔离 R 和 Python 会话。 你还可以控制可以执行脚本的用户，并且可以指定可通过代码访问的数据库。 你可以控制分配给运行时的资源量，以防止大量计算危及整体服务器性能。

+ **调度和审计**。 在 SQL Server 中运行外部脚本作业时，可以控制和审核数据科学家使用的数据。 还可以安排包含外部 R 或 Python 脚本的作业和创作者工作流，就像安排任何其他 T-SQL 作业或存储过程一样。

要利用 SQL Server 中的资源管理和安全功能，部署过程可能包括以下任务：

+ 将代码转换为可在存储过程中以最佳方式运行的函数
+ 设置安全性并锁定特定任务使用的包
+ 启用资源管理（需要企业版）

有关详细信息, 请参阅适用于 R 和[r 包管理的](r/install-additional-r-packages-on-sql-server.md)[资源调控](r/resource-governance-for-r-services.md)SQL Server。

## <a name="version-history"></a>版本历史记录

SQL Server 2017 机器学习服务是 SQL Server 2016 R 服务的下一代，增强后包含 Python。 下表是所有产品版本的完整列表 - 从初始发布到当前发布。 

| 产品名称 | 引擎版本 | 发布日期 |
|--------------|---------|--------------|
| SQL Server 2017 机器学习服务 (数据库内) | R Server 9.2。1 <br/> Python Server 9.2 | 2017 年 10 月 |
| SQL Server 2017 Machine Learning Server (独立版) | R Server 9.2。1 <br/> Python Server 9.2 | 2017 年 10 月 |
| SQL Server 2016 R Services (数据库内) | R Server 9。1  | 2017年7月  |
| SQL Server 2016 R Server (独立版)  |  R Server 9。1 | 2017年7月 |

有关按发布排列的包版本，请参阅[升级 R 和 Python 组件](install/upgrade-r-and-python.md#version-map)中的版本地图。

## <a name="portability-and-related-products"></a>可移植性和相关产品

自定义 R 和 Python 代码的可移植性通过多个产品中内置的包分发和解释器得以实现。 SQL Server 中提供的包在其他几种 Microsoft 产品和服务中也同样提供，包括非 SQL 版本的 [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/)。 

包含我们的 R 和 Python 解释器的免费客户端包括 [Microsoft R Client](https://docs.microsoft.com/machine-learning-server/r-client/what-is-microsoft-r-client) 和 [Python 库](https://docs.microsoft.com/machine-learning-server/install/python-libraries-interpreter)。

在 Azure 中，Microsoft 的 R 和 Python 包和解释器也在 Azure 机器学习、Azure 服务（如 [HDInsight](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview)）以及 [Azure 虚拟机](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-azure-vm-on-linux) 中提供。 [Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) 具有一个配备齐全的开发工作站，其中包含多家供应商提供的工具以及 Microsoft 的库和解释器。

## <a name="see-also"></a>请参阅

[安装 SQL Server 机器学习服务](install/sql-machine-learning-services-windows-install.md)
