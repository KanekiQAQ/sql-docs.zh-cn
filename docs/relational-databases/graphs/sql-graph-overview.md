---
title: 图形处理与 SQL Server 和 Azure SQL 数据库 |Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, overview
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eb84f1cc40a05078910d10a48de67f1ac3467fe3
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68035897"
---
# <a name="graph-processing-with-sql-server-and-azure-sql-database"></a>图形处理与 SQL Server 和 Azure SQL 数据库
[!INCLUDE[tsql-appliesto-ss2017-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdb-xxxx-xxx-md.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供对多对多关系建模的图形数据库功能。 图形关系已集成到[!INCLUDE[tsql-md](../../includes/tsql-md.md)]和接收的使用好处[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]作为基础数据库管理系统。


## <a name="what-is-a-graph-database"></a>图形数据库是什么？  
图数据库是节点（或顶点）和边缘（或关系）的集合。 节点表示实体（例如，个人或组织），边缘表示其连接的两个节点（例如，赞或朋友）之间的关系。 节点和边缘都可能具有与之关联的属性。 下面是一些功能，使图形数据库唯一：  
-   边缘或关系的图形数据库中的第一类实体，并且可以具有特性或属性与之关联。 
-   单条边可以灵活地连接多个节点中的图形数据库。
-   可以轻松地表达模式匹配和多跃点导航查询。
-   可以轻松地表达传递闭包和多态查询。

## <a name="when-to-use-a-graph-database"></a>何时使用图形数据库

没有任何图形数据库可以实现，这不能使用关系数据库来实现。 但是，图形数据库可以进行更易于表示某种类型的查询。 此外，使用特定的优化操作，某些查询可能会更好地执行。 您决定选择哪一个可以基于以下因素：  
-   你的应用程序具有分层数据。 HierarchyID 数据类型可用于实现层次结构，但它具有一些限制。 例如，它不允许您存储多个父节点。
-   应用程序具有复杂的多对多关系;当应用程序升级后，将添加新关系。
-   您需要相互关联的数据和关系数据进行分析。

## <a name="graph-features-introduced-in-includesssqlv14includessssqlv14-mdmd"></a>中引入的图形功能 [!INCLUDE[sssqlv14](../../includes/sssqlv14-md.md)] 
我们开始将图形扩展添加到 SQL Server，为了简化存储和查询图形数据。 在第一个版本中引入以下功能。 


### <a name="create-graph-objects"></a>创建图形对象
[!INCLUDE[tsql-md](../../includes/tsql-md.md)] 扩展将允许用户创建节点或边界表。 节点和边缘可以具有与其相关联的属性。 由于以表的形式存储节点和边缘，支持对节点或边界表在关系表中支持的所有操作。 以下是示例：  

```   
CREATE TABLE Person (ID INTEGER PRIMARY KEY, Name VARCHAR(100), Age INT) AS NODE;
CREATE TABLE friends (StartDate date) AS EDGE;
```   

![朋友的 person 表](../../relational-databases/graphs/media/person-friends-tables.png "/people/person 节点和好友边缘表")  
以表的形式存储节点和边缘  

### <a name="query-language-extensions"></a>查询语言扩展  
新`MATCH`子句引入支持模式匹配和通过 graph 多跃点导航。 `MATCH`函数使用 ASCII 图文样式语法进行模式匹配。 例如：  

```   
-- Find friends of John
SELECT Person2.Name 
FROM Person Person1, Friends, Person Person2
WHERE MATCH(Person1-(Friends)->Person2)
AND Person1.Name = 'John';
```   
 
### <a name="fully-integrated-in-includessnoversionincludesssnoversion-mdmd-engine"></a>完全集成在[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]引擎 
图形扩展完全集成在[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]引擎。 使用相同的存储引擎、 元数据，查询处理器等存储和查询的图形数据。 跨关系图和单个查询中的关系数据的查询。 图形功能结合其他[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]等列存储中，高可用性，R services，技术等。SQL 图形数据库还支持所有的安全性和符合性功能适用于[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。
 
### <a name="tooling-and-ecosystem"></a>工具和生态系统

从现有的工具和生态系统中受益的[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]提供。 工具，例如备份和还原、 导入和导出，BCP 现成的工作。 其他工具或 SSIS、 SSRS、 或 Power BI 等服务会使用图形表，它们使用的关系表的方式。

## <a name="edge-constraints"></a>边缘约束
边缘约束定义图形边缘表上，对给定的边缘类型可以连接的节点表。 这为用户提供更好地控制其图形架构。 边缘约束的帮助的用户可以将限制给定的边缘可以连接的节点的类型。 

若要了解有关如何创建和使用边缘约束的详细信息请参阅[边缘约束](../../relational-databases/tables/graph-edge-constraints.md)

## <a name="merge-dml"></a>合并 DML 
[合并](../../t-sql/statements/merge-transact-sql.md)语句执行插入、 更新或删除基于与源表联接的结果对目标表的操作。 例如，您可以通过插入、 更新或删除基于目标表与源表之间的差异对目标表中的行同步两个表。 Azure SQL 数据库和 SQL Server vNext 现在支持在 MERGE 语句中使用匹配的谓词。 也就是说，现可以使用匹配谓词用于在单个语句，而不是单独的 INSERT/UPDATE/DELETE 语句中指定关系图的关系的新数据合并当前图形数据 （节点或边界表）。

若要了解有关如何在合并 DML 中使用匹配项的详细信息请参阅[MERGE 语句](../../t-sql/statements/merge-transact-sql.md)

## <a name="shortest-path"></a>最短路径
[SHORTEST_PATH](./sql-graph-shortest-path.md)函数查找在关系图或从给定的节点开始到关系图中的所有其他节点中任何 2 个节点之间最短路径。 最短路径还可以用于查找可传递闭包或针对任意长度遍历图形中。 

 ## <a name="next-steps"></a>后续步骤  
读取[SQL 图形数据库-体系结构](./sql-graph-architecture.md)
   

