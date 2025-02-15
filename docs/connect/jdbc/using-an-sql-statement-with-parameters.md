---
title: 使用带参数的 SQL 语句 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 3202b88f-ce13-44dd-982c-c6a3b0260378
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6716f670c42172f1721968124d7113e8f1936ddc
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67916377"
---
# <a name="using-an-sql-statement-with-parameters"></a>使用带参数的 SQL 语句

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

若要使用包含 IN 参数的 SQL 语句处理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中的数据，可使用 [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) 类的 [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverpreparedstatement.md) 方法返回包含所请求数据的 [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md)。 若要执行此操作，必须首先使用 [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md) 类的 [prepareStatement](../../connect/jdbc/reference/preparestatement-method-sqlserverconnection.md) 方法创建一个 SQLServerPreparedStatement 对象。

构造 SQL 语句时，可使用 ?（问号）字符指定 IN 参数， 该问号将用作随后传递到 SQL 语句中的参数值的占位符。 若要指定参数的值, 可以使用 SQLServerPreparedStatement 类的 setter 方法之一。 使用的 setter 方法由要传递到 SQL 语句中的值的数据类型确定。

向 setter 方法传递值时，不仅需要指定要在 SQL 语句中使用的实际值，还必须指定参数在 SQL 语句中的序数位置。 例如, 如果 SQL 语句包含单个参数, 则其序数值将为1。 如果该语句包含两个参数，则第一个序数值为 1，第二个序数值为 2。

在下面的实例中，将向此函数传递 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] 示例数据库的一个打开连接，并会构造一条预定义 SQL 语句，使用一个 String 参数值运行该语句，然后从结果集中读取结果。

[!code[JDBC#UsingSQLWithParams1](../../connect/jdbc/codesnippet/Java/using-an-sql-statement-w_1_1.java)]

## <a name="see-also"></a>另请参阅

[使用 SQL 语句](../../connect/jdbc/using-statements-with-sql.md)
