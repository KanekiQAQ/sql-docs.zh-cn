---
title: “执行 SQL Server 代理作业”任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executesqlserveragentjobtask.f1
helpviewer_keywords:
- Execute SQL Server Agent Job task [Integration Services]
- jobs [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: 3aa3bc0e-1a1c-452e-81b8-b4e3422ea053
author: janinezhang
ms.author: janinez
ms.openlocfilehash: ec6fc06bdcb7a7e622590b66698257b671b4dd98
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67988356"
---
# <a name="execute-sql-server-agent-job-task"></a>“执行 SQL Server 代理作业”任务

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  “执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业”任务运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理是一个 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 服务，它运行那些已在 SQL Server 实例中定义的作业。 您可以创建作业来执行 Transact-SQL 语句和 ActiveX 脚本、执行 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 和复制的维护任务或运行包。 还可以配置作业以监视 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并激发警报。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业通常用来自动执行需要重复执行的任务。 有关详细信息，请参阅 [执行作业](../../ssms/agent/implement-jobs.md)。  
  
 通过使用“执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理”作业任务，包可以执行与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件有关的管理任务。 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业可以运行系统存储过程（如 **sp_enum_dtspackages** ）来获取文件夹中包的列表。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理必须处于运行状态，本地或多服务器管理作业才能自动运行。  
  
 此任务封装 **sp_start_job** 系统过程并把 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的名称作为参数传递给该过程。 有关详细信息，请参阅 [sp_start_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)。  
  
## <a name="configuring-the-execute-sql-server-agent-job-task"></a>配置“执行 SQL Server 代理作业”任务  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器来设置属性。 此任务位于 **设计器中** “工具箱” **的** “维护计划中的任务” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 部分。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请单击以下主题：  
  
-   [“执行 SQL Server 代理作业”任务（维护计划）](../../relational-databases/maintenance-plans/execute-sql-server-agent-job-task-maintenance-plan.md)  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](https://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
  
