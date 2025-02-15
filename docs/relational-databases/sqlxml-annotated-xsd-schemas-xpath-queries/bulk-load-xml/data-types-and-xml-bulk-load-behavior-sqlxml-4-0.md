---
title: 数据类型和 XML 大容量加载行为 (SQLXML 4.0) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- bulk load [SQLXML], data types
- data types [SQLXML], XML Bulk Load
- XML Bulk Load [SQLXML], data types
ms.assetid: d1ac1939-1f6c-4398-b7a7-a79ca608a4f1
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 820d2b083544542d1c1414f978105fe992b0ce36
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2019
ms.locfileid: "67915152"
---
# <a name="data-types-and-xml-bulk-load-behavior-sqlxml-40"></a>数据类型和 XML 大容量加载行为 (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  在映射架构中指定的数据类型 (XSD 或 XDR 类型和**sql: datatype**) 通常被忽略，在以下情况下除外：  
  
 在 XSD 中：  
  
-   如果类型为**dateTime**或**时间**，则必须指定**sql: datatype**因为 XML 大容量加载将执行数据转换之前将数据发送到 Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   当进行大容量加载的列**uniqueidentifier**中键入[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]XSD 值是一个 GUID，包括大括号 （{和}），您必须指定**sql: datatype ="uniqueidentifier"** 到值插入列前删除大括号。 如果**sql: datatype**未指定，则将括在括号内发送的值并且插入失败。  
  
 有关详细信息**sql: datatype**，请参阅[数据类型强制转换和 sql: datatype 批注&#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/data-type-coercions-and-the-sql-datatype-annotation-sqlxml-4-0.md)。  
  
 在 XDR 中：  
  
-   如果**dt: type**是**datetime**，**时间**， **dateTime.tz**，或**time.tz**，则必须同时指定**dt: type**并**sql: datatype**数据类型，因为 XML 大容量加载会发送到的数据之前执行数据转换[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。  
  
-   如果您的 XML 数据的类型**uuid**， **sql: datatype**必须指定;**dt: type ="uuid"** 也是必需的如果数据不是字符串数据。 如果未指定**dt:uuid**，XML 大容量加载接受带有大括号的字符串 （并根据需要删除）。  
  
-   如果 XML 数据**bin.base64**或**bin.hex**，必须指定 XML 数据类型与**dt: type**。 XML 大容量加载然后将数据以十六进制形式加载到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。  
  
  
