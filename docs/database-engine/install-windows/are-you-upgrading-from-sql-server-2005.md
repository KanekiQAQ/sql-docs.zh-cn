---
title: 是否从 SQL Server 2005、2008 或 2008R2 进行升级？ | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: ad40e66f-71fe-4ee6-9ce3-17127e7b1d7a
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 62389a315befed467497f87dd3b86c3f52171e91
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68051208"
---
# <a name="are-you-upgrading-from-sql-server-2005-2008-or-2008r2"></a>是否从 SQL Server 2005、2008 或 2008R2 进行升级？

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]
 
 对 SQL Server 的扩展支持已结束，这是需立即升级到较新版本的 SQL Server 和 Azure SQL 数据库的原因之一。 通过升级，你不仅可以维护安全性和合规性、获取突破性的性能，还可以优化你的数据平台基础结构。  
  
 有关计划和自动化升级或迁移的详细信息、指南和工具，请参阅 [SQL Server 2005 终止支持](https://www.microsoft.com/sql-server/sql-server-2005)和 [SQL Server 2008 终止支持](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)。  
  
## <a name="why-upgrade"></a>升级理由  
  
> [!IMPORTANT]  
>  对 SQL Server 2005 的延长支持已于 2016 年 4 月 12 日结束。 如果 2016 年 4 月 12 日后仍在运行 SQL Server 2005，将不会再收到安全更新。  

> [!IMPORTANT]  
>  对 SQL Server 2008 和 2008r2 的外延支持已于 2019 年 7 月 9 日停止提供。 如果 2019 年 7 月 9 日后仍在运行 SQL Server 2008 或 2008R2，将不会再收到安全更新。 有关详细信息，请查看博客[宣布推出适用于 SQL Server 2008 的新选项](https://azure.microsoft.com/blog/announcing-new-options-for-sql-server-2008-and-windows-server-2008-end-of-support/)。 若要免费获取外延支持，可以将 SQL Server 迁移到 Azure VM。 有关详细信息，请参阅[使用 Azure 获取对 SQL Server 2008 和 2008 R2 的外延支持](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)。  
  
## <a name="choose-your-upgrade-option"></a>选择升级选项  
如果要从较旧版本的 SQL Server 升级关系数据库，下面是用于 Microsoft 平台上关系存储的选项。  
  
若要查看对这些选项的更全面分析，请参阅 [PaaS 与 IaaS](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)。  
  
|关系存储选项|优势|要考虑的其他因素|  
|-------------------------------|--------------|-------------------------------|  
|**Azure 虚拟机上托管的 SQL Server**<br /><br /> 如果你需要以下内容，请考虑此选项：<br /><br /> 迁移到托管环境的好处。<br /><br /> 对操作环境的控制。<br /><br /> SQL Server 的熟悉功能集。|**可以免费获取对 SQL Server 2008 和 2008 R2 的[外延支持](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)，最长可达 3 年。** <br /><br /> 可以从虚拟机映像库快速进行部署。<br /><br /> 获得完整的 SQL Server 功能集。<br /><br /> 节约硬件和服务器软件的成本。 只需支付每小时的使用费用。|必须同时管理 SQL Server 和操作系统软件。<br /><br /> <br /><br /> 有关详细信息，请参阅 [Azure 虚拟机上的 SQL Server 概述](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/)。<br /><br /> 有关迁移的详细信息，请参阅 [将数据库迁移到 Azure VM 上的 SQL Server](https://azure.microsoft.com/documentation/articles/virtual-machines-migrate-onpremises-database/)。|  
|**Azure SQL 数据库托管实例 (PaaS)** <br /><br /> 如果想要实现较少维护的低成本解决方案，请考虑此选项。<br /><br /> 托管实例类似于 Microsoft SQL Server 数据库引擎实例，为数据库提供共享资源，并提供其他实例范围内功能。 <br /><br />托管实例支持从本地迁移数据库，所需的数据库更改极少或没有。|在同一个托管实例中利用跨数据库查询，并获取 CLR 和 SQL 作业支持。 <br /><br /> 99.995% 的可用性保证。<br /><br /> 此服务的成本不仅包括存储费用，还包括高可用性、修补和自动备份费用。|Azure SQL 数据库托管实例和 SQL Server 本地之间存在一些 Transact-SQL (T-SQL) 差异。 有关详细信息，请参阅 [Azure SQL 数据库托管实例 T-SQL 信息](/azure/sql-database/sql-database-managed-instance-transact-sql-information)。<br /><br /> 若要详细了解 SQL 数据库托管实例，请参阅 [Azure SQL 数据库托管实例概述](/azure/sql-database/sql-database-managed-instance-index)和 [Azure SQL 数据库托管实例功能](/azure/sql-database/sql-database-managed-instance)。<br /><br /> 若要详细了解如何迁移，请参阅[将 SQL Server 迁移到 Azure SQL 数据库托管实例](/azure/sql-database/sql-database-managed-instance-migrate)。|  
|**Azure SQL 数据库单一数据库或弹性池 (PaaS)** <br /><br /> 如果想要实现较少维护的低成本解决方案，请考虑此选项。<br /><br /> 如果开发人员工作效率和新解决方案快速上市时间至关重要，或必须提供外部访问权限，此选项特别适用于云设计的应用程序。 <br /><br />最常用的 SQL Server 功能可用，但不及 Azure SQL 数据库托管实例那么多。 |可以快速部署并轻松纵向扩展。<br /><br /> 99.995% 的可用性保证。<br /><br /> 可以按秒或小时支付使用量费用。 <br /><br /> 此服务的成本不仅包括存储费用，还包括高可用性、修补和自动备份费用。|Azure SQL 数据库和 SQL Server 本地之间存在一些 Transact-SQL (T-SQL) 差异。 有关详细信息，请参阅 [Azure SQL 数据库 Transact-SQL 信息](https://azure.microsoft.com/documentation/articles/sql-database-transact-sql-information/)。<br /><br /> Azure SQL 数据库还有数据库大小上限 100TB（相较于 SQL Server 的 524PB）。 有关详细信息，请参阅[适用于单一数据库的资源限制](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases)<br /><br /> 有关 SQL 数据库的详细信息，请参阅 [Azure SQL 数据库概述](https://azure.microsoft.com/services/sql-database/)和 [Azure SQL 数据库文档](/azure/sql-database/sql-database-technical-overview)。<br /><br /> 有关迁移的详细信息，请参阅 [将 SQL Server 数据库迁移到 Azure SQL 数据库](/azure/sql-database/sql-database-single-database-migrate)。|  
|**本地 SQL Server**<br /><br /> 对于任何类型的数据库应用程序（从交易系统到数据仓库），请考虑此选项。|因为你管理硬件和软件，因此你对功能和可伸缩性具有最大控制权。<br /><br /> 如果从 SQL Server 的旧实例升级，这是最相似的环境。|你必须做出最大的前期投资并进行日常管理，因为你需要购买、维护和管理你自己的硬件和软件。<br /><br /> 有关详细信息，请参阅 [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm)。|  

你还可能想要针对某些数据和应用程序考虑使用非关系或 NoSQL 解决方案。  
  
|非关系解决方案|优势|  
|------------------------------|--------------|  
|**Azure Cosmos DB**<br /><br /> 对于新式、可伸缩、移动和 Web 应用程序（使用 JSON 数据并要求结合使用功能强大的查询和事务数据处理），请考虑此选项。<br /><br /> 有关详细信息，请参阅 [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)。<br /><br /> 有关导入数据的信息，请参阅[将数据导入到 Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/import-data/)。|已为你的文档编制索引，你可以使用熟悉的 SQL 语法来查询它们。<br /><br /> 该数据库未设计架构。<br /><br /> 无需重新生成索引即可向文档添加属性。<br /><br /> 直接在数据库引擎内获取 JSON 和 JavaScript 支持。<br /><br /> 获取对地理空间数据和集成其他 Azure 服务（包括 Azure Search、HDInsight 和 Data Factory）的本机支持。<br /><br /> 获得低延迟、高性能存储并保留吞吐量级别。|  
|**Azure 表存储**<br /><br /> 请考虑此选项以使用经济高效的解决方案存储数千兆半结构化数据。<br /><br /> 有关详细信息，请参阅 [表存储](https://azure.microsoft.com/services/storage/tables/)。|无需使数据离线即可扩展你的应用和表架构。<br /><br /> 无需分片数据集即可纵向扩展。<br /><br /> 获取跨多个区域复制数据的异地冗余存储。|  
  
## <a name="plan-your-upgrade"></a>规划升级  
  
-   阅读以下 SQL Server 团队博客文章系列中有关如何规划升级 SQL Server 2005 实例的内容。 
    - 规划从 SQL Server 2005 进行有效的升级：[步骤 1/3](https://blogs.technet.com/b/dataplatforminsider/archive/2015/12/10/planning-an-efficient-upgrade-from-sql-server-2005-step-1-of-3.aspx)、[步骤 2/3](https://blogs.technet.com/b/dataplatforminsider/archive/2015/12/15/planning-an-efficient-upgrade-from-sql-server-2005-step-2-of-3.aspx)、[步骤 3/3](https://blogs.technet.com/b/dataplatforminsider/archive/2015/12/17/planning-an-efficient-upgrade-from-sql-server-2005-step-3-of-3.aspx)
- 做好应对 [SQL Server 2008 终止支持](https://www.microsoft.com/sql-server/sql-server-2008)的准备。
  
-   查看[规划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)中的要求和注意事项，包括[安装 SQL Server 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。  
  
-   阅读有关如何升级的内容。  
  
    -   查看[升级数据库引擎](../../database-engine/install-windows/upgrade-database-engine.md)一文中的可用升级方法并了解计划和测试方法。  
  
        > [!IMPORTANT]  
        >- 不能就地将 SQL Server 2005 实例升级到 SQL Server 2017 服务器。 必须先安装 SQL Server 2017 的实例，然后再将 SQL Server 2005 数据库迁移到新安装。 有关详细信息，请参阅[选择数据库引擎升级方法](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md)一文中的“新安装升级”部分。  
        >- 可以将现有的 SQL 2008 和 SQL 2008r2 升级到 SQL 2017。 有关详细信息，请参阅[支持的版本及版本升级](supported-version-and-edition-upgrades-2017.md)。 


-    有关计划和自动化升级或迁移的详细信息、指南和工具，请参阅 [SQL Server 2005 终止支持](https://www.microsoft.com/sql-server/sql-server-2005)和 [SQL Server 2008 终止支持](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)。  
  
## <a name="get-sql-server"></a>获取 SQL Server  
 若要下载评估版 SQL Server，请单击 [SQL Server 下载](https://www.microsoft.com/sql-server/sql-server-downloads)。  
  
## <a name="next-steps"></a>Next Steps  
 [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017)   
 [SQL Server 2005 终止支持](https://www.microsoft.com/sql-server/sql-server-2005)   
 [SQL Server 2008 终止支持](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)
  
  
