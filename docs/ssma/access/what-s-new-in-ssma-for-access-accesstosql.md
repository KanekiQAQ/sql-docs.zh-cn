---
title: SSMA for Access (AccessToSQL) 中的新增功能 |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 07/31/2019
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: a24d3fc0-6911-4bfa-828a-197abf222e02
author: HJToland3
ms.author: Shamikg
ms.openlocfilehash: 39f8d7da95fc8e2102d1208216a2eb43bb038fea
ms.sourcegitcommit: a154b3050b6e1993f8c3165ff5011ff5fbd30a7e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68632068"
---
# <a name="whats-new-in-ssma-for-access-accesstosql"></a>SSMA for Access (AccessToSQL) 中的新增功能

本文列出了每个版本中的访问更改 SQL Server 迁移助手 (SSMA)。  

## <a name="ssma-v83"></a>SSMA v 8。3

SSMA for Access 的 v 8.3 版本通过旨在提高质量和转换指标的目标修补程序进行了增强。 此外, 此版本的 SSMA for Access 还提供了以下修补程序:

* 解决辅助功能问题
* 在 SQL Server 中添加 "hierarchyid" 类型的基本支持

> [!IMPORTANT]
> 对于 SSMA 7.4 和更高版本, .Net 4.5.2 是必备组件。

## <a name="ssma-v82"></a>SSMA v8.2

用于访问的 SSMA 的版本8.2 版本利用旨在提高质量和转换指标的目标修补程序进行了增强。

> [!NOTE]
> 自动更新的一个已知问题可能会导致从 SSMA v4.0 到 v4.0 8.2 的更新失败。 如果遇到此错误, 请下载并手动安装新版本。

> [!IMPORTANT]
> 对于 SSMA 7.4 和更高版本, .Net 4.5.2 是必备组件。

## <a name="ssma-v81"></a>SSMA v8.1

用于访问的 SSMA 的7.4 版使用旨在提高质量和转换指标的目标修补程序进行了增强。

> [!NOTE]
> 自动更新的一个已知问题可能会导致从 SSMA 8.0 到 app-v 8.1 的更新失败。 如果遇到此错误, 请下载并手动安装新版本。

## <a name="ssma-v80"></a>SSMA v8.0

使用旨在提高质量和转换指标的目标修补程序, 增强了 SSMA for Access 的8.0 版本。 此版本还提供以下新增功能:

* 支持作为目标**Azure SQL 数据库托管实例**。 你现在可以创建面向 Azure SQL 数据库托管实例的新项目:

  ![SQL DB MI 项目](../media/ssma-newproject-sqldbmi.png)

* 转换后**修补顾问**。 [在此处](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/)了解详细信息。

