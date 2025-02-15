---
title: XML 大容量加载 (SQLXML 4.0) 简介 |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- nontransacted XML Bulk Load operations
- XML Bulk Load [SQLXML], about XML Bulk Load
- bulk load [SQLXML], about bulk load
- transacted XML Bulk Load operations
- streaming XML data
ms.assetid: 38bd3cbd-65ef-4c23-9ef3-e70ecf6bb88a
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4c00dc0f155ed79cddd715aad96238cfe8723a10
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68046551"
---
# <a name="introduction-to-xml-bulk-load-sqlxml-40"></a>XML 大容量加载简介 (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  XML 大容量加载为独立的 COM 对象，可用于将半结构化的 XML 数据加载到 Microsoft[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]表。  
  
 您可以使用 INSERT 语句和 OPENXML 函数将 XML 数据插入到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库；但是，当需要插入大量 XML 数据时，大容量加载实用工具提供了更好的性能。  
  
 XML 大容量加载对象模型的 Execute 方法采用两个参数：  
  
-   带批注的 XML 架构定义 (XSD) 或 XML 数据精简 (XDR) 架构。 XML 大容量加载实用工具解释此映射架构和在标识要插入 XML 数据的 SQL Server 表时在该架构中指定的批注。  
  
-   XML 文档或文档片段（文档片段是没有单个顶级元素的文档）。 可以指定 XML 大容量加载可读取的文件名或流。  
  
 XML 大容量加载解释此映射架构，并标识要插入 XML 数据的表。  
  
 本部分假定您熟悉以下 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 功能：  
  
-   带批注的 XSD 架构和 XDR 架构。 有关带批注的 XSD 架构的详细信息，请参阅[带批注的 XSD 架构简介&#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md)。 有关带批注的 XDR 架构的信息，请参阅[带批注的 XDR 架构&#40;在 SQLXML 4.0 中不推荐使用&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md)。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 大容量插入机制，例如 [!INCLUDE[tsql](../../../includes/tsql-md.md)] BULK INSERT 语句和 bcp 实用工具。 有关详细信息，请参阅[BULK INSERT &#40;TRANSACT-SQL&#41; ](../../../t-sql/statements/bulk-insert-transact-sql.md)并[bcp 实用工具](../../../tools/bcp-utility.md)。  
  
## <a name="streaming-of-xml-data"></a>XML 数据的流式处理  
 由于源 XML 文档可能很大，因此无法将整个文档读入内存以进行大容量加载处理。 XML 大容量加载而是将 XML 数据解释为流并读取它。 当该实用工具读取数据时，该工具标识数据库表，并根据 XML 数据源生成相应记录，然后再将这些记录发送到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 以便插入。  
  
 例如，以下源 XML 文档组成 **\<客户 >** 元素和 **\<顺序 >** 子元素：  
  
```  
<Customer ...>  
    <Order.../>  
    <Order .../>  
     ...  
</Customer>  
...  
```  
  
 当 XML 大容量加载读取 **\<客户 >** 元素，则会生成 Customertable 一条记录。 当它读取 **\</Customer >** 结束标记时，XML 大容量加载将该记录到表中插入[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 在同一个说一句，当它读取 **\<顺序 >** 元素中，XML 大容量加载为 Ordertable，生成一条记录，然后插入到该记录[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]表在读取 **\</ 订购 >** 结束标记。  
  
## <a name="transacted-and-nontransacted-xml-bulk-load-operations"></a>事务和非事务 XML 大容量加载操作  
 XML 大容量加载可以以事务或非事务模式运行。 性能是如果您是在非事务模式下执行大容量加载通常最佳： 即，事务属性设置为 FALSE)，并且以下条件之一为 true:  
  
-   要向其大容量加载数据的表为空，且没有任何索引。  
  
-   表具有数据和唯一索引。  
  
 非事务方法不能保证在大容量加载进程发生错误时回滚（但是可以进行部分回滚）。 非事务大容量加载适用于数据库为空的情况。 因此，如果发生错误，您可以清除数据库并重新启动 XML 大容量加载。  
  
> [!NOTE]  
>  在非事务模式下，XML 大容量加载使用并提交默认的内部事务。 当事务属性设置为 TRUE 时，XML 大容量加载不调用此事务的提交。  
  
 如果事务属性设置为 TRUE，XML 大容量加载将创建临时文件，分别对应于在映射架构中标识每个表。 XML 大容量加载首先将源 XML 文档中的记录存储到这些临时文件中。 接着，[!INCLUDE[tsql](../../../includes/tsql-md.md)] BULK INSERT 语句检索这些文件中的上述记录，并将其存储到相应的表中。 可以使用 TempFilePath 属性来指定这些临时文件的位置。 您必须确保用于 XML 大容量加载的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 帐户有权访问此路径。 如果未指定 TempFilePath 属性，TEMP 环境变量中指定的默认文件路径用于创建临时文件。  
  
 如果事务属性设置为 FALSE （默认设置），XML 大容量加载使用大容量加载数据的 OLE DB IRowsetFastLoad 接口。  
  
 如果 ConnectionString 属性设置连接字符串，并且事务属性设置为 TRUE，XML 大容量加载会在其自己的事务上下文运行。 （例如，XML 大容量加载启动其自己的事务，并根据需要提交或回滚。）  
  
 如果 ConnectionCommand 属性将设置连接并与现有连接对象的事务属性设置为 TRUE，XML 大容量加载不会分别发出 COMMIT 或 ROLLBACK 语句在成功或失败。 如果出现错误，XML 大容量加载则返回相应的错误消息。 执行 COMMIT 或 ROLLBACK 语句由启动该大容量加载的客户端决定。 用于 XML 大容量加载的连接对象应是类型 ICommand 的也是 ADO 命令对象。  
  
 在 SQLXML 4.0 中，ConnectionObject 不能用于将事务属性设置为 FALSE。 因为它是根本不能在传入的会话上打开多个 IRowsetFastLoad 接口与 ConnectionObject 不支持非事务模式。  
  
  
