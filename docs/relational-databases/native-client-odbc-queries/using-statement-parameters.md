---
title: 使用语句参数 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, parameters
- parameters [SQL Server Native Client], ODBC
- ODBC, parameters
- statements [ODBC], parameters
- parameter markers [ODBC]
- SQL Server Native Client ODBC driver, statements
- ODBC applications, statements
ms.assetid: 2427d886-ec6c-49d7-b0b6-0d998b64cdb9
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e1168c81d8eeb6a25b84f473858bc924f529a572
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "68058675"
---
# <a name="using-statement-parameters"></a>使用语句参数
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../includes/snac-deprecated.md)]

  参数在 SQL 语句中是一种变量，它使 ODBC 应用程序能够：  
  
-   有效地为表中的列提供值。  
  
-   在构造查询条件时增强用户交互。  
  
-   管理**文本**， **ntext**，并**图像**数据和[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-特定的 C 数据类型。  
  
 例如，**部件**表中包含名为的列**PartID**，**说明**，以及**价格**。 若要添加不带参数的部分，需要构造 SQL 语句，例如：  
  
```  
INSERT INTO Parts (PartID, Description, Price) VALUES (2100, 'Drive shaft', 50.00)  
```  
  
 尽管使用已知的值集插入一行时可接受此语句，但要求应用程序插入多行时使用此语句会很不适合。 ODBC 可通过使应用程序将 SQL 语句中的任何数据值替换为参数标记来解决此问题。 这种参数标记由问号 (?) 表示。 在下面的示例中，三个数据值被替换为参数标记：  
  
```  
INSERT INTO Parts (PartID, Description, Price) VALUES (?, ?, ?)  
```  
  
 参数标记然后被绑定到应用程序变量。 若要插入新行，应用程序只需设置变量的值并执行此语句。 然后，驱动程序检索变量的当前值并将其发送至数据源。 如果多次执行此语句，则应用程序可通过准备此语句使该进程更加有效。  
  
 每个参数标记均按从左到右的顺序由其分配至参数的序号引用。 在 SQL 语句中，最左侧的参数标记的序号值为 1；下一个的序号为 2，以此类推。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [绑定参数](../../relational-databases/native-client-odbc-queries/using-statement-parameters-binding-parameters.md)  
  
## <a name="see-also"></a>请参阅  
 [执行查询&#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
