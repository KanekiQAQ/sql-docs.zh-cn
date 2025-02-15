---
title: 核心 SQLXML 的安全注意事项 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- security [SQLXML], about security
ms.assetid: 330cd2ff-d5d5-4c8e-8f93-0869c977be94
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d739eb3d4a9a1466a9eef441d683aa0acd81d0a7
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68026932"
---
# <a name="core-sqlxml-security-considerations"></a>SQLXML 核心安全性注意事项
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  以下是使用 SQLXML 进行数据访问的安全性准则。  
  
-   Sqlxmloledb 访问接口公开**StreamFlags**属性，它允许您设置标志，指示应启用或已为每个特定实例禁用哪些 SQLXML 功能。 您可以使用此属性自定义对 SQLXML 的使用，并确保只启用所需组件。 有关详细信息，请参阅[sqlxmloledb 访问接口&#40;SQLXML 4.0&#41;](https://msdn.microsoft.com/library/fc489682-690a-4bb0-b5ac-237d376dc110)。  
  
-   当出现 SQLXML 错误并返回这些错误时，它们可以包含有关数据库架构的信息，如表名、列名或类型信息。 处理这些错误时应非常小心，以使用户无法轻易发现不打算使用或不需要使用的有关您的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装的信息。  
  
-   当使用 SQLXML 查询或向 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 发送更新时，SQLXML 未限制可以交换的数据量，也没有在尝试处理 SQLXML 负载中的数据之前对其大小进行任何检查。 使用 SQLXML 开发应用程序时，您有责任确保系统上有足够的内存来处理该数据。 例如，查询服务器上的数据时，应验证客户端上是否具有足够的内存空间来接收该数据。 同样，如果向服务器加载数据，则需要验证该服务器上是否具有足够的可用内存来处理该数据，并且该服务器上是否具有足够的可用磁盘存储空间来存储该数据。  
  
-   SQLXML 动态生成 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 查询和更新命令，并将它们发送到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 执行。 这是 SQLXML 查询和更新服务器的唯一方式。 结果将以 XML 流或行集的形式接收。  
  
-   当接收查询结果时，SQLXML 不会根据收到的数据内容执行任何操作。 不会根据数据类型或内容执行额外的处理。 该数据永远不会作为执行操作所使用的代码处理。  
  
-   执行 XML 模板时，SQLXML 将提交的模板内包含的 XPath 和 DBObject 查询转换为 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 命令，再针对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 执行这些命令。 这些命令仅影响现有数据。 SQLXML 生成的命令永远不会更改数据库的结构。 用户必须执行显式命令才能更改数据库结构。 例如，通过包括在**sql:query**块的模板。  
  
-   当通过映射文件执行 DBObject 查询和 XPath 语句时，SQLXML 不会以任何方式更改数据库中的数据。  
  
-   SQLXML 可以根据 XML 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据模型之间的差异更改给定数据的格式。 例如，二者指定时间所采用的格式各不相同。 SQLXML 将尝试解析这些差异。 因此，可能会丢失某些精度信息。  
  
-   SQLXML 未限制其处理数据所需的时间长短。 处理将继续进行，直到出现错误或处理完成为止。  
  
-   SQLXML 未能写入文件系统。 如果用户希望保存从数据库检索到的数据，则必须使用用户自己的代码来执行此操作。  
  
-   SQLXML 允许用户对数据库执行所需的任何 SQL 查询。 此功能决不能向不安全或不受控制的源公开，因为这实质上是公开 SQL 数据库，而不限定任何用户。  
  
-   执行 Updategram 时，SQLXML 将转换**updg:sync**针对 DELETE、 UPDATE 和 INSERT 命令块[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例。 这些命令仅影响现有数据。 SQLXML 生成的命令永远不会更改数据库。 用户必须执行显式命令才能更改数据库结构。 例如，通过包括在**sql:query**块的模板。  
  
-   执行 DiffGram 时，SQLXML 根据 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例将 DiffGram 转换为 DELETE、UPDATE 和 INSERT 命令。 这些命令仅影响现有数据。 SQLXML 生成的命令永远不会更改数据库。 用户必须执行显式命令才能更改数据库结构。 例如，通过包括在**sql:query**块的模板。  
  
## <a name="see-also"></a>请参阅  
 [SQLXML 4.0 安全注意事项](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/sqlxml-4-0-security-considerations.md)  
  
  
