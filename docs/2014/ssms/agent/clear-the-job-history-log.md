---
title: 清除作业历史记录日志 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], history
- clearing job history log
- logs [SQL Server], jobs
- SQL Server Agent jobs, history
- historical information [SQL Server], jobs
ms.assetid: 34b9398a-c409-4040-8ea1-0deceb18f961
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 5c9bc55a6a5af15d6d21c4d3e7d74e2dc8296e84
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68211487"
---
# <a name="clear-the-job-history-log"></a>Clear the Job History Log
  本主题说明如何使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Server 管理对象在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中删除 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] or SQL Server Management Objects.  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要清除作业历史记录日志，请使用：**  
  
     [SQL Server Management Studio](#SSMS)  
  
     [Transact-SQL](#TSQL)  
  
     [SQL Server 管理对象](#SMO)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Security"></a> 安全性  
 有关详细信息，请参阅[实现 SQL Server 代理安全性](implement-sql-server-agent-security.md)。  
  
##  <a name="SSMS"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-clear-the-job-history-log"></a>清除作业历史记录日志  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的实例，然后展开该实例。  
  
2.  展开 **“SQL Server 代理”** ，再展开 **“作业”** 。  
  
3.  右键单击某个作业，再单击 **“查看历史记录”** 。  
  
4.  在 **“日志文件查看器”** 中，选择要清除其历史记录的作业，再执行下列操作：  
  
    -   单击 **“删除”** ，再单击 **“删除历史记录”** 对话框中的 **“删除所有历史记录”** 。 可以删除所有作业历史记录，或者仅删除早于指定日期的历史记录。 若要删除所有作业历史记录，请单击 **“删除所有历史记录”** 。 如果只删除较早的作业历史记录日志，请单击 **“删除以下时间之前的历史记录”** ，然后指定日期。  
  
    -   单击 **“作业状态”** （如果要清除多服务器作业的历史记录日志）。 单击 **“作业”** ，单击某个作业名，再单击 **“查看远程作业历史记录”** 。  
  
5.  单击 **“删除”** 。  
  
##  <a name="TSQL"></a> 使用 Transact-SQL  
  
#### <a name="to-clear-the-job-history-log"></a>清除作业历史记录日志  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行”  。  
  
    ```  
    -- example removes the history for a job named NightlyBackups.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_purge_jobhistory  
        @job_name = N'NightlyBackups' ;  
    GO  
    ```  
  
##  <a name="SMO"></a> 使用 SQL Server 管理对象  
 **清除作业历史记录日志**  
  
 通过使用所选的编程语言（如 Visual Basic、Visual C# 或 PowerShell）来使用 `PurgeJobHistory` 类的 `JobServer` 方法。 有关详细信息，请参阅 [SQL Server 管理对象 (SMO)](https://msdn.microsoft.com/library/ms162169.aspx)。  
  
  
