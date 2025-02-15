---
title: 在 Windows 上安装 SQL Server 机器学习服务 (数据库内)
description: 有关 Windows 上 SQL Server 2017 机器学习服务 SQL Server 安装步骤的 SQL Server 或 Python 中的 R。
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/22/2019
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.openlocfilehash: 2bc932a16155472f7dfa5cfa22eba6a4a5c59cd0
ms.sourcegitcommit: 9062c5e97c4e4af0bbe5be6637cc3872cd1b2320
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/24/2019
ms.locfileid: "68470448"
---
# <a name="install-sql-server-machine-learning-services-on-windows"></a>在 Windows 上安装 SQL Server 机器学习服务

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

从 SQL Server 2017 开始, 对数据库内分析的 R 和 Python 支持在**SQL Server 机器学习服务**中提供, SQL Server 2016 中引入[SQL Server R Services](../r/sql-server-r-services.md)的后续版本。 函数库在 R 和 Python 中可用，并作为外部脚本在数据库引擎实例上运行。 

本文介绍如何通过运行 SQL Server 安装向导并按照屏幕上的提示来安装机器学习组件[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。

## <a name="bkmk_prereqs"></a>预安装清单

+ 如果要安装 R 或 Python 语言支持机器学习服务, 则需要安装 SQL Server 2017 (或更高版本)。 而如果你拥有 SQL Server 2016 安装介质，则可以安装 [SQL Server 2016 R Services （数据库内）](sql-r-services-windows-install.md)以获得 R 语言支持。

+ 数据库引擎实例为必需项。 不能只安装 R 或 Python 功能，但可以将它们逐步添加到现有实例中。

+ 对于业务连续性, 机器学习服务支持[Always On 可用性组](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)。 必须在每个节点上安装机器学习服务和配置包。

+ SQL Server 2017 中的故障转移群集不支持安装机器学习服务。 但是，SQL Server 2019 支持此操作。 
 
+ 不要在域控制器上安装机器学习服务。 安装程序的机器学习服务部分将失败。

+ 请勿在运行数据库内实例的同一台计算机上安装“共享功能” > “Machine Learning Server”（独立版）。 独立服务器将争夺相同的资源，从而损害两种安装的性能。

+ 支持与其他版本的 R 和 Python 并行安装，但不建议这样做。 之所以支持并行安装，是因为 SQL Server 实例使用自己的开源 R 和 Anaconda 发行版副本。 但是不建议这样做，因为在 SQL Server外部的 SQL Server 计算机上运行使用 R 和 Python 的代码会导致各种问题：
    
  + 与在 SQL Server 中运行相比，你使用不同的库和不同的可执行文件，并获得不同的结果。
  + 外部库中运行的 R 和 Python 脚本不能由 SQL Server 管理，这会导致资源争用。
  
> [!IMPORTANT]
> 安装完成后，请确保完成本文中所述的后期配置步骤。 这些步骤包括让 SQL Server 能够使用外部脚本，以及添加 SQL Server 代表你运行 R 和 Python 作业所需的帐户。 配置更改通常需要重启实例，或者重启 Launchpad 服务

## <a name="get-the-installation-media"></a>获取安装介质

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

## <a name="run-setup"></a>运行安装程序

对于本地安装，必须以管理员身份运行安装程序。 如果从远程共享安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，则必须使用对远程共享具有读取和执行权限的域帐户。

1. 为 SQL Server 2017 启动安装向导。 
  
2. 在“安装”选项卡上，选择“新的 SQL Server 独立安装或向现有安装添加功能”。

   ![新 SQL Server 独立安装](media/2017setup-installation-page-mlsvcs.PNG)
   
3. 在“功能选择”  页上，选择以下选项：
  
    -   **数据库引擎服务**
  
         若要将 R 和 Python 与 SQL Server 一起使用, 必须安装数据库引擎的实例。 可以使用默认实例或命名实例。
  
    -   **机器学习服务（数据库内）**
  
         此选项安装支持 R 和 Python 脚本执行的数据库服务。

    -   **R**

        选中此选项以添加 Microsoft R 包、解释器和开源 R。 

    -   **Python**

        选中此选项以添加 Microsoft Python 包、Python 3.5 可执行文件, 并从 "Anaconda" 分发中选择 "库"。
        
        ![R 和 Python 的功能选项](media/2017setup-features-page-mls-rpy.png "Python 的设置选项")

        > [!NOTE]
        > 
        > 请勿选择 " **Machine Learning Server (独立)** " 选项。 在**共享功能**下安装 Machine Learning Server 的选项应在单独的计算机上使用。

4. 在 "**同意安装 R** " 页上, 选择 "**接受**"。 本许可协议涵盖了 Microsoft R Open, 其中包括开源入门版软件包和工具的分发版, 以及 Microsoft 开发团队提供的增强 R 包和连接提供程序。

5. 在 "**同意安装 Python** " 页上, 选择 "**接受**"。 Python 开源许可协议还介绍了 Anaconda 和相关工具, 以及 Microsoft 开发团队中的一些新 Python 库。
     
     ![协议到 Python 许可证](media/2017setup-python-license.png "Python 许可协议")
  
    > [!NOTE]
    >  如果正在使用的计算机无法访问 internet, 则可以在此时暂停安装程序以单独下载安装程序。 有关详细信息, 请参阅在[不访问 internet 的情况下安装机器学习组件](../install/sql-ml-component-install-without-internet-access.md)。
  
     选择 "**接受**", 等到 "**下一步**" 按钮变为活动状态, 然后选择 "**下一步**"。
  
6. 在 "**准备安装**" 页上, 验证是否已包括这些选择, 并选择 "**安装**"。
  
    + 数据库引擎服务
    + 机器学习服务（数据库内）
    + R 和/或 Python

    请注意路径 `..\Setup Bootstrap\Log` 下的文件夹位置，配置文件存储在此位置。 安装完成后，可以在摘要文件中查看已安装的组件。

7. 安装完成后，如果系统指示重启计算机，请立即重启。 安装完成后，请务必阅读来自安装向导的消息。 有关详细信息，请参阅 [View and Read SQL Server Setup Log Files](https://docs.microsoft.com/sql/database-engine/install-windows/view-and-read-sql-server-setup-log-files)。

## <a name="set-environment-variables"></a>设置环境变量

仅适用于 R 功能集成, 应设置**MKL_CBWR**环境变量, 以[确保](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr)从 Intel 数学内核库 (MKL) 计算中进行一致的输出。

1. 在控制面板中, 单击 "**系统和安全** > **系统** > " "**高级系统设置** > " "**环境变量**"。

2. 创建新的用户或系统变量。 

  + 将变量名称设置为`MKL_CBWR`
  + 将变量值设置为`AUTO`

此步骤需要重新启动服务器。 如果要启用脚本执行, 则可以在完成所有配置工作之前, 暂停重启。

<a name="bkmk_enableFeature"></a>

## <a name="enable-script-execution"></a>启用脚本执行

1. 打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。 

    > [!TIP]
    > 你可以从此页面下载并安装相应的版本:[下载 SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)。
    > 
    > 你还可以使用[Azure Data Studio](../../azure-data-studio/what-is.md), 它支持管理任务和针对 SQL Server 的查询。
  
2. 连接到安装了机器学习服务的实例，单击“新建查询”以打开查询窗口，然后运行以下命令：

    ```sql
    sp_configure
    ```

    属性 `external scripts enabled` 的值目前应为 **0**。 这是因为该功能默认情况下处于关闭状态。 在运行 R 或 Python 脚本之前，必须由管理员显式启用该功能。
    
3.  若要启用外部脚本功能, 请运行以下语句:
    
    ```sql
    EXEC sp_configure  'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    ```
    
    如果已为 R 语言启用此功能, 请不要再次运行 Python 的重新配置。 底层扩展性平台支持这两种语言。

## <a name="restart-the-service"></a>重新启动服务。

安装完成后，重新启动数据库引擎，然后继续执行下一步，启用脚本执行。

重新启动服务也会自动重启[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]相关的服务。

要想重启该服务，可以在 SSMS 中右键单击实例的“重启”命令、使用“控制面板”中的“服务”面板，或使用 [SQL Server 配置管理器](../../relational-databases/sql-server-configuration-manager.md)。

## <a name="verify-installation"></a>验证安装

在 "[自定义报表](../r/monitor-r-services-using-custom-reports-in-management-studio.md)" 或 "安装日志" 中检查实例的安装状态。

使用以下步骤验证用于启动外部脚本的所有组件是否正在运行。

1. 在 SQL Server Management Studio 中, 打开一个新的查询窗口, 并运行以下命令:
    
    ```sql
    EXEC sp_configure  'external scripts enabled'
    ```

    **run_value** 现在应已设置为 1。
    
2. 打开“服务”面板或 SQL Server 配置管理器，验证“SQL Server Launchpad 服务”是否正在运行。 应具备针对安装了 R 或 Python 的每个数据库引擎实例的一项服务。 有关该服务的更多信息，请参阅[可扩展性框架](../concepts/extensibility-framework.md)。 
   
3. 如果启动板正在运行, 你应该能够运行简单的 R 和 Python 脚本, 以验证外部脚本运行时是否可以与 SQL Server 通信。

   在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中打开一个新的“查询”窗口，然后运行如下脚本：
    
    + 对于 R
    
    ```sql
    EXEC sp_execute_external_script  @language =N'R',
    @script=N'
    OutputDataSet <- InputDataSet;
    ',
    @input_data_1 =N'SELECT 1 AS hello'
    WITH RESULT SETS (([hello] int not null));
    GO
    ```

    + 用于 Python
    
    ```sql
    EXEC sp_execute_external_script  @language =N'Python',
    @script=N'
    OutputDataSet = InputDataSet;
    ',
    @input_data_1 =N'SELECT 1 AS hello'
    WITH RESULT SETS (([hello] int not null));
    GO
    ```

    **结果**

    对于第一次加载外部脚本运行时，脚本可能需要一段时间才能运行。 结果应如下所示：

    | Hello |
    |----|
    | 1|

> [!NOTE]
> 在 Python 脚本中使用的列或标题不是按设计返回的。 若要为输出添加列名称, 必须指定返回数据集的架构。 为此, 请使用存储过程的 WITH RESULTS 参数, 命名列并指定 SQL 数据类型。
> 
> 例如, 可以添加以下行来生成任意列名:`WITH RESULT SETS ((Col1 AS int))`

<a name="apply-cu"></a>

## <a name="apply-updates"></a>应用更新

建议将最新的累积更新应用于数据库引擎和机器学习组件。

在已连接 Internet 的设备上，累积更新通常通过 Windows 更新进行应用，但也可以使用以下步骤进行有控制的更新。 当你为数据库引擎应用更新时, 安装程序将为你在同一实例上安装的任何 R 或 Python 功能拉取累积更新。 

在连接已断开的服务器上，需执行额外的步骤。 有关更多信息，请参阅[在没有 internet 访问权限的计算机上安装 > 应用累积更新](sql-ml-component-install-without-internet-access.md#apply-cu)。

1. 开始使用已安装的基线实例:SQL Server 2017 初始版本

2. 请参阅累积更新列表:[SQL Server 2017 更新](https://sqlserverupdates.com/sql-server-2017-updates/)

3. 选择最新的累积更新。 可执行文件将自动下载和提取。

4. 运行安装程序。 接受许可条款，然后在功能选择页面上，查看应用累积更新的功能。 应该会看到为当前实例安装的每个功能, 包括机器学习功能。 安装程序会下载更新所有功能所需的 CAB 文件。

  ![已安装功能摘要](media/cumulative-update-feature-selection.png)

5. 继续执行向导, 接受 R 和 Python 分发的许可条款。 

## <a name="additional-configuration"></a>其他配置

如果外部脚本验证步骤成功, 则可以从 SQL Server Management Studio、Visual Studio Code 或可以向服务器发送 T-sql 语句的任何其他客户端运行 R 或 Python 命令。

如果在运行命令时出错，请查看本节中的其他配置步骤。 可能需要对服务或数据库进行其他适当的配置。

在实例级别, 其他配置可能包括:

* [SQL Server 机器学习服务的防火墙配置](../../advanced-analytics/security/firewall-configuration.md)
* [启用其他网络协议](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)
* [启用远程连接](../../database-engine/configure-windows/configure-the-remote-access-server-configuration-option.md)
* [创建 SQLRUserGroup 的登录名](../../advanced-analytics/security/create-a-login-for-sqlrusergroup.md)
* [管理磁盘配额](https://docs.microsoft.com/windows/desktop/fileio/managing-disk-quotas), 以避免运行耗尽磁盘空间的任务的外部脚本

<a name="bkmk_configureAccounts"></a> 
<a name="permissions-external-script"></a> 

在数据库上, 可能需要以下配置更新:

* [向用户授予 SQL Server 的权限机器学习服务](../../advanced-analytics/security/user-permission.md)

> [!NOTE]
> 是否需要额外的配置取决于你的安全架构、安装 SQL Server 的位置, 以及你希望用户如何连接到数据库并运行外部脚本。

## <a name="suggested-optimizations"></a>建议的优化

现已完成了所有工作，你可能还希望优化服务器以支持机器学习，或者安装预先训练的模型。

### <a name="add-more-worker-accounts"></a>添加更多的辅助角色帐户

如果希望很多用户同时运行脚本, 则可以增加分配给快速启动板服务的辅助角色帐户的数目。 有关详细信息, 请参阅[Modify the user account pool for SQL Server 机器学习服务](../administration/modify-user-account-pool.md)。

### <a name="optimize-the-server-for-script-execution"></a>优化服务器执行脚本

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的默认设置旨在针对数据库引擎支持的各种服务优化服务器的平衡，这些服务可能包括提取、转换和加载 (ETL) 流程、报告、审计以及使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据的应用程序。 因此，在默认设置下，你可能会发现机器学习的资源有时会受到限制或阻止，尤其是在内存密集型操作中。

为了确保按优先级确定机器学习作业的优先级, 并对其进行了适当的资源划分, 建议使用 SQL Server Resource Governor 来配置外部资源池。 你可能还需要更改分配给[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]数据库引擎的内存量, 或者增加在该[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]服务下运行的帐户数。

- 若要配置用于管理外部资源的资源池, 请参阅[创建外部资源池](../../t-sql/statements/create-external-resource-pool-transact-sql.md)。
  
- 若要更改数据库保留的内存量，请参阅[服务器内存配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)。
  
- 若要更改可以通过 [!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)] 启动的 R 帐户数量，请参阅[修改机器学习的用户帐户池](../administration/modify-user-account-pool.md)。

如果使用的是 Standard Edition 并且没有 Resource Governor, 则可以使用动态管理视图 (Dmv) 和扩展事件以及 Windows 事件监视来帮助管理服务器资源。 有关详细信息, 请参阅[监视和管理 R Services](../r/managing-and-monitoring-r-solutions.md)和[监视和管理 Python 服务](../python/managing-and-monitoring-python-solutions.md)。

### <a name="install-additional-r-packages"></a>安装其他 R 包

为 SQL Server 创建的R解决方案可以调用基本 R 函数、与 SQL Server 一起安装的专有包中的函数以及与 SQL Server 安装的开源 R 版本兼容的第三方 R 包。

要通过 SQL Server 使用的包必须安装在实例使用的默认库中。 如果在计算机上单独安装 R，或者将软件包安装到用户库，则无法使用 T-SQL 中的这些包。

在 SQL Server 2016 和 SQL Server 2017 中，安装和管理 R 包的过程是不同的。 在 SQL Server 2016 中，数据库管理员必须安装用户需要的 R 包。 在 SQL Server 2017 中，可以设置用户组以在每个数据库级别上共享包，或者配置数据库角色以使用户能够安装自己的包。 有关详细信息, 请参阅[在 SQL Server 中安装新的 R 包](../r/install-additional-r-packages-on-sql-server.md)。


## <a name="next-steps"></a>后续步骤

R 开发人员可以开始使用一些简单的示例, 并了解 R 如何与 SQL Server 相关的基础知识。 下一步, 请参阅以下链接:

+ [教程：在 T-sql 中运行 R](../tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md)
+ [教程：适用于 R 开发人员的数据库内分析](../tutorials/sqldev-in-database-r-for-sql-developers.md)

Python 开发人员可以通过以下教程了解如何将 Python 与 SQL Server 配合使用:

+ [教程：在 T-sql 中运行 Python](../tutorials/run-python-using-t-sql.md)
+ [教程：适用于 Python 开发人员的数据库内分析](../tutorials/sqldev-in-database-python-for-sql-developers.md)

若要查看基于实际场景的机器学习示例，请参阅[机器学习教程](../tutorials/machine-learning-services-tutorials.md)。