* 初步的数据库/架构选择。

    连接到源时, 用户现在可以选择数据库/感兴趣的架构。 仅选择你计划迁移的架构将在初始连接期间节省时间, 并提高总体 SSMA 性能。

   ![SSMA 筛选器对象](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v7.10

用于访问的 SSMA 的7.10 版本得到了增强, 旨在提供额外的安全性和隐私保护, 以满足全局要求的变化。

## <a name="ssma-v79"></a>SSMA v7.9

用于访问的 SSMA 的 v 7.9 版本包含以下更改:

* 改进质量和转换指标的目标修补程序。
* 支持在 SSMA 命令行中更改数据类型映射和项目首选项。
* 还更改了 SSMA 中的 Azure SQL 数据库连接对话框以指定完全限定的服务器名称。 在以前版本的 SSMA 中, 必须在项目设置中显式提到 Azure SQL 数据库前缀。

## <a name="ssma-v78"></a>SSMA v7.8

用于访问的 SSMA 的版本7.8 版本包含以下更改:

* 更改项目设置中突出显示的类型映射。
* 允许用户禁用遥测数据。

## <a name="ssma-v77"></a>SSMA v7.7

用于访问的 SSMA 的 v4.0 版本包含以下更改:

* 改进质量和转换指标的目标修补程序。
* 基于常用需求, SSMA for Access 的32位版本已恢复。 与之前的实现 (在7.4 之前) 相比, 有两个安装包, 但不能并行安装。 因此, 你必须根据你拥有的连接组件选择最适合的版本。 如果可能, 始终最好使用64位版本。

## <a name="ssma-v76"></a>SSMA v7.6

用于访问的 SSMA 的版本7.6 版本通过改进质量和转换指标的目标修补程序进行了增强, 并支持 SQL Server 2017 (公共预览版)。 对 Windows 和 Linux 上的 SQL Server 2017 的支持是公共预览版, 不应用于生产迁移。

## <a name="ssma-v75"></a>SSMA v7.5

用于访问的 SSMA 的7.5 版本增强了几项改进, 以确保为残障人士提供更好的辅助功能。

## <a name="ssma-v74"></a>SSMA v7.4

用于访问的 SSMA 的7.4 版本包含以下更改:

* 在源和目标中的架构对象发现期间, 现在可以使用 "**查询超时**" 选项。

  ![query timeout 选项](../media/query-timeout_red.png)

* 根据客户的反馈, 改进了质量和转换指标。

  > [!IMPORTANT]
  > .Net 4.5.2 是安装 SSMA 7.4 的必备组件。 此外, 从7.4 版开始, 32 位版本的 SSMA 已不再使用。

## <a name="ssma-v73"></a>SSMA v7.3

用于访问的 SSMA 的7.3 版本包含以下更改:

* 根据客户的反馈, 改进了质量和转换指标与目标修复。
* 通过以下各项公开的 SSMA 扩展性框架:
  * 将功能导出到 SQL Server Data Tools (SSDT) 项目。
    * 你现在可以将 SSMA 中的架构脚本导出到 SSDT 项目。 您可以使用架构脚本来进行其他架构更改并部署您的数据库。

        ![另存为 SSDT 项目命令](../media/export-schema-scripts_red.png)
  * 可供 SSMA 使用的库, 用于执行自定义转换。
    * 你现在可以构造代码, 该代码可以处理 SSMA 之前未处理的自定义语法转换和转换。
      * 此博客文章中提供了有关如何构造自定义转换器的说明,[扩展了 SQL Server 迁移助手的转换功能](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/)。
      * 下载此[博客文章](https://blogs.msdn.microsoft.com/datamigration/ssmafororacleconversionsample/)中用于转换的示例项目。

## <a name="ssma-v72"></a>SSMA v7.2

SSMA for Access 的7.2 版本包含以下更改:

* 根据客户的反馈, 改进了质量和转换指标与目标修复。
* 遥测增强功能可提供更好的数据点以解决客户问题并提高 SSMA 的转换率。

## <a name="ssma-v71"></a>SSMA v7.1

用于访问的 SSMA 的7.1 版本包含以下更改:

* Windows 和 Linux 上的 SQL Server 2017 现在是支持迁移的目标平台。 此功能在 technical preview 中, 支持将架构和数据移动到目标 SQL server。
* SSMA 现在支持自动更新, 以下载最新版本的 SSMA。
* SSMA 可安装二进制文件现在通过 Windows installer 包文件 (.msi) 传递。

## <a name="may-2016"></a>可能为2016

Access 的 SSMA 2016 版本包含以下更改:  
  
* 添加了对 SQL Server 2016 的官方支持
* 移除了 .Net 2.0 的安装程序检查。
* 修复了用于 SSMA 控制台的 "保存项目" 和 "打开项目" 命令。
* 修复了 SSMA 控制台的 "securepassword" 命令。
* 修复了初始加载的对象计数。
* 固定表数据加载以供访问的 UI 选项卡。
* 修复了全局设置中的 bug。 

## <a name="march-2016"></a>2016 年 3 月

SSMA for Access 的2016年3月预览版增加了对迁移到 SQL Server 2016 的支持。  

## <a name="january-2016"></a>2016年1月

用于访问的 SSMA 的2016年1月维护版本包含以下更改:  
  
* 修复了默认 GUID 字段的无效函数 (RFC 3894811)。  
* 修复了将记录导入到 SQL 数据库 (Azure) 时挂起 (RFC 4919573)。  
* 向 SSMA 添加了 "查看日志" 菜单项 (RFC 5706203)。  
* 添加了遥测。
  
## <a name="july-2014"></a>2014 年 7 月

SSMA 的2014年7月发行版本包含以下更改:  
  
* 改进了 Azure SQL DB 代码转换。  
* 已将扩展包功能移到架构以支持 Azure SQL DB。  
* 针对具有超过10k 对象的数据库测试性能改进。  
* 添加了用于处理大量对象的 UI 改进。  
* 添加了对 "知名" LOB 架构的突出显示的支持 (以便在转换时忽略它们)。  
* 增加了转换速度改进。
* 添加了对在 UI 中显示对象计数的支持。
* 将报表大小减少 25% 以上。
* 改进了未分析构造的错误消息。  
  
## <a name="april-2014"></a>2014年4月

用于访问的 SSMA 2014 年4月版包含以下更改:  
  
* 添加了对 MS SQL Server 2014 的支持。
* 修复了有关转换到 Azure 的 bug。  
* 修复了 IE 10 中不可见报表页的相关 bug。  
  
## <a name="january-2012"></a>2012年1月

用于访问的 SSMA 的2012年1月版本包含以下更改:  
  
* 提供了在迁移后不保留 MS Access 链接表的用户名和密码的选项。  
* 将循环引用的层叠操作设置为 "无操作"。  
* 提供了指示循环引用级联操作的正确消息已设置为 "无操作"。  
  
## <a name="july-2011"></a>2011年7月

2011年7月发布的 SSMA for Access 增加了数据迁移期间的错误报告功能。  
  
## <a name="april-2011"></a>2011年4月

用于访问的 SSMA 2011 年4月版包含以下更改:  
  
* 添加了 "SSMA for Access" 的单个可安装, 该[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]程序支持[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008、"Denali" 和 Azure SQL。  
* 添加了连接[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] "Denali" 的功能。  
* 为实现向后兼容性而添加了用于访问控制台版本支持的 SSMA。 可以打开先前版本的 SSMA v 5.0 创建的项目。
* 添加了一种功能, 可将 SSMA v 5.0 产品并行 (SxS) 与较旧版本的 SSMA 产品一起安装。  
  
## <a name="july-2010"></a>2010年7月  
SSMA 的2010年7月发行版本包含以下更改:  
  
* 添加了对迁移到 SQL Server 2008 R2 和 Azure SQL 的支持。
* 添加了与 SQL Server 和 Azure SQL 的安全连接。  
* 添加了对 Access 2010 数据库的支持。
* 添加了新的 SSMA 控制台应用程序, 用于执行命令行。
* 添加了对 SQL Server DateTime2 数据类型的支持。
  
## <a name="june-2008"></a>2008年6月

2008年6月发布的 SSMA for Access 增加了对 Access 2007 数据库的支持。  
  
## <a name="may-2007"></a>可能为2007

Access 的 SSMA 2007 版本包含以下更改:  
  
* 添加了对使用工作组策略的 Access 数据库的支持。  
* 提供了从 SQL Server 元数据资源管理器中删除转换后的对象的功能。  
* 在 SQL Server 格式的 SQL 模式下添加了对用户输入的注释的支持。  
* 增加了对象转换的改进。  
  
## <a name="november-2006"></a>2006年11月

用于访问的 SSMA 11 月2006版包含以下更改:  
  
* 添加了新的数据库迁移向导, 可引导您完成从访问到[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的单个数据库的整个步骤。  
* 添加了新的 "转换"、"加载" 和 "迁移" 命令, 用于转换 Access [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]数据库、将转换后[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的对象加载到中, 并在一个步骤中将数据迁移到全部。  
* 改进的查询迁移。 查询迁移现在会将更多的选择查询转换为视图。 有关详细信息, 请参阅[转换 Access 数据库对象](converting-access-database-objects-accesstosql.md)。  
* 添加了在 " [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **表**" 选项卡上编辑表和索引属性的功能。  
* 添加了新的全局设置:
  * 您可以选择在编辑器窗口中显示行号。  
  * 您可以将 SSMA 配置为提示替换重复的对象, 或者在架构转换期间始终或从不替换重复的对象。  
* 添加了一个新的转换选项, 该选项使你可以指定在复杂查询包含通配符时 SSMA 是否显示警告。  
  
## <a name="july-2006"></a>2006年7月

SSMA for Access 的2006年7月发行版是初始版本。
