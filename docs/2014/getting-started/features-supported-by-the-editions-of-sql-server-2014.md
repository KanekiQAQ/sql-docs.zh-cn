---
title: SQL Server 2014 的版本支持的功能 |Microsoft Docs
ms.custom: ''
ms.date: 05/24/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: 5da61ff5-12b9-48e6-b3c8-0dacca1751c4
author: mightypen
ms.author: genemi
manager: craigg
ms.openlocfilehash: f23c3ff4d5bf55609e1dab2462b19a5fa273986f
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "62843486"
---
# <a name="features-supported-by-the-editions-of-sql-server-2014"></a>SQL Server 2014 各个版本支持的功能


  本主题提供有关不同版本的 [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]所支持的功能的详细信息。 

 > **注意：** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]是评估版具有 180 天试用期。 有关详细信息，请参阅 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [试用软件网站](https://go.microsoft.com/fwlink/?LinkId=190955)。  
> 
> **注意**：有关 Evaluation 和 Developer 版本支持的功能，请参阅[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]Enterprise 功能集。  
  
 若要导航到某种 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 技术相应的表，请单击其链接：  
  
 [转换箱规模限制](#CrossBoxScale)  
  
 [高可用性](#High_availability)  
  
 [可伸缩性和性能](#Scalability)  
  
 [安全性](#Enterprise_security)  
  
 [复制](#Replication)  
  
 [管理工具](#Mgmt_Tools)  
  
 [RDBMS 可管理性](#RDBMS_mgmt)  
  
 [开发工具](#Dev_tools)  
  
 [可编程性](#Programmability)  
  
 [Integration Services](#SSIS)  
  
 [Integration Services-高级适配器](#SSIS_AA)  
  
 [Integration Services-高级转换](#SSIS_AT)  
  
 [Master Data Services](#MDS)  
  
 [数据仓库](#Data_warehouse)  
  
 [Analysis Services](#SSAS)  
  
 [BI 语义模型 （多维）](#BISemModel_multi)  
  
 [BI 语义模型（表格）](#BISemModel_tabular)  
  
 [PowerPivot for SharePoint](#PowerPivot)  
  
 [数据挖掘](#DataMining)  
  
 [Reporting Services](#Reporting)  
  
 [Business Intelligence 客户端](#BIClients)  
  
 [空间和位置服务](#Spatial)  
  
 [其他数据库服务](#Add_DBServices)  
  
 [其他组件](#Other_Components)  
  
##  <a name="CrossBoxScale"></a>转换箱规模限制  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|单个实例使用的最大计算能力 ([!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]数据库引擎)<sup>1</sup>|操作系统支持的最大值|限制为 4 个插槽或 16 核，取二者中的较小值|限制为 4 个插槽或 16 核，取二者中的较小值|限制为 4 个插槽或 16 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|  
|使用单个实例 (Analysis Services、 Reporting Services) 的最大计算能力<sup>1</sup>|操作系统支持的最大值|操作系统支持的最大值|限制为 4 个插槽或 16 核，取二者中的较小值|限制为 4 个插槽或 16 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|限制为 1 个插槽或 4 核，取二者中的较小值|  
|利用的最大内存（每个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库引擎实例）|操作系统支持的最大值|128 GB|128 GB|64 GB|1 GB|1 GB|1 GB|  
|利用的最大内存（每个 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]实例）|操作系统支持的最大值|操作系统支持的最大值|64 GB|不可用|不可用|不可用|不可用|  
|利用的最大内存（每个 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]实例）|操作系统支持的最大值|操作系统支持的最大值|64 GB|64 GB|4 GB|不可用|不可用|  
|最大关系数据库大小|524 PB|524 PB|524 PB|524 PB|10 GB|10 GB|10 GB|  
  
 <sup>1</sup> Enterprise Edition 配合服务器 + 客户端访问许可证 (CAL) 基于许可 （对新协议不可用），限制为最多 20 个内核，每个[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]实例。 基于内核的服务器许可模型没有限制。 有关详细信息，请参阅 [Compute Capacity Limits by Edition of SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md)。  
  
##  <a name="High_availability"></a> 高可用性  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|Server Core 支持<sup>1</sup>|是|是|是|是|是|是|是|  
|日志传送|是|是|是|是||||  
|数据库镜像|是|支持（仅支持“完全”安全级别）|支持（仅支持“完全”安全级别）|仅见证服务器|仅见证服务器|仅见证服务器|仅见证服务器|  
|备份压缩|是|是|是|||||  
|数据库快照|是|||||||  
|AlwaysOn 故障转移群集实例|是 (节点支持：操作系统支持的最大值|是 (节点支持：2)|是 (节点支持：2)|||||  
|AlwaysOn 可用性组|支持（最多 8 个辅助副本，包括 2 个同步辅助副本）|||||||  
|连接控制器|是|||||||  
|联机页面和文件还原|是|||||||  
|联机索引|是|||||||  
|联机架构更改|是|||||||  
|快速恢复|是|||||||  
|镜像备份|是|||||||  
|热添加内存和 CPU<sup>2</sup>|是|||||||  
|数据库恢复顾问|是|是|是|是|是|是|是|  
|加密备份|是|是|是|||||  
|智能备份|是|是|是|否||||  
  
 <sup>1</sup>有关安装的详细信息[!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]Server core，请参阅[Server Core 上安装 SQL Server 2014](../database-engine/install-windows/install-sql-server-on-server-core.md)。  
  
 <sup>2</sup>此功能功能仅适用于 64 位[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。  
  
##  <a name="Scalability"></a> 可伸缩性和性能  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|多实例支持|50|50|50|50|50|50|50|  
|表和索引分区|是|||||||  
|数据压缩|是|||||||  
|Resource Governor|是|||||||  
|分区表并行度|是|||||||  
|多个 Filestream 容器|是|||||||  
|可识别 NUMA 的大型页内存和缓冲区数组分配|是|||||||  
|缓冲池扩展<sup>1</sup>|是|是|是|||||  
|IO 资源调控|是|||||||  
|内存中 OLTP <sup>1</sup>|是|||||||  
|延迟持续性|是|是|是|是|是|是|是|  
  
 <sup>1</sup>此功能功能仅适用于 64 位[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。  
  
##  <a name="Enterprise_security"></a> 安全性  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|基本审核|是|是|是|是|是|是|是|  
|精细审核|是|||||||  
|透明数据库加密|是|||||||  
|可扩展的密钥管理|是|||||||  
|用户定义的角色|是|是|是|是|是|是|是|  
|包含的数据库|是|是|是|是|是|是|是|  
|备份加密|是|是|是|||||  
  
##  <a name="Replication"></a> Replication  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 更改跟踪|是|是|是|是|是|是|是|  
|合并复制|是|是|是|支持（仅订阅服务器）|支持（仅订阅服务器）|支持（仅订阅服务器）|支持（仅订阅服务器）|  
|事务复制|是|是|是|支持（仅订阅服务器）|支持（仅订阅服务器）|支持（仅订阅服务器）|支持（仅订阅服务器）|  
|快照复制|是|是|是|支持（仅订阅服务器）|支持（仅订阅服务器）|支持（仅订阅服务器）|支持（仅订阅服务器）|  
|异类订阅服务器|是|是|是|||||  
|Oracle 发布|是|||||||  
|对等事务复制|是|||||||  
  
##  <a name="Mgmt_Tools"></a> Management Tools  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|SQL 管理对象 (SMO)|是|是|是|是|是|是|是|  
|SQL 配置管理器|是|是|是|是|是|是|是|  
|SQL CMD（命令提示工具）|是|是|是|是|是|是|是|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Studio|是|是|是|是|是|是||  
|Distributed Replay - 管理工具|是|是|是|是|是|是||  
|分布式重播 - 客户端|是|否|是|是||||  
|分布式重播 - 控制器|支持（Enterprise 版支持最多 16 个客户端、Developer 版仅支持 1 个客户端）|否|支持（仅支持 1 个客户端）|支持（仅支持 1 个客户端）||||  
|SQL Profiler|是|是|是|不<sup>2</sup>|不<sup>2</sup>|不<sup>2</sup>|不<sup>2</sup>|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理|是|是|是|是||||  
|Microsoft System Center Operations Manager 管理包|是|是|是|是||||  
|数据库优化顾问 (DTA)|是|是|是<sup>3</sup>|是<sup>3</sup>||||  
|将 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 数据库部署到 Windows Azure VM 向导|是|是|是|是|是|是|是|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Windows Azure 中的数据文件|是|是|是|是|是|是|是|  
  
 <sup>2</sup> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] web [!INCLUDE[ssExpress](../includes/ssexpress-md.md)]， [!INCLUDE[ssExpress](../includes/ssexpress-md.md)] with Tools 和[!INCLUDE[ssExpress](../includes/ssexpress-md.md)]具有高级服务可以使用探查[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]标准和[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]Enterprise edition。  
  
 <sup>3</sup>仅对 Standard 版本功能启用优化。  
  
##  <a name="RDBMS_mgmt"></a> RDBMS Manageability  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|用户实例|||||是|是|是|  
|LocalDB|||||是|是||  
|专用管理连接|是|是|是|是|支持（受跟踪标志控制）|支持（受跟踪标志控制）|支持（受跟踪标志控制）|  
|PowerShell 脚本支持|是|是|是|是|是|是|是|  
|SysPrep 支持<sup>1</sup>|是|是|是|是|是|是|是|  
|支持数据层应用程序组件操作-提取、 部署、 升级和删除|是|是|是|是|是|是|是|  
|策略自动执行（检查计划和更改）|是|是|是|是||||  
|性能数据收集器|是|是|是|是||||  
|能够作为多实例管理中的托管实例注册|是|是|是|是||||  
|标准性能报表|是|是|是|是||||  
|计划指南和计划指南的计划冻结|是|是|是|是||||  
|使用 NOEXPAND 提示的索引视图的直接查询|是|是|是|是||||  
|自动的索引视图维护|是|是|是|是||||  
|分布式分区视图|是|部分支持。 不能更新分布式分区视图|部分支持。 不能更新分布式分区视图|部分支持。 不能更新分布式分区视图|部分支持。 不能更新分布式分区视图|部分支持。 不能更新分布式分区视图|部分支持。 不能更新分布式分区视图|  
|并行索引操作|是|||||||  
|查询优化器自动使用索引视图|是|||||||  
|并行一致性检查|是|||||||  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实用工具控制点|是|||||||  
|包含的数据库|是|是|是|是|是|是|是|  
|缓冲池扩展<sup>2</sup>|是|是|是|||||  
  
 <sup>1</sup> 有关详细信息，请参阅 [使用 SysPrep 安装 SQL Server 的注意事项](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md)。  
  
 <sup>2</sup>此功能功能仅适用于 64 位[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。  
  
##  <a name="Dev_tools"></a> Development Tools  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|[!INCLUDE[msCoName](../includes/msconame-md.md)] Visual Studio 集成|是|是|是|是|是|是|是|  
|Intellisense（[!INCLUDE[tsql](../includes/tsql-md.md)] 和 MDX）|是|是|是|是|是|是|是|  
|[!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]|是|是|是|是|是|||  
|SQL 查询编辑和设计工具<sup>1</sup>|是|是|是|||||  
|版本控制支持<sup>1</sup>|是|是|是|||||  
|MDX 编辑、 调试和设计工具<sup>1</sup>|是|是|是|||||  
  
 <sup>1</sup>此功能不适用于 Standard 版的 64 位版本。  
  
##  <a name="Programmability"></a> Programmability  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|公共语言运行时 (CLR) 集成|是|是|是|是|是|是|是|  
|本机 XML 支持|是|是|是|是|是|是|是|  
|XML 索引|是|是|是|是|是|是|是|  
|MERGE 和 UPSERT 功能|是|是|是|是|是|是|是|  
|FILESTREAM 支持|是|是|是|是|是|是|是|  
|FileTable|是|是|是|是|是|是|是|  
|日期和时间数据类型|是|是|是|是|是|是|是|  
|国际化支持|是|是|是|是|是|是|是|  
|全文和语义搜索|是|是|是|是|是|||  
|查询中的语言规范|是|是|是|是|是|||  
|Service Broker（消息传递）|是|是|是|不支持（仅客户端）|不支持（仅客户端）|不支持（仅客户端）|不支持（仅客户端）|  
|[!INCLUDE[tsql](../includes/tsql-md.md)] 端点|是|是|是|是||||  
  
##  <a name="SSIS"></a> Integration Services  
  
|功能|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|-------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 导入和导出向导|是|是|是|是|是|是|是|  
|内置数据源连接器|是|是|是|是|是|是|是|  
|SSIS 设计器和运行时|是|是|是|||||  
|基本转换|是|是|是|||||  
|基本数据探查工具|是|是|是|||||  
|Change Data Capture Service for Oracle by Attunity|是|||||||  
|Change Data Capture Designer for Oracle by Attunity|是|||||||  
  
###  <a name="SSIS_AA"></a> Integration Services - 高级适配器  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|高性能 Oracle 目标|是|||||||  
|高性能 Teradata 目标|是|||||||  
|SAP BW 源和目标|是|||||||  
|数据挖掘模型定型目标适配器|是|||||||  
|维度处理目标适配器|是|||||||  
|分区处理目标适配器|是|||||||  
|Attunity 提供的变更数据捕获组件|是|||||||  
|Attunity 提供的开放式数据库连接 (ODBC)|是|||||||  
  
###  <a name="SSIS_AT"></a> Integration Services - 高级转换  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|持续性（高性能）查找|是|||||||  
|数据挖掘查询转换|是|||||||  
|模糊分组和查找转换|是|||||||  
|术语提取和查找转换|是|||||||  
  
##  <a name="MDS"></a> Master Data Services  
  
> [!NOTE]  
>  -   [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 仅适用于 Business Intelligence 版和 Enterprise 版的 64 位版本。  
  
|功能|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|-------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] 数据库|是|是||||||  
|[!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Web 应用程序|是|是||||||  
  
##  <a name="Data_warehouse"></a> Data Warehouse  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|在无需数据库的情况下创建多维数据集|是|是|是|||||  
|自动生成临时和数据仓库架构|是|是|是|||||  
|变更数据捕获|是|||||||  
|星型联接查询优化|是|||||||  
|可扩展的只读 Analysis Services 配置|是|||||||  
|针对已分区表和索引的并行查询处理|是|||||||  
|xVelocity 内存优化的列存储索引|是|||||||  
|全局批处理集成|是|||||||  
  
##  <a name="SSAS"></a> Analysis Services  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|可扩展的共享数据库（附加/分离，只读数据库）|是|是||||||  
|备份/还原、附加/分离数据库|是|是|是|||||  
|同步数据库|是|是||||||  
|高可用性|是|是|是|||||  
|可编程性（AMO、ADOMD.Net、OLEDB、XML/A、ASSL）|是|是|是|||||  
  
###  <a name="BISemModel_multi"></a> BI 语义模型 （多维）  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|半累加性度量值|是|是|不<sup>1</sup>|||||  
|层次结构|是|是|是|||||  
|KPI|是|是|是|||||  
|透视|是|是||||||  
|操作|是|是|是|||||  
|帐户智能|是|是|是|||||  
|时间智能|是|是|是|||||  
|自定义汇总|是|是|是|||||  
|写回多维数据集|是|是|是|||||  
|写回维度|是|是||||||  
|写回单元|是|是|是|||||  
|钻取|是|是|是|||||  
|高级层次结构类型（父子、不规则层次结构）|是|是|是|||||  
|高级维度（引用维度、多对多维度）|是|是|是|||||  
|链接度量值和维度|是|是||||||  
|翻译|是|是|是|||||  
|Aggregations|是|是|是|||||  
|多个分区|是|是|支持，最多 3 个|||||  
|主动缓存|是|是||||||  
|自定义程序集（存储过程）|是|是|是|||||  
|MDX 查询和脚本|是|是|是|||||  
|DAX 查询|是|是||||||  
|基于角色的安全模型|是|是|是|||||  
|维度和单元级别的安全性|是|是|是|||||  
|可扩展字符串存储|是|是|是|||||  
|MOLAP、ROLAP、HOLAP 存储模式|是|是|是|||||  
|二进制和压缩的 XML 传输|是|是|是|||||  
|推送模式处理|是|是||||||  
|直接写回|是|是||||||  
|度量值表达式|是|是||||||  
  
 <sup>1</sup>standard edition 支持 LastChild 半累加性度量值，但其他半累加性度量值，例如 None、 FirstChild、 FirstNonEmpty、 LastNonEmpty、 AverageOfChildren 和 ByAccount。 在所有版本上都支持累加性度量值（如 Sum、Count、Min 和 Max）和非累加性度量值 (DistinctCount)。  
  
###  <a name="BISemModel_tabular"></a> BI Semantic Model (Tabular)  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|层次结构|是|是||||||  
|KPI|是|是||||||  
|透视|是|是||||||  
|翻译|是|是||||||  
|DAX 计算、DAX 查询、MDX 查询|是|是||||||  
|行级安全性|是|是||||||  
|“度量值组”|是|是||||||  
|内存中和 DirectQuery 存储模式（仅限表格）|是|是||||||  
  
###  <a name="PowerPivot"></a> PowerPivot for SharePoint  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|基于共享服务体系结构的 SharePoint 场集成|是|是||||||  
|用量报告|是|是||||||  
|运行状况监视规则|是|是||||||  
|PowerPivot 库|是|是||||||  
|PowerPivot 数据刷新|是|是||||||  
|PowerPivot 数据馈送|是|是||||||  
  
###  <a name="DataMining"></a> 数据挖掘  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|标准算法|是|是|是|||||  
|数据挖掘工具（向导、编辑器、查询生成器）|是|是|是|||||  
|交叉验证|是|是||||||  
|挖掘结构数据的筛选子集的模型|是|是||||||  
|时序：自定义混和 ARTXP 和 ARIMA 方法之间|是|是||||||  
|时序：使用新数据的预测|是|是||||||  
|无限制并发数据挖掘查询|是|是||||||  
|高级的配置和优化数据挖掘算法的选项|是|是||||||  
|支持插件算法|是|是||||||  
|并行模型处理|是|是||||||  
|时序：跨序列预测|是|是||||||  
|关联规则的无限制属性|是|是||||||  
|序列预测|是|是||||||  
|Naïve Bayes、神经网络和逻辑回归的多个预测目标|是|是||||||  
  
##  <a name="Reporting"></a> Reporting Services  
  
###  <a name="Reporting_features"></a> Reporting Services 功能  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|支持的目录 DB [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本|Standard 或更高版本|Standard 或更高版本|Standard 或更高版本|Web|Express|||  
|支持的数据源 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本|所有   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本|所有 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本|所有 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本|Web|Express|||  
|报表服务器|是|是|是|是|是|||  
|报表设计器|是|是|是|是|是|||  
|报表管理器|是|是|是|是|是|||  
|基于角色的安全性|是|是|是|是|是|||  
|Word 导出和 RTF 格式支持|是|是|是|是|是|||  
|增强的仪表和图表|是|是|是|是|是|||  
|导出到 Excel、PDF 和图像|是|是|是|是|是|||  
|自定义身份验证|是|是|是|是|是|||  
|作为数据馈送的报表|是|是|是|是|是|||  
|模型支持|是|是|是|是||||  
|为基于角色的安全性创建自定义角色|是|是|是|||||  
|模型项安全性|是|是|是|||||  
|无限制链接点击|是|是|是|||||  
|共享组件库|是|是|是|||||  
|电子邮件和文件共享订阅和计划|是|是|是|||||  
|报表历史记录、执行快照和缓存|是|是|是|||||  
|SharePoint 集成|是|是|是|||||  
|远程和非 SQL 数据源支持<sup>1</sup>|是|是|是|||||  
|数据源、传递和呈现、RDCE 可扩展性|是|是|是|||||  
|数据驱动报表订阅|是|是||||||  
|扩展部署（Web 场）|是|是||||||  
|警报<sup>2</sup>|是|是||||||  
|[!INCLUDE[ssCrescent](../includes/sscrescent-md.md)] <sup>2</sup>|是|是||||||  
  
 <sup>1</sup>有关详细信息中支持的数据源[!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)]，请参阅[支持的 Reporting Services 数据源&#40;SSRS&#41;](../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md)。  
  
 <sup>2</sup>需要[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]在 SharePoint 模式下。 有关详细信息，请参阅[Reporting Services SharePoint 模式下安装&#40;SharePoint 2010 和 SharePoint 2013&#41;](../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)。  
  
### <a name="report-server-database-server-edition-requirements"></a>报表服务器数据库的服务器版本要求  
 创建报表服务器数据库时，并非所有版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 均可用来承载数据库。 下表显示了可用于特定 [!INCLUDE[ssDE](../includes/ssde-md.md)] 版本的 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]版本。  
  
|对于此 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Reporting Services 版本|使用此版本的数据库引擎实例来承载数据库|  
|----------------------------------------------------------------------|---------------------------------------------------------------------------|  
|Enterprise|Standard Edition、Business Intelligence Enterprise Edition（本地或远程）|  
|Business Intelligence|Standard Edition、Business Intelligence Enterprise Edition（本地或远程）|  
|标准|Standard Edition、Enterprise Edition（本地或远程）|  
|Web|Web Edition（仅用于本地）|  
|Express with Advanced Services|Express with Advanced Services（仅用于本地）。|  
|Evaluation|Evaluation|  
  
##  <a name="BIClients"></a> Business Intelligence 客户端  
 以下软件客户端应用程序在 Microsoft 下载中心提供，它们旨在帮助您创建在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例上运行的商业智能文档。 当您在某一服务器环境中承载这些文档时，请使用支持该文档类型的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本。 下表标识哪一 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本包含承载在这些客户端应用程序中创建的文档所需的服务器功能。  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|[!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)]|是|是|是|||||  
|用于 Excel 和 Visio 2010 的数据挖掘外接程序|是|是|是|||||  
|[!INCLUDE[ssGeminiClient](../includes/ssgeminiclient-md.md)] 2010|是|是||||||  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]|是|是||||||  
  
> [!NOTE]
>  1.  [!INCLUDE[ssGeminiClient](../includes/ssgeminiclient-md.md)] 是 Excel 外接程序，而不依赖于[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。 但是，在 SharePoint 中与 [!INCLUDE[ssGeminiShort](../includes/ssgeminishort-md.md)] 工作簿共享和协作需要 [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] 。此功能在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise 和 Business Intelligence 版本中提供。  
> 2.  上表标识了仅启用这些客户端工具所需的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 版本；但是，这些功能可以访问在任何版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]上承载的数据。  
  
##  <a name="Spatial"></a> Spatial and Location Services  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|空间索引|是|是|是|是|是|是|是|  
|平面和大地测量数据类型|是|是|是|是|是|是|是|  
|高级空间库|是|是|是|是|是|是|是|  
|导入/导出业界标准的空间数据格式|是|是|是|是|是|是|是|  
  
##  <a name="Add_DBServices"></a> Additional Database Services  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 迁移助手|是|是|是|是|是|是|是|  
|数据库邮件|是|是|是|是||||  
  
##  <a name="Other_Components"></a>其他组件  
  
|功能名称|Enterprise|Business Intelligence|标准|Web|Express with Advanced Services|Express with Tools|Express|  
|------------------|----------------|---------------------------|--------------|---------|------------------------------------|------------------------|-------------|  
|Data Quality Services|是|是||||||  
|StreamInsight|StreamInsight Premium Edition|StreamInsight Standard Edition|StreamInsight Standard Edition|StreamInsight Standard Edition||||  
|StreamInsight HA|StreamInsight Premium Edition|||||||  
  
## <a name="see-also"></a>请参阅  
 [SQL Server 2014 的产品规格](../../2014/getting-started/sql-server-2014-product-specifications.md)   
 [SQL Server 2014 安装](../database-engine/install-windows/installation-for-sql-server.md)   
 [SQL Server 2014 安装快速入门](../../2014/getting-started/quick-start-installation-of-sql-server-2014.md)  
  
  
