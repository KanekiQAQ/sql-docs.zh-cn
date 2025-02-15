---
title: 同时对多个服务器执行语句 (SQL Server Management Studio) |Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- multiserver queries
- executing queries against multiple servers
- queries [SQL Server], multiserver
ms.assetid: 197760f3-0a06-43de-8162-69c27d3fbe56
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 2747b7b13d2eda5aeda1677631ba04d3ed840d59
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2019
ms.locfileid: "68211235"
---
# <a name="execute-statements-against-multiple-servers-simultaneously-sql-server-management-studio"></a>Execute Statements Against Multiple Servers Simultaneously (SQL Server Management Studio)
  本主题说明如何在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]中，通过以下方法同时查询多个服务器：创建一个本地服务器组，或者创建一个中央管理服务器以及一个或多个服务器组，在这些组中创建一个或多个已注册的服务器，然后查询整个组。 可以将查询返回的结果合并到单个结果窗格中，也可以在单独结果窗格中返回这些结果。 结果集可能包含额外的列，即每个服务器上的查询所使用的服务器名和登录名。 只能使用 Windows 身份验证来注册中央管理服务器和从属服务器。 可以使用 Windows 身份验证或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证来注册本地服务器组中的服务器。  
  
> [!NOTE]  
>  在执行以下过程之前，请先创建中央管理服务器和服务器组。 有关详细信息，请参阅[创建中央管理服务器和服务器组 (SQL Server Management Studio)](create-a-central-management-server-and-server-group.md)。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要执行对多个服务器，使用的语句：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="Security"></a> 安全性  
  
####  <a name="Permissions"></a> Permissions  
 由于中央管理服务器维护的连接是在用户的上下文中通过使用 Windows 身份验证执行的，因此它们在各个已注册的服务器上的有效权限可能有所不同。 例如，用户可能是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] A 实例上 sysadmin 固定服务器角色的成员，但仅具有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] B 实例的有限权限。  
  
##  <a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-execute-statements-against-multiple-configuration-targets-simultaneously"></a>同时对多个配置目标执行语句  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的 **“视图”** 菜单上，单击 **“已注册的服务器”** 。  
  
2.  展开一个中央管理服务器，右键单击某个服务器组，指向“连接”  ，然后单击“新建查询”  。  
  
3.  在查询编辑器中，键入并执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，例如：  
  
    ```  
    USE master  
    GO  
    SELECT * FROM sysdatabases;  
    GO  
    ```  
  
     默认情况下，结果窗格合并来自服务器组中所有服务器的查询结果。  
  
#### <a name="to-change-the-multiserver-results-options"></a>更改多服务器结果选项  
  
1.  在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]的 **“工具”** 菜单中，单击 **“选项”** 。  
  
2.  依次展开 **“查询结果”** 和 **“SQL Server”** ，然后单击 **“多服务器结果”** 。  
  
3.  在 **“多服务器结果”** 页上，指定所需的选项设置，然后单击 **“确定”** 。  
  
## <a name="see-also"></a>请参阅  
 [使用中央管理服务器管理多台服务器](../../relational-databases/administer-multiple-servers-using-central-management-servers.md)  
  
  
