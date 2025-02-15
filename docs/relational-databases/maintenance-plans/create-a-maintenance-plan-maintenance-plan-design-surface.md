---
title: 创建维护计划（维护计划设计图面）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Maintenance Plan Design Surface
ms.assetid: 2ef803ee-a9f8-454a-ad63-fedcbe6838d1
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7b39b4391780a8133dae199e39638a6db77d73aa
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68083906"
---
# <a name="create-a-maintenance-plan-maintenance-plan-design-surface"></a>创建维护计划（维护计划设计图面）
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  本主题说明如何在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中使用维护计划设计图面创建单个服务器或多服务器维护计划。 尽管 **“维护计划向导”** 是创建基本维护计划的最佳方法，但使用设计图面创建计划允许您使用增强的工作流。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   [使用维护计划设计图面创建维护计划](#SSMSProcedure)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Restrictions"></a> 限制和局限  
  
-   若要创建多服务器维护计划，必须配置包含一个主服务器和一个（或多个）目标服务器的多服务器环境。 必须在主服务器上创建和维护多服务器维护计划。 在目标服务器上可以查看这些计划，但不能进行维护。  
  
-   **db_ssisadmin** 和 **dc_admin** 角色的成员可以将其特权提升为 **sysadmin**。 因为这些角色可以修改 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包，而 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 **代理的** sysadmin [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全上下文可以执行这些包，所以可以实现特权提升。 若要防止在运行维护计划、数据收集组和其它 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包时提升特权，请将运行包的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业配置为具有有限特权的代理帐户，或仅将 **sysadmin** 成员添加到 **db_ssisadmin** 和 **dc_admin** 角色。  
  
###  <a name="Security"></a> Security  
  
####  <a name="Permissions"></a> 权限  
 若要创建或管理维护计划，您必须是 **sysadmin** 固定服务器角色的成员。 对象资源管理器只为属于 **sysadmin** 固定服务器角色成员的用户显示 **“维护计划”** 节点。  
  
##  <a name="SSMSProcedure"></a> 使用维护计划设计图面  
  
#### <a name="to-create-a-maintenance-plan"></a>创建维护计划  
  
1.  在对象资源管理器中，单击加号以便展开您要创建维护计划的服务器。  
  
2.  单击加号以便展开 **“管理”** 文件夹。  
  
3.  右键单击“维护计划”  文件夹，然后选择“新建维护计划”  。  
  
4.  在 **“新建维护计划”** 对话框的 **“名称”** 框中，为该计划键入一个名称，然后单击 **“确定”** 。 这将打开工具箱和 *maintenance_plan_name* **[设计]** 图面，其中包含在主网格中创建的 **Subplan_1** 子计划。  
  
     在设计空间的标头中提供以下选项。  
  
     **添加子计划**  
     添加可以配置的子计划。  
  
     **子计划属性**  
     为在主网格中选择的子计划显示“子计划属性”  对话框。 或者，还可以在网格中双击某一子计划，以显示“子计划属性”  对话框。 下面提供了有关此对话框的详细信息。  
  
     **删除所选子计划**  
     删除所选子计划。  
  
     **子计划的计划**  
     为选择的子计划显示“新建作业计划”  对话框。  
  
     **删除计划**  
     从所选子计划中删除计划。  
  
     **管理连接**  
     显示“管理连接”  对话框。 用于向维护计划添加其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例连接。 下面提供了有关此对话框的详细信息。  
  
     **报告和记录**  
     显示“报告和记录”  对话框。 下面提供了有关此对话框的详细信息。  
  
     **服务器**  
     显示“服务器”  对话框，用于选择要运行子计划中的任务的服务器。 此选项仅在多服务器环境中的主服务器上启用。 有关详细信息，请参阅[创建多服务器环境](../../ssms/agent/create-a-multiserver-environment.md)和[维护计划（服务器）](../../relational-databases/maintenance-plans/maintenance-plan-servers.md)。  
  
     **名称**  
     显示维护计划的名称。 对于新建的维护计划，该名称是在打开维护计划设计器之前在一个对话框中指定的。 若要重命名维护计划，请在对象资源管理器中右键单击该计划，再单击“重命名”  。  
  
     **Description**  
     查看或指定维护计划的说明。 说明的最大长度为 512 个字符。  
  
     **设计器图面**  
     设计和维护维护计划。 使用设计器图面，可以向计划中添加维护任务、从计划中删除任务、指定任务之间的优先链接以及指示任务分支和并行情况。  
  
     两个任务之间的优先链接会在任务之间建立关系。 只有当第一项任务（前置任务  ）的执行结果与指定的条件相匹配时，才执行第二项任务（依赖任务  ）。 通常，指定的执行结果为 **“成功”** 、 **“失败”** 或 **“完成”** 。 有关详细信息，请参阅下面的步骤 **8** 。  
  
5.  在设计图面的标头中，双击 **Subplan_1** ，然后在“子计划属性”  对话框中输入子计划的名称和说明。  
  
     在 **“子计划属性”** 对话框中提供以下选项。  
  
     **“名称”**  
     子计划的名称。  
  
     **Description**  
     子计划的简短说明。  
  
     **计划**  
     指示子计划将会运行的计划。 单击 **“子计划的计划”** 以便打开 **“新建作业计划”** 对话框。 单击 **“删除计划”** 可从子计划中删除计划。  
  
      “运行身份”列表  
     选择要用于运行此子任务的帐户。  
  
6.  单击 **“子计划的计划”** 以便在 **“新建作业计划”** 对话框中输入计划详细信息。  
  
7.  若要生成子计划，请将 **“工具箱”** 中的任务流元素拖放到计划设计图面。 双击任务打开对话框来配置任务选项。  
  
     在 **“工具箱”** 中提供以下维护计划任务：  
  
    -   **“备份数据库”任务**  
  
    -   **“检查数据库完整性”任务**  
  
    -   **“执行 SQL Server 代理作业”任务**  
  
    -   **执行 T-SQL 语句任务**  
  
    -   **“清除历史记录”任务**  
  
    -   **“清除维护”任务**  
  
    -   **“通知操作员”任务**  
  
    -   **“重新生成索引”任务**  
  
    -   **“重新组织索引”任务**  
  
    -   **收缩数据库任务**  
  
    -   **“更新统计信息”任务**  
  
     向 **“工具箱”** 中添加任务：  
  
    1.  在 **“工具”** 菜单上，单击 **“选择工具箱项”** 。  
  
    2.  选择想要显示在 **“工具箱”** 中的工具，然后单击 **“确定”** 。  
  
     向 **“工具箱”** 中添加维护计划任务也会使这些任务可用于 **“维护计划向导”** 中。 有关上述各个任务的详细信息，请参阅“启动维护计划向导”下的  [使用维护计划向导](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure)。  
  
8.  定义各任务之间的工作流：  
  
    1.  右键单击前置任务，然后选择“添加优先约束”  。  
  
    2.  在 **“控制流”** 对话框的 **“到”** 列表中，选择依赖任务，然后单击 **“确定”** 。  
  
    3.  双击两个任务之间的连接器以便打开 **“优先约束编辑器”** 对话框。  
  
         在 **“优先约束编辑器”** 对话框中提供以下选项。  
  
         **约束选项**  
         定义约束在两个任务之间的工作方式。  
  
           “求值运算”列表  
         指定优先约束使用的求值运算。 运算包括：“约束”  、“表达式”  、“表达式和约束”  和“表达式或约束”  。  
  
          “值”列表  
         指定约束值：“成功”  、“失败”  或“完成”  。 **“成功”** 的默认值。  
  
        > [!NOTE]  
        >  优先约束线的含义：绿色表示“成功”  ，红色表示“失败”  ，蓝色表示“完成”  。  
  
         **“表达式”**  
         若要使用“表达式”  、“表达式和约束”  或“表达式或约束”  运算，请键入表达式。 表达式的计算结果必须为布尔值。  
  
         **测试**  
         验证表达式。  
  
         **多个约束**  
         定义多个约束如何交互操作以便控制约束任务的执行。  
  
         **逻辑与**  
         选择此选项可以指定：同一个可执行文件的多个优先约束必须一起计算。 所有约束的计算结果都必须为 True。 此选项为默认值。  
  
        > [!NOTE]  
        >  这种类型的优先约束显示为绿色、红色或蓝色实线。  
  
         **逻辑或**  
         选择此选项可以指定：同一个可执行文件的多个优先约束必须一起计算。 至少必须有一个约束的计算结果为 True。  
  
        > [!NOTE]  
        >  这种类型的优先约束显示为绿色、红色或蓝色点线。  
  
9. 若要添加包含在其他计划中运行的任务的另一个子计划，请单击工具栏上的 **“添加子计划”** 以便打开 **“子计划属性”** 对话框。  
  
10. 添加与其他服务器的连接：  
  
    1.  在设计空间的工具栏中，单击 **“管理连接”** 。  
  
    2.  在 **“管理连接”** 对话框中，单击 **“添加”** 。  
  
    3.  在 **“连接属性”** 对话框的 **“连接名称”** 框中，输入要创建的连接的名称。  
  
    4.  在“指定下列选项以连接到 SQL Server 数据”下的“选择或输入服务器名称”框中，输入要使用的 SQL Server 的名称，或者单击省略号 (…) 并在 SQL Server 对话框中选择某一服务器     。 如果您从 **SQL Server** 对话框中选择某一服务器，则单击 **“确定”** 。  
  
    5.  在 **“输入登录服务器所需的信息”** 下，选择 **“使用 Windows NT 集成安全性”** 或 **“使用特定用户名和密码”** 。 如果您选择使用特定的用户名和密码，则分别在 **“用户名”** 和 **“密码”** 框中输入该信息。  
  
    6.  单击 **“连接属性”** 对话框中的 **“确定”** 。  
  
    7.  在 **“管理连接”** 对话框中，单击 **“关闭”** 。  
  
11. 指定报告选项：  
  
    1.  在设计空间的工具栏中，单击 **“报告和记录”** 。  
  
    2.  在 **“报告和记录”** 对话框的 **“报告”** 下，选择 **“生成文本文件报告”** 和/或 **“将报告发送给电子邮件收件人”** 。  
  
        1.  如果您选择了 **“生成文本文件报告”** ，则选择 **“创建新文件”** 或 **“追加到文件”** 。  
  
        2.  根据上面选择的选项，通过在 **“文件夹”** 或 **“文件名”** 框中输入信息，输入新文件或要追加的文件的名称和完整路径。 或者，单击省略号 (...) 并从“定位文件夹 -server\_name”或“定位数据库文件 -server\_name”对话框中选择该文件夹的路径或文件名      。  
  
        3.  如果您选择 **“将报告发送给电子邮件收件人”** ，则在 **“代理操作员”** 列表上，选择以电子邮件形式发送的报告的收件人。  
  
            > [!NOTE]  
            >  为发送电子邮件，SQL Server 代理必须配置为使用数据库邮件。 有关详细信息，请参阅 [Configure SQL Server Agent Mail to Use Database Mail](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
    3.  若要保存更详细的信息，请在 **“记录”** 下选择 **“记录扩展信息”** 。  
  
    4.  若要将维护计划结果信息写入其他服务器，请选择 **“在远程服务器上进行日志记录”** ，并且或者从 **“连接”** 列表中选择某一服务器连接，或者单击 **“新建”** 并在 **“连接属性”** 对话框中输入连接信息。  
  
    5.  在 **“报告和记录”** 对话框中，单击 **“确定”** 。  
  
12. 若要在日志文件查看器中查看结果，请在“对象资源管理器”  中右键单击“维护计划”  文件夹或特定维护计划，然后选择“查看历史记录”  。  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

     The following options are available on the **Log File Viewer -**_server\_name_ dialog box.  
  
     **Load Log**  
     Open a dialog box where you can specify a log file to load.  
  
     **Export**  
     Open a dialog box that lets you export the information that is shown in the **Log file summary** grid to a text file.  
  
     **Refresh**  
     Refresh the view of the selected logs. The **Refresh** button rereads the selected logs from the target server while applying any filter settings.  
  
     **Filter**  
     Open a dialog box that lets you specify settings that are used to filter the log file, such as **Connection**, **Date**, or other **General** filter criteria.  
  
     **Search**  
     Search the log file for specific text. Searching with wildcard characters is not supported.  
  
     **Stop**  
     Stops loading the log file entries. For example, you can use this option if a remote or offline log file takes a long time to load, and you only want to view the most recent entries.  
  
     **Log file summary**  
     This information panel displays a summary of the log file filtering. If the file is not filtered, you will see the following text, **No filter applied**. If a filter is applied to the log, you will see the following text, **Filter log entries where:** \<filter criteria>.  
  
     **Date**  
     Displays the date of the event.  
  
     **Source**  
     Displays the source feature from which the event is created, such as the name of the service (MSSQLSERVER, for example). This does not appear for all log types.  
  
     **Message**  
     Displays any messages associated with the event.  
  
     **Log Type**  
     Displays the type of log to which the event belongs. All selected logs appear in the log file summary window.  
  
     **Log Source**  
     Displays a description of the source log in which the event is captured.  
  
     **Selected row details**  
     Select a row to display additional details about the selected event row at the bottom of the page. The columns can be reordered by dragging them to new locations in the grid. The columns can be resized by dragging the column separator bars in the grid header to the left or right. Double-click the column separator bars in the grid header to automatically size the column to the content width.  
  
     **Instance**  
     The name of the instance on which the event occurred. This is displayed as *computer name*\\*instance name*.  
  
  
