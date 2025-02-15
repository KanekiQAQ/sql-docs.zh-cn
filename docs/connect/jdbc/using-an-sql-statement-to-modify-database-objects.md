---
title: 使用 SQL 语句修改数据库对象 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: f49ea499-df3c-4e85-9fc7-450fb99622a6
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 28c71784f8e51600aef111649b12f81b5878b324
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67916387"
---
# <a name="using-an-sql-statement-to-modify-database-objects"></a>使用 SQL 语句修改数据库对象

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

若要使用 SQL 语句修改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象，可以使用 [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md) 类的 [executeUpdate](../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md) 方法。 executeUpdate 方法会将此 SQL 语句传递给数据库进行处理，然后返回值 0（因为所有行都不受影响）。

若要执行此操作，必须首先使用 [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) 类的 [createStatement](../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md) 方法创建一个 SQLServerStatement 对象。

> [!NOTE]  
> 修改数据库中对象的 SQL 语句称为“数据定义语言 (Data Definition Language, DDL)”语句。 `CREATE TABLE`其中包括`DROP TABLE` 、、和`DROP INDEX`之类的语句。 `CREATE INDEX` 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的 DDL 语句类型的详细信息，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书。

在下面的实例中，将向此函数传递 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] 示例数据库的打开连接，并构造一条用于在数据库中创建简单的 TestTable 的 SQL 语句，然后运行该语句并显示返回值。

[!code[JDBC#UsingSQLToModifyDBObjects1](../../connect/jdbc/codesnippet/Java/using-an-sql-statement-t_0_1.java)]

## <a name="see-also"></a>另请参阅

[使用 SQL 语句](../../connect/jdbc/using-statements-with-sql.md)
